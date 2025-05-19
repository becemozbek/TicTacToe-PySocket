#Tic Tac Toe – Neon Edition

Marmara Üniversitesi Teknoloji Fakültesi “Bilgisayar Ağlarına Giriş” projesi, Büşra Ecem ÖZBEK ve Ahmet Eren BAŞALİ tarafından gerçekleştirilmiştir.



🚀 Proje Özeti

Tic Tac Toe – Neon Edition; Python, CustomTkinter ve TCP/IP soketleri ile geliştirilen, iki oyunculu gerçek zamanlı bir Tic Tac Toe oyunudur. Modern bir neon tema, animasyonlu kazanan efekti, chat ve rematch (yeniden oyun) özellikleri ile zengin bir kullanıcı deneyimi sunar.

🔧 Özellikler

Gerçek Zamanlı Oyun: TCP socket tabanlı, sunucu-istemci mimarisi.

Chat: Oyuncular arası yazılı iletişim.

Rematch: Oyun bittiğinde yeniden başlatma isteği ve kabul/red­detme akışı.

Animasyon: Kazanan hattın 6 kez yanıp sönme efekti.

Neon Tema: Neon pembe (#FF00FF) ve neon yeşil (#39FF14) renkler.

Platform: Windows, Linux, MacOS (Python 3.12).

🏗️ Proje Yapısı

├── server.py       # Sunucu uygulaması
├── client.py       # İstemci (GUI) uygulaması
├── oyun.py         # Oyun mantığı (Game sınıfı)
├── README.md       # Bu dosya
└── screenshots/    # Ekran görüntüleri
    ├── oyun_ekran.png
    ├── animasyon.png
    └── github.png

⚙️ Kurulum & Çalıştırma

Gereksinimler

Python 3.12 veya üzeri

customtkinter kütüphanesi

pip install customtkinter

Sunucu Başlatma

Terminal açın.

server.py dosyasının bulunduğu klasöre gidin.

Aşağıdaki komutu çalıştırın:

python server.py

Sunucu PORT 12345’te dinlemeye başlar.

İstemci Başlatma

İstemci makinede (veya aynı makinede) terminal açın.

client.py dosyasının bulunduğu klasöre gidin.

Aşağıdaki komutu çalıştırın:

python client.py

Sunucu IP adresini ve portu (ör. 192.168.1.10:12345) girin.

GUI açılır ve oyun başlar.

💻 Çalışma Ortamı

Geliştirme: Oracle VM VirtualBox üzerinde Debian 11 VM (Bridged Adapter ile yerel ağa dahil).

IDE: PyCharm Professional.

📡 Protokol Detayları

ROLE;X / ROLE;O: Oyuncu işareti atanması.

BOARD;...: Tahta durumu (3 satır, ; ayracı).

PROMPT;Your turn / Waiting: Oyun akışı kontrolü.

CHAT;X;Merhaba: Chat mesajı, X veya O işaretiyle.

RESULT;X wins / Draw: Oyun sonucu.

REMATCH;REQUEST / ACCEPT / DECLINE / START: Yeniden oyun akışı.

🎮 Kullanım

İki client başlatın ve sunucuya bağlanın.

Tahtada X ve O ile hamlelerinizi yapın.

Kazanmanın Keyfini Çıkarın. 


