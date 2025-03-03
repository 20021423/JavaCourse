# **Factory Method Design Pattern trong Java**

---

## **1. Factory Method Pattern là gì?**
**Factory Method Pattern** là một **mẫu thiết kế sáng tạo (Creational Pattern)** cung cấp một **phương thức để tạo đối tượng**, nhưng **để các lớp con quyết định sẽ tạo loại đối tượng nào**.

💡 **Ý tưởng chính:**
- **Tránh khởi tạo đối tượng trực tiếp bằng `new`**, giúp code dễ bảo trì và mở rộng.
- **Cho phép tạo đối tượng mà không cần chỉ định lớp cụ thể**, giúp hỗ trợ tính đa hình (Polymorphism).

---

## **2. Khi nào sử dụng Factory Method Pattern?**
✅ Khi cần **tạo đối tượng nhưng không muốn chỉ định lớp cụ thể**.  
✅ Khi **có nhiều loại đối tượng khác nhau**, nhưng cùng thực hiện một hành vi chung.  
✅ Khi cần **tách rời logic khởi tạo đối tượng khỏi phần còn lại của chương trình**.

📌 **Ví dụ thực tế:**  
🔹 **Hệ thống thông báo (Notification System)**: Email, SMS, Push Notification.  
🔹 **Trình quản lý tài liệu**: Tạo Word, PDF, Excel từ cùng một Factory.  
🔹 **Hệ thống thanh toán**: PayPal, Visa, Bitcoin, nhưng chỉ gọi một `PaymentFactory`.  
🔹 **Trình duyệt Web**: Chrome, Firefox, Safari đều dùng chung một giao diện `BrowserFactory`.

---

## **3. Cấu trúc của Factory Method Pattern**
Factory Method Pattern có **4 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Product (interface)** | Định nghĩa phương thức chung cho tất cả các loại sản phẩm. |
| **ConcreteProduct** | Cài đặt cụ thể của `Product`, mỗi loại có thể có cách hoạt động riêng. |
| **Creator (interface/abstract class)** | Định nghĩa phương thức `createProduct()` nhưng không triển khai. |
| **ConcreteCreator** | Cài đặt phương thức `createProduct()`, quyết định tạo ra loại `Product` nào. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Client không trực tiếp tạo đối tượng bằng `new`**, mà gọi `createProduct()`.  
2️⃣ `Factory Method` quyết định **tạo đối tượng nào** dựa trên tham số hoặc logic nội bộ.  
3️⃣ **Dễ dàng mở rộng**: Thêm một loại `Product` mới chỉ cần tạo `ConcreteProduct` và `ConcreteCreator` mới.

---

## **4. Triển khai Factory Method Pattern trong Java**
### **Ví dụ: Hệ thống thông báo (Notification System)**
Hệ thống hỗ trợ **3 loại thông báo**:
- **Email Notification**
- **SMS Notification**
- **Push Notification**

---

### **Bước 1: Tạo interface `Notification` (Product)**
```java
interface Notification {
    void notifyUser();
}
```
🔹 **Mọi loại thông báo phải triển khai phương thức `notifyUser()`.**

---

### **Bước 2: Cài đặt các `ConcreteProduct` (Các loại thông báo)**
#### **Email Notification**
```java
class EmailNotification implements Notification {
    @Override
    public void notifyUser() {
        System.out.println("📧 Gửi thông báo qua Email.");
    }
}
```

#### **SMS Notification**
```java
class SMSNotification implements Notification {
    @Override
    public void notifyUser() {
        System.out.println("📩 Gửi thông báo qua SMS.");
    }
}
```

#### **Push Notification**
```java
class PushNotification implements Notification {
    @Override
    public void notifyUser() {
        System.out.println("🔔 Gửi thông báo Push Notification.");
    }
}
```
🔹 **Mỗi loại thông báo có cách hoạt động riêng nhưng tuân theo `Notification` interface.**

---

### **Bước 3: Tạo Factory `NotificationFactory`**
```java
class NotificationFactory {
    public static Notification createNotification(String type) {
        if (type.equalsIgnoreCase("EMAIL")) {
            return new EmailNotification();
        } else if (type.equalsIgnoreCase("SMS")) {
            return new SMSNotification();
        } else if (type.equalsIgnoreCase("PUSH")) {
            return new PushNotification();
        } else {
            throw new IllegalArgumentException("Loại thông báo không hợp lệ: " + type);
        }
    }
}
```
🔹 **`NotificationFactory` quyết định loại `Notification` nào sẽ được tạo ra dựa trên tham số `type`.**

---

### **Bước 4: Kiểm thử Factory Method Pattern**
```java
public class FactoryMethodPatternDemo {
    public static void main(String[] args) {
        Notification notification1 = NotificationFactory.createNotification("EMAIL");
        notification1.notifyUser(); // 📧 Gửi thông báo qua Email.

        Notification notification2 = NotificationFactory.createNotification("SMS");
        notification2.notifyUser(); // 📩 Gửi thông báo qua SMS.

        Notification notification3 = NotificationFactory.createNotification("PUSH");
        notification3.notifyUser(); // 🔔 Gửi thông báo Push Notification.
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
📧 Gửi thông báo qua Email.
📩 Gửi thông báo qua SMS.
🔔 Gửi thông báo Push Notification.
```
📌 **Nhận xét:**
- **Client không cần biết lớp cụ thể (`EmailNotification`, `SMSNotification`, `PushNotification`)**, chỉ cần gọi `createNotification()`.
- **Dễ dàng thêm loại thông báo mới** mà không sửa đổi code hiện có.

---

## **6. Ứng dụng thực tế của Factory Method Pattern**
✅ **Hệ thống thanh toán** 🏦 (PayPal, Visa, Bitcoin).  
✅ **Trình quản lý tài liệu** 📄 (Word, PDF, Excel).  
✅ **Trình duyệt Web** 🌍 (Chrome, Firefox, Safari).  
✅ **Kết nối cơ sở dữ liệu** 🗄 (MySQL, PostgreSQL, SQLite).

---

## **7. Ưu điểm & Nhược điểm của Factory Method Pattern**
✅ **Ưu điểm:**  
✔️ **Dễ mở rộng**, chỉ cần thêm một `ConcreteProduct` và cập nhật Factory.  
✔️ **Giảm sự phụ thuộc giữa Client và các lớp cụ thể**, giúp code linh hoạt hơn.  
✔️ **Dễ bảo trì**, vì toàn bộ logic khởi tạo tập trung tại một nơi (`Factory`).

❌ **Nhược điểm:**  
⚠️ **Có thể tạo quá nhiều lớp**, vì mỗi loại `Product` cần một `ConcreteCreator` riêng.  
⚠️ **Nếu Factory chứa nhiều `if-else`, có thể gây khó bảo trì**, cần sử dụng `Enum` hoặc `Reflection` để tối ưu.

---

## **8. Bài tập thực hành**
Hãy triển khai một **hệ thống tạo phương tiện giao thông (`VehicleFactory`)**, gồm:
1. **Car (`Ô tô`)**
2. **Bike (`Xe máy`)**
3. **Truck (`Xe tải`)**

👉 **Mục tiêu**: Client chỉ cần gọi `VehicleFactory.createVehicle("CAR")` mà không cần biết chi tiết bên trong.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀