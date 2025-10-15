# 🛣️ Windows 11 Launcher: 1 Yıllık Kapsamlı Yol Haritası (ROADMAP.md)

Bu yol haritası, bir Android Launcher uygulamasının sıfırdan, tamamen **Cloud-First Development** (Bulut Tabanlı Geliştirme) metodolojisiyle (Android Studio kullanılmadan) geliştirilmesi için tasarlanmış 7 ana faz ve 40'tan fazla detaylı görevden oluşur. Proje, titizlikle planlanmış test aşamalarına ve Türkçe temelli kodlama ve dökümantasyona odaklanmıştır.

**Geliştirme Süresi Tahmini:** 12 Ay
**Odak:** Hata Toleransı, Performans ve Windows 11 UI/UX Sadakati.

---

## 🟢 Faz 0: Temel Ortam Kurulumu ve Altyapı (1. Ay)
**Odak:** Geliştirme ortamının kurulması, CI/CD'nin (Sürekli Entegrasyon/Sürekli Dağıtım) aktif edilmesi.

| Görev ID | Görev Başlığı | Detaylar | Test/Doğrulama Aşaması |
| :--- | :--- | :--- | :--- |
| **0.1** | GitHub Repository Oluşturma | Repository'yi `windows11-launcher` adıyla, Public olarak oluştur. `README.md` ve `.gitignore` (Android şablonu) ekle. | İlk commit'in başarılı olup olmadığını kontrol et. |
| **0.2** | GitHub Codespaces Hazırlığı | `devcontainer.json` dosyası oluşturularak Codespaces ortamı Java 17 ve gerekli Kotlin/Gradle VS Code eklentileri ile yapılandır. | Codespaces'i başlat ve terminalde `./gradlew --version` komutunun başarılı çalışıp çalışmadığını kontrol et. |
| **0.3** | Lisans ve Telif Hakkı Uyumu | Proje lisansının (GPLv3) ana dosyalara eklendiği ve telif hakkı beyanlarının doğru olduğu doğrulanır. | `LICENSE` dosyasının projenin kök dizininde var olduğunu teyit et. |
| **0.4** | Branching Stratejisi Belirleme | `main` (stabil) ve `develop` (ana geliştirme) dalları oluşturulur. Her yeni özellik için `feature/gorev-adi` dalları kullanılacaktır. | İlk `develop` dalına geçiş başarılı mı? |

---

## 🔵 Faz 1: Temel İskelet ve Manifest/Gradle Optimizasyonu (2. Ay)
**Odak:** Android Studio şablonları kullanmadan Gradle Kotlin DSL ile projenin çalışır vaziyette iskeletini oluşturma ve CI/CD'yi bağlama.

| Görev ID | Görev Başlığı | Detaylar | Test/Doğrulama Aşaması |
| :--- | :--- | :--- | :--- |
| **1.1** | Gradle Project Setup (Kotlin DSL) | `settings.gradle.kts`, `build.gradle.kts` (root), `app/build.gradle.kts` dosyalarını manuel oluştur ve Gradle 8+ ile Kotlin 1.9+ ayarlarını yap. | Codespaces'te `assembleDebug` komutunu çalıştır ve başarılı olup olmadığını kontrol et. |
| **1.2** | Compose ve Coil Bağımlılıkları | `app/build.gradle.kts` dosyasına Jetpack Compose BOM, Material 3 ve Async Ikon yükleme için Coil kütüphanelerini ekle. | Bağımlılıkların senkronizasyonu hatasız gerçekleşiyor mu? |
| **1.3** | Temel Manifest ve Launcher Ayarı | `AndroidManifest.xml` oluşturularak `ACTION_MAIN`, `CATEGORY_HOME` ve `CATEGORY_DEFAULT` intent filtreleri eklenir. `QUERY_ALL_PACKAGES` izni eklenir. | Manuel test: ADB üzerinden APK yüklendikten sonra ana ekran tuşuna basıldığında Launcher seçeneği çıkıyor mu? |
| **1.4** | GitHub Actions Build Otomasyonu | Her push ve PR'da otomatik APK oluşturan ve Artifacts'e yükleyen `android-build.yml` workflow'u yazılır. | İlk commit sonrası Actions sekmesinin otomatik çalışması ve bir `app-debug.apk` üretmesi doğrulanır. |

---

## 🟣 Faz 2: Uygulama Veri Katmanı ve İş Mantığı (3. ve 4. Ay)
**Odak:** Uygulama listesini alma, yönetme ve arama fonksiyonelliğini (Core Logic) Jetpack Compose ve ViewModel ile bağlama.

| Görev ID | Görev Başlığı | Detaylar | Test/Doğrulama Aşaması |
| :--- | :--- | :--- | :--- |
| **2.1** | Veri Modeli Tanımlaması | `data/AppInfo.kt` veri sınıfı oluşturulur: `name`, `packageName`, `icon` (Drawable), `launchIntent` alanları eklenir. | Modelin veri tutma yeteneği ve Kotlin veri sınıfları kurallarına uygunluğu doğrulanır. |
| **2.2** | Uygulama Repository | `data/AppRepository.kt` sınıfı yazılır. `getInstalledApps()` metodu ile `PackageManager` kullanılarak cihazdaki tüm launcher uygulamalarının listesi alınır ve alfabetik olarak sıralanır. | Logcat Testi: Cihazda kurulu 100+ uygulama varsa, Logcat'e kaç uygulamanın başarıyla yüklendiği loglanır. (Görev 4.1 ile test edilir) |
| **2.3** | Uygulama Başlatma Metodu | `AppRepository.kt` içine `launchApp(packageName: String)` metodu eklenir. | Unit Test: Hangi paket adı ile çağrıldığında doğru Intent'in oluşturulduğu mock edilerek test edilir. |
| **2.4** | Launcher ViewModel (State Management) | `ui/LauncherViewModel.kt` oluşturulur. `AppRepository`'yi kullanarak uygulama listesini yükler. `StateFlow` ile `allApps` ve `filteredApps` state'leri yönetilir. | Unit Test: ViewModel'in Repository'den gelen veriyi başarıyla yüklediği doğrulanır. |
| **2.5** | Arama ve Filtreleme Mantığı | `updateSearchQuery(query: String)` metodu eklenerek, arama sorgusuna göre `filteredApps` listesi güncellenir (case-insensitive arama). | Unit Test: "chrome" araması yapıldığında sadece Chrome'u (varsa) içeren listenin döndüğünden emin olunur. |

---

## 🎨 Faz 3: Windows 11 UI/UX Tasarımı ve Bileşenler (5., 6. ve 7. Ay)
**Odak:** Windows 11 Başlat Menüsü görünümünü Jetpack Compose ile oluşturma, görsel sadakat ve akıcılık.

| Görev ID | Görev Başlığı | Detaylar | Test/Doğrulama Aşaması |
| :--- | :--- | :--- | :--- |
| **3.1** | Temel UI İskeleti (LauncherScreen) | `ui/LauncherScreen.kt` oluşturulur. `ViewModel`'den verileri çeker ve `SearchBar`, `AppGrid` bileşenlerini dikey olarak düzenler. Arka plan siyah/yarı saydam olarak ayarlanır. | Emülatör/Cihaz Testi: Basit bir siyah ekranda metin ve boş bileşen yerleri düzgün görünüyor mu? |
| **3.2** | SearchBar Bileşeni | Windows 11 stiline uygun, Material 3 `OutlinedTextField` kullanılarak arama çubuğu tasarlanır. Renkler (beyaz/yarı saydam) ve şekil ayarlanır. | Fonksiyonel Test: Arama çubuğuna yazılan metnin `ViewModel`'e doğru şekilde iletildiği ve uygulanan filtreyi anlık güncellediği doğrulanır. |
| **3.3** | AppGrid Bileşeni (LazyVerticalGrid) | Uygulama listesini göstermek için 4 sütunlu `LazyVerticalGrid` kullanılır. | Performans Testi: Cihazda 100+ uygulama varken bile kaydırmanın akıcı (60 FPS) olduğu profilleyici (profiler) ile kontrol edilir. |
| **3.4** | AppIcon Bileşeni ve Efektler | Her uygulama için ayrı bir `AppIcon` Composable'ı tasarlanır. Coil ile asenkron ikon yüklemesi yapılır. **"Glassmorphism"** etkisine yakın bir yarı saydam `Surface` kullanılır. | Görsel Test: İkonların yüklenmesi sırasında placeholder görünümü ve yüklenme sonrası ikonların netliği kontrol edilir. |
| **3.5** | Tıklama Animasyonu ve Geri Bildirim | İkonlara tıklandığında Windows 11'e benzer kısa bir ölçeklenme (scaling) animasyonu (`scale(if (isPressed) 0.9f else 1f)`) eklenir ve uygulama başlatılır. | Kullanıcı Deneyimi Testi: Tıklama animasyonunun gecikme (delay) süresi (150ms) test edilerek akıcı bir deneyim sağlanır. |

---

## 🛠️ Faz 4: Gelişmiş Özellikler ve Dinamiklik (8., 9. ve 10. Ay)
**Odak:** Kullanıcı deneyimini zenginleştiren, özelleştirilebilir ve dinamik özelliklerin entegrasyonu.

| Görev ID | Görev Başlığı | Detaylar | Test/Doğrulama Aşaması |
| :--- | :--- | :--- | :--- |
| **4.1** | Dinamik İkon Boyutu Yönetimi | `data/PreferencesManager.kt` oluşturularak `gridColumns` ve `iconSize` ayarları `SharedPreferences` ile yönetilir. | Ayarlar Testi: `PreferencesManager`'daki değer değiştirildiğinde grid boyutunun anlık olarak değiştiği doğrulanır. |
| **4.2** | Ayarlar Ekranı | Launcher'ın görünümünü özelleştirmek için basit bir Ayarlar ekranı (`SettingsScreen.kt`) tasarlanır. Grid sütun sayısı ve ikon boyutu ayarları buraya bağlanır. | Kullanıcı Girişi Testi: Ayarlar kaydedildiğinde verilerin kalıcı olarak depolandığı doğrulanır. |
| **4.3** | Uygulama Grubu/Kategori Mantığı | Uygulamaları (örneğin "Oyunlar", "Sosyal Medya") otomatik olarak gruplayan basit bir kategori mantığı eklenir. | Veri Tutarlılığı Testi: Gruplama mantığının farklı cihazlarda doğru çalışıp çalışmadığı kontrol edilir. |
| **4.4** | Sabitlenmiş Uygulamalar (Pinned Apps) | Başlat menüsünün üst kısmında yer alacak, kullanıcının belirlediği 8-10 uygulamanın gösterildiği bir "Sabitleme" özelliği eklenir ve bu liste kalıcı olarak saklanır. | Kullanılabilirlik Testi: Uygulama sabitleme ve kaldırma işlemlerinin kolay ve hızlı olduğu doğrulanır. |
| **4.5** | Hata İzleme (Crashlytics) Entegrasyonu | Uygulama çökme (crash) ve hata raporlaması için Firebase Crashlytics veya benzeri bir hizmet entegre edilir. | Canlı Hata Testi: Bilerek bir `RuntimeException` tetiklenerek hatanın sunucuya doğru şekilde raporlandığı kontrol edilir. |

---

## 🟠 Faz 5: Kapsamlı Kalite Güvencesi ve Beta Süreci (11. Ay)
**Odak:** Hata tespiti, performans profilleyici ile darboğazları giderme ve gerçek kullanıcı geri bildirimlerini toplama.

| Görev ID | Görev Başlığı | Detaylar | Test/Doğrulama Aşaması |
| :--- | :--- | :--- | :--- |
| **5.1** | Remote Debugging ve Logcat Kurulumu | Fiziksel cihazda Kablosuz ADB ile bağlantı kurularak Logcat takibi ve uzaktan hata ayıklama ortamı hazırlanır. | Bağlantı Testi: Cihaz ve bilgisayar arasında ADB bağlantısının sürekli ve stabil olduğu doğrulanır. |
| **5.2** | Performans Profilleme (Jank Tespiti) | Android Profiler veya `adb shell dumpsys gfxinfo` araçları kullanılarak UI akıcılığı (özellikle kaydırma ve animasyonlar) test edilir. FPS düşüşleri (Jank) tespit edilip giderilir. | Metrik Test: Uygulama başlatıcının, kaydırma sırasında sürekli 60 FPS'e yakın çalıştığı doğrulanır. |
| **5.3** | Beta Testi ve Geri Bildirim Yönetimi | GitHub Issues'da bug raporlama şablonu oluşturulur (Device, Android Version, Steps to Reproduce). 10-20 kişilik kapalı bir beta grubu oluşturulur. | Geri Bildirim Toplama Testi: Toplanan ilk 5 bug'ın yol haritasına dahil edilip çözüldüğü ve `main` branch'e merge edildiği doğrulanır. |
| **5.4** | Pil ve Bellek Optimizasyonu | Uygulamanın arka plan bellek kullanımı ve pil tüketimi incelenir. Gereksiz kaynak tutulmasının önüne geçilir. | Metrik Test: Launcher'ın, uzun süre kullanımdan sonra bile Android'in "uyku" modunda düşük pil tüketimi yaptığı profilleyici ile kontrol edilir. |

---

## 💖 Faz 6: Son Dokunuşlar ve Resmi Yayın (12. Ay)
**Odak:** Yayınlama (Release) için gerekli son düzenlemeler, güvenlik, dökümantasyon ve pazarlama.

| Görev ID | Görev Başlığı | Detaylar | Test/Doğrulama Aşaması |
| :--- | :--- | :--- | :--- |
| **6.1** | Release Build Yapılandırması | Keystore oluşturulur ve `app/build.gradle.kts` dosyasına Release için imzalama ayarları eklenir. `signingConfigs` ve `buildTypes` tanımlanır. `isMinifyEnabled = true` yapılır. | Gizli değişkenlerin (Keystore şifresi) GitHub Secrets'a doğru şekilde eklenip eklenmediği kontrol edilir. |
| **6.2** | Resmi Sürüm Etiketleme ve Yayınlama | Tüm Faz 5 hataları giderildikten sonra `git tag -a v1.0.0` ile versiyon etiketi oluşturulur ve push edilir. | GitHub Actions'ın otomatik olarak **Signed Release APK**'sını yayınladığı ve Release sayfasının doğru oluşturulduğu doğrulanır. |
| **6.3** | Dökümantasyon ve README.md Güncellemesi | Son özellikler, ekran görüntüleri ve indirme bağlantılarıyla `README.md` son kez güncellenir. | Dökümantasyonun eksiksiz, Türkçe ve güncel olduğu kontrol edilir. |
| **6.4** | Sürekli Bakım Planı (Post-Release) | V1.0.0 sonrası gelen ilk 3 kritik hata için bir yama (`v1.0.1`) planı oluşturulur. | Proje aktif olarak izlenir ve ilk 1 aylık stabilite hedefi konulur. |
