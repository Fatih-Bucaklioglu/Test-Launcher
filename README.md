# 🪟 Windows 11 Launcher for Android (Bulut Tabanlı Geliştirme)

Windows 11 tasarımından ilham alan, modern ve minimalist Android başlatıcısı.

Bu proje, geleneksel Android Studio kullanımı olmadan, tamamen **GitHub, GitHub Actions ve Codespaces** gibi bulut tabanlı araçlarla geliştirilmektedir. Bu yaklaşım, **Cloud-First Development** olarak adlandırılmaktadır.

## 📁 Proje Klasör Yapısı (İskelet)

Projenin temel yapısı, Android Gradle Kotlin DSL standartlarına ve temiz mimari pratiklerine uygun olarak düzenlenmiştir:

windows11-launcher/
├── .github/
│   └── workflows/      # GitHub Actions CI/CD dosyaları
│       └── android-build.yml
├── .gitignore          # Gradle ve Android Studio için özel ayarlar
├── README.md           # Proje tanıtımı
├── ROADMAP.md          # 1 Yıllık Kapsamlı Yol Haritası
├── app/
│   ├── src/
│   │   └── main/
│   │       ├── java/
│   │       │   └── com/yourname/launcher/
│   │       │       ├── data/         # Veri (Repository, Model, Preferences)
│   │       │       │   ├── AppInfo.kt
│   │       │       │   └── AppRepository.kt
│   │       │       └── ui/           # Kullanıcı Arayüzü (ViewModel, Composable'lar)
│   │       │           ├── LauncherViewModel.kt
│   │       │           └── ui/components/ # UI bileşenleri
│   │       └── res/      # Kaynaklar (ikon, tema, stil)
│   │       └── AndroidManifest.xml # Uygulama Manifest dosyası
│   └── build.gradle.kts  # Uygulama modülü Gradle dosyası (Compose bağımlılıkları dahil)
├── build.gradle.kts    # Kök dizin Gradle dosyası
├── settings.gradle.kts # Proje ayarları (Modül dahil etme)
└── gradlew


## ✨ Özellikler
* **Tasarım:** Windows 11 stili kullanıcı arayüzü (UI)
* **Arama:** Hızlı uygulama arama fonksiyonu
* **Tema:** Koyu tema desteği
* **Performans:** Hafif ve akıcı yapı
* **Geliştirme Metodu:** GitHub Codespaces üzerinden tarayıcı tabanlı tam IDE deneyimi, GitHub Actions ile sürekli entegrasyon ve otomatik APK build.

## 🛣️ 1 Yıllık Kapsamlı Yol Haritası Özeti
Proje, titizlikle hazırlanmış 7 ana fazdan oluşan ve her aşamada kapsamlı testleri içeren 1 yıllık bir plan dahilinde ilerleyecektir. Detaylar için **ROADMAP.md** dosyasına bakınız.

* **Faz 0:** Temel Ortam Kurulumu ve Altyapı (1 Ay)
* **Faz 1:** Temel İskelet ve Manifest/Gradle Optimizasyonu (1 Ay)
* **Faz 2:** Uygulama Veri Katmanı ve İş Mantığı (2 Ay)
* **Faz 3:** Windows 11 UI/UX Tasarımı ve Bileşenler (3 Ay)
* **Faz 4:** Gelişmiş Özellikler ve Dinamiklik (3 Ay)
* **Faz 5:** Kapsamlı Kalite Güvencesi ve Beta Süreci (1 Ay)
* **Faz 6:** Son Dokunuşlar ve Resmi Yayın (1 Ay)

## 📦 İndirme
[En Son APK'yı İndir](https://github.com/username/windows11-launcher/releases/latest) (GitHub Actions tarafından otomatik olarak oluşturulur ve yayınlanır).

## 🛠️ Kullanılan Teknolojiler
* **Dil:** Kotlin
* **UI Çerçevesi:** Jetpack Compose (Material 3)
* **Görüntü Yükleme:** Coil (Asenkron ikon yükleme için)
* **Build Sistemi:** Gradle Kotlin DSL

## 🤝 Katkıda Bulunma
Katkı talepleri (Pull Requests) memnuniyetle kabul edilir. Lütfen her yeni özellik için ayrı bir dal (branch) kullanın ve ana dalın (`main`) stabil kalmasını sağlayın.

## 📄 Lisans
Bu proje, GNU General Public License v3 (GPLv3) lisansı altındadır. Detaylar için [LICENSE](LICENSE)
