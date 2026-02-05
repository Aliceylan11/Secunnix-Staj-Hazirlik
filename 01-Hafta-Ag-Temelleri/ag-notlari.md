## 1. OSI Referans Modeli (Yedi Katmanlı Yapı)

OSI bir standarttır. Verinin bir bilgisayardan çıkıp diğerine gidene kadar uğradığı durakları temsil eder.

* **7. Uygulama (Application):** Kullanıcı arayüzü (HTTP, FTP, SSH).
* **6. Sunum (Presentation):** Veri formatı ve şifreleme (SSL/TLS, JPEG).
* **5. Oturum (Session):** Bağlantı yönetimi (SQL, RPC).
* **4. Taşıma (Transport):** Verinin uçtan uca iletimi (TCP, UDP).
* **3. Ağ (Network):** Yönlendirme ve IP adresleme (IP, ICMP).
* **2. Veri Bağı (Data Link):** MAC adresleme ve anahtarlama (Ethernet, Wi-Fi).
* **1. Fiziksel (Physical):** Kablolar, bitler ve sinyaller.

---

## 2. TCP/IP Modeli (Güncel Standart)

OSI teorik bir modelken, **TCP/IP** günümüzde internetin gerçekten üzerinde çalıştığı pratik modeldir. OSI'nin 7 katmanını 4 ana katmana indirger:

1. **Uygulama Katmanı:** OSI'deki ilk 3 katmanı kapsar (Uygulama, Sunum, Oturum).
2. **Taşıma Katmanı:** OSI ile aynıdır. Verinin bütünlüğünü sağlar.
3. **İnternet Katmanı:** OSI'deki Ağ katmanına denk gelir. Paketlerin hedefe ulaşmasından sorumludur.
4. **Ağ Erişim Katmanı:** OSI'deki Veri Bağı ve Fiziksel katmanları birleştirir.

---

## 3. Temel Protokoller: TCP ve UDP

Taşıma katmanında verinin nasıl gönderileceğini bu iki protokol belirler:

* **TCP (Transmission Control Protocol):** Güvenilirdir. Veri gitmezse tekrar gönderir. "Üçlü El Sıkışma" (Three-way Handshake) ile bağlantı kurar. (Örn: Web sayfaları, E-posta).
* **UDP (User Datagram Protocol):** Hızlıdır ama güvenli değildir. Onay beklemeden gönderir. (Örn: Canlı yayınlar, Online oyunlar, DNS).

### 🔎 TCP 3'lü El Sıkışma (Three-Way Handshake)
İki bilgisayar (Client ve Server) TCP ile konuşmadan önce şu 3 adımla anlaşır:
1.  **SYN (Senkronizasyon):** İstemci, sunucuya "Konuşabilir miyiz?" der.
2.  **SYN-ACK (Senkronizasyon + Onay):** Sunucu, "Tabii, seni duydum, konuşalım" der.
3.  **ACK (Onay):** İstemci, "Tamam, başlıyoruz" der.
*Siber Not:* Eğer birisi sürekli SYN gönderip ACK göndermezse, sunucuyu kilitler (SYN Flood Saldırısı).

### 🔗 ARP (Address Resolution Protocol)
"Bu IP adresi hangi fiziksel cihaza (MAC Adresine) ait?" sorusunun cevabını bulur.
* **Mantık:** Bilgisayar ağa bağırır: "192.168.1.5 kimde?"
* **Cevap:** İlgili cihaz cevap verir: "O benim, MAC adresim de AA:BB:CC..."
* *Siber Not:* Saldırganlar "O IP benim" diye yalan söylerse trafiği kendi üzerine çeker (ARP Spoofing).

### 🛠️ Temel Ağ Komutları
Bir siber güvenlikçinin alet çantasıdır:
* **PING (ICMP):** Karşı taraf "Canlı mı?" diye kontrol eder. (Örn: `ping google.com`)
* **TRACERT (Traceroute):** Paketin hedefe giderken hangi duraklardan (Router) geçtiğini gösterir.
* **IPCONFIG (Linux: ifconfig):** Kendi IP ve MAC adresini öğrenmeni sağlar.
* **NETSTAT -AN:** Bilgisayarında o an hangi portların açık olduğunu ve kiminle konuştuğunu listeler.
* **Örnek Gösterim** tracert google.com > tracert-cikti.txt 
---

## 4. Portlar (Dijital Kapılar)

IP adresi bir apartmanın (bilgisayarın) adresi gibidir, **Portlar** ise o apartmandaki daire numaralarıdır. Bir bilgisayarda binlerce port vardır (0-65535).

### En Çok Bilmen Gereken Portlar:

| Port No | Protokol | Kullanım Alanı |
| --- | --- | --- |
| **21** | FTP | Dosya Aktarımı |
| **22** | SSH | Güvenli Uzak Bağlantı (Siber güvenlikte çok önemli) |
| **23** | Telnet | Güvensiz Uzak Bağlantı |
| **25** | SMTP | E-posta Gönderme |
| **53** | DNS | Alan Adı Çözümleme |
| **80** | HTTP | Şifresiz Web Trafiği |
| **443** | HTTPS | Şifreli Güvenli Web Trafiği |
| **3306** | MySQL | Veritabanı Bağlantısı (Projelerinde kullanabilirsin) |

---
### 🌍 Pratik Analiz: Bir Verinin Yolculuğu (Tracert)

Bu hafta teoride öğrendiğim "Hop" (Sıçrama) kavramını somutlaştırmak için `tracert google.com` komutunu kullandım ve çıktısını `tracert-cikti.txt` dosyasına kaydettim.

**Yaptığım Analiz Sonucu:**
Kendi bilgisayarımdan Google'a gönderdiğim basit bir isteğin saniyeler içinde şu rotayı izlediğini kanıtladım:

1.  **Evden Çıkış (Hop 1):** Paketim önce kendi modemimden/ağımdan (`192.168.43.1`) çıktı.
2.  **ISP İç Ağı (Hop 3-7):** Türk Telekom'un yerel ağında (`10.x.x.x` blokları) dolaştı.
3.  **Şehirlerarası Yolculuk (Hop 8-10):** Paketimin fiziksel olarak **Erzurum** sunucularından (`25-erzurum...`) geçip, oradan **Ankara Ulus** santraline (`06-ulus...`) iletildiğini loglarda gördüm.
4.  **Yurt Dışı Çıkışı (Hop 12-16):** Verim Ankara'dan sonra Bulgaristan/Sofya hattına (`sof...`) yönlendi ve orada Google sunucularına ulaştı.

**Çıkarımım:** İnternet "bulutta" değil, yerin altındaki kablolarla şehirden şehre gezen fiziksel bir yapıdır.