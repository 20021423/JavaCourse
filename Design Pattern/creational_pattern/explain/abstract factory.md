# **Abstract Factory Design Pattern trong Java**

---

## **1. Abstract Factory Pattern là gì?**
**Abstract Factory Pattern** là một **mẫu thiết kế sáng tạo (Creational Pattern)** cung cấp **một giao diện (interface) để tạo ra các nhóm đối tượng liên quan** mà không cần chỉ định lớp cụ thể.

💡 **Ý tưởng chính:**
- **Mở rộng Factory Method**, cho phép tạo **các họ (families) đối tượng** liên quan với nhau.
- **Tách rời logic khởi tạo**, giúp dễ mở rộng và thay đổi mà không ảnh hưởng đến mã nguồn hiện có.

---

## **2. Khi nào sử dụng Abstract Factory Pattern?**
✅ Khi cần **tạo ra nhiều loại đối tượng liên quan đến nhau**.  
✅ Khi muốn **giảm sự phụ thuộc vào các lớp cụ thể**, giúp mở rộng dễ dàng.  
✅ Khi có **nhiều họ sản phẩm** nhưng muốn sử dụng chúng một cách thống nhất.

📌 **Ví dụ thực tế:**  
🔹 **Giao diện đồ họa (GUI Frameworks)**: Tạo `Button`, `Checkbox`, `Textbox` cho Windows và macOS.  
🔹 **Hệ thống thanh toán**: PayPal, Visa, Bitcoin đều có `Transaction` và `Receipt` riêng.  
🔹 **Công cụ vẽ hình học**: Mỗi `ShapeFactory` có thể tạo `Circle`, `Rectangle`, `Square` theo cách riêng.  
🔹 **Hệ thống cơ sở dữ liệu (Database)**: MySQL, PostgreSQL, Oracle có `Connection` và `Query` riêng.

---

## **3. Cấu trúc của Abstract Factory Pattern**
Abstract Factory Pattern có **5 thành phần chính**:

| **Thành phần** | **Vai trò** |
|---------------|------------|
| **AbstractProduct** | Định nghĩa phương thức chung cho các loại sản phẩm. |
| **ConcreteProduct** | Cài đặt cụ thể của `AbstractProduct`, mỗi sản phẩm có thể có cách hoạt động riêng. |
| **AbstractFactory** | Định nghĩa phương thức để tạo ra các họ sản phẩm. |
| **ConcreteFactory** | Cài đặt cụ thể của `AbstractFactory`, tạo ra các nhóm sản phẩm cụ thể. |
| **Client** | Sử dụng `AbstractFactory` để tạo ra các sản phẩm mà không cần biết chi tiết bên trong. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Client không khởi tạo sản phẩm trực tiếp**, mà gọi `createProduct()` từ `AbstractFactory`.  
2️⃣ `ConcreteFactory` quyết định **tạo sản phẩm nào**, giúp dễ mở rộng và thay đổi.  
3️⃣ **Dễ dàng thay đổi nhóm sản phẩm** mà không sửa mã nguồn của Client.

---

## **4. Triển khai Abstract Factory Pattern trong Java**
### **Ví dụ: Hệ thống giao diện UI cho Windows và macOS**
Hệ thống hỗ trợ **2 họ sản phẩm**:
- **Windows UI** (WindowsButton, WindowsCheckbox).
- **macOS UI** (MacButton, MacCheckbox).

---

### **Bước 1: Tạo interface `Button` và `Checkbox` (AbstractProduct)**
```java
interface Button {
    void render();
}

interface Checkbox {
    void check();
}
```
🔹 **Mọi nút bấm (`Button`) và hộp kiểm (`Checkbox`) phải tuân theo giao diện này.**

---

### **Bước 2: Cài đặt `ConcreteProduct` cho Windows**
```java
class WindowsButton implements Button {
    @Override
    public void render() {
        System.out.println("🖱 Windows Button được hiển thị.");
    }
}

class WindowsCheckbox implements Checkbox {
    @Override
    public void check() {
        System.out.println("☑ Windows Checkbox được chọn.");
    }
}
```

---

### **Bước 3: Cài đặt `ConcreteProduct` cho macOS**
```java
class MacButton implements Button {
    @Override
    public void render() {
        System.out.println("🖱 macOS Button được hiển thị.");
    }
}

class MacCheckbox implements Checkbox {
    @Override
    public void check() {
        System.out.println("☑ macOS Checkbox được chọn.");
    }
}
```
🔹 **Windows và macOS có `Button` và `Checkbox` riêng, nhưng chúng tuân theo cùng một giao diện.**

---

### **Bước 4: Tạo `AbstractFactory`**
```java
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}
```
🔹 **Mọi `GUIFactory` phải có khả năng tạo `Button` và `Checkbox`.**

---

### **Bước 5: Cài đặt `ConcreteFactory` cho Windows**
```java
class WindowsFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WindowsButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}
```

---

### **Bước 6: Cài đặt `ConcreteFactory` cho macOS**
```java
class MacFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new MacButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}
```
🔹 **Mỗi `Factory` tạo các sản phẩm phù hợp với hệ điều hành.**

---

### **Bước 7: Kiểm thử Abstract Factory Pattern**
```java
public class AbstractFactoryPatternDemo {
    public static void main(String[] args) {
        // Chọn hệ điều hành cần khởi tạo UI
        GUIFactory factory;

        String os = "macOS"; // Thay đổi thành "Windows" để dùng Windows UI

        if (os.equalsIgnoreCase("Windows")) {
            factory = new WindowsFactory();
        } else {
            factory = new MacFactory();
        }

        // Tạo sản phẩm từ Factory
        Button button = factory.createButton();
        Checkbox checkbox = factory.createCheckbox();

        // Hiển thị UI
        button.render();
        checkbox.check();
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
🖱 macOS Button được hiển thị.
☑ macOS Checkbox được chọn.
```
📌 **Nhận xét:**
- **Không cần biết cụ thể là Windows hay macOS**, chỉ cần gọi `createButton()` và `createCheckbox()`.
- **Dễ dàng thay đổi hệ điều hành**, chỉ cần thay `GUIFactory`.

---

## **6. Ứng dụng thực tế của Abstract Factory Pattern**
✅ **Hệ thống UI đa nền tảng** 🖥 (Windows, macOS, Linux).  
✅ **Kết nối cơ sở dữ liệu** 🗄 (MySQL, PostgreSQL, MongoDB).  
✅ **Xử lý file (File Reader)** 📂 (TXT, CSV, JSON).  
✅ **Hệ thống thanh toán** 💳 (PayPal, Stripe, Bitcoin).

---

## **7. Ưu điểm & Nhược điểm của Abstract Factory Pattern**
✅ **Ưu điểm:**  
✔️ **Tách biệt logic khởi tạo sản phẩm**, giúp code dễ mở rộng và bảo trì.  
✔️ **Hỗ trợ nhiều họ sản phẩm**, giúp thay đổi toàn bộ giao diện UI chỉ bằng cách đổi Factory.  
✔️ **Đảm bảo tính nhất quán**, các sản phẩm trong cùng một Factory luôn tương thích với nhau.

❌ **Nhược điểm:**  
⚠️ **Có thể tạo quá nhiều lớp**, gây khó khăn trong quản lý.  
⚠️ **Cần cập nhật Factory nếu thêm sản phẩm mới**, trừ khi dùng `Reflection`.

---

## **8. Bài tập thực hành**
Hãy triển khai một hệ thống **kết nối cơ sở dữ liệu (`DatabaseFactory`)**, gồm:
1. **MySQL** (`MySQLConnection`, `MySQLQuery`).
2. **PostgreSQL** (`PostgreSQLConnection`, `PostgreSQLQuery`).

👉 **Mục tiêu**: Dùng `DatabaseFactory.createConnection()` và `DatabaseFactory.createQuery()` mà không cần biết chi tiết bên trong.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀