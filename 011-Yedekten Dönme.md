## Yedekten Geri Dönme (Restore)

LDAP veritabanını yedekten geri yüklemek için aşağıdaki adımlar izlenir. Bu işlem **veritabanını tamamen yedekten alır ve mevcut veriyi siler**, bu yüzden dikkatli olunmalıdır.

### Adımlar

1. mdb ile:



2. slapcat yedeklemesi ile:

- LDAP servisini durdur:
```bash
systemctl stop slapd.service
```

- Mevcut veritabanını temizle:
```bash
rm -rf /var/lib/ldap/*
```

- Yedek dosyaları geri yükle:
```bash
slapadd -F /etc/ldap/slapd.d -b dc=example,dc=com -l /backup/ldap_backup.ldif
```

- İzinleri düzelt:
```
chown -R openldap:openldap /etc/ldap/slapd.d/
```

- Servisi tekrar başlat:
```bash
systemctl start slapd.service
```



### Kaynaklar

- https://documentation.ubuntu.com/server/how-to/openldap/backup-and-restore/
- https://openldap.org/doc/admin25/maintenance.html
