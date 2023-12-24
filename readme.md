# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın.

1. Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

SELECT ograd,ogrsoyad,islem.atarih from ogrenci, islem WHERE ogrenci.ogrno=islem.ogrno;

1.  Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

        SELECT kitapadi, turadi
        FROM kitap, tur
        WHERE kitap.turno = tur.turno
        turadi IN("Fıkra","Hikaye");

    1. 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

    SELECT ogrenci.ogrno,ogrenci.ograd,ogrenci.ogrsoyad,kitap.kitapadi
    FROM ogrenci,kitap,islem
    WHERE ogrenci.ogrno=islem.ogrno
    AND kitap.kitapno=islem.kitapno
    AND ogrenci.sinif IN("10B","10C");

    #join ile yazın 4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    SELECT ogrenci.ograd,ogrenci.ogrsoyad,islem.atarih
    FROM ogrenci
    JOIN islem ON ogrenci.ogrno=islem.ogrno;

    2. 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

    SELECT ogrenci.ogrno,ogrenci.ograd,ogrenci.ogrsoyad,kitap.kitapadi FROM islem
    JOIN kitap ON kitap.kitapno=islem.kitapno
    JOIN ogrenci ON ogrenci.ogrno=islem.ogrno
    AND ogrenci.sinif IN("10B","10C");

    3. Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

    SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci as o
    LEFT JOIN islem as i
    ON o.ogrno=i.ogrno
    ORDER BY i.atarih ASC;

    4. Kitap almayan öğrencileri listeleyin.

    SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci AS o
    LEFT JOIN islem AS i
    ON o.ogrno=i.ogrno
    WHERE i.atarih IS NULL;

    5. Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

    SELECT i.kitapno,k.kitapadi,COUNT(i.kitapno) AS TOTAL
    FROM islem AS i
    INNER JOIN kitap AS k
    ON i.kitapno=k.kitapno
    GROUP BY i.kitapno, k.kitapadi
    ORDER BY k.kitapno ASC

    6. Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

    SELECT i.kitapno,k.kitapadi,COUNT(i.kitapno) AS TOTAL
    FROM kitap AS k
    LEFT JOIN islem AS i
    ON i.kitapno=k.kitapno
    GROUP BY i.kitapno, k.kitapadi
    ORDER BY TOTAL ASC

    7. Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

    SELECT o.ograd,o.ogrsoyad,k.kitapadi FROM ogrenci AS o
    INNER JOIN islem AS i
    ON o.ogrno=i.ogrno
    INNER JOIN kitap AS k
    ON i.kitapno=k.kitapno;

    8. Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

       SELECT o.ograd,o.ogrsoyad,k.kitapadi, y.yazarad,y.yazarsoyad,t.turadi,i.atarih
       FROM ogrenci o
       LEFT JOIN islem i
       ON o.ogrno=i.ogrno
       LEFT JOIN kitap k
       ON i.kitapno=k.kitapno
       LEFT JOIN yazar y
       ON y.yazarno=k.yazarno
       LEFT JOIN tur t
       ON t.turno=k.turno
       ORDER BY k.kitapno

    9. 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

       SELECT o.sinif, o.ograd, o.ogrsoyad, COUNT(\*) AS kitap_sayisi
       FROM ogrenci o
       INNER JOIN islem i
       ON o.ogrno = i.ogrno
       WHERE o.sinif IN ("10A", "10B")
       GROUP BY o.sinif,o.ograd, o.ogrsoyad
       ORDER BY kitap_sayisi DESC;

    10. Tüm kitapların ortalama sayfa sayısını bulunuz.
        #AVG

        SELECT AVG(k.sayfasayisi) FROM kitap k

    11. Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

        SELECT \* FROM kitap k
        WHERE k.sayfasayisi > (select AVG(k.sayfasayisi)
        FROM kitap k)

    12. Öğrenci tablosundaki öğrenci sayısını gösterin

        SELECT COUNT(ogrno) FROM ogrenci;

    13. Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

    SELECT COUNT(ogrno) as ToplamSayi FROM ogrenci;

    14. Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

    SELECT COUNT(DISTINCT ograd) FROM ogrenci

    15. En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

        SELECT sayfasayisi FROM kitap
        ORDER BY sayfasayisi DESC
        LIMIT 1;

        SELECT MAX(sayfasayisi) FROM kitap

    1.  En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

        SELECT k.kitapadi, MAX(k.sayfasayisi) AS max_sayfasayisi
        FROM kitap k
        GROUP BY k.kitapadi
        LIMIT 1

    1.  En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

        SELECT MIN(sayfasayisi) FROM kitap

    1.  En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

        SELECT k.kitapadi, MIN(k.sayfasayisi) AS min_sayfasayisi
        FROM kitap k
        GROUP BY k.kitapadi
        ORDER BY min_sayfasayisi ASC
        LIMIT 1;

    1.  Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

        SELECT MAX(k.sayfasayisi) FROM kitap k
        INNER JOIN tur t
        ON t.turno=k.turno
        WHERE t.turadi="Dram";

    1.  numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

        SELECT o.ograd,k.kitapadi ,SUM(k.sayfasayisi) AS TOPLAM_SAYFA
        FROM ogrenci o
        INNER JOIN islem i
        ON o.ogrno=i.ogrno
        INNER JOIN kitap k
        ON i.kitapno=k.kitapno
        WHERE o.ogrno=15
        GROUP BY o.ograd,k.kitapadi

    1.  İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

        SELECT o.ograd, COUNT(o.ogrno) from ogrenci o
        GROUP BY o.ograd;

    1.  Her sınıftaki öğrenci sayısını bulunuz.

        SELECT o.sinif, COUNT(o.ogrno) from ogrenci o
        GROUP BY o.sinif;

    1.  Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

        SELECT o.cinsiyet, COUNT(o.ogrno) from ogrenci o
        GROUP BY o.cinsiyet;

    1.  Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

        SELECT o.ograd,o.ogrsoyad, SUM(k.sayfasayisi) AS SUM_READ_BOOK FROM ogrenci o
        INNER JOIN islem i
        ON o.ogrno=i.ogrno
        INNER JOIN kitap k
        ON i.kitapno=k.kitapno
        GROUP BY o.ograd,o.ogrsoyad
        ORDER BY SUM_READ_BOOK DESC;

    1.  Her öğrencinin okuduğu kitap sayısını getiriniz.
        SELECT o.ograd,o.ogrsoyad, COUNT(i.islemno) AS COUNT_READ_BOOK
        FROM ogrenci o
        INNER JOIN islem i
        ON o.ogrno=i.ogrno
        GROUP BY o.ograd, o.ogrsoyad
        ORDER BY COUNT_READ_BOOK DESC;
