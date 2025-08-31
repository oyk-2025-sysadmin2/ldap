# LDAP Nedir?
## LDAP (Lightweight Directory Access Protocol) - Hafif Dizin Erişim Protokolü  


**LDAP (Lightweight Directory Access Protocol)**, dizin bilgi hizmetlerine erişmek ve bu bilgileri yönetmek için kullanılan bir **ağ protokolüdür**. Temel amacı, merkezi bir veritabanında (dizin) saklanan bilgilere (örneğin kullanıcı hesapları, cihaz bilgileri) TCP/IP ağları üzerinden erişimi standartlaştırmaktır. LDAP, **X.500** gibi karmaşık dizin standartlarının hafif ve internet dostu bir versiyonu olarak geliştirilmiştir.

**Dizin hizmeti nedir?**
Dizin, temel arama ve güncelleme işlevlerini desteklemenin yanı sıra, arama ve göz atma için özel olarak tasarlanmış özel bir veritabanıdır.

-  Dizin hizmetine **erişmek ve verileri sorgulamak/değiştirmek** için kullanılan **iletişim standardı**.
-  **LDAP bir protokoldür**, dizin hizmetinin kendisi değildir.
-   Örneğin, **Microsoft Active Directory**, LDAP protokolünü kullanarak dizin hizmeti sunar.
-   Diğer örnekler: OpenLDAP, 389 Directory Server, Apache Directory Server. 
 - LDAP için standart TCP bağlantı noktaları, şifrelenmemiş iletişim için 389 ve TLS şifreli kanal üzerinden LDAP için 636'dır.



### **Tarihsel Gelişim**

LDAP, **X.500** standartının internet için optimize edilmiş bir türevi olarak ortaya çıkmıştır:

1.  **1980'ler: X.500 Standardı**
    
    -   ITU(Uluslararası Telekomünikasyon Birliği) ve ISO(Uluslararası Standartlar Teşkilatı)  tarafından geliştirilen X.500, telekomünikasyon ağları için bir dizin hizmeti standardıydı.
    -   Ancak X.500, **DAP (Directory Access Protocol)** kullanıyordu ve TCP/IP ağları için çok ağır (karmaşık) kabul ediliyordu.
2.  **1991-1993: LDAP v1 ve v2**
    
    -   **Tim Howes** ve ekibi (Michigan Üniversitesi), X.500'ün DAP yerine **TCP/IP** kullanan hafif bir protokol tasarladı.
    -   **LDAP v1** (1991): İlk basit sürüm (RFC 1487).
    -   **LDAP v2** (1993): Daha yaygın kullanım için iyileştirildi (RFC 1777). Ancak güvenlik ve esneklik eksikti.
3.  **1997: LDAP v3 (Güncel Standart)**
    
    -   IETF tarafından **RFC 2251** ile standartlaştırıldı.
    -   Temel geliştirmeler:
        
        -   **SASL (Simple Authentication and Security Layer)** ile güçlü kimlik doğrulama.
        -   **StartTLS** desteği (şifreli iletişim).
        -   **Referrals** (sorguları diğer sunuculara yönlendirme).
        -   **Şema esnekliği** ve uluslararası karakter desteği.
        
    -   Günümüzde neredeyse tüm uygulamalar **LDAP v3** kullanır.

Bir araç satışı için "Tesla Model 3, siyah" arıyorsunuz.

-   **Dizin Hizmeti = Araç Stok Alanı**
    
    -   Tüm araçlar (marka, model, renk) **stok alanına düzenli yerleştirilmiştir**.
    -   Stok, **fiziksel veri deposudur** (dizin hizmeti gibi).
    
-   **LDAP = Satış Elemanının Tablet'i**
    
    -   Tablet'e "Tesla Model 3 siyah" yazdığında **stok konumunu gösterir** (örneğin, _Blok B, Sıra 4_).
    -   Tablet, **stok verisine erişim protokolüdür** (LDAP gibi).

#### **Neden Aynı Değil?**

-   Stok **olmadan** tablet boş bilgi verir (araç yok).
-   Tablet **olmadan** tüm blokları gezerek aramak zorunda kalırsınız (LDAP olmadan sorgu imkânsız).

   
