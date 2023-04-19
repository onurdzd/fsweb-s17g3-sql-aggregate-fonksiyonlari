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
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
SELECT ograd,ogrsoyad, atarih FROM ogrenci, islem WHERE ogrenci.ogrno = islem.ogrno;	
	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
SELECT kitapadi,turadi from kitap,tur where kitap.turno=tur.turno and (trim(turadi)="Fıkra" or trim(turadi)="Hikaye")

	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
SELECT o.ogrno,ograd,ogrsoyad,kitapadi from ogrenci as o,islem as i,kitap as k where o.ogrno=i.ogrno and i.kitapno=k.kitapno and (sinif="10B"  or sinif="10C")	

	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	 SELECT ograd,ogrsoyad,atarih from ogrenci as o INNER JOIN islem as i on i.ogrno=o.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	 SELECT ograd,ogrsoyad,atarih from ogrenci as o INNER JOIN islem as i on i.ogrno=o.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno INNER JOIN tur as t on t.turno=k.turno where trim(turadi)="Fıkra" or trim(turadi)="Hikaye"
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	 SELECT ograd,ogrsoyad,kitapadi from ogrenci as o INNER JOIN islem as i on i.ogrno=o.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno INNER JOIN tur as t on t.turno=k.turno where sinif="10B" or sinif="10C" group by ograd,ogrsoyad,kitapadi  
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	 SELECT ograd,ogrsoyad,atarih from ogrenci as o INNER JOIN islem as i on i.ogrno=o.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno INNER JOIN tur as t on t.turno=k.turno 
	
	8) Kitap almayan öğrencileri listeleyin.
	
	 SELECT ograd,ogrsoyad from ogrenci as o INNER JOIN islem as i on i.ogrno=o.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno INNER JOIN tur as t on t.turno=k.turno where atarih is null

	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
 SELECT kitapadi,k.kitapno,Count(atarih) from kitap as k INNER JOIN islem as i on i.kitapno=k.kitapno where atarih is not null group by k.kitapno, kitapadi order by k.kitapno 
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
SELECT kitapadi,k.kitapno,Count(atarih) from kitap as k INNER JOIN islem as i on i.kitapno=k.kitapno group by k.kitapno, kitapadi order by k.kitapno

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
SELECT ograd,ogrsoyad,kitapadi from ogrenci as o INNER JOIN islem as i on i.ogrno=o.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno INNER JOIN tur as t on t.turno=k.turno
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
SELECT ograd,ogrsoyad,kitapadi,atarih,yazarad,yazarsoyad,turadi from ogrenci as o INNER JOIN islem as i on i.ogrno=o.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno INNER JOIN tur as t on t.turno=k.turno INNER JOIN yazar as y on y.yazarno=k.yazarno	
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
SELECT ograd,ogrsoyad,Count(atarih) from ogrenci as o INNER JOIN islem as i on i.ogrno=o.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno where sinif="10A" or sinif="10B" group by ograd,ogrsoyad 	
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
SELECT AVG(sayfasayisi) from kitap

	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
SELECT kitapadi from kitap where sayfasayisi>(SELECT AVG(sayfasayisi) from kitap)	
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
SELECT Count(ogrno) from ogrenci	
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
SELECT Count(ogrno) as toplam_sayi from ogrenci	
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
SELECT Count(distinct ograd) as toplam_sayi from ogrenci	
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
select Max(sayfasayisi) from kitap	
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
SELECT kitapadi,Max(sayfasayisi) from kitap where sayfasayisi=(select Max(sayfasayisi) from kitap) group by kitapadi	
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
SELECT Min(sayfasayisi) from kitap
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
SELECT kitapadi,Min(sayfasayisi) from kitap where sayfasayisi=(select Min(sayfasayisi) from kitap) group by kitapadi	
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
 SELECT Max(sayfasayisi) from kitap as k INNER JOIN tur as t on t.turno=k.turno  where trim(turadi)="Dram"	
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
 SELECT o.ogrno,SUM(sayfasayisi) from kitap as k INNER JOIN islem as i on i.kitapno=k.kitapno INNER JOIN ogrenci as o on o.ogrno=i.ogrno where o.ogrno=15 group by ogrno
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
 SELECT ograd,Count(ograd) from ogrenci group by ograd
	
	26) Her sınıftaki öğrenci sayısını bulunuz.
 SELECT sinif,Count(ogrno) from ogrenci group by sinif
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
 SELECT sinif,cinsiyet,Count(ogrno) from ogrenci group by sinif,cinsiyet
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
SELECT ograd,ogrsoyad,sum(sayfasayisi) from ogrenci as o INNER JOIN islem as i on o.ogrno=i.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno group by ograd,ogrsoyad	
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.
SELECT ograd,ogrsoyad,count(atarih) from ogrenci as o INNER JOIN islem as i on o.ogrno=i.ogrno INNER JOIN kitap as k on k.kitapno=i.kitapno group by ograd,ogrsoyad	