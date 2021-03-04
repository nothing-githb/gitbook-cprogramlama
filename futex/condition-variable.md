---
description: Koşul Değişkenleri
---

# Condition Variable

Koşul değişkenlerinin temel fonksiyonu aşağıdaki 3 aşamada özetlenebilir:

1. Atomic olarak mutex'in kilitini açar ve süreli bekleme durumuna geçer. Süre dolmasıyla veya signal gelmesiyle uyanır ve mutex'i kilitler.
2. Uyuyan thread'lerden birini uyandır.
3. Uyuyan thread'lerin hepsini uyandır.

## Futex

Genellikle thread senkronizasyonu için  user-space'de bulunan programlar sistem çağrılarını kullanırlar\(eğer thread'ler user-space'de implemente edildiyse context-switch için de sistem çağrısı yapılacaktır\). Eğer bir thread bloke olarak uyuyacaksa veya başka bir thread uyandırılacaksa sistem çağrısı yapmaktan başka çareniz bulunmamaktadır. Daha iyi performans almak ve sistem çağrısına ihtiyaç olmayacak durumlarda atomik işlemler kullanılmaktadır.

Örneğin uncontended durumda mutex'i kilitlemek sistem çağrısı gerektirmemektedir. Ya da eğer başka bir thread beklemiyorsa mutex'in kilidini açmak sistem çağrısı gerektirmemektedir.

### Futex Operations

glibc direk olarak futex'i kullandırmak yerine POSIX thread ve POSIX semaphore'ları ile futex özelliklerini sağlar.

Aslında futex bir çok işlevi gerçekleştiren tek bir sistem çağrısıdır. 

Futex'in işlevlerinden sadece bir kaç tanesini inceleyeceğiz.

#### FUTEX\_WAIT

```c
#include <stdatomic.h>
#include <time.h>
#include <linux/futex.h>

int futex_wait(atomic_int *addr, int val, const struct timespec *to)
{
        return sys_futex(addr, FUTEX_WAIT_PRIVATE, val, to, NULL, 0);
}
```

Futex fonksiyonu **FUTEX\_WAIT**_**\_**_**PRIVATE** parametresi ile çağrıldığında user-space'de mevcut _addr_ adresindeki değer ile _val_ değeri kontrol edilir ve aynı iseler çağrıyı yapan thread bloke olur. _to_ parametresi _NULL_ ise süresiz değilse süreli olarak bloke olmaktadır. 

**FUTEX\_WAIT**_**\_**_**PRIVATE** == **\(FUTEX\_WAIT \| FUTEX\_PRIVATE\_FLAG\)** 

**FUTEX\_PRIVATE\_FLAG** bayrağı ise _addr_ parametresinin çağrıyı yapan process'e görünür olduğu anlamına gelmektedir. Dolayısıyla çekirdek sanal adres dönüşümünü gerçekleştirme ihtiyacı duymayacaktır.

#### FUTEX\_WAKE

```c
int futex_wake(atomic_int *addr, int nr)
{
    return sys_futex(addr, FUTEX_WAKE_PRIVATE, nr, NULL, NULL, 0);
}

```

 Bu çağrı nr parametresi ile belirtilen sayıda thread uyandırır. Genelde 1 veya INT\_MAX parametreleri kullanılır. Ağaşıda örnek verilmiştir:

```c
int futex_signal(atomic_int *addr)
{
        return (futex_wake(addr, 1) >= 0) ? 0 : -1;
}

int futex_broadcast(atomic_int *addr)
{
        return (futex_wake(addr, INT_MAX) >= 0) ? 0 : -1;
}
```

**FUTEX\_WAKE**_**\_**_**PRIVATE** == **\(FUTEX\_WAKE \| FUTEX\_PRIVATE\_FLAG\)** 

## Futex ile Koşul Değişkenleri Uygulanması

Öncesinde mutex implementasyonumuz olduğunu varsayarak koşul değişkenlerini futex ile implemente etmeye çalışacağız.

### İlk örnek

```c
typedef struct cnd
{
        atomic_int value;
} cnd_t;

/* Our static initializer */
#define CND_INITIALIZER_NP { ATOMIC_VAR_INIT(0) }

int cnd_init(cnd_t *cv)
{
        atomic_init(&cv->value, 0);
        return thrd_success;
}

void cnd_destroy(cnd_t *cv)
{
        (void) cv;
}

int cnd_wait(cnd_t *cv, mtx_t *mtx)
{
        mtx_unlock(mtx),
        futex_wait(&cv->value, 0, NULL);
        mtx_lock(mtx);
        return thrd_success;
}

/* cnd_timedwait() omitted for simplicity */

int cnd_signal(cnd_t *cv)
{
        futex_signal(&cv->value);
        return thrd_success;
}

int cnd_broadcast(cnd_t *cv)
{
        futex_broadcast(&cv->value);
        return thrd_success;
}
```



