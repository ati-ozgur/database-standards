## Şema, Tablo, Kolon İsimlendirme

Şema ismi olarak kullandığınız isimleri tablo isimleri olarak kullanmayın.
Yani eğer BORDRO diye bir şemanız varsa, bu şema içinde veya diğer şemaların içinde BORDRO diye bir tablonuz olmamalıdır.

VS.NET içinde proje ismi olarak kullanacağınız bir ismi şema ismi olarak kullanmayınız.
Yada şema isimlerinizi, VS.NET içinde proje ismi olarak vermeyin.
C# Derleyici (C# Compiler)  bu durumda sorun çıkarmaktadır.

Veritabanında oluşturulan Tablo ve Şema isimleri tüm harfler büyük, Kelimeler arasında ise _ olacak şekilde seçilmelidir.

- INSAN_KAYNAKLARI.ILAC_ALIMI_ALINAN_ILACLAR
- ORTAK.KISI_EK_BILGILER
- MUHASEBE.FIS
- MUHASEBE.FIS_DETAY

### Kolon İsimlendirme

Oracle kolon isimlerini verirken, ne yazıkkı büyük küçük harf ayrımı yapmamaktadır.
Bundal dolayı kolon isimleri verilirken BAGIRAN_SNAKE_CASE kullanmak zorundayız.
Tüm harfler büyük olmalı ve kelimeler alt çizgi _ ile ayrılmalıdır.
Kolon isimleri kullanılırken türkçe karakterler (İ,Ü,ı ..) kullanılmamalıdır.
Bakınız aşağıdaki örnekler.

- AD
- SOYAD
- IKINCI_ADI
- TC_KIMLIK_NO
- SEHIR_NO

Raporlama açısından bütün tabloların ve anlamı açık olmayan kolanların tanımlarının (description) yazılması tavsiye edilir.
Ama tanım yazılması iyi isim verilmesinden daha önemsizdir.
Tablo ve kolon isimlerinin iyi seçilmesi son derece önemlidir.
Bu sayede veritabanından otomatik rapor çıkaran araçların raporları daha anlamlı olacaktır.

### Kolon İsimlendirme - Primary Key

Primary key - Birincil anahtar isimlerinin nasıl olması gerektiğine proje başında karar verilmeli ve bu isimlendirmeye proje boyunca uyulmalıdır.
Buna göre birincil anahtar için aşağıdaki kelimelerden biri seçilmelidir.

- KEY
- ID

Bazı projelerde KAYITNO kullanılmaktadır.
Bu kelime uzun olduğu için bana göre yukarıdakiler daha iyi seçimlerdir.

Key anahtar, Id Identity (kimlik) kelimelerinin kısaltmasıdır.
Projede bunlardan sadece birinin kullanılması daha uygun olacaktır.

Aşağıdaki örneklerde Key kelimesi seçilmiştir.

Birincil Anahtarları TABLO ADI + Key olarak verilmesi tavsiye edilmektedir.
Örneğin:

| Tablo Adı           | Birinci Anahtar (Primary Key) Adı |
|---------------------|-----------------------------------|
| ORTAK.KISI           | KISI_KEY                           |
| MUHASEBE.FIS         | FIS_KEY					|
| BORDRO.ILAC_ALIMI_ALINAN_ILACLAR | ILAC_ALIMI_ALINAN_ILACLAR_KEY |
| ORTAK.KISI_EK_BILGILER | KISI_EK_BILGILER_KEY |


Eğer tanım tablolarının primary key'ı seçiliyor ise TabloIsmi + (NO, TURU, TUR_NO, TIPI, TIP_NO) gibi kolon isminden sonra türkçe okunmaya uygun bir ek kullanılmalıdır. Örneğin:


| Tablo Adı           | Birinci Anahtar (Primary Key) Adı |
|---------------------|-----------------------------------|
| TT_ORTAK.CINSIYET   | CINSIYET_TIP_NO                           |
| TT_ORTAK.SEHIR        | SEHIR_NO|
| TT_OZGECMIS.EGITIM_TIP | EGITIM_TIP_NO |
| TT_OZGECMIS.YABANCI_DIL_SINAV_TUR | YABANCI_DIL_SINAV_TUR_NO |

### Kolon İsimlendirme - Foreign Key

Kolonlarda ikincil anahtar isimleri verilirken dikkat edilmesi gereken kural,
referans edilen tablonun bir tanım tablosu mu yoksa bir ana tablomu olduğu kuralıdır.

- Eğer bir ana tabloya referans veriliyorsa TabloIsmi + KEY kullanılmalıdır.
- Eğer bir tanım tablosuna referans veriliyorsa TabloIsmi + (NO, TURU, TURNO,TUR_NO, TIPI, TIP_NO,TIPNO) gibi kolon isminden sonra türkçe okunmaya uygun bir ek kullanılmalıdır.
TURNO veya TUR_NO kullanılabilir ama bir tanesinin seçilip tutarlı olunması tavsiye olunur.

Bu sayede bir tablo incelenirken hangi tablolara referans verdiği daha rahat bir
sekilde anlaşılacaktır.
Ayrıca verilen referansın bir tanım tablosuna yoksa bir ana tabloya verildiği
daha iyi anlaşılacaktır.

Not eğer verilen ikincil anahtar kolonu bir iş gereği yüzünden ise, kolon isminde bu durumun belirtilmesi ve aynı zamanda tablo isminin kullanılması daha iyi olur.

Örneğin Bilgi Edinme için tasarlanan tablo isimlerine bir bakalım.

{{< include ./mermaid-dilekce-oracle.qmd >}}


Bu tabloyu okuduğumuzda 

- DILEKCE_KEY primary key, 
- BASVURU_SAHIBI_KISI_KEY ana tablolara referans eden foreign key
-  VATANDAS_BASVURU_REFERANS_KEY ana tablolara referans eden foreign key,
-  CEVAP_ISTEK_TURNO tanım tablolarına referans eden bilgiler. 
-  GELIS_YOLU_TURNO tanım tablolarına referans eden bilgiler.

Burada BasvuruSahibiKisiKey ORTAK.KISI tablosuna büyük ihtimal ile referans vermektedir.
KISI_KEY yerine BASVURU_SAHIBI_KISI_KEY kullanılması bu anahtarı dilekce basvuranı ifade ettiğini göstermektedir. 
Bu anahtar kullanılarak kisi adı soyadı gibi bilgilere ulaşılabilir.

CEVAP_ISTEK_TURNO ve GELIS_YOLU_TURNO kolonları ile TT_BILGI_EDINME şemasında veya başka bir TT şemasında CEVAP_ISTEK ve GELIS_YOLU isminde tablolar olduğunu biliyoruz.
