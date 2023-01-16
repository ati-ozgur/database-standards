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

### Kolon İsimlendirme

Kolon isimleri verilirken .NET isimlendirme konvansiyonuna uygun olarak her kelime bü harf ile başlamalı, diğer harfler küçük harf olmalıdır.
Kolon isimleri kullanılırken türkçe karakterler (İ,Ü,ı ..) kullanılmamalıdır.
Bakınız aşağıdaki örnekler.

- Ad
- Soyad
- IkinciAdi
- TcKimlikNo
- SehirNo

Raporlama açısından bütün tabloların ve anlamı açık olmayan kolanların tanımlarının (description) yazılması tavsiye edilir.
Ama tanım yazılması iyi isim verilmesinden daha önemsizdir.
Tablo ve kolon isimlerinin iyi seçilmesi son derece önemlidir.
Bu sayede veritabanından otomatik rapor çıkaran araçların raporları daha anlamlı olacaktır.

### Kolon İsimlendirme - Primary Key

Primary key - Birincil anahtar isimlerinin nasıl olması gerektiğine proje başında karar verilmeli ve bu isimlendirmeye proje boyunca uyulmalıdır.
Buna göre birincil anahtar için aşağıdaki kelimelerden biri seçilmelidir.

- Key
- ID
- Id

Bazı projelerde KayitNo kullanılmaktadır.
Bu kelime uzun olduğu için bana göre yukarıdakiler daha iyi seçimlerdir.

Key anahtar, Id Identity (kimlik) kelimelerinin kısaltmasıdır.
Projede bunlardan sadece birinin kullanılması daha uygun olacaktır.
Aynı şekilde ID ve Id olarak karar verilmeli ve hep aynı şekilde kullanılmalıdır.

Aşağıdaki örneklerde Key kelimesi seçilmiştir.

Birincil Anahtarları TABLO ADI + Key olarak verilmesi tavsiye edilmektedir.
Örneğin:

| Tablo Adı           | Birinci Anahtar (Primary Key) Adı |
|---------------------|-----------------------------------|
| ORTAK.KISI           | KisiKey                           |
| MUHASEBE.FIS         | FisKey|
| BORDRO.ILAC_ALIMI_ALINAN_ILACLAR | IlacAlimiAlinanIlaclarKey |
| ORTAK.KISI_EK_BILGILER | KisiEkBilgilerKey |


Eğer tanım tablolarının primary key'ı seçiliyor ise TabloIsmi + (No, Turu, TurNo, Tipi, TipNo) gibi kolon isminden sonra türkçe okunmaya uygun bir ek kullanılmalıdır. Örneğin:


| Tablo Adı           | Birinci Anahtar (Primary Key) Adı |
|---------------------|-----------------------------------|
| TT_ORTAK.CINSIYET   | CinsiyetTipNo                           |
| TT_ORTAK.SEHIR        | SehirNo|
| TT_OZGECMIS.EGITIM_TIP | EgitimTipNo |
| TT_OZGECMIS.YABANCI_DIL_SINAV_TUR | YabanciDilSinavTurNo |

### Kolon İsimlendirme - Foreign Key

Kolonlarda ikincil anahtar isimleri verilirken dikkat edilmesi gereken kural,
referans edilen tablonun bir tanım tablosu mu yoksa bir ana tablomu olduğu kuralıdır.

- Eğer bir ana tabloya referans veriliyorsa TabloIsmi + Key kullanılmalıdır.
- Eğer bir tanım tablosuna referans veriliyorsa TabloIsmi + (No, Turu, TurNo, Tipi, TipNo) gibi kolon isminden sonra türkçe okunmaya uygun bir ek kullanılmalıdır.

Bu sayede bir tablo incelenirken hangi tablolara referans verdiği daha rahat bir
sekilde anlaşılacaktır.
Ayrıca verilen referansın bir tanım tablosuna yoksa bir ana tabloya verildiği
daha iyi anlaşılacaktır.

Not eğer verilen ikincil anahtar kolonu bir iş gereği yüzünden ise, kolon isminde bu durumun belirtilmesi ve aynı zamanda tablo isminin kullanılması daha iyi olur.

Örneğin Bilgi Edinme için tasarlanan tablo isimlerine bir bakalım.

{{< include ./mermaid-dilekce.qmd >}}


Bu tabloyu okuduğumuzda 

- DilekceKey primary key, 
- BasvuruSahibiKisiKey ana tablolara referans eden foreign key
-  VatandasBasvuruReferansKey ana tablolara referans eden foreign key,
-  CevapIstekTurNo tanım tablolarına referans eden bilgiler. 
-  GelisYoluTurNo tanım tablolarına referans eden bilgiler.

Burada BasvuruSahibiKisiKey ORTAK.KISI tablosuna büyük ihtimal ile referans vermektedir.
KisiKey yerine BasvuruSahibiKisiKey kullanılması bu anahtarı dilekce basvuranı ifade ettiğini göstermektedir. 
Bu anahtar kullanılarak kisi adı soyadı gibi bilgilere ulaşılabilir.

CevapIstekTurNo ve GelisYoluTurNo kolonları ile TT_BILGI_EDINME şemasında veya başka bir TT şemasında CEVAP_ISTEK ve GELIS_YOLU isminde tablolar olduğunu biliyoruz.
