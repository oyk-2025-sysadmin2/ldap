## Yedekten Geri Dönme (Restore)

LDAP veritabanını yedekten geri yüklemek için aşağıdaki adımlar izlenir. Bu işlem **veritabanını tamamen yedekten alır ve mevcut veriyi siler**, bu yüzden dikkatli olunmalıdır.

## Adımlar

### mdb ile:

- LDAP servisini durdur (açıksa):
```bash
systemctl stop slapd.service
```

- Yedek dosyaları geri yükle:
```bash
mdb_copy /backup/ldap_backup/ /var/lib/ldap/
```
NOT: Bu, halihazırda var olan `/var/lib/ldap/*` dosyalarını temizleyecektir.

- İzinleri düzelt:
```
sudo chown -R openldap:openldap /var/lib/ldap
```

- Servisi tekrar başlat:
```bash
systemctl start slapd.service
```

### LDIF dosyanız varsa (`slapcat` metodu):

- LDAP servisini durdur (açıksa):
```bash
systemctl stop slapd.service
```

- Mevcut veritabanını temizle:
```bash
rm -rf /var/lib/ldap/*
```
Not: Ekstra önlem amacıyla dizini silmek yerine dizinin adını değiştirebilirsiniz. 

- Yedek dosyaları geri yükle:
```bash
slapadd -n 1 -l /backup/ldap_backup.ldif
```

- İzinleri düzelt:
```
sudo chown -R openldap:openldap /var/lib/ldap
```

- Servisi tekrar başlat:
```bash
systemctl start slapd.service
```



## Kaynaklar

- https://documentation.ubuntu.com/server/how-to/openldap/backup-and-restore/
- https://openldap.org/doc/admin25/maintenance.html
