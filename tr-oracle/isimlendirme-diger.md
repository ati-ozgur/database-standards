## İsimlendirme Konvansiyonu
  
### İsimlendirme Konvansiyonu - Stored Procedure - Gömülü Yordamlar

Stored Procudure'lara isim verirken modül/ ana tablo ismi + yaptığı işlem şeklinde isim verin.
    
- Modül Ismi + yaptığı iş 
- PersonelAraAdiVeSoyadiIle 
- Personel_Oku_Tum 
- MuhasebeYillikMizanHesapla
- BordroAylikBordroHesapla


Not bu isimler örnek olsun diye verilmiştir. 
Veritabanında basit CRUD ve arama işlemleri için stored procedure olmamalıdır.

### İsimlendirme Konvansiyonu - Table and View-Tablo ve Görüntü


- INSAN_KAYNAKLARI.MUSTERI 
- TANIM_TABLOLARI.NUFUS_CUZDANI_VERILIS_NEDENI



Tablo ve görüntülere (view) ayrı isimler vermiyoruz.
Bunun nedeni ihtiyaça göre tablo veya görüntünün değiştirilebilmesine izin vermektir.
Örneğin çok kullanılan bir view, tablo yapılabilir.
Yada bilgiler daha farklı tutulduğu için eski bir tablo, view yapılabilir.
Bu tür durumlarda isim aynı kaldığı için istemci programları sorunsuz çalışacaktır.


### İsimlendirme Konvansiyonu - Column-Kolon

Her kelimenin ilk harfi büyük yazılır. 

- MusteriKey 
- SonDegistirmeTarihi
- TipNo
- MusteriKey


### İsimlendirme Konvansiyonu - Index

IX_TabloIsmi_KolonIsimleri  veya UQ_TabloIsmi_KolonIsimleri şeklinde kullanılmalıdır.
Her kelimenin ilk harfi büyük yazılır.
Unique indexler UQ ile başlarken, diğer indexler IX ile başlamalıdır.

 
- IX_Musteri_MusteriNo
- IX_Satislar_TipNo
- IX_Satislar_MusteriKey
- UQ_Musteri_VergiNo

### İsimlendirme Konvansiyonu - Yabancı anahtar (Foreign key)

- FK_TabloIsmi1KolonIsmi1FK_TabloIsmi2KolonIsmi2 
- FK_MusteriMusteriNoSiparisMusteriNo

Her kelimenin ilk harfi büyük yazılır. 


### İsimlendirme Konvansiyonu - Default - Varsayılan

- DF_
- DF_PERSONEL_IlkGirisTarihi 


SQL Server DF veriyor.
Management Studio icinde Default'ları verirseniz, İsimlendirme düzgün olur. 

### İsimlendirme Konvansiyonu - Constraint 

- CK_
- CK_OKUL_BILGILERI_GirisSenesi  

  

SQL Server CK veriyor. 
Management Studio icinde Constarint yazarsanız, İsimlendirme düzgün olur.
  
### İsimlendirme Konvansiyonu - Trigger - Tetikleyici  

utrX + Tablo Ismi, yerine triggerin hangi işlem için olduğunu söyleyen bir kelime gelicektir. 

- utrX_ + tablo ismi + islem (insert (i), update (u), delete (d))
- utrX_ORTAK_KISI_Delete
