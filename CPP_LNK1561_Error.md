# C++/CLI Windows Forms – Common Errors & Fixes

Bu rehber, C++/CLI Windows Forms projelerinde sık karşılaşılan hataları ve çözüm yollarını özetler.

---

## 1️⃣ Hata: LNK1561 – "entry point must be defined"

**Durum:**  
Visual Studio projeni derlerken aşağıdaki hatayı alıyorsun:


**Olası Nedenler:**  
- Projede `main` veya `WinMain` fonksiyonu yok.  
- Proje tipi (Console / Windows) ile main fonksiyonun uyumsuz.  
- Yanlış SubSystem ayarı.

**Çözüm Yolu:**  
1. Windows Forms için tipik `main` fonksiyonunu ekle:

```cpp
[STAThread]
int main(array<String^>^ args)
{
    Application::EnableVisualStyles();
    Application::SetCompatibleTextRenderingDefault(false);
    Project2::MyForm frm;
    Application::Run(%frm);
    return 0;
}
```
2. Proje ayarlarını kontrol et:

Sağ tık proje → Properties → Linker → System → SubSystem

Ayarı: Windows (/SUBSYSTEM:WINDOWS) olarak değiştir.

Rebuild Solution yapmadan önce Clean Solution yapın.

## 2️⃣ Hata: Hem konsol hem form açılıyor

**Durum:**
Windows Forms uygulaması başlatıldığında, gereksiz bir konsol penceresi de açılıyor.

**Olası Nedenler:**

Proje SubSystem ayarı Console (/SUBSYSTEM:CONSOLE) olarak kalmış.

main fonksiyonu void main tipinde, bazı durumlarda konsol açılmasına neden oluyor.

**Çözüm Yolu:**

1. main fonksiyonunu int main olarak değiştir:
   ```cpp
   [STAThread]
    int main(array<String^>^ args)
      {
        Application::EnableVisualStyles();
        Application::SetCompatibleTextRenderingDefault(false);
        Project2::MyForm frm;
        Application::Run(%frm);
        return 0;
      }
   ```

 2. SubSystem ayarını kontrol et:

Sağ tık proje → Properties → Linker → System → SubSystem

Windows (/SUBSYSTEM:WINDOWS) seçili olmalı.

Tüm konfigürasyonlar için Clean + Rebuild Solution yap.

## 3️⃣ Alternatif: WinMain kullanmak

Eğer yukarıdaki çözüm işe yaramazsa, klasik Windows giriş noktası kullanabilirsiniz:
```cpp
#include "MyForm.h"
using namespace System;
using namespace System::Windows::Forms;

[STAThread]
int WINAPI WinMain(HINSTANCE, HINSTANCE, LPSTR, int)
{
    Application::EnableVisualStyles();
    Application::SetCompatibleTextRenderingDefault(false);
    Project2::MyForm frm;
    Application::Run(%frm);
    return 0;
}
```
