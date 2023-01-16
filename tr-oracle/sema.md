## Şema (Schema) Kavramı

Oracle veri tabanında şema (schema) ve kullanıcılar (users) aynı şeydir.
Şema (schema) kavramını oracle GUI araçlarında çok iyi desteklemese, diğer açılardan şema desteği son derece düzgün çalışır.
Bu yüzden şema yapısının oracle veri tabanı tasarımı yapılırken kullanılması tavsiye edilir.
Bu yapıda her ana modül için, "bir ana şema" + "bir tanım tablosu şema"'sı açılması tavsiye edilmektedir.
Bilgi tutarlılığı (Referential Integrity) korunması açısından bütün uygulama tek bir veritabanında çalışmalıdır.

Örneğin ihtiyaç duyduğunuz modüllerin Insan Kaynakları, Muhasebe ve Bordro olduğunu düşünelim.
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
Burada Kişi için tutulacak bilgiler herkes için kesin tutulması gereken bilgilerdir.
Örneğin ad soyad, tc kimlik no, resim gibi.
Ama eger sadece yazılımı kullanan kurumdaki, kişilere ait  (örneğin kurum sicil no, kurum eposta, kurum telefon) gibi bilgiler var ise bunların farklı şemada tutulması daha yararlı olacaktır.

Buraya FIRMALAR şeması muhasebe firmalarının fatura kesecekleri firma bilgileri için eklenmiştir.
Firma, Kişi bilgileri bir çok modül tarafından ihtiyaç duyulan bilgiler oldukları için bunların ayrı şemalarda tutulmaları daha yararlı olacaktır.

Son olarak Özlük bilgileri, Bordro ve insan kaynaklarının ihtiyaç duydukları ortak bilgileri taşıyacaktır.
Burada bu bilgiler sadece insan kaynakları şeması yada bordro şeması yerine ikisinin ortak kullandığı bir şemada olması daha uygundur.
Yani diğer bir deyişle; eğer bir tablo birden fazla modül tarafından kullanılıyor ise, bu ortaklığı belirtecek yeni bir şema gerekirse açılmalıdır.
Bu şema ismi eğer dışarıdan veri alınıyorsa bunu anlatan bir isim, eğer 2 modülün ortak noktası ise, bu ortak nokta olabilir.
Sadece 2 modül tarafından kullanılacak bir tablo ORTAK şemasını eklenmemelidir.

Ana modül şemalarından 10 taneden fazla tablo bulunması bu modülün büyük bir modül olduğunu göstermektedir.
Bu modül 2-3 ana modül olarak ayrılabilir.
Örneğin personel için aşağıdaki gibi bir ayrım olabilir.

- PERSONEL
- PERSONEL_IZIN
- PERSONEL_HARCAMALAR

Ayrıca geçici daha sonra silinebilecek tabloların tutulması için GECICI veya TEMP isminde bir şemada eklenmesi uygun olacaktır.
Aynı şekilde kısa süreli yedek işlemleri için, karışık bir güncelleme öncesinde ihtiyaç duyulabilecek yedek işlemleri için YEDEK şeması eklenebilir.

Her modül ilişkileri (relations) gösteren diyagramlara sahip olmalıdır.

TODO: Örnek diagram ekle
