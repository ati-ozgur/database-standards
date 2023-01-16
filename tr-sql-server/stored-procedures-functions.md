## Stored Procedures - Functions

Yazılım geliştiriciler Stored Procedure yazma gereksinimi duyarlarsa bunu veritabanı yöneticisi ve takım liderlerinin onayını	aldıktan sonra yapabileceklerdir.
Veri tabanında mümkün olduğunca stored procedure ve trigger olmamalı, bunların yerine .NET kodları ile çözüm bulunmaya çalışılmalıdır.

Stored procedure, function ve trigger'lar için macar notasyonu kullanıp kullanılmayacağına başta karar verilmelidir.
Yazılım geliştirme araçlarının daha gelişmiş olması sebebi ile artık macar notasyonuna olan ihtiyaç azalmıştır.
Ama eğer macar notasyonu kullanıyorsanız: 
Stored Procudures usp_ ile, functionlar ufn_ ile, triggerler ise utnX_ ile başlamalıdır (X-->;i:insert, u:update, d:delete).

Note: eğer bir stored Procedure sp veya xp ile başlarsa SQL server bunun System stored procedure'u olduğunu düşündüğü için ilk önce master veritabanında arayacaktır. 
  
