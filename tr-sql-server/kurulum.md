## Kurulum Notları Collation

Veritabanı kurulurken, SQL Server içinden Collation olarak aşağıdakilerden biri seçilmelidir.

- Turkish_CI_AS : Turkish, case-insensitive, accent-sensitive, kana type-insensitive, width-insensitive
- Turkish_100_CI_AS_SC_UTF8: Turkish-100, case-insensitive, accent-sensitive, kana type-insensitive, width-insensitive, supplementary characters, UTF8 (SQL Server 2019)

Buradaki kelimelerin anlamı aşağıdaki gibidir.

- CI: Case Insensitive (Büyük küçük harf ayrımı yok)
- CS: Case sensitive (Büyük küçük harf ayrımı var)
- AS: Accent-sensitive (ı ve i aynı değildir)

Nchar unicode olarak değerleri tuttuğu için 2 katı yer kaplamakta  ve daha yavaş çalışmaktadır.
Eğer veritabanında aynı anda iki dil bilgisi tutulmayacaksa, yani hem rusça hem de türkçe bilgi, yazı değerleri veri tabanında varchar yada char olarak tutulmalıdır.
SQL Server 2019 ve sonrasını kullanıyorsanız, UTF8 seçmeniz tavsiye edilir.
