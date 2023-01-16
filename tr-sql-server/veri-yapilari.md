## Kolon Veri Yapıları

### Primary Key

Birincil anahtar (Primary Key) konulmayan tablo olmayacaktır.
Primary key değerleri olarak uniqueidentifier veya Identity seçilmeli proje boyunca benzer bir yapı kullanılmalıdır.
Primary key Sentetik Anahtar (Surrogate Key) olarak int IDENTITY değerleri yerine uniqueidentifier tercih edilmesi, kod yazılması kolaylığı açısından değerlendirilmeli ve proje başlangıcında karar verilmelidir.
Uniqueidentifier eğer PK olarak kullanılıyorsa default değer olarak newid() olmalıdır.

### Foreign Key

Eğer bir tablo içindeki kolon başka bir tabloya referans veriyorsa kesinlikle yabancı anahtar (foreign key) tanımlanmalıdır.
Bu yabancı anahtar kolonun ismi TabloIsmiTurNo yada TabloIsmiKey olarak tanımlanmalıdır.
Tanım tabloları için TurNo kullanılmalıdır.
Normal modül tabloları için Key kullanılmalıdır.
TurNo yerine referans verilen tanımın türkçesine göre TipNo gibi diğer kelimelerde kullanılabilir.
Eğer bir kolon ismi TurNo, TipNo, Turu, Tipi, RenkNo, BilgiNo gibi bir kelime ile bitiyorsa ise Tanım Tablolarına referans veriliyordur.
TurNo(...) okunduğu zaman bir tanım tablosuna referans düşünülürken, Key ise 1..* veya 1..1 ilişkisini ana modül tablolarına tanımlamalıdır.

### Yazı Değerler (varchar...)

Yazı değerleri eğer değişken ise varchar ile, eğer sabit bir değer ise char olarak tutulmalıdır. 
Örneğin:

- Eğer bilgi girilen tip daima aynı sayıda ise örnek: Posta Kodu gibi char olarak tanımlanmalıdır.
- Eğer bilgi girilen değer değişken bir değer ise, ad soyad gibi varchar olarak tanımlanmalıdır.

Eğer veri tabanında kullanılan genel dilden (türkçe olmayan) farklı değer girilebilir ise nchar,nvarchar tanımlanmalıdır.

### Sayı Değerler

Sayı değerleri için alabilecekleri aralıkları düşünerek tiny int, short int , int veya big int kullanınız. 

-   Tablodaki değer  -32,768 - 32,767   smallint veri yapısını kullanın.
-   eğer tamsayı değer  -2,147,483,648 - 2,147,483,647 arası ise int veri yapısını kullanın.

### Para ve Ondalık Değerler

-  Money SQL standartında yoktur. Kullanılmamalıdır. 
-  Float ve double tam değerler değildir, yaklaşık değer olarak çalışırlar. 
Para hesaplarında kesinlikle kullanılmamaları gerekir.

- numeric ve  decimal aynı veri tipidir.
Bir birlerinin diğer ismi (alias) olarak tanımlıdırlar. 

- Eğer para değerlerinde daha fazla küsürata ihtiyaç var ise  numeric(18,4)-decimal(18,4) gibi artırılabilir. 

### Tarih Değerleri

Tarih değerleri 	

|Tarih Veri tipi |doğruluk (accuracy)|  Başlangıç Tarihi | Bitiş Tarihi  |
|---|---|---|---|
|datetime|3.33 milliseconds accuracy   | Jan 1, 1753  | Dec 31, 9999  |
|smalldatetime| 1 minute accuracy  | Jan 1, 1900  | Jun 6, 2079  |

TODO: daha sonra türkçe yap

değerleri arasını göstermektedir. 

.NET üstünde doğru kabul edilen,  örn: 01.01.1600 gibi bir değer SQL Server üzerinde hata verebilir.
Tarih değerlerinin bu aralıkta olduğuna emin olunuz.

- Tarih bilgilerini tutarken smalldatetime tercih ediniz. 			
- Eğer sadece yıl bilgisi tutacaksanız, Mezuniyet yılı smallint tutabilirsiniz. 

```sql
 CHECK  (([GirisSene]>=(1900) AND   [GirisSene]<=datepart(year,getdate()))) 
```
Yukarıdakı kısıtlama giris senesinin 1900 ile şu an bulunduğumuz sene arasında kalmasını sağlamaktadır.


### Resim Dosya gibi Binary Değerler

Resim veya binary data tutmak için varbinary(max) tercih edin. 
Veri tipi image SQL Server'ın yeni versiyonlarında kullanılmayacaktır.
 
### Diger Notlar


-   Tablolarda mümkün olduğunca girilen bilgiler not null yapılmalıdır.
-   Eğer anlamlı bir default/varsayılan değer verilebiliyorsa verilmelidir. 
- Örneğin GuncellemeTarih default now() verilebilir.

