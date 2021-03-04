---
description: Futex kullanarak mutex implemente edilmesi.
---

# Mutex

Mutex ve Condition değişkenleri spinlock ve spin read\_write lock'lardan ayrılmaktadır. Çünkü thread'ler uyku durumuna geçmek için sistem çağrışarı vasıtasıyla kernel ile ile irtibata geçmektedirler. Sistem çağrıları, atomik işlemlere göre daha yavaş olduğu için sistem çağrısına ihtiyaç kalmadan user seviyesinde mutex için gerekli işlemleri gerçekleştirmek futex "API" nın ana hedefidir.

Mutex ve Condition değişkenleri spinlock ve spin read\_write lock'lardan ayrılmaktadır. Çünkü thread'ler uyku durumuna geçmek için sistem çağrışarı vasıtasıyla kernel ile ile irtibata geçmektedirler. Sistem çağrıları, atomik işlemlere göre daha yavaş olduğu için sistem çağrısına ihtiyaç kalmadan user seviyesinde mutex için gerekli işlemleri gerçekleştirmek futex "API" nın ana hedefidir.

Futex'in implementasyonu oldukça zordur. Bir çok race condition durumu meydana gelebilmektedir. Dolayısıyla mutex ve condition değişkenleri implemente edilmeden önce temel çalışma prensipleri dikkatlice incelenmelidir.

Ulrich Drepper yazdığı makale ile bu konudaki zorlukları örneklerle göstermiştir.\([Futexes Are Tricky](http://www.akkadia.org/drepper/futex.pdf)\)

{% hint style="info" %}
h
{% endhint %}





