# Tic Tac Toe – Neon Edition

Marmara Üniversitesi Teknoloji Fakültesi “Bilgisayar Ağlarına Giriş” projesi, Büşra Ecem ÖZBEK ve Ahmet Eren BAŞALİ tarafından gerçekleştirilmiştir.

---

**İçindekiler**

1. Projenin Amacı
2. Projede Kullanılan Teknolojiler
3. Projede Neler Yapıldı?
4. Ağ Mimarisi ve Altyapı Kurulumu
5. Sistemin Çalışma Mantığı
6. Sonuç ve Değerlendirme
7. Proje Arayüzü
8. Canlı Demo ve Kaynak Kod

---

### 1. Projenin Amacı

Bu proje, TCP/IP soketleri kullanarak ağ üzerinden iki oyuncunun gerçek zamanlı olarak "Tic Tac Toe – Neon Edition" oynamasını sağlar. Amaç, bilgisayar ağları dersinde istemci-sunucu mimarisi, bağlantı yönetimi, veri iletişimi, protokol tasarımı ve eşzamanlı işlemleri uygulamalı olarak öğretmek ve pekiştirmektir.

---

### 2. Projede Kullanılan Teknolojiler

* **Python 3.12**: Uygulama geliştirme dili
* **socket** modülü: TCP/IP bağlantı ve veri iletimi
* **threading** modülü: Çoklu iş parçacığı ile eşzamanlı ağ dinleme
* **queue.Queue**: Thread-safe mesaj kuyruğu
* **customtkinter**: Modern GUI tasarımı
* **Oracle VM VirtualBox**: Debian tabanlı VM, Bridged Adapter ile ana sisteme entegre ağ
* **PyCharm Professional**: IDE

---

### 3. Projede Neler Yapıldı?

1. **server.py** yazıldı:

   * Dinleme (`listen`, `accept`), bağlantı yönetimi
   * Oyun döngüsü: `broadcast_board`, `handle_game`, `handle_rematch`
   * Protokol mesaj işleme: ROLE, BOARD, PROMPT, CHAT, RESULT, REMATCH
2. **client.py** geliştirildi:

   * GUI: 3×3 buton grid, durum etiketi, chat kutusu, rematch paneli
   * Dinleme thread, `queue` ile ana thread’e mesaj aktarımı
   * Olay yakalama: buton tıklama, chat, rematch
3. **oyun.py**:

   * `Game` sınıfı ile hamle doğrulama, kazanan kontrolü, berabere kontrolü

---

### 4. Ağ Mimarisi ve Altyapı Kurulumu

* **Sunucu**: Tek bir Python süreci, 12345 numaralı portta dinler. Gelen iki bağlantıyı eşleştirip her bir oyun için yeni thread oluşturur.
* **İstemci**: VM ve ana makinede çalıştırılabilir. Bridged Adapter ile VM ana ağda görünür ve iki ayrı istemci birbirini görebilir.
* **Protokol**: Metin tabanlı, satır sonu `\n`, `;` ayracı.

| Katman          | Bileşen                        |
| --------------- | ------------------------------ |
| Uygulama        | Tic Tac Toe protokolü          |
| Sunum           | UTF-8 kodlaması                |
| Oturum          | TCP bağlaması (connect/accept) |
| Taşıma          | TCP (SOCK\_STREAM)             |
| Ağ              | IP yönlendirme (OS katmanı)    |
| Veri Bağlantısı | Ethernet/Wi-Fi (VM Bridge)     |

---

### 5. Sistemin Çalışma Mantığı

1. **Bağlantı**: İstemci `connect()` ile sunucuya bağlanır. Sunucu `accept()` ile alır ve `ROLE;X` veya `ROLE;O` mesajını gönderir.
2. **Oyun Döngüsü**: Sırası gelen istemci `PROMPT;Your turn`, diğeri `PROMPT;Waiting` alır. Hamleler `r,c\n` formatında gönderilir.
3. **Tahta Güncelleme**: Sunucu `broadcast_board` ile güncel tahtayı `BOARD;...` mesajıyla yayınlar.
4. **Sonuç**: `RESULT;X wins` veya `RESULT;Draw` mesajı gönderilir ve istemci animasyonu tetikler.
5. **Rematch**: `REMATCH;REQUEST`, `ACC/DECLINE`, `START` akışı ile yeni oyun başlar.

---

### 6. Sonuç ve Değerlendirme

* **Başarı**: Gerçek zamanlı oyun, chat ve rematch özellikleri beklendiği gibi çalıştı.
* **Öğrenim**: TCP/IP, threading, GUI entegrasyonu, protokol tasarımı konularında uygulamalı deneyim sağlandı.


---

### 8. Canlı Demo ve Kaynak Kod

* **Canlı Demo**: [http://tic-tac-toe-neon.metricopt.com/](http://tic-tac-toe-neon.metricopt.com/)
* **GitHub**: [https://github.com/becemozbek/TicTacToe-PySocket]

*Proje VM’de Oracle VirtualBox aracılığıyla sunulmuştur. Bridged Adapter kullanılarak ana ağa dahil edilmiş ve iki VM arasında test edilmiştir.*


Tahtada X ve O ile hamlelerinizi yapın.

Kazanmanın Keyfini Çıkarın. 


```mermaid
sequenceDiagram
    participant Client1 as İstemci A
    participant Server  as Sunucu
    participant Client2 as İstemci B

    Client1->>Server: CONNECT
    Server-->>Client1: ROLE;X

    Client2->>Server: CONNECT
    Server-->>Client2: ROLE;O

    loop Oyun Döngüsü
        Server-->>Client1: BOARD;XXX;O O;
        Server-->>Client2: BOARD;XXX;O O;
        alt Client1 Sırası
            Server-->>Client1: PROMPT;Your turn
            Client1->>Server: 1,2
        else Client2 Sırası
            Server-->>Client2: PROMPT;Your turn
            Client2->>Server: 0,0
        end
    end

    Server-->>Client1: RESULT;X wins
    Server-->>Client2: RESULT;X wins

    Client1->>Server: REMATCH;REQUEST
    Server-->>Client2: REMATCH;REQUEST
    Client2->>Server: REMATCH;ACCEPT
    Server-->>Client1: REMATCH;START
```

