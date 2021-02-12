---
description: >-
  Genel olarak programlar "Sistem Yaşam Döngüsü"(System Life Cycle) adı verilen
  bir süreç içerisinden geçerler. Bu sürecin aşamaları aşağıdaki gibi
  tanımlanabilir.
---

# Veri Yapıları

## 1. Gereksinimler

Bütün programlar, projenin amacını belirten şartların tanımlanması ile başlar. Gereksinimler, programcıya verilen girdiler\(input\) ve bu girdiler sonucunda üretilmesi gereken çıktının\(output\) ne olacağı sorusuyla tanımlanır.

## 2. Analiz - Çözümleme

Sistemin gereksinimlerini belirledikten sonra ilk yapılacak işlerden biri problemi analiz etmektir. İki yolu vardır:

* **Bottom-up \(Aşağıdan yukarıya\):** Problemin parçalara bölünerek her bir parçanın üzerinde odaklanılır.
* **Top-down \(Yukarıdan aşağıya\):** Proje konusunda bir **master plan** yapılır. Projenin amacı nedir, son ürün ne olacak sorularıyla problem yönetilebilir.  Problem alt parçalara bölünür. Bu safhada ve bu yaklaşımda çeşitli teknikler söz konusudur.

## 3. Tasarım

Bu aşama analiz aşamasındaki çalışmaların devamıdır. Tasarımcı, veri objelerini \(_data objects_\) ve bu objelerin arasındaki olası işlemleri bu safhada tanımlar. Veri objeleri, soyut veri türünün \(_abstract data type_\) tanımlanmasını; işlemler ise algoritmaların tanımlanmasını gerektirir. Her ikisi de \(veri objeleri, işlemler\) **programlama dilinden bağımsızdır**. \(_language independent_\)

Örneğin bir öğrenci veri kütüğünün  \(öğrenci veri objesi\) içermesi gereken alt verileri belirleriz, fakat bu kütük için belirli bir gerçekleştirimi yapmamış olabiliriz. Diğer bir deyişle kodlama ayrıntılarına yer vermemiş olabiliriz. \(dizi, bağlaçlı liste, ağaç veri yapısı, vb.\)

Gerçekleştirme erteleme ile **daha etkili gerçekleştirimi seçme** fırsatı yakalamamız olasıdır.

> Kodlamayı ne kadar geciktirirsen, arka planda o kadar iyi bir metot yakalayabilirsin.
>
> #### Mustafa Ege

## 4. İnceltme ve Kodlama

Belirlenen veri objeleri üzerinde işlem yapacak algoritmaları bu aşamada kodlarız. **Veri objesi gösterimi**, algoritma etkinliğini belirlemede önemli rol oynayabilir. Benzer bir projede çalışmış bir arkadaşımızla yapacağımız sohbet veya ürettiğimiz alternatif çözümlerden biri, çözüm için en iyi yaklaşımı verebilir.

## 5. Doğrulama

Bu safhada, geliştirilen programın doğruluğunun ispatı, geniş bir veri grubu üzerinde test etme ve hatalardan arındırma işlemleri gerçekleştirilir. Bunların her biri bir araştırma konusudur.

* **Correctness proofs:** Matematikte bazı teknikleri kullanarak programın doğruluğu ispatlanabilir. Fakat bu işlem büyük projeler için hem zordur, hem de zaman alıcıdır. Büyük projeler için baştan sona bir ispat geliştirmek zaman kısıtlayıcısı nedeniyle neredeyse imkansızdır. Daha önceden doğruluğu ispatlanmış algoritmaları kullanmak, hata sayısını azaltabilir.
* **Testing:** Algoritmayı bir programlama diliyle kodlama ihtiyacı yok iken, kodlama safhası öncesi ve kodlama safhası sırasında doğrulama ispatlarını yapabiliriz. Fakat test etme işlemi için çalışan bir kod ve test verisine bu aşamada gereksinim vardır. Test verisi, programa ait tüm olası senaryoları içerecek biçimde hazırlanmaya çalışılır. Acemi programcılar, programı sözdizimi hatası \(_syntax error_\) vermeden çalışmışsa, programın doğru olduğunu zannederler. İyi bir test verisi, programın her kesiminin doğru olarak çalıştığını onaylamalıdır. Bir programın hatadan arındırılmış \(_error-free_\) bir program olmasının yanı sıra programın işletim zamanı \(_running-time_\) da önemlidir. Hatadan arındırılmış, fakat yavaş çalışan bir programın da fazla değeri yoktur.
* **Error removal:** Doğruluk ispatı ve sistem testleri hatalı kod ile uğraştığımıza işaret ederse tasarım ve kodlama kararlarına bağlı olarak bu hataları yok edebiliriz.

#### Kaynaklar

* Mustafa Ege - Veri Yapıları Ders Notları

