---
title: LDAP İstemcisi ve Komutları
updated: 2025-08-31 08:00:55Z
created: 2025-08-31 07:55:35Z
latitude: 39.75054500
longitude: 37.01502170
altitude: 0.0000
---

# LDAP İstemcisi ve Komutları

## 📑 İçindekiler
1. [İstemci (Client) Nedir?](#1-istemci-client-nedir)
2. [LDAP İstemcisi Nedir?](#2-ldap-istemcisi-nedir)
3. [LDAP İstemci Çeşitleri](#3-ldap-istemci-çeşitleri)
4. [Temel LDAP Komutları](#4-temel-ldap-komutları)
5. [Örnek Kullanım Senaryoları](#5-örnek-kullanım-senaryoları)
6. [Güvenlik Notları](#6-güvenlik-notları)
7. [Özet](#-özet)

---

## 1. 🖥️ İstemci (Client) Nedir?

**Basit Tanım:**  
Hizmet talep eden yazılım veya cihazdır.

**Gerçek Hayat Örnekleri:**
- Web tarayıcıları (Chrome, Firefox)
- E-posta programları (Outlook, Thunderbird)
- Oyun konsolları
- Akıllı telefon uygulamaları

**İstemci-Sunucu İlişkisi:**

┌─────────────┐    İstek    ┌─────────────┐
│   İstemci   │ -----------> │   Sunucu    │
│             │ <----------- │             │
└─────────────┘    Yanıt    └─────────────┘



---

## 2. 📡 LDAP İstemcisi Nedir?

**Elektronik Rehber Sorgulama Aracı**

**Ne İşe Yarar?**
- Kullanıcı kimlik doğrulama
- Dizin servisleri sorgulama
- Ağ kaynaklarına erişim yönetimi
- Merkezi kullanıcı yönetimi

**Çalışma Mantığı:**


Kullanıcı → LDAP İstemcisi → LDAP Sunucusu → Veritabanı
(Sorgu) (Yanıt) (Bilgi Deposu)


---

## 3. 🛠️ LDAP İstemci Çeşitleri

**Komut Satırı Araçları**
- `ldapsearch` → Dizin sorgulama
- `ldapadd` → Kayıt ekleme
- `ldapmodify` → Kayıt düzenleme
- `ldapdelete` → Kayıt silme

**Grafik Arayüzler**
- Apache Directory Studio
- LDAP Admin Tool
- phpLDAPadmin (Web tabanlı)

**Entegre Çözümler**
- Windows Active Directory İstemcisi
- Linux PAM LDAP Entegrasyonu

---

## 4. ⚡ Temel LDAP Komutları
Parametreler:

-x : Basit kimlik doğrulama

-h : LDAP sunucu adresi

-b : Arama başlangıç noktası

-D : Bağlanacak kullanıcı

-W : Şifre sor


### 🔍 `ldapsearch` - Arama Sorgulama
```bash
# Temel kullanım
ldapsearch -x -h ldap.sunucu.com -b "dc=sirket,dc=com" "(cn=ahmet*)"

# Detaylı sorgu
ldapsearch -x -D "cn=admin,dc=sirket,dc=com" -W -b "dc=sirket,dc=com" "(objectClass=person)"

➕ ldapadd - Kayıt Ekleme

ldapadd -x -D "cn=admin,dc=sirket,dc=com" -W -f yeni_kullanici.ldif

Örnek LDIF dosyası:

dn: uid=ahmet,ou=users,dc=sirket,dc=com
objectClass: inetOrgPerson
cn: Ahmet Yılmaz
sn: Yılmaz
mail: ahmet@sirket.com


ldapmodify -x -D "cn=admin,dc=sirket,dc=com" -W -f degisiklikler.ldif

dn: uid=ahmet,ou=users,dc=sirket,dc=com
changetype: modify
replace: mail
mail: ahmet.yeni@sirket.com

🗑️ ldapdelete - Kayıt Silme

# Tek kullanıcı silme
ldapdelete -x -D "cn=admin,dc=sirket,dc=com" -W "uid=ahmet,ou=users,dc=sirket,dc=com"

# Çoklu silme
ldapdelete -x -D "cn=admin,dc=sirket,dc=com" -W -f silinecekler.ldif

Örnek Kullanım Senaryoları

Senaryo 1: Kullanıcı Doğrulama

ldapwhoami -x -D "uid=ahmet,ou=users,dc=sirket,dc=com" -W


Senaryo 2: Tüm Kullanıcıları Listeleme

ldapsearch -x -b "ou=users,dc=sirket,dc=com" "(objectClass=person)"


Senaryo 3: Grup Üyeliklerini Sorgulama

ldapsearch -x -b "cn=yoneticiler,ou=groups,dc=sirket,dc=com" member
