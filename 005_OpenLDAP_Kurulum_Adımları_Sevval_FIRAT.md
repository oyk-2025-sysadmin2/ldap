**1.adım: OpenLDAP kurulumu için sistem güncellenir ve paket kurulumları yapılır.**

<img width="975" height="146" alt="image" src="https://github.com/user-attachments/assets/d8fdd577-9204-44fe-b232-b08df41c03ab" />

**Kurulumla birlikte slapd servisi ve kullanacağımız araçlar (ldapadd, ldapsearch, slappasswd vb.) gelir.**

**2. adım: Slapd servisi başlatılır.**

<img width="975" height="420" alt="image" src="https://github.com/user-attachments/assets/ee0e4046-494e-4a02-91fe-61fcf8ba6475" />

**slapd, arka planda LDAP protokolünü (389/636 TCP portlarında) dinleyen servistir.**

Görevi, LDAP istemcilerinden gelen sorguları (kimlik doğrulama, kullanıcı arama, obje ekleme/silme/güncelleme vb.) işlemek ve LDAP veritabanındaki bilgileri istemciye sunmaktır.

**3.adım: Firewall' da gerekli izinler verilir.**

<img width="875" height="125" alt="image" src="https://github.com/user-attachments/assets/23c47ae9-2c7b-4285-8604-79243c386492" />

 **4.adım:slappasswd ile daha sonra kullanacağımız hash şifre oluşturulur.**

<img width="555" height="108" alt="image" src="https://github.com/user-attachments/assets/7d4557de-3184-4553-a320-e98928e22e1b" />

**5.adım: LDIF dosyasını oluşturabiliriz.**

LDAP ağacındaki nesneleri eklemek, silmek, değiştirmek veya dışa aktarmak için kullanılır.

<img width="472" height="80" alt="image" src="https://github.com/user-attachments/assets/b2681ec8-5dc0-4a73-922a-7d4710faaca3" />
<img width="847" height="527" alt="image" src="https://github.com/user-attachments/assets/064841ad-7595-45e6-84de-54e57666cb1e" />

ldif dosyasındaki bilgileri kendi sunucumuza göre ayarlamalıyız.

**"ldapsearch -Y EXTERNAL -H ldapi:/// -b cn=config dn olcSuffix"** komutu ile bu bilgileri öğrenebiliriz.

<img width="975" height="639" alt="image" src="https://github.com/user-attachments/assets/7d7f7dd9-db56-4914-ac51-0375f4c76f07" />

**6. adım db.ldif dosyasını LDAP' a eklemeyi deneriz.***

<img width="975" height="202" alt="image" src="https://github.com/user-attachments/assets/9818fa14-8fe4-4ad4-9281-f4123a2e24db" />
