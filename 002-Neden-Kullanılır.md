## LDAP(Lightweight Directory Access Protocol)) neden kullanılır?

Bir dizin hizmeti olan LDAP çok kullancılı sistemlerde bu kullanıcılara önceden tanımlanmış şemaya uygun şekilde uzaktaki bir objeye erişmeyi kolay ve hızlı hale getirmek amacıyla geliştirilmiş bir protokoldür.
> LDAP bir veritabanı veya uygulama da değildir.
> kullanıcı, grup, cihaz bilgilerini hiyerarşik yapıda saklar.

'''
dc=example,dc=com                                 --->   Organization kısmı 'dc= (domain component) veya o= (organization)'
 └── ou=MDR Team Member                           --->   Organizational Unit alt grubu 
      └── cn=Kerem Gül                            --->   Individuals  'cn= (common name)'
           ├── userCertificate;binary (X.509)     --->   Resources
           └── mail=kerem.gul@example.com         --->   Resources

'''
bu kaydı sorgulayan bir sistem hem sertifikasını hem e-posta adresini bulabilir.
###  Asıl nerelerde kullanılıyor?

- Printerlar için kullanıcılar sorgulayıp bağlanabilsin, kim ne kadar kullanıyor takip edilebilsin..
- NFS üzerinden paylaşılan bir dizine LDAP sayesinde kullanıcı hangi istemciden login olursa olsun paylaştığı dizinine erişebilsin.
- Özellikle büyük sunucularda Postfix, Sendmail, Dovecot gibi mail sunucuları kullanıcıları LDAP kullanılır,örneğin bir kullanıcıyı bu 3 sistem için tek tek oluşturmak yerine LDAP'ta bir kere tanımlamak.
