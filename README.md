# 🛸 MAVSDK-C++ ve PX4 Gazebo Simülasyon Rehberi

Bu rehber, **Çelebi İHA Takımı** yazılım geliştirme süreçleri için Ubuntu (22.04/24.04) üzerinde MAVSDK-C++, PX4 Autopilot ve Gazebo Simülatörü kurulumunu ve kullanımını kapsayan uçtan uca bir kılavuzdur.

---

## 🛠 1. Sistem Bağımlılıkları
Sistemdeki temel derleme araçlarını ve kütüphaneleri kurmak için terminalde aşağıdaki komutları çalıştırın:

sudo apt-get update
sudo apt-get install build-essential cmake git -y

---

## ✈️ 2. PX4 Autopilot Kurulumu (Drone Beyni)
Drone'un uçuş algoritmalarını ve Gazebo simülatör altyapısını kurun. Bu işlem alt modüller nedeniyle internet hızınıza bağlı olarak zaman alabilir:

# Ana dizine gidin ve kaynak kodu submodülleriyle birlikte klonlayın
cd ~
git clone [https://github.com/PX4/PX4-Autopilot.git](https://github.com/PX4/PX4-Autopilot.git) --recursive

# Bağımlılıkları kuran kurulum scriptini çalıştırın
cd ~/PX4-Autopilot
bash ./Tools/setup/ubuntu.sh

> **⚠️ Önemli:** Kurulum bittikten sonra bilgisayarı yeniden başlatmanız (reboot) önerilir.
> **Not:** Eğer derleme hatası (jsonschema) alırsanız: pip3 install --user jsonschema komutunu kullanın.

---

## 📦 3. MAVSDK-C++ Kütüphane Kurulumu
C++ üzerinden drone'a komut gönderebilmek için gerekli SDK paketini yükleyin:

# İndirdiğiniz .deb paketini kurun (Versiyonun uygunluğunu kontrol edin)
sudo dpkg -i libmavsdk-dev_3.0.0_ubuntu24.04_amd64.deb

# Eksik bağımlılıkları onarın
sudo apt-get install -f -y

---

## 💻 4. Örnek Projenin Derlenmesi
MAVSDK örnek kodlarını kullanarak otonom uçuş projesini derleyin:

# Örnekleri klonlayın
git clone [https://github.com/mavlink/MAVSDK-C-Plus-Plus-Examples.git](https://github.com/mavlink/MAVSDK-C-Plus-Plus-Examples.git)
cd MAVSDK-C-Plus-Plus-Examples/takeoff_and_land

# Build klasörü oluşturun ve CMake ile derleyin
mkdir build && cd build
cmake ..
make

---

## 🚀 5. Simülasyonun Başlatılması (SITL)



Simülasyonun çalışması için iki ayrı terminal kullanılması gerekmektedir.

### Adım 1: Simülatörü Başlatın (Terminal 1)
cd ~/PX4-Autopilot
# Yeni nesil Gazebo (Ubuntu 22.04+ / Gazebo Sim v8) için:
make px4_sitl gz_x500

### Adım 2: Otonom Görevi Çalıştırın (Terminal 2)
cd ~/MAVSDK-C-Plus-Plus-Examples/takeoff_and_land/build
./takeoff_and_land udp://:14540

---

## 🏗 Sistem Mimarisi
- **PX4 SITL:** Drone'un beyni; tüm uçuş algoritmalarını simüle eder.
- **Gazebo Sim:** Fizik motoru; drone gövdesini ve çevreyi 3D modeller.
- **MAVSDK-C++:** Geliştirilen otonom görevleri MAVLink protokolü ile drone'a iletir.

---
**Hazırlayan:** Esra Cüm  
