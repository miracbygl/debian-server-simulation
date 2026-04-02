# 🖥️ Debian Sunucu Altyapı Simülasyonu

**Bursa Teknik Üniversitesi - Sunucu İşletim Sistemi Projesi**

Bu proje, Debian tabanlı bir sunucu üzerinde gerçek bir hosting altyapısının nasıl inşa edileceğini göstermek amacıyla hazırlanmıştır. Sistem; **Web (Apache)**, **DNS (Bind9)**, **FTP (VSFTPD)** servislerini ve **Subdomain** mimarisini barındıran tam kapsamlı bir yerel ağ simülasyonudur.

---

## 🚀 Mimari Yapı ve Kurulan Servisler

* **🌐 Ağ ve İşletim Sistemi (Debian 12):**
    Sunucunun izole ve güvenli bir ortamda çalışması için Sanal Makine üzerinde **Host-Only Adapter** ağı tercih edilmiştir. Cihazın ağ üzerindeki adresi statik olarak `192.168.56.101` olarak belirlenmiştir.
* **🌍 Web Sunucusu (Apache2):**
    Ana alan adı `miracbayoglul.com` için özel bir VirtualHost dosyası oluşturularak, gelen HTTP istekleri doğrudan `/var/www/miracbayoglul.com` dizinine yönlendirilmiştir.
* **🔍 DNS Yönetimi (Bind9):**
    İstemcilerin IP adresi yerine alan adı ile bağlanabilmesi için Bind9 DNS servisi kurulmuştur. Zone dosyasına **SOA, NS ve A** kayıtları işlenerek trafik doğru sunucuya yönlendirilmiştir.
* **🗂️ Subdomain (Alt Alan Adı) Yapılandırması:**
    Ana projeden tamamen bağımsız çalışan `yazilim.miracbayoglul.com` alt alan adı oluşturulmuştur. Bind9 üzerinde yeni A kayıtları girilmiş ve Apache VirtualHost üzerinden `/var/www/yazilim` dizinine bağlanmıştır.
* **📁 FTP Dosya Sunucusu (VSFTPD):**
    Web dizinlerine dışarıdan güvenli dosya aktarımı yapabilmek için VSFTPD yapılandırılmış, `write_enable=YES` parametresiyle yazma ve dosya yükleme izinleri aktif edilmiştir.

---

## 🛠️ Karşılaşılan Sorunlar ve Çözüm Yaklaşımları

Sistemin kurulum ve test aşamalarında karşılaşılan teknik engeller ve uygulanan çözümler aşağıdadır:

* **Dosya Erişim İhlali (HTTP 403 Forbidden):**
    * **Sorun:** FTP ile yüklenen dosyalara tarayıcıdan erişilememesi.
    * **Çözüm:** Terminal üzerinden `chown` ve `chmod 755` komutları kullanılarak dosya sahipliği Apache kullanıcısına devredildi ve gerekli okuma izinleri tanımlandı.
* **İstemci Tarafında DNS Çözümleme Hatası:**
    * **Sorun:** Windows istemcisinin kendi varsayılan DNS'ini kullanması sebebiyle oluşturulan yerel alan adına ulaşamaması.
    * **Çözüm:** Windows Ağ Bağdaştırıcısı (IPv4) ayarlarından birincil DNS sunucusu manuel olarak Debian sunucusunun IP'si (`192.168.56.101`) ile değiştirildi.
* **Subdomain Yönlendirme Çakışması:**
    * **Sorun:** Subdomain adresine girildiğinde ana sayfa içeriğinin yüklenmesi.
    * **Çözüm:** Apache VirtualHost yapılandırmasındaki `ServerName` parametresi eklendi ve `DocumentRoot` yolu yeni dizini işaret edecek şekilde ayrıştırıldı.

---

## 👨‍💻 Geliştirici
* **Muhammet Miraç Bayoğlu**

> *Bu depo, akademik bir proje çıktısı olarak sistem yönetimi yetkinliklerini sergilemek amacıyla oluşturulmuştur.*
