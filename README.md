# 🧠 Otizm Erken Teşhis Farkındalık Projesi – Frontend

Bu proje, **otizm spektrum bozukluğu (OSB)** farkındalığını artırmak ve **erken teşhis sürecini desteklemek** amacıyla geliştirilmiş web tabanlı bir sistemin **ön yüz (frontend)** kısmını içerir.  
Kullanıcılar interaktif bir oyun oynarken kamera kaydı alınır; bu video backend tarafından analiz edilerek otizm spektrumuna dair olası göstergeler değerlendirilir.

---

## 📁 Proje Yapısı

frontend/

│

├── home.html # Açılış ve KVKK onay ekranı

├── index.html # Kullanıcı giriş sayfası (isim/soyisim)

├── record.html # Oyun alanı ve video kayıt sayfası

└── admin_login.html # Yetkili kullanıcı giriş ekranı


---

## 🏠 `home.html`
Bu sayfa, kullanıcı sisteme giriş yapmadan önce **KVKK (Kişisel Verilerin Korunması Kanunu)** onayını almak için kullanılır.

**Öne çıkan özellikler:**
- Arka planda sinir ağı animasyonu.  
- Kullanıcı onay vermezse sisteme giriş engellenir.  
- Onay bilgisi `localStorage` üzerinde tutulur.  
- Onay sonrası otomatik olarak `index.html` sayfasına yönlendirme yapılır.

---

## 👤 `index.html`
Kullanıcıdan yalnızca **isim** ve **soyisim** bilgisi alınır. Bu bilgiler oyun sürecinde kullanıcıyı kişisel olarak hitap etmek için kullanılır.

**Özellikler:**
- Modern blur + gradient arka plan.  
- Form üzerinden `/record` sayfasına yönlendirme yapılır.  
- Sağ alt köşedeki **“Yetkili Girişi”** butonu `/admin_login.html` sayfasına yönlendirir.

---

## 🎮 `record.html`
Projenin en etkileşimli kısmıdır.  
Kullanıcı kameradan görüntü alırken küçük testler ve oyunlar oynar. Sistem bu sırada kullanıcının yüz, göz ve tepki hareketlerini gözlemler.

**İşlevler:**
- Tarayıcı kamerasına erişim (`navigator.mediaDevices.getUserMedia`).  
- **Video kaydı:** `MediaRecorder API` ile alınır.  
- **Göz takibi:** `MediaPipe FaceMesh` modeli ile yapılır.  
- **Sesli yönlendirme:** `SpeechSynthesis API` ile kullanıcıya rehberlik edilir.  
- **Oyun aşamaları:**
  1. Ekranda beliren nesnelere gözle bakma.  
  2. Kullanıcının ismiyle çağırılıp **el sallama animasyonu** gösterilmesi.  
  3. Mutlu yüz videosu ile tepki testi.  
- Kayıt tamamlandığında video otomatik olarak backend’e gönderilir.  

**Kullanılan medya dosyaları:**
static/el-sallama.gif # El sallama animasyonu
static/mutlu_video.mp4 # Tepki videosu
static/pop.mp3 # Etkileşim sesi
static/sound.mp3 # Sesli uyarı


---

## 🔐 `admin_login.html`
Yetkili kullanıcıların sisteme giriş yaptığı sayfadır.  
Admin kullanıcılar bu sayfadan backend paneline erişir.

**Özellikler:**
- Giriş formu (kullanıcı adı + şifre).  
- Backend ile `/admin_login` endpoint üzerinden iletişim.  
- Başarılı girişte admin paneline yönlendirme.

---

## 📊 `admin.html`

Sistemde toplanan analiz sonuçlarını görüntülemek için kullanılan **admin paneli** sayfasıdır.  
Her kullanıcıya ait video, modelin hesapladığı olasılıklar ve segment bazlı hareket tespitleriyle birlikte tablo halinde sunulur.
Video uzantısına tıkladığı durumda o videoya ait alt segmentlerin sonuçlarını da inceleyebilir.

**Görsel önizleme:**  
![Admin Panel Tablosu](static/admin_main_table.jpg)

---

### 🔹 Sayfa Özellikleri

- 🔍 **Arama ve filtreleme:** İsim, soyisim, final tahmin alanlarına göre dinamik filtreleme  
- 📊 **Segment detayları:** Tablo altına açılıp kapanabilen alt tablo görünümü  
- 🎨 **Renk kodlu tahmin göstergeleri:**  
  - 🟥 **Otizm olabilir** → kırmızı arka plan  
  - 🟩 **Otizm değil** → yeşil arka plan  

---

## 🔗 Backend Entegrasyonu

Frontend, aşağıdaki endpoint’lerle backend ile iletişim kurar:

| Endpoint | Metot | Açıklama |
|-----------|--------|-----------|
| `/upload` | `POST` | Kullanıcının video kaydını backend'e gönderir. |
| `/finish` | `POST` | Oyun tamamlandığında oturum verilerini iletir. |
| `/admin_login` | `POST` | Yetkili kullanıcı kimlik doğrulama. |
| `/results` | `GET` | (Opsiyonel) Admin panelinden sonuç görüntüleme. |

---

## 🧩 Kullanılan Teknolojiler

| Teknoloji | Amaç |
|------------|-------|
| **HTML5** | Sayfa yapısı |
| **CSS3** | Arayüz tasarımı (gradient, blur, animasyonlar) |
| **JavaScript (ES6)** | Etkileşim ve oyun kontrolü |
| **MediaRecorder API** | Kamera ve video kaydı |
| **MediaPipe FaceMesh** | Yüz/göz takibi |
| **SpeechSynthesis API** | Sesli yönlendirme |
| **Fetch API** | Backend bağlantısı |

---


