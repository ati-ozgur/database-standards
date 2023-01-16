  
## Yazılım Geliştirme Yetkileri

Yazılım geliştiriciler normal geliştirme yaparken yüksek yetkili kullanıcılar (database owner veya schema owner) ile geliştirici (development) veri tabanına bağlanmalarından bir sakınca yoktur.
Ama test ve canlı (production) veri tabanlarına kesinlikle yüksek yetkili kullanıcılar ile bağlanmamalıdırlar.

## Uygulama Kullanıcıları

Uygulama kullanıcısı olarak farklı bir kullanıcı tanımlanmalıdır. 
Aşağıda örnek bir kullanıcının oracle veri tabanında açılması için sql örneği verilmiştir.
Bu konuda daha fazla bilgi için Documentation2013Configuring ve @Hall2007Schema  okuyabilirsiniz.

{{< include ./Application-User-Create.sql.md >}}

Açılan bu kullanıcıya sınırlı yetkiler verilmelidir.
Aşağıdaki sql cümlesi uygulamanın ihtiyaç duyduğu şemaların içindeki tablolar üzerinde gerekli yetkileri oluşturan sql cümlesi verilmiştir.

{{< include ./Application-User-Grants.sql.md >}}



