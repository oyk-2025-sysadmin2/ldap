## LDAP(Lightweight Directory Access Protocol)) neden kullanılır?

Bir dizin hizmeti olan LDAP çok kullancılı sistemlerde bu kullanıcılara önceden tanımlanmış şemaya uygun şekilde uzaktaki kişinin mail gibi bilgilerine ulaşmayı kolay hale getirmek amacıyla geliştirilmiş bir protokoldür.
> LDAP bir veritabanı veya uygulama da değil, protokoldür.
> kullanıcı, grup, cihaz bilgilerini hiyerarşik yapıda saklar.

"""
dc=example,dc=com                                 --->   Organization kısmı 'dc= (domain component) veya o= (organization)'
 └── ou=MDR Team Member                           --->   Organizational Unit alt grubu 
      └── cn=Kerem Gül                            --->   Individuals  'cn= (common name)'
           ├── userCertificate;binary (X.509)     ---> Resources
           └── mail=kerem.gul@example.com         ---> Resources

"""
Böylece bu kaydı sorgulayan bir sistem hem sertifikasını hem e-posta adresini bulabilir.
