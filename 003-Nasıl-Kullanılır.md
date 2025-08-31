# LDAP nasıl kullanılır?
## LDAP kullanım tanımı
- LDAP temel olarak bir sunucu-istemci mantığında uzakta çalışan bir veritabanıdır. Bu yüzden LDAP'ın kullanımını kullanıcı ve yönetici perspektiflerinden inceleyebiliriz.
### Kullanıcı gözünden LDAP
- Kullanıcı farkında olmasa da LDAP'ı kullanır. Bir sisteme girişini yapar erişebileceği verilere ulaşabilir veya bu verilerde işlemlerini gerçekleştirir.
- Kullanıcı -> LDAP İstemcisi -> LDAP Sunucusu -> Doğrulama -> Erişim şeklinde özetlenebilir.
### Yönetici gözünden LDAP
- Yönetici bu veritabanındaki hiyerarşiyi, kullanıcı-grup oluşturup yetki dağıtımlarını yönetir.
# ✅ Doğru Kullanım
- Merkezi Doğrulama Sistemi kullanımı
- Gruplarla yetki dağıtımı
- Bağlantı kısmında sertifika sorgusu
# ❌ Yanlış Kullanım
- Kullanıcılara gereğinden fazla yetki verilmesi
- Her bir uygulama için farklı hesap açılması
- Şifresiz bağlantı kullanımı
- Düzensiz ağaç yapısı kurgusu
