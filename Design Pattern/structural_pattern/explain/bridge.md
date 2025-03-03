# **Bridge Design Pattern trong Java**

---

## **1. Bridge Pattern là gì?**
**Bridge Pattern** là một **mẫu thiết kế cấu trúc (Structural Pattern)** giúp **tách biệt phần trừu tượng (Abstraction) và phần cài đặt (Implementation)** để chúng có thể thay đổi độc lập.

💡 **Ý tưởng chính:**
- **Tách giao diện (interface) khỏi cài đặt (implementation)**, giúp code dễ mở rộng.
- **Giảm sự phụ thuộc giữa các lớp**, giúp thay đổi một phần mà không ảnh hưởng đến phần còn lại.

---

## **2. Khi nào sử dụng Bridge Pattern?**
✅ Khi cần **tách biệt hai phần của hệ thống** để có thể mở rộng chúng độc lập.  
✅ Khi một lớp có **nhiều biến thể**, cần giảm số lượng class con bằng cách tách abstraction & implementation.  
✅ Khi muốn **tránh kế thừa nhiều cấp** (class explosion).

📌 **Ví dụ thực tế:**  
🔹 **Hệ thống thiết bị điện tử (Điều khiển từ xa & thiết bị)**: TV, Radio, v.v. có thể điều khiển bằng nhiều loại remote khác nhau.  
🔹 **Hệ thống thanh toán**: Visa, PayPal, Bitcoin có thể được tích hợp vào nhiều nền tảng khác nhau.  
🔹 **Xuất dữ liệu (Export)**: Có thể xuất sang PDF, CSV, XML, nhưng cách lấy dữ liệu có thể khác nhau.  
🔹 **Hệ thống giao thông**: Xe hơi, xe máy có thể chạy trên đường phố hoặc đường cao tốc.

---

## **3. Cấu trúc của Bridge Pattern**
Bridge Pattern có **4 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Abstraction** | Lớp trừu tượng, đại diện cho phần giao diện của hệ thống. |
| **RefinedAbstraction** | Mở rộng `Abstraction`, có thể chứa logic bổ sung. |
| **Implementor (interface)** | Định nghĩa các phương thức cài đặt cụ thể. |
| **ConcreteImplementor** | Cài đặt cụ thể của `Implementor`, chứa logic thực tế. |

📌 **Quy tắc hoạt động:**  
1️⃣ `Abstraction` không trực tiếp thực hiện hành vi, mà ủy quyền cho `Implementor`.  
2️⃣ `ConcreteImplementor` chứa logic cụ thể, có thể thay đổi mà không ảnh hưởng đến `Abstraction`.  
3️⃣ `RefinedAbstraction` có thể mở rộng `Abstraction` để thêm tính năng mới.

---

## **4. Triển khai Bridge Pattern trong Java**
### **Ví dụ: Hệ thống điều khiển thiết bị điện tử (Remote & Device)**
Hệ thống hỗ trợ **điều khiển từ xa** cho **TV và Radio** bằng **2 loại remote khác nhau**:
- **Remote thông thường** (`BasicRemote`).
- **Remote nâng cao** (`AdvancedRemote`).

---

### **Bước 1: Tạo `Implementor` (Giao diện thiết bị)**
```java
interface Device {
    void turnOn();
    void turnOff();
    void setVolume(int percent);
}
```
🔹 **Mọi thiết bị (TV, Radio) đều phải tuân theo giao diện này.**

---

### **Bước 2: Cài đặt `ConcreteImplementor` cho TV và Radio**
#### **Cài đặt `TV`**
```java
class TV implements Device {
    private int volume = 50;

    @Override
    public void turnOn() {
        System.out.println("📺 TV bật.");
    }

    @Override
    public void turnOff() {
        System.out.println("📺 TV tắt.");
    }

    @Override
    public void setVolume(int percent) {
        this.volume = percent;
        System.out.println("📺 TV âm lượng: " + volume + "%");
    }
}
```

#### **Cài đặt `Radio`**
```java
class Radio implements Device {
    private int volume = 30;

    @Override
    public void turnOn() {
        System.out.println("📻 Radio bật.");
    }

    @Override
    public void turnOff() {
        System.out.println("📻 Radio tắt.");
    }

    @Override
    public void setVolume(int percent) {
        this.volume = percent;
        System.out.println("📻 Radio âm lượng: " + volume + "%");
    }
}
```
🔹 **TV và Radio có thể bật, tắt, chỉnh âm lượng, nhưng hoạt động khác nhau.**

---

### **Bước 3: Tạo `Abstraction` (Remote điều khiển)**
```java
abstract class RemoteControl {
    protected Device device;

    public RemoteControl(Device device) {
        this.device = device;
    }

    public void turnOn() {
        device.turnOn();
    }

    public void turnOff() {
        device.turnOff();
    }

    public void setVolume(int percent) {
        device.setVolume(percent);
    }
}
```
🔹 **`RemoteControl` không quan tâm đến thiết bị cụ thể, chỉ ủy quyền cho `Device`.**

---

### **Bước 4: Tạo `RefinedAbstraction` (Các loại Remote)**
#### **Remote cơ bản**
```java
class BasicRemote extends RemoteControl {
    public BasicRemote(Device device) {
        super(device);
    }

    public void mute() {
        System.out.println("🔇 Tắt âm.");
        device.setVolume(0);
    }
}
```

#### **Remote nâng cao**
```java
class AdvancedRemote extends RemoteControl {
    public AdvancedRemote(Device device) {
        super(device);
    }

    public void boostVolume() {
        System.out.println("🔊 Tăng âm lượng.");
        device.setVolume(100);
    }
}
```
🔹 **Remote có thể thêm các tính năng mà không cần sửa đổi `Device`.**

---

### **Bước 5: Kiểm thử Bridge Pattern**
```java
public class BridgePatternDemo {
    public static void main(String[] args) {
        Device tv = new TV();
        Device radio = new Radio();

        RemoteControl basicRemoteForTV = new BasicRemote(tv);
        RemoteControl advancedRemoteForRadio = new AdvancedRemote(radio);

        System.out.println("\n📺 Điều khiển TV:");
        basicRemoteForTV.turnOn();
        basicRemoteForTV.setVolume(70);
        ((BasicRemote) basicRemoteForTV).mute();
        basicRemoteForTV.turnOff();

        System.out.println("\n📻 Điều khiển Radio:");
        advancedRemoteForRadio.turnOn();
        ((AdvancedRemote) advancedRemoteForRadio).boostVolume();
        advancedRemoteForRadio.turnOff();
    }
}
```

---

## **6. Kết quả khi chạy chương trình**
```
📺 Điều khiển TV:
📺 TV bật.
📺 TV âm lượng: 70%
🔇 Tắt âm.
📺 TV âm lượng: 0%
📺 TV tắt.

📻 Điều khiển Radio:
📻 Radio bật.
🔊 Tăng âm lượng.
📻 Radio âm lượng: 100%
📻 Radio tắt.
```
📌 **Nhận xét:**
- **Remote có thể điều khiển bất kỳ thiết bị nào (`TV`, `Radio`) mà không cần sửa đổi mã nguồn.**
- **Dễ dàng thêm `SmartRemote` hoặc `Speaker` mà không ảnh hưởng đến các lớp khác.**

---

## **7. Ứng dụng thực tế của Bridge Pattern**
✅ **Hệ thống điều khiển từ xa** 🎮 (Điều khiển TV, Radio, Smart Speaker).  
✅ **Hệ thống thanh toán** 💳 (Visa, PayPal, Bitcoin có thể dùng trên Mobile App, Web App).  
✅ **Xuất báo cáo** 📊 (CSV, PDF, XML có thể xuất từ MySQL, PostgreSQL).  
✅ **Hệ thống giao thông** 🚗 (Xe hơi, Xe máy có thể chạy trên Đường phố hoặc Cao tốc).

---

## **8. Ưu điểm & Nhược điểm của Bridge Pattern**
✅ **Ưu điểm:**  
✔️ **Giảm số lượng class con**, thay vì kế thừa, ta tách `Abstraction` & `Implementation`.  
✔️ **Dễ mở rộng**, có thể thêm `Device` hoặc `RemoteControl` mới mà không ảnh hưởng mã nguồn hiện tại.  
✔️ **Linh hoạt**, giúp sử dụng cùng một giao diện với nhiều loại đối tượng khác nhau.

❌ **Nhược điểm:**  
⚠️ **Tăng độ phức tạp**, cần thêm nhiều class trung gian.  
⚠️ **Có thể dư thừa**, nếu hệ thống không cần thay đổi `Implementation`.

---

## **9. Bài tập thực hành**
Hãy triển khai một **hệ thống thanh toán (`PaymentSystem`)**, gồm:
1. **Loại thanh toán**: `VisaPayment`, `PayPalPayment`.
2. **Nền tảng hỗ trợ**: `WebApp`, `MobileApp`.

👉 **Mục tiêu**: Thanh toán có thể thực hiện trên cả web và mobile mà không cần sửa đổi từng loại thanh toán. 🚀