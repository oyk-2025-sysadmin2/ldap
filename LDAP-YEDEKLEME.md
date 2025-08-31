#  LDAP YEDEKLEME (BACKUP)

## 1. LDAP Yedekleme Nedir?

 LDAP (Lightweight Directory Access Protocol) dizin servisleri, kullanıcı hesapları, gruplar, erişim yetkileri gibi kritik verileri barındırır. Bu nedenle **yedekleme** veri kaybı durumlarında sistemi orijinal haline geri döndürebilmek için, sistem sürekliliği ve güvenliği açısından kritik öneme sahiptir. 
 

 ## 2. Neden LDAP Yedeklemesi Gereklidir?
 - **Kullanıcı ve Kimlik Verileri** : LDAP, organizasyonun tüm kullanıcı hesapları, şifreleri, grup üyelikleri ve kimlik bilgilerini saklar. Bu veriler kaybolursa veya bozulursa, kullanıcılar sisteme erişemez ve sistemin çalışması durabilir.
 - **Teknik Arızalar** : Donanım arızası, veri bozulması veya yanlış yapılandırmalar durumunda yedekler, sistemin tekrar çalışmasını sağlar.
 - **İnsan Hataları** : Yanlışlıkla kullanıcı veya grup silme, hatalı konfigürasyon değişiklikleri veya yanlış komut çalıştırma gibi insan kaynaklı hatalara karşı yedekler, verilerin korunmasını ve sistemin geri yüklenmesini mümkün kılar.


## 3. LDAP Yedekleme Yöntemleri
### a) Mantıksal Yedekleme (LDIF Export)
### b) Fiziksel Yedekleme (Database File Copy / Binary)
### c) Anlık Görüntü (Snapshot) Yedekleme

## a) Mantıksal Yedekleme (LDIF Export)

**Açıklama:**  
LDAP verileri **LDIF (LDAP Data Interchange Format)** dosyasına aktarılır. İnsan tarafından okunabilir ve taşınabilir bir format sağlar.

**Araç:** `slapcat`

**Örnek Komut:**
```bash
slapcat -n 1 -l /backup/ldap_backup.ldif 
```
**Avantajlar:**
- Platform bağımsızdır, farklı sistemlerde kullanılabilir.
- `slapadd` ile kolayca geri yüklenebilir.

**Dezavantajlar:**
- Büyük veritabanlarında yavaş çalışabilir.
- Canlı veritabanında bazı dinamik veriler dışlanabilir.

## b) Fiziksel Yedekleme (Database File Copy / Binary)

**Açıklama:**  
LDAP’in kullandığı veritabanı dosyaları doğrudan kopyalanır. Symas OpenLDAP için LMDB backend’de `mdb_copy` kullanımı önerilir.

**Yöntem:**
1. Servisi durdur
2. DB dosyalarını kopyala
3. Servisi başlat

**LMDB Örnek Komut:**
```bash
mdb_copy /var/symas/openldap-data/ /backup/ldap_backup/
```

**Avantajlar:**
1. Çok hızlıdır ve birebir kopya çıkarır.
2. Veri bütünlüğü korunur.

**Dezavantajlar:**
1. Donanım ve işletim sistemi bağımlıdır.
2. Geri yükleme için ilgili araçlar (`mdb_copy`) gerekir.

## c) Anlık Görüntü (Snapshot) Yedekleme

**Açıklama:**  
Dosya sistemi veya sanallaştırma ortamında **snapshot** alınarak yedekleme yapılır.

**Avantajlar:**
- Büyük kurumsal ortamlar için idealdir.
- Çok hızlıdır, sistem kesintisi minimumdur.

**Dezavantajlar:**
- Ek altyapı ve depolama gerektirir.
- Yedekler genellikle platforma bağımlıdır.

## 4. Yedekleme Sıklığı

1. **Kritik sistemler** → Günlük yedek önerilir.
2. **Orta ölçekli sistemler** → Haftalık tam yedek + günlük artımlı yedek.
3. **Test ortamları** → Daha seyrek olabilir.