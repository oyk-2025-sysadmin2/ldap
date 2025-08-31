### **LDAP ile Kullanıcı ve Grup Yönetimi**

LDAP, kullanıcı ve grup bilgilerini merkezi bir yerde tuttuğu için bu bilgilerin yönetimini oldukça kolaylaştırır.

#### **Kullanıcı Yönetimi**

LDAP'da bir kullanıcı, benzersiz bir **Ayırt Edici Ad** (Distinguished Name - DN) ile tanımlanır. Bu DN, kullanıcının dizindeki tam konumunu belirtir.

**Örnek bir kullanıcı girdisi:**

```dn
dn: cn=kerem gullu,ou=people,dc=example,dc=com
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetOrgPerson
cn: kerem gullu
sn: gullu
givenName: kerem
uid: keremg
mail: kerem@cartcurt.com
userPassword: {SSHA}hashedsifre
```

Bu örnekte, **kerem gullu** kullanıcısının malumatlarini `ou=people` birimi altında saklanmaktadır. `uid` (kullanıcı kimliği), `cn` (ortak ad) ve `mail` gibi nitelikler (attributes) bu kullanıcıya ait malumatlari taşır.

**Yapabileceğin temel işlemler:**

- **Kullanıcı Ekleme:** Yeni bir kullanıcı için yukarıdaki gibi bir girdi (entry) oluşturup LDAP dizinine eklersin.
- **Kullanıcı Bilgilerini Güncelleme:** Kullanıcının adını, soyadını veya e-posta adresini değiştirebilirsin.
- **Kullanıcı Silme:** Kullanıcı artık çalışmıyorsa veya bilgileri gereksizse, girdisini dizinden silebilirsin.
- **Şifre Sıfırlama:** Kullanıcının şifresini güvenli bir şekilde güncelleyebilirsin.

#### **Grup Yönetimi**

Gruplar, belirli yetkilere veya rollere sahip kullanıcıları bir araya getirmek için kullanılır. LDAP'da bir grup da tıpkı bir kullanıcı gibi bir girdi ile tanımlanır.

**Örnek bir grup girdisi:**

```
dn: cn=developers,ou=groups,dc=example,dc=com
objectClass: top
objectClass: groupOfNames
cn: developers
description: Yazılım geliştirme ekibi
member: cn=kerem gullu,ou=people,dc=example,dc=com
member: cn=morgan bedavaadam,ou=people,dc=example,dc=com
```

Bu örnekte, `developers` (geliştiriciler) grubu, Kerem ve morgan isimli kullanıcıları içermektedir.

Dosyayı hazırladıktan sonra, `ldapadd` komutunu kullanarak dosyayı LDAP sunucusuna gönderin.

```bash 
ldapadd -x -D "cn=admin,dc=example,dc=com" -W -f grup-kayit.ldif
```

- `-x`: Basit kimlik doğrulama kullanır.
- `-D`: Bağlanacağın yönetici kullanıcısını belirtir (**D**istinguished **N**ame).
- `-W`: Şifreyi komut satırında yazmak yerine, güvenli bir şekilde sorar (**W**ith password).
- `-f`: İşlem yapılacak `.ldif` dosyasını belirtir (**F**ile).
- 
**Yapabileceğin temel işlemler:**

- **Grup Oluşturma:** Yeni bir grup (örneğin, `sales`) oluşturabilirsin.
- **Kullanıcıları Gruba Ekleme/Çıkarma:** Bir kullanıcıyı gruba `member` niteliği olarak ekleyebilir veya çıkarabilirsin.
- **Grup Silme:** Bir grubun artık gerekli olmadığı durumlarda silebilirsin.

### **LDAP Yönetimi için Araçlar**

LDAP'i komut satırı ile yönetmek zor olabilir. Bu nedenle, kullanımı kolaylaştıran çeşitli araçlar mevcuttur:

- **LDAP Admin:** LDAP dizinini grafik arayüz üzerinden yönetmek için kullanılan bir araçtır.
    
- **Apache Directory Studio:** Özellikle ApacheDS sunucusu için geliştirilmiş, LDAP dizinini yönetmek ve geliştirmek için güçlü bir araçtır.
    
- **Komut Satırı Araçları:** `ldapadd`, `ldapmodify`, `ldapdelete` gibi temel komut satırı araçları da bulunur.

LDAP'ı komut satırı araçlarıyla kullanmak, hem otomasyon hem de hızlı işlemler için oldukça kullanışlıdır. En yaygın kullanılan araçlar `ldapadd`, `ldapmodify`, `ldapsearch` ve `ldapdelete`'dir. Bu araçlar, LDAP dizinine bağlanmak, bilgi eklemek, değiştirmek, aramak ve silmek için kullanılır.

Aşağıda, bu araçları kullanarak kullanıcı ve grup yönetimi için yapabileceğin temel işlemleri anlattım.

---

### **1. Kullanıcı Ekleme (`ldapadd`)**

Yeni bir kullanıcı eklemek için önce `.ldif` (LDAP Data Interchange Format) uzantılı bir dosya oluşturman gerekir. Bu dosya, kullanıcının bilgilerini içerir.

**Örnek: `kullanici-ekle.ldif`**

LDIF
![[Pasted image 20250831114043.png]]
```dn
dn: cn=morgan bedavaadam,ou=people,dc=example,dc=com
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetOrgPerson
cn: morgan bedavaadam
sn: morgan
givenName: bedavaadam
uid: morganb
mail: morgan@freeman.com
userPassword: {SSHA}hashedsifre
```

Bu dosya ile kullanıcıyı eklemek için aşağıdaki komutu kullanabilirsin:

Bash

```bash
ldapadd -x -D "cn=admin,dc=example,dc=com" -W -f kullanici-ekle.ldif
```

- `-x`: Basit kimlik doğrulama kullanır.
- `-D`: Bağlanacağın yönetici kullanıcısını belirtir (**D**istinguished **N**ame).
- `-W`: Şifreyi komut satırında yazmak yerine, güvenli bir şekilde sorar (**W**ith password).
- `-f`: İşlem yapılacak `.ldif` dosyasını belirtir (**F**ile).

---

### **2. Kullanıcı Arama (`ldapsearch`)**

Dizindeki bir kullanıcıyı veya bir grup bilgiyi bulmak için `ldapsearch` kullanılır. Bu, en sık kullanacağın komutlardan biridir.

**Örnek 1: Bütün kullanıcıları listeleme**

Bash

```bash
ldapsearch -x -b "ou=people,dc=example,dc=com" "(objectClass=inetOrgPerson)"
```

- `-b`: Aramanın başlayacağı ana dizini belirtir (**B**ase DN).
- `(objectClass=inetOrgPerson)`: Arama filtresidir. Bu filtre, sadece `inetOrgPerson` sınıfına ait girdileri getirir.

**Örnek 2: Belirli bir kullanıcıyı arama**

Bash

```bash
ldapsearch -x -b "ou=people,dc=example,dc=com" "cn=morgan bedavaadam"
```

Bu komut, `morgan bedavaadam` adlı kullanıcının tüm bilgilerini döndürür.

---

### **3. Kullanıcı Bilgilerini Değiştirme (`ldapmodify`)**

Bir kullanıcının bilgilerini güncellemek için yine bir `.ldif` dosyası oluşturulur. Bu dosyada, yapılacak değişikliğin türü (`add`, `replace`, `delete`) belirtilir.
Bu temel komutlar, LDAP dizininin yönetimini komut satırından etkili bir şekilde yapmanı sağlar. Özellikle toplu işlemler veya otomasyon senaryoları için bu araçlar vazgeçilmezdir.
Örnek: bilgi-guncelle.ldif

morgan'nin e-posta adresini değiştirmek için:

LDIF

```ldif
dn: cn=morgan bedavaadam,ou=people,dc=example,dc=com
changetype: modify
replace: mail
mail: morgan@bedavamiadam.com
```

Bu `.ldif` dosyası ile değişikliği yapmak için:

Bash

```bash
ldapmodify -x -D "cn=admin,dc=example,dc=com" -W -f bilgi-guncelle.ldif
```

---

### **4. Kullanıcı Silme (`ldapdelete`)**

Bir kullanıcıyı veya herhangi bir dizin girdisini silmek oldukça basittir. Sadece kullanıcının **Ayırt Edici Adı**'nı (DN) belirtmen yeterli.

**Örnek:**

Bash

```
ldapdelete -x -D "cn=admin,dc=example,dc=com" -W "cn=morgan bedavaadam,ou=people,dc=example,dc=com"
```

Bu komut, `morgan bedavaadam` kullanıcısını LDAP dizininden tamamen siler.

---

### **5. Grup Yönetimi**

Grup yönetimi de benzer mantıkla çalışır.
Grup Oluşturma (ldapadd)
Örnek: grup-ekle.ldif

LDIF

```
dn: cn=developers,ou=groups,dc=example,dc=com
objectClass: top
objectClass: groupOfNames
cn: developers
description: Yazilim Gelistirme Ekibi
```

Bash

```bash
ldapadd -x -D "cn=admin,dc=example,dc=com" -W -f grup-ekle.ldif
```

Kullanıcıyı Gruba Ekleme (ldapmodify)

Bir kullanıcıyı gruba eklemek için member niteliğini kullanırız.

Örnek: grup-uye-ekle.ldif
LDIF

```dn
dn: cn=developers,ou=groups,dc=example,dc=com
changetype: modify
add: member
member: cn=morgan bedavaadam,ou=people,dc=example,dc=com
```

Bash

```bash
ldapmodify -x -D "cn=admin,dc=example,dc=com" -W -f grup-uye-ekle.ldif
```

Harika, görseli istediğin isimlerle güncelleyelim. İşte Kerem Güllü ve Morgan Bedavaadam isimleriyle yenilenmiş LDAP dizin yapısı şeması:


```
      dc=example
          |
          +-- dc=com
                  |
                  +-- ou=people
                  |       |
                  |       +-- cn=kerem gullu (objectClass: inetOrgPerson)
                  |               |
                  |               +-- uid: keremg
                  |               +-- mail: keremg@example.com
                  |
                  |       +-- cn=morgan bedavaadam (objectClass: inetOrgPerson)
                  |               |
                  |               +-- uid: morganb
                  |               +-- mail: morganb@example.com
                  |
                  +-- ou=groups
                          |
                          +-- cn=developers (objectClass: groupOfNames)
                          |       |
                          |       +-- member: cn=kerem gullu,ou=people,dc=example,dc=com
                          |
                          +-- cn=deneme-grubu (objectClass: groupOfNames)
                                  |
                                  +-- member: cn=kerem gullu,ou=people,dc=example,dc=com
                                  +-- member: cn=morgan bedavaadam,ou=people,dc=example,dc=com
```