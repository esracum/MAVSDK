# QML TEMEL YAPI TAŞLARI VE MİMARİ REHBERİ

Bu döküman; QML (Qt Modeling Language) dilinin çalışma mantığını, hiyerarşisini ve modern kullanıcı arayüzü geliştirme süreçlerindeki rolünü açıklar.

---

## 1. Örnek Kod Yapısı
Aşağıdaki kod; bir ana pencere, içinde bir dikdörtgen ve bir olay yakalayıcıyı içerir.

import QtQuick
import QtQuick.Window

Window {
    id: root              // [ID]: Nesneye verilen benzersiz isim.
    width: 400            // [Property]: Nesnenin özelliği.
    height: 400
    visible: true
    title: "QML Temel Mimari"

    Rectangle {
        // [Binding]: root.width değiştikçe bu değer otomatik güncellenir.
        width: root.width / 2 
        height: root.height / 2
        anchors.centerIn: parent
        color: "#2c3e50"
        radius: 10
    }

    // [Signal Handler]: Genişlik her değiştiğinde bu blok çalışır.
    onWidthChanged: {
        console.log("Pencere genişliği güncellendi: " + width)
    }
}

---

## 2. Kavramsal Açıklamalar

### A. Nesne Ağacı (Tree of Elements)
QML nesneleri hiyerarşik bir ağaç yapısında tanımlanır. Üstteki nesne (Parent), içindeki nesneleri (Children) yönetir. Görsel elemanlar bu ağaç yapısına göre ekrana çizilir. Üst nesne taşındığında veya gizlendiğinde tüm alt nesneler bu durumdan etkilenir.



### B. id Özniteliği (Identifier)
Nesneye verilen benzersiz bir etikettir. Tanımlandığı dosya içinde tek olmalıdır. Diğer nesnelerin özelliklerine bu isim üzerinden erişilir (Örn: root.width). Küçük harf veya alt çizgi ile başlamalıdır.

### C. Özellikler (Properties)
Nesnenin durumunu (boyut, renk, konum vb.) tutan değişkenlerdir. Örn: width, height, color. Kullanıcı kendi özel özelliklerini de "property int sayac: 0" şeklinde tanımlayabilir.

### D. Özellik Bağlama (Property Bindings)
Bir özelliğin değerini statik bir sayı yerine, dinamik bir ifadeye bağlamaktır. "width: root.width / 2" yazıldığında, ana pencere büyütüldüğü an dikdörtgen de otomatik olarak büyür.



### E. Sinyal ve Yakalayıcılar (Signal and Handler)
Uygulama içindeki olaylara (tıklama, veri değişimi vb.) verilen tepkilerdir. Sinyal bir olay bildirisidir, Handler ise bu bildiri gelince çalışan kod bloğudur (Örn: onWidthChanged).

---

## 3. Özet Tablo

| Kavram      | Açıklama            | Benzetme                     |
|-------------|---------------------|------------------------------|
| Object      | Temel yapı taşı      | Bir makine                   |
| Property    | Nesnenin durumu     | Makinenin çalışma hızı        |
| Binding     | Dinamik ilişki      | Hız göstergesinin motora bağı |
| Signal      | Olay bildirimi      | "Yakıt Bitti" alarmı         |

---
Hazırlayan: Esra Cüm - QML Eğitim Notları
