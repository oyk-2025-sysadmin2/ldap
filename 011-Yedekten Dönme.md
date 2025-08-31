## Yedekten Geri Dönme (Restore)

LDAP veritabanını yedekten geri yüklemek için aşağıdaki adımlar izlenir. Bu işlem **veritabanını tamamen yedekten alır ve mevcut veriyi siler**, bu yüzden dikkatli olunmalıdır.

### Adımlar

1. LDAP servisini durdur:
```bash
systemctl stop slapd.service
```

- Mevcut veritabanını temizle:
```bash
rm -rf /var/lib/ldap/*
```

- Yedek dosyaları geri yükle:
```bash
slapadd -F /etc/ldap/slapd.d -b cn=config -l /backup/path
slapadd -F /etc/ldap/slapd.d -b dc=example,dc=com -l /backup/path2
```

- İzinleri düzelt:
```
chown -R openldap:openldap /etc/ldap/slapd.d/
chown -R openldap:openldap /var/lib/ldap/
```

- Servisi tekrar başlat:
```
systemctl start slapd.service
```
