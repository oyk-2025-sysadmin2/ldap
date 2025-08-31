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
              └─ uid=serif
```
- En üst seviye = domain (dc=com ve dc=example)

- Alt birim = organizasyon birimi (ou=users)

- Yaprak = kullanıcı veya grup entry’si (uid=serif)
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
**E-posta güncelleme:**

``` dn: uid=ali,ou=users,dc=example,dc=com
changetype: modify
replace: mail
mail: ali.vural@example.com
```

**Komut:**

```
ldapmodify -x -D "cn=Manager,dc=example,dc=com" -w secret -f modify.ldif


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

-   LDAP veritabanında performans, arama ve ekleme işlemlerinin hızlı olması anlamına gelir.
-   Aramalarda hız için indeks kullanılır.

**Örnek: `uid` alanını indeksleme (slapd.conf):**

    index uid eq

------------------------------------------------------------------------

## 5. Güvenlik

-   LDAP güvenliği, verilerin yetkisiz kişilerce görülmesini veya değiştirilmesini engellemeye odaklanır.

**Örnek ACL (cn=admin her şeyi görsün, kullanıcılar sadece kendi
bilgilerini):**

    access to *
      by dn="cn=admin,dc=example,dc=com" write
      by self read
      by * none

------------------------------------------------------------------------

## 6. Yedekleme ve Kurtarma

-   LDAP veritabanı, kritik bilgiler içerir. Bu yüzden yedek almak ve gerektiğinde geri yüklemek çok önemlidir.
-   `slapcat` ile yedek alınır.
-   `slapadd` ile geri yüklenir.

**Örnek Yedekleme:**

``` bash
slapcat -l backup.ldif
```

**Örnek Geri Yükleme:**

``` bash
slapadd -l backup.ldif
```

- Geri yükleme sonrası, veritabanını kontrol etmek için:
```
ldapsearch -x -b "dc=example,dc=com" "(objectClass=*)"

```

------------------------------------------------------------------------

## 7. Temizlik ve Arşivleme

-   Kullanılmayan veya eski kullanıcı hesapları devre dışı bırakılır veya silinir.
-   Ama önce arşivleme yapılması önerilir, çünkü bilgiler geri alınabilir olmalı.
-   Silinen kullanıcılar "çöp kutusu"na taşınabilir.

**Örnek Silme:**

``` bash
ldapdelete -x "uid=ayse,ou=users,dc=example,dc=com"
```

------------------------------------------------------------------------

## 8. İzleme

-   LDAP sunucusu yaptığı tüm işlemleri log dosyalarına yazar.
-   Loglar sayesinde kim neyi değiştirdi, hangi hatalar oluştu takip edilebilir.

**Örnek LDAP sorgu sayısını log'dan kontrol et:**

``` bash
tail -f /var/log/slapd.log
```

------------------------------------------------------------------------

## 9. Büyük Sistemlerde Yönetim

-   Büyük şirketlerde veya kurumlarda LDAP veritabanı çok fazla kullanıcı ve grup içerir. Bu durumda performans, güvenlik ve veri yönetimi stratejileri önem kazanır.
-   Kullanıcı sayısı çok olursa veriler bölünür.
-   Replikasyon ile yük dağıtılır.

