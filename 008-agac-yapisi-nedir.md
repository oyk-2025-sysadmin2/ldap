---
title: Ağaç Yapısı Nedir
author: Taha <mt190502@mtaha.dev>
created: 2025-08-31T10:55:35+03
---
## Ağaç Yapısı Nedir?

- Ağaç Yapısı, verilerin hiyerarşik olarak organize edilmesi için kullanılan bir yapıdır. Gerçek hayattaki gibi ağacın dallanma mantığına benzer şekilde üstten alta doğru genişleyen mantıksal yapıyı temsil eder.

## Örnek Bir Ağaç Yapısı

```mermaid
graph TD;
A[Bolu Abant İzzet Baysal Üniversitesi]

A --> B[Mühendislik Fakültesi] --> B1[Bolu Yaz Kampı]
A --> C[İlahiyat Fakültesi]
A --> D[Tıp Fakültesi]

subgraph B1[Bolu Yaz Kampı]
 B11[Linux'un İç Yapısı: eBPF'e Giriş] --> X1[Rıza Engür Pişirici]
 B12[GNU/Linux Sistem Yönetimi 1. Düzey] --> X2[Doruk Fişek]
 B13[GNU/Linux Sistem Yönetimi 2. Düzey] --> X3[Ahmetcan İrdem]
end
```

## LDAP'ta Ağaç Yapısı ve Bazı Temel Bileşenleri

| LDAP Niteligi     | Nitelik Kodu | Nitelik Aciklamasi       |
| ----------------- | ------------ | ------------------------ |
| Country           | c            | Ülke                     |
| State             | st           | İl/Eyalet                |
| Domain Component  | dc           | Etki Alanı Öğesi         |
| Organization      | o            | Organizasyon Adı         |
| Organization Unit | ou           | Organizasyon Ünitesi Adı |
| Common Name       | cn           | Ortak İsim               |
| UserID            | uid          | Kullanıcı ID'si          |

## Örnek Ağaç Yapısının LDAP'ta Gösterimi

```mermaid
graph TD;
A["dc=ibu,dc=edu,dc=tr"]

A --> B["dc=mf"]
A --> C["dc=ilahiyat"]
A --> D["dc=tip"]

B --> B1["o=bolu-yaz-kampi"]

subgraph B1["o=bolu-yaz-kampi"]
 B11["ou=linux-ebpf"] --> X1["cn=Rıza Engür Pişirici"]
 B12["ou=gnu-linux-sysadmin-1"] --> X2["cn=Doruk Fişek"]
 B13["ou=gnu-linux-sysadmin-2"] --> X3["cn=Ahmetcan İrdem"]
end
```

## Kaynaklar

- <https://bidb.itu.edu.tr/seyir-defteri/blog/2013/09/06/ldap-(lightweight-directory-access-protocol---hafifletilmi%C5%9F-dizin-eri%C5%9Fim-protokol%C3%BC)>
- <https://en.wikipedia.org/wiki/Tree_structure>
- <https://en.wikipedia.org/wiki/Directory_service>
- <https://www3.rocketsoftware.com/rocketd3/support/documentation/Uniface/10/uniface/dbmsSupport/dbmsDrivers/LDAP/concepts/LDAP_data_structure.htm>
