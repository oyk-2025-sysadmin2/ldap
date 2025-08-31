
**1.adım: OpenLDAP kurulumu için sistem güncellenir ve paket kurulumları yapılır.**

![[Pasted image 20250831115559.png]]

**Kurulumla birlikte slapd servisi ve kullanacağımız araçlar (ldapadd, ldapsearch, slappasswd vb.) gelir.**

**2. adım: Slapd servisi başlatılır.**

![[Pasted image 20250831115956.png]]

**slapd, arka planda LDAP protokolünü (389/636 TCP portlarında) dinleyen servistir.**

Görevi, LDAP istemcilerinden gelen sorguları (kimlik doğrulama, kullanıcı arama, obje ekleme/silme/güncelleme vb.) işlemek ve LDAP veritabanındaki bilgileri istemciye sunmaktır.

**3.adım: Firewall' da gerekli izinler verilir.**

![[Pasted image 20250831120155.png]]

 **4.adım:slappasswd ile daha sonra kullanacağımız hash şifre oluşturulur.**

![[Pasted image 20250831120317.png]]

**5.adım: LDIF dosyasını oluşturabiliriz.**

LDAP ağacındaki nesneleri eklemek, silmek, değiştirmek veya dışa aktarmak için kullanılır.

![[Pasted image 20250831120817.png]]
![[Pasted image 20250831120910.png]]

ldif dosyasındaki bilgileri kendi sunucumuza göre ayarlamalıyız.

**"ldapsearch -Y EXTERNAL -H ldapi:/// -b cn=config dn olcSuffix"** komutu ile bu bilgileri öğrenebiliriz.

![[Pasted image 20250831121110.png]]

**6. adım db.ldif dosyasını LDAP' a eklemeyi deneriz.***

![[Pasted image 20250831121305.png]]