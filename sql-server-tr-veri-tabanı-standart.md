# Karkas SQL Server Veri tabanı Standartları

## Kurulum Notları

Veritabanı kurulurken, SQL Server içinden Collation olarak  Turkish_CI_AS seçilmelidir. 
Nchar unicode olarak değerleri tuttuğu için 2 katı yer kaplamakta  ve daha yavaş çalışmaktadır. 
Eğer veritabanında aynı anda iki dil bilgisi tutulmayacaksa, yani hem rusça hem de türkçe bilgi, yazı değerleri veri tabanında  varchar yada char olarak tutulmalıdır.


## Şema (Schema) Kavramı

SQL Server şema (schema) kavramını en iyi şekilde uygulayan veri tabanları arasındadır.
Bu yüzden schema yapısının SQL server veri tabanı tasarımı yapılırken kullanılması tavsiye edilir.
Bu yapıda her ana modul için, "bir ana şema" + "bir tanım tablosu şema"'sı açılması tavsiye edilmektedir.
Referential  Integrity korunması açısından bütün uygulama tek bir veritabanında çalışmalıdır.

Örneğin ihtiyaç duyduğunuz modullerin Insan Kaynakları, Muhasebe ve Bordro olduğunu düşünelim. 
Bu yapıda aşağıdaki şemaların açılması uygun olacaktır.


- ORTAK
- TT_ORTAK
- MUHASEBE
- TT_MUHASEBE
- INSAN_KAYNAKLARI
- TT_INSAN_KAYNAKLARI
- BORDRO
- TT_BORDRO
- OZLUK_BILGILERI
- TT_OZLUK_BILGILERI
- FIRMALAR
- TT_FIRMALAR




Burada muhasebe, insan kaynakları ve bordro şemaları zaten ihtiyaç duyulan şemalardır.
Bunlara ek olarak kişi bilgilerinin tutulması için ad, soyad, kimlik bilgileri etc ORTAK şeması önerilmektedir. 
TT_ORTAK şeması buradaki bilgiler için gerekli tanım tablolarını tutacaktır.
Örneğin TT_ORTAK.CINSIYET ismindeki bir tanım tablosu cinsiyet bilgileri tutabilir.
Burada Kisi için tutulacak bilgiler herkes için kesin tutulması gereken bilgilerdir. 
Örneğin ad soyad, tc kimlik no, resim gibi. 
Ama eger sadece yazılımı kullanan kurumdaki, kişilere ait  (örneğin kurum sicil no, kurum eposta, kurum telefon) gibi bilgiler  var ise  bunların farklı şemada tutulması daha yararlı olacaktır. 

Buraya FIRMALAR şeması muhasebe firmalarının fatura kesecekleri firma bilgileri için eklenmiştir.
Firma, Kisi bilgileri bir çok modul tarafından ihtiyaç duyulan bilgiler oldukları için bunların ayrı şemalarda tutulmaları daha yararlı olacaktır.

Son olarak Ozluk bilgileri, Bordro ve insan kaynaklarının ihtiyaç duydukları ortak bilgileri taşıyacaktır.
Burada bu bilgiler sadece insan kaynakları şeması yada bordro şeması yerine ikisinin ortak kullandığı bir şemada olması daha uygundur.
Yani diğer bir deyişle; eğer bir tablo birden fazla modul tarafından kullanılıyor ise, bu ortaklığı belirtecek yeni bir şema gerekirse açılmalıdır. 
Bu şema ismi eğer dışarıdan veri alınıyorsa bunu anlatan bir isim, eğer 2 modulün ortak noktası ise, bu ortak nokta olabilir.
Sadece 2 modul tarafından kullanılacak bir tablo ORTAK şemasını eklenmemelidir.

Ana modul şemalarından 10 taneden fazla tablo bulunması bu modulun buyuk olduğunu göstermektedir.
Bu modul 2-3 ana modul olarak ayrılabilir. 
Örneğin personel için aşağıdaki gibi bir ayrım olabilir.


- PERSONEL
- PERSONEL_IZIN
- PERSONEL_HARCAMALAR


Ayrıca gecici daha sonra silinebilecek tabloların tutulması için GECICI veya TEMP isminde bir şemada eklenmesi uygun olacaktır.
Aynı şekilde kısa süreli yedek işlemleri için, karışık bir güncelleme öncesinde ihtiyaç duyulabilecek yedek işlemleri için YEDEK şeması eklenebilir.



Her modul ilişkileri (relations) gösteren diagramlara sahip olmalıdır.

TODO: Örnek diagram ekle

## Şema, Tablo, Kolon İsimlendirme

Şema ismi olarak kullandığınız isimleri tablo isimleri olarak kullanmayın. 
Yani eğer BORDRO diye bir şemanız varsa, bu şema içinde veya diğer şemaların içinde BORDRO diye bir tablonuz olmamalıdır.

VS.NET içinde proje ismi olarak kullanacağınız bir ismi şema ismi  olarak kullanmayınız.
Yada şema isimlerinizi, VS.NET içinde proje ismi olarak vermeyin.
C# Derleyici (C# Compiler)  bu durumda sorun çıkarmaktadır.


 


Veritabanında oluşturulan Tablo ve Şema isimleri tüm harfler büyük, Kelimeler arasında ise _ olacak şekilde seçilmelidir.


- INSAN_KAYNAKLARI.ILAC_ALIMI_ALINAN_ILACLAR
- ORTAK.KISI_EK_BILGILER
- MUHASEBE.FIS
- MUHASEBE.FIS_DETAY


\subsubsection{Kolon İsimlendirme}
Kolon isimleri verilirken .NET isimlendirme konvansiyonuna uygun olarak her kelime buyuk harf ile baslamalı, diğer harfler kucuk harf olmalıdır.
Kolon isimleri kullanılırken türkçe karakterler (İ,Ü,ı ..) kullanılmamalıdır.
Bakınız aşağıdaki örnekler.


\begin{enumerate}
\item Ad
\item Soyad
\item IkinciAdi
\item TcKimlikNo
\item SehirNo
\end{enumerate}

Raporlama açısından bütün tablo'ların ve anlamı açık olmayan kolanların descriptionlarının yazılması tavsiye edilir.
Bu sayede veritabanından otomatik rapor çıkaran araçların raporları daha anlamlı olacaktır.
	
  
  
\subsubsection{Kolon İsimlendirme - Primary Key}

Primary key - Birincil anahtar isimlerinin nasıl olması gerektiğine proje başında karar verilmeli ve bu isimlendirmeye proje boyunca uyulmalıdır.
Buna göre birincil anahtar için aşağıdaki kelimelerden biri seçilmelidir.

\begin{enumerate}
\item Key
\item ID
\item Id
\end{enumerate}

Bazı projelerde KayitNo kullanılmaktadır.
Bu kelime uzun olduğu için bana göre yukarıdakiler daha iyi seçimlerdir.


Key anahtar, Id Identity (kimlik) kelimelerinin kısaltmasıdır.
Projede bunlardan sadece birinin kullanılması daha uygun olacaktır.
Aynı şekilde ID ve Id olarak karar verilmeli ve hep aynı şekilde kullanılmalıdır.

Aşağıdaki örneklerde Key kelimesinin seçilmiştir.

Birincil Anahtarları TABLO ADI + Key olarak verilmesi tavsiye edilmektedir.
Örneğin:

\begin{tabular}{@{}ll@{}}

\toprule

Tablo Adı & Primary Key Adı \\ 
\midrule
ORTAK.KISI & KisiKey \\ 
MUHASEBE.FIS & FisKey \\ 
BORDRO.ILAC_ALIMI_ALINAN_ILACLAR & IlacAlimiAlinanIlaclarKey \\ 
ORTAK.KISI_EK_BILGILER & KisiEkBilgilerKey \\ 
\bottomrule
\end{tabular} 


Eğer tanım tablolarının primary key'ı seçiliyor ise TabloIsmi + (No, Turu, TurNo, Tipi, TipNo) gibi kolon isminden sonra türkçe okunmaya uygun bir ek kullanılmalıdır. Örneğin:


\begin{tabular}{@{}ll@{}}

\toprule

Tablo Adı & Primary Key Adı \\ 

\midrule

TT_ORTAK.Cinsiyet & CinsiyetTipNo \\ 
TT_ORTAK.SEHIR & SehirNo  \\ 
TT_OZGECMIS.EGITIM_TIP & EgitimTipNo \\ 
TT_OZGECMIS.YABANCI_DIL_SINAV_TUR & YabanciDilSinavTurNo \\ 
\bottomrule
\end{tabular}



\subsubsection{Kolon İsimlendirme - Foreign Key}

Kolonlarda ikincil anahtar isimleri verilirken dikkat edilmesi gereken kural,
referans edilen tablonun bir tanım tablosumu yoksa bir ana tablomu olduğu kuralıdır.
Eğer bir ana tabloya referans veriliyorsa TabloIsmi + Key kullanılmalıdır.
Eğer bir tanım tablosuna referans veriliyorsa TabloIsmi + (No, Turu, TurNo, Tipi, TipNo)
gibi kolon isminden sonra türkçe okunmaya uygun bir ek kullanılmalıdır.
Bu sayede bir tablo incelenirken hangi tablolara referans verdiği daha rahat bir
sekilde anlaşılacaktır. 
Ayrıca verilen referansın bir tanım tablosunamı yoksa bir ana tabloyamı yapıldığı
daha iyi anlaşılacaktır.

Not eğer verilen ikincil anahtar kolonu bir iş gereği yüzünden ise,
kolon isminde bu durumun belirtilmesi ve aynı zamanda tablo isminin kullanılması daha iyi 
olur.

Örneğin Bilgi Edinme için tasarlanan tablo isimlerine bir bakalım.
\begin{itemize}

\item BILGI_EDINME.DILEKCE
	\begin{itemize}
		\item DilekceKey
		\item BasvuruSahibiKisiKey
		\item GonderenIP
		\item DilekceIcerik
		\item CevapIstekTip
		\item GelisYoluTip
		\item VatandasBasvuruReferansKey
	\end{itemize}

\end{itemize}

Bu tabloyu okuduğumuzda DilekceKey primary key, 
BasvuruSahibiKisiKey ve VatandasBasvuruReferansKey ana tablolara referans eden foreign key,
CevapIstekTip ve GelisYoluTip tanım tablolarına referans eden bilgiler.

Burada BasvuruSahibiKisiKey ORTAK.KISI tablosuna buyuk ihtimal ile referans vermektedir.
KisiKey yerine BasvuruSahibiKisiKey kullanılması bu anahtarı dilekce basvuranı ifade ettiğini
göstermektedir. Bu anahtar kullanılarak kisi adı soyadı gibi bilgilere ulaşılabilir.

CevapIstekTip ve GelisYoluTip kolonları ile TT_BILGI_EDINME şemasında
CEVAP_ISTEK ve GELIS_YOLU isminde tablolar olduğunu biliyoruz.







## Kolon Veri Yapıları}

\subsubsection{Primary Key}

Birincil anahtar (Primary Key) konulmayan tablo olmayacaktır.
Primary key değerleri olarak uniqueidentifier veya Identity seçilmeli proje boyunca benzer bir yapı kullanılmalıdır.
Primary key Sentetik Anahtar (Surrogate Key) olarak int IDENTITY değerleri yerine uniqueidentifier tercih edilmesi,
kod yazılması kolaylığı açısından değerlendirilmeli ve proje başlangıcında karar verilmelidir.
Uniqueidentifier eğer PK olarak kullanılıyorsa default değer olarak newid() olmalıdır.

\subsubsection{Foreign Key}

Eğer bir tablo içindeki kolon başka bir tabloya referans veriyorsa 
kesinlikle foreign key tanımlanmalı ve bu			kolonun ismi TabloIsmiTipNo yada 
TabloIsmiKey olarak tanımlanmalı. TipNo tanım tabloları için. Key ise normal 
tablolar			için kullanılmalıdır.	
TipNo yerine referans verilen tanımın türkçesine göre TurNo
gibi diğer kelimelerde kullanılabilir.
Eğer bir kolon ismi TurNo, TipNo, Turu, Tipi, RenkNo, BilgiNo gibi bir kelime ile bitiyorsa ise çok büyük ihtimal ile Tanım Tablolarına referans veriliyordur.
TipNo(...) okunduğu zaman bir tanım tablosuna referans düşünülürken, Key ise   1..* veya 1..1 ilişkisini tanımlamalıdır.		 


\subsubsection{Yazı Değerler}

Yazi degerleri eger değişken ise varchar ile, eğer sabit bir değer ise char olarak tutulmalıdır. Örneğin:

\begin{itemize}
\item Eğer bilgi girilen tip daima aynı sayıda ise örnek: Posta Kodu gibi char olarak tanımlanmalıdır.		 
\item Eğer bilgi girilen değer değişken bir değer ise, ad soyad gibi varchar olarak tanımlanmalıdır.
\end{itemize} 

Eğer veri tabanında kullanılan genel dilden (türkçe olmayan) farklı değer girilebilir ise nchar,nvarchar tanımlanmalıdır.

\subsubsection{Sayı Değerler}

Sayı değerleri için alabilecekleri aralıkları düşünerek tiny int, short int , int veya big int kullanınız. 

\begin{itemize}

\item   Tablodaki değer  -32,768 - 32,767   smallint veri yapısını kullanın.
\item   eğer tamsayı değer  -2,147,483,648 - 2,147,483,647 arası ise int veri yapısını kullanın.

\end{itemize} 

\subsubsection{Para ve Ondalık Değerler}

\begin{itemize}

\item  Money SQL standartında olmadığından kullanılmamalıdır. 
\item  Float ve double tam değerler değildir, yaklaşık değer olarak çalışırlar. 
Para hesaplarında kesinlikle kullanılmamaları gerekir.
\item Para değerleri numeric(18,2) veya decimal(18,2) olmalıdır. numeric ve decimal bir birlerinin alias'ıdır. 
\item Eğer para değerlerinde daha fazla küsürata ihtiyaç var ise   numeric(18,4)-decimal(18,4) gibi artırılabilir. 

\end{itemize} 
 			
\subsubsection{Tarih Değerler}

Tarih değerleri 	datetime (3.33 milliseconds accuracy)  	Jan 1, 1753  --- Dec 31, 9999
,smalldatetime (1 minute accuracy) 	Jan 1, 1900  --- Jun 6, 2079		
değerleri arasını göstermektedir. 
.NET üstünde doğru kabul edilen, 
örn: 01.01.1600 gibi bir değer SQL Server üzerinde hata verebilir.
Tarih değerlerinin bu aralıkta olduğuna emin olunuz.

\begin{itemize}

\item Tarih bilgilerini tutarken smalldatetime tercih ediniz. 			
\item Eğer sadece  yıl bilgisi tutacaksanız, Mezuniyet yılı smallint tutabilirsiniz. 
Bu tür bir kolona constraint ekleyin.örn: 
  $  CHECK  (([GirisSene]>=(1900) AND   [GirisSene]<=datepart(year,getdate()))) $
Yukarıdakı kısıtlama giris senesinin 1900 ile şu an bulunduğumuz sene arasında kalmasını sağlamaktadır.
\end{itemize}  			


\subsubsection{Resim Dosya gibi Binary Değerler}
Resim veya binary data tutmak için varbinary(max) tercih edin. 
Veri tipi image SQL Server'ın yeni versiyonlarında kullanılmayacaktır.
 
\subsubsection{Diger Notlar}

\begin{itemize}
\item   Tablolarda mümkün olduğunca girilen bilgiler not null yapılmalıdır.
\item   Eğer anlamlı bir default/varsayılan değer verilebiliyorsa verilmelidir. Örneğin GuncellemeTarih default now() verilebilir.
\end{itemize}  			
  		 
  
\section{Yazılım Geliştirme Yetkileri}  									 
Yazılım geliştiriciler için sınırlı yetkili veri tabanı kullanıcılar açılması 
ve SQL Sever Management Studio üzerinde işlem yapılırken bu 
kullanıcılar ile sisteme girilmesi.düşünülmelidir.
En doğrusu yazılım geliştirici kullanıcılarının bir role atanması ve bu rolün kısıtlanması olabilir.

  	 
\section{Stored Procedures - Functions}  									 
Yazılım geliştiriciler Stored Procedure yazma gereksinimi duyarlarsa 
bunu veritabanı yöneticisi ve takım liderlerinin onayını	aldıktan sonra   yapabileceklerdir.
Veri tabanında mümkün olduğunca stored procedure ve trigger olmamalı,
bunların yerine .NET kodları ile çözüm bulunmaya çalışılmalıdır.


Stored Procudures usp_ ile, functionlar ufn_ ile, triggerler ise utnX_ ile başlamalıdır (X-->;i:insert, u:update, d:delete). 			
Note: eğer bir stored Procedure sp veya xp ile başlarsa SQL server 
bunun System stored procedure'u olduğunu düşündüğü için ilk önce master veritabanında arayacaktır. 
  
  
\section{İsimlendirme Konvansiyonu}  
  
## İsimlendirme Konvansiyonu - Stored Procedure - Gömülü Yordamlar }  

Stored Procudure'lara isim verirken tablo ismi + yaptığı işlem şeklinde  isim verin.
    

\begin{itemize}
\item Tablo Ismi + yaptığı iş 
\item usp_PersonelAraAdiVeSoyadiIle 
\item usp_Personel_Oku_Tum 
\item usp_MuhasebeYillikMizanHesapla
\item usp_BordroAylikBordroHesapla
\end{itemize}

Not bu isimler örnek olsun diye verilmiştir. 
Veritabanında basit CRUD ve arama işlemleri için stored procedure olmamalıdır.

## İsimlendirme Konvansiyonu - Table and View-Tablo ve Görüntü}  

\begin{itemize}
\item INSAN_KAYNAKLARI.MUSTERI 
\item TANIM_TABLOLARI.NUFUS_CUZDANI_VERILIS_NEDENI
\end{itemize}  

Schema ismi + Tablo ismi, 2 kelimeden fazla olan isimler _ ile ayrılıyor. 
Tablo ve görüntülere (view) ayrı isimler vermiyoruz.		  

## İsimlendirme Konvansiyonu - Column-Kolon}  
Her kelimenin ilk harfi büyük yazılır. 
\begin{itemize}
\item MusteriKey 
\item SonDegistirmeTarihi
\item TipNo
\item MusteriKey
\end{itemize}  


## İsimlendirme Konvansiyonu - Index}  

IX_TabloIsmi_KolonIsimleri  veya UQ_TabloIsmi_KolonIsimleri şeklinde kullanılmalıdır.
Her kelimenin ilk harfi büyük yazılır.
Unique indexler UQ ile başlarken, diğer indexler IX ile başlamalıdır.


\begin{itemize}
\item 
\item IX_Musteri_MusteriNo
\item IX_Satislar_TipNo
\item IX_Satislar_MusteriKey
\item UQ_Musteri_VergiNo

\end{itemize}  






## İsimlendirme Konvansiyonu - Yabancı anahtar (Foreign key)}  
\begin{itemize}
\item  FK_TabloIsmi1KolonIsmi1FK_TabloIsmi2KolonIsmi2 
\item FK_MusteriMusteriNoSiparisMusteriNo
\end{itemize}
Her kelimenin ilk harfi büyük yazılır. 
  
## İsimlendirme Konvansiyonu - Default - Varsayılan}  
\begin{itemize}
\item DF_
\item DF_PERSONEL_IlkGirisTarihi 
\end{itemize}  
  
SQL Server DF veriyor. Management Studio icinde Default'ları verirseniz, İsimlendirme düzgün olur. 		  

## İsimlendirme Konvansiyonu - Constraint }  
\begin{itemize}
\item CK_
\item CK_OKUL_BILGILERI_GirisSenesi  
\end{itemize}  
  
SQL Server CK veriyor. Management Studio icinde Constarint yazarsanız, İsimlendirme düzgün olur. 				 
  
## İsimlendirme Konvansiyonu - Trigger - Tetikleyici  }  
utrX + Tablo Ismi, yerine triggerin hangi işlem için olduğunu söyleyen bir kelime gelicektir. 

\begin{itemize}
\item utrX_ + tablo ismi + islem (insert (i), update (u), delete (d))
\item utrX_ORTAK_KISI_Delete
\end{itemize}  

  
  
\section{Düzenleme Geçmişi} 

\begin{tabular}{@{}llll@{}}

\toprule

 Versiyon & Yazarlar & Tarih & Yapılan Değişiklikler  \\ 

\midrule

 0.1 &  Atilla Özgür & 2006 & İlk Yazım   \\ 
 0.2 &  Atilla Özgür & 2008 & Fatih ile İç İşlerinde yapılan değişiklik \\ 
 0.3 &  Atilla Özgür & 2013-02-13 & Çok uzun bir aradan sonra bazı düzeltmeler  \\ 
 0.4 &  Atilla Özgür & 2013-07-04 & Latex çevrimi, sectionlara ayırma , tümüyle tekrar yazıldı.  \\ 
 0.5 &  Atilla Özgür & 2017-12-12 & İŞKUR eğitimden önce değişiklikler. latex booktabs'a geçiş.   \\ 
\bottomrule
\end{tabular}


\end{document}
