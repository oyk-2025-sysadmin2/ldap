# 004 LDAP Alternatifleri



#  Dizin Servisleri, Kimlik Yönetimi ve Kimlik Doğrulama Protokolleri

# Kimlik Yönetimi Bileşenleri

## 1. LDAP Tabanlı Dizin Servisleri
**Tanım:**  
Kullanıcı, grup, bilgisayar, cihaz gibi nesneleri saklayan, genelde ağaç yapılı (directory tree) bir veritabanı.  
LDAP (Lightweight Directory Access Protocol) ile sorgulanır.  

**Görev:**  
"Kim var?" → Kullanıcı listesi, şifre hash’i, grup üyeliği gibi bilgileri tutar.  

**Örnekler:**  
- Microsoft Active Directory (AD)  
- FreeIPA  
- OpenLDAP  
- 389 Directory Server  
- Apache Directory  
- SambaBox  

**Benzerlik:** Hepsi kullanıcı/kimlik bilgilerini saklar.  
**Fark:** Bazıları sadece dizin (**OpenLDAP**), bazıları entegre (**AD = LDAP + Kerberos + DNS**).  

---

## 2. Kimlik / Federasyon Yönetimi
**Tanım:**  
Farklı sistemler arasında kimlik doğrulamayı merkezi hale getiren çözümler.  
Amaç: Tek oturum açma (SSO), kimlik federasyonu (bir kurum hesabıyla başka servislere giriş).  

**Görev:**  
"Kim giriş yapabilir?" ve "Hangi uygulamaya erişebilir?" sorularını çözer.  

**Örnekler:**  
- Keycloak  
- CAS (Central Authentication Service)  
- Okta  
- Auth0  
- Gluu  
- Azure AD (bulut)  

**Benzerlik:** LDAP/AD gibi dizinlerle entegre çalışır, oradaki kullanıcıları kullanır.  
**Fark:** Dizin sadece kullanıcı bilgisi tutarken, federasyon sistemi uygulamalar arası oturum açmayı yönetir.  

---

## 3. Kimlik Doğrulama Protokolleri
**Tanım:**  
Kimlik bilgilerini nasıl doğrulayacağımızı belirleyen iletişim kurallarıdır.  

**Görev:**  
"Kullanıcı gerçekten kim?" sorusunu güvenli şekilde cevaplar.  

**Örnekler:**  
- **Kerberos** → bilet tabanlı, AD/FreeIPA’da kullanılır  
- **RADIUS / TACACS+** → ağ cihazları ve VPN kimlik doğrulaması  
- **SAML** → XML tabanlı web federasyonu  
- **OAuth2 / OpenID Connect (OIDC)** → modern web/mobil kimlik federasyonu  

**Benzerlik:** Hepsi kimliği doğrular.  
**Fark:** Hedef alanları farklıdır → Kerberos (ağ içi), RADIUS (ağ cihazı), OAuth2 (web/mobil).  

---

##  Özet – Farklar ve Benzerlikler
- **Dizin Servisi** = Kullanıcı bilgilerini depolar. *(Kim var?)*  
- **Kimlik / Federasyon Yönetimi** = Kullanıcıların uygulamalara erişimini düzenler. *(Kim nereye girebilir?)*  
- **Kimlik Doğrulama Protokolü** = Kullanıcının gerçekten o kişi olup olmadığını kanıtlar. *(Kim olduğunu nasıl kanıtlar?)*  

Çoğu ortamda üçü birlikte çalışır:  

 **Örnek: Microsoft Active Directory**  
- **Dizin servisi:** LDAP  
- **Kimlik doğrulama:** Kerberos  
- **Federasyon:** ADFS / Azure AD entegrasyonu  



## 1. LDAP Tabanlı Dizin Servisleri
- **Tanım:** Kullanıcı, grup, bilgisayar ve cihaz gibi nesneleri saklayan hiyerarşik (directory tree) bir veritabanıdır; LDAP protokolü ile erişilir.  
- **Görev:** "Kim var?" sorusuna cevap verir — kullanıcı listesi, şifre hash’i, grup üyelikleri vs.  
- **Kaynak:** [LDAP - Wikipedia](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol)

| **Teknoloji / Servis** | **Türü** | **Temel Rolü** | **Kullanım Alanı** |
|------------------------|----------|----------------|---------------------|
| [**OpenLDAP**](https://en.wikipedia.org/wiki/OpenLDAP) | Özgür/Açık Kaynak | Saf LDAP | Küçük/orta ölçek, uygulama entegrasyonu |
| [**389 Directory Server**](https://directory.fedoraproject.org/) | Özgür/Açık Kaynak | Yüksek performanslı LDAP | Kurumsal sistemler, üniversiteler |
| [**Apache Directory**](https://directory.apache.org/) | Özgür/Açık Kaynak | Java-tabanlı LDAP | Test, eğitim, küçük işletmeler |
| [**Samba / Samba4 AD DC**](https://wiki.samba.org/) | Özgür/Açık Kaynak | AD uyumlu LDAP + Kerberos + SMB | Linux/BSD’de AD alternatifi |
| [**SambaBox**](https://sambabox.io/2024/04/05/openldap-active-directory-sambabox-comparison/) | Özgür/Açık Kaynak | Samba + LDAP + Kerberos | Açık kaynak AD benzeri çözüm |
| [**Liderahenk (TÜBİTAK)**](https://liderahenk.org.tr/) | Özgür/Açık Kaynak | LDAP-tabanlı merkezi yönetim | Kamu kurumları, istemci yönetimi |
| [**Univention Corporate Server (UCS)**](https://www.univention.com/products/ucs/) | Özgür/Açık Kaynak | Samba + LDAP AD uyumlu | Açık kaynak AD alternatifi |
| [**FreeIPA**](https://en.wikipedia.org/wiki/FreeIPA) | Özgür/Açık Kaynak | LDAP + Kerberos + DNS + CA | Linux tabanlı kimlik yönetimi |
| [**ForgeRock DS**](https://backstage.forgerock.com/docs/ds) | Açık Kaynak kökenli, ticarî | LDAP tabanlı dizin | Kurumsal dizin altyapısı |
| [**Microsoft Active Directory (AD)**](https://en.wikipedia.org/wiki/Active_Directory) | Ticarî | LDAP + Kerberos + DNS | Windows domain, kurumsal ağ |
| [**Azure AD (Entra ID)**](https://learn.microsoft.com/en-us/entra/identity/) | Ticarî/Bulut | Bulut tabanlı AD benzeri | Microsoft 365, SaaS entegrasyonları |
| [**JumpCloud**](https://jumpcloud.com/) | Ticarî/Bulut | Directory-as-a-Service | AD/LDAP alternatifi, SaaS entegrasyonu |

---

## 2. Kimlik / Federasyon Yönetimi
- **Tanım:** Farklı sistemler arasında kimlik doğrulamayı ve uygulamalar arası Single Sign-On (SSO) sağlamak için kullanılan çözümler.  
- **Görev:** "Kim giriş yapabilir?" ve "Hangi uygulamaya erişebilir?" sorularına cevap verir.  
- **Kaynak:** [Identity Federation - NIST](https://csrc.nist.gov/glossary/term/identity_federation)

| **Teknoloji / Servis** | **Türü** | **Temel Rolü** | **Kullanım Alanı** |
|------------------------|----------|----------------|---------------------|
| [**Keycloak**](https://en.wikipedia.org/wiki/Keycloak) | Özgür/Açık Kaynak | OAuth2, OIDC, SAML desteği | Web/mobil uygulamalar için IAM |
| [**CAS (Central Authentication Service)**](https://apereo.github.io/cas/6.6.x/index.html) | Özgür/Açık Kaynak | Web tabanlı SSO | Üniversite/kampüs sistemleri |
| [**Gluu Server**](https://gluu.org/) | Özgür/Açık Kaynak | LDAP + SAML + OIDC desteği | Kurumsal federasyon |
| [**Auth0**](https://auth0.com/docs) (Okta’ya bağlı) | Ticarî/Bulut | OAuth2/OIDC kimlik platformu | Web ve mobil uygulamalar |
| [**Okta**](https://www.okta.com/) | Ticarî/Bulut | Kimlik yönetimi (IDaaS) | SaaS entegrasyonu |
| [**Azure AD**](https://learn.microsoft.com/en-us/entra/identity/) (tekrar) | Ticarî/Bulut | Dizin + federasyon işlevi | Microsoft 365, bulut entegrasyonu |

---

## 3. Kimlik Doğrulama Protokolleri
- **Tanım:** Kimlik doğrulamanın nasıl yapılacağını tanımlayan, standart iletişim yöntemleri/protokolleridir.  
- **Görev:** "Kullanıcı gerçekten kim?" sorusunu güvenli bir şekilde çözmek.  
- **Kaynak:** [Single Sign-On - NIST](https://csrc.nist.gov/glossary/term/single_sign_on)

| **Protokol** | **Türü** | **Temel Rolü** | **Kullanım Alanı** |
|--------------|----------|----------------|---------------------|
| [**Kerberos**](https://web.mit.edu/kerberos/) | Açık Standart | Bilet tabanlı kimlik doğrulama | AD, FreeIPA gibi dizin servislerinde |
| [**RADIUS**](https://en.wikipedia.org/wiki/RADIUS) | Açık Standart (IETF) | Ağ cihazları kimlik doğrulama | VPN, Wi-Fi, 802.1X |
| [**TACACS+**](https://en.wikipedia.org/wiki/TACACS%2B) (Cisco) | Açık Standart | Komut düzeyinde yetkilendirme | Ağ cihazları yönetimi |
| [**SAML**](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) | Açık Standart (OASIS) | XML tabanlı federasyon protokolü | Web SSO, kurumsal sistemler |
| [**OAuth 2.0**](https://oauth.net/2/) | Açık Standart (IETF) | Yetkilendirme protokolü | Web/mobil uygulamalar |
| [**OpenID Connect (OIDC)**](https://openid.net/connect/) | Açık Standart (IETF) | OAuth2 üzerine kimlik doğrulama | Modern federasyon sistemleri |

