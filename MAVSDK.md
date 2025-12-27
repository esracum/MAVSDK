# 🛸 MAVSDK-C++ ve PX4 Gazebo Simülasyon Rehberi

Bu rehber, **Hezarfen İHA Takımı** yazılım geliştirme süreçleri için Ubuntu (22.04/24.04) üzerinde MAVSDK-C++, PX4 Autopilot ve Gazebo Simülatörü kurulumunu ve kullanımını kapsayan uçtan uca bir kılavuzdur.

---

## 🛠 1. Sistem Bağımlılıkları
Sistemdeki temel derleme araçlarını ve kütüphaneleri kurmak için terminalde aşağıdaki komutları çalıştırın:

```bash
# Sistem paket listesini günceller
sudo apt-get update
# Derleme araçlarını, CMake ve Git'i yükler
sudo apt-get install build-essential cmake git -y
```

---

## ✈️ 2. PX4 Autopilot Kurulumu (Drone Beyni)
Drone'un uçuş algoritmalarını ve Gazebo simülatör altyapısını kurun. Bu işlem alt modüller nedeniyle zaman alabilir:

```bash
# Ana dizine gider
cd ~
# Kaynak kodlarını alt modülleriyle birlikte klonlar
git clone https://github.com/PX4/PX4-Autopilot.git --recursive

# PX4 bağımlılıklarını ve Gazebo'yu kuran scripti çalıştırır
cd ~/PX4-Autopilot
bash ./Tools/setup/ubuntu.sh
```

> **⚠️ Önemli:** Kurulum bittikten sonra bilgisayarı yeniden başlatmanız önerilir.
> **Not:** Eğer derleme hatası (jsonschema) alırsanız şu komutu kullanın: 
```bash
# Eksik Python paketini yükler
pip3 install --user jsonschema
```

---

## 📦 3. MAVSDK-C++ Kütüphane Kurulumu
C++ üzerinden drone'a komut gönderebilmek için gerekli SDK paketini yükleyin:

```bash
# İndirilen .deb paketini yükler
sudo dpkg -i libmavsdk-dev_3.0.0_ubuntu24.04_amd64.deb

# Bağımlılık hatalarını onarır
sudo apt-get install -f -y
```

---

## 💻 4. Örnek Projenin Derlenmesi
MAVSDK örnek kodlarını kullanarak otonom uçuş projesini derleyin:

```bash
# Örnekleri klonlar
git clone https://github.com/mavlink/MAVSDK-C-Plus-Plus-Examples.git
# İlgili dizine girer
cd MAVSDK-C-Plus-Plus-Examples/takeoff_and_land

# Build klasörü oluşturur ve derleme işlemini başlatır
mkdir build && cd build
cmake ..
make
```

---

## 🚀 5. Simülasyonun Başlatılması (SITL)

Simülasyonun çalışması için iki ayrı terminal kullanılması gerekmektedir.

### Adım 1: Simülatörü Başlatın (Terminal 1)
```bash
# PX4 dizinine gider
cd ~/PX4-Autopilot
# Yeni nesil Gazebo ve x500 modeli ile simülasyonu başlatır
make px4_sitl gz_x500
```

### Adım 2: Otonom Görevi Çalıştırın (Terminal 2)
```bash
# Derlenen dosyanın dizinine gider
cd ~/MAVSDK-C-Plus-Plus-Examples/takeoff_and_land/build
# UDP portu üzerinden drone'a bağlanır ve otonom kodu çalıştırır
./takeoff_and_land udp://:14540
```

---

## 🏗 Sistem Mimarisi
| Bileşen | Görev |
| :--- | :--- |
| **PX4 SITL** | Drone'un beyni; tüm uçuş algoritmalarını simüle eder. |
| **Gazebo Sim** | Fizik motoru; drone gövdesini ve çevreyi 3D modeller. |
| **MAVSDK-C++** | Otonom görevleri MAVLink üzerinden drone'a iletir. |

---
**Hazırlayan:** Esra Cüm  
**Kurum:** Hezarfen İHA Takımı
