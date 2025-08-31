# LDAP'ta Veri Yönetimi 
## 1. Veri Modeli ve Şema
### 1.1 Veri modeli

-   LDAP, bilgileri entry adı verilen kayıtlar halinde saklar.
-   Her entry bir nesnedir: kullanıcı, grup, cihaz vb.
-   Objeler belirli bir şemaya göre tanımlanır.

**Örnek:**

``` ldif
dn: uid=serif,ou=users,dc=example,dc=com
objectClass: inetOrgPerson
cn: serif ceylan
sn: ceylan
mail: serif@example.com
uid: serif
```
### 1.2 Şema
-  LDAP şeması, hangi tipte entry’lerin olacağını ve hangi attribute’ların olacağını belirler.
-  Şema olmadan LDAP, hangi bilgileri saklayacağını bilemez.

    -  inetOrgPerson → Klasik kullanıcı bilgilerini tutar

    - organizationalUnit → Organizasyon birimi

    - groupOfNames → Grup bilgisi

### 1.3 LDAP Ağacındaki Yerleşim

- LDAP veritabanı ağaç (DIT – Directory Information Tree) şeklindedir:

```
dc=com
  └─ dc=example
        └─ ou=users
              └─ uid=ali
```
- En üst seviye = domain (dc=com ve dc=example)

- Alt birim = organizasyon birimi (ou=users)

- Yaprak = kullanıcı veya grup entry’si (uid=ali)
------------------------------------------------------------------------

## 2. Kullanıcı Yaşam Döngüsü

-   **Oluşturma:** Kullanıcı ekleme
-   **Güncelleme:** Bilgi değiştirme
-   **Silme:** Kullanıcıyı kapatma

**Örnek Kullanıcı Ekleme (LDIF):**

``` ldif
dn: uid=serif,ou=users,dc=example,dc=com
objectClass: inetOrgPerson
cn: serif ceylan
sn: ceylan
mail: serif@example.com
uid: serif
```

**Kullanıcı Ekleme Komutu(LDIF):**

```
ldapadd -x -D "cn=Manager,dc=example,dc=com" -w secret -f ali.ldif

```

------------------------------------------------------------------------

## 3. Tutarlılık ve Doğruluk

-   Aynı e-posta 2 kişide olmamalı.
-   Silinen grup üyelikleri temizlenmeli.

**Örnek Arama (aynı e-postayı kontrol et):**

``` bash
ldapsearch -x -b "dc=example,dc=com" "(mail=ali@example.com)"
```
- x: basit kimlik doğrulama
- b: arama veya işlem yapılacak başlangıç noktasını belirtir

------------------------------------------------------------------------

## 4. Performans

-   Aramalarda hız için indeks kullanılır.

**Örnek: `uid` alanını indeksleme (slapd.conf):**

    index uid eq

------------------------------------------------------------------------

## 5. Güvenlik

-   Herkes her bilgiyi göremez.
-   Şifreler şifreli tutulur.

**Örnek ACL (cn=admin her şeyi görsün, kullanıcılar sadece kendi
bilgilerini):**

    access to *
      by dn="cn=admin,dc=example,dc=com" write
      by self read
      by * none

------------------------------------------------------------------------

## 6. Yedekleme ve Kurtarma

-   `slapcat` ile yedek alınır.\
-   `slapadd` ile geri yüklenir.

**Örnek Yedekleme:**

``` bash
slapcat -l backup.ldif
```

**Örnek Geri Yükleme:**

``` bash
slapadd -l backup.ldif
```

------------------------------------------------------------------------

## 7. Temizlik ve Arşivleme

-   Silinen kullanıcılar "çöp kutusu"na taşınabilir.

**Örnek Silme:**

``` bash
ldapdelete -x "uid=ayse,ou=users,dc=example,dc=com"
```

------------------------------------------------------------------------

## 8. İzleme

-   Kim, neyi değiştirmiş → audit log
-   Performans ölçümü → monitoring

**Örnek LDAP sorgu sayısını log'dan kontrol et:**

``` bash
tail -f /var/log/slapd.log
```

------------------------------------------------------------------------

## 9. Büyük Sistemlerde Yönetim

-   Kullanıcı sayısı çok olursa veriler bölünür.
-   Replikasyon ile yük dağıtılır.

**Örnek Replikasyon Config (syncrepl):**

    syncrepl rid=001
      provider=ldap://ldap-master.example.com
      type=refreshAndPersist
      searchbase="dc=example,dc=com"
      bindmethod=simple
      binddn="cn=replicator,dc=example,dc=com"
      credentials=secret


