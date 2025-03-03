# **Adapter Design Pattern trong Java**

---

## **1. Adapter Pattern là gì?**
**Adapter Pattern** là một **mẫu thiết kế cấu trúc (Structural Pattern)** giúp **tương thích hai lớp có giao diện khác nhau** mà không cần sửa đổi mã nguồn của chúng.

💡 **Ý tưởng chính:**
- **Chuyển đổi giao diện của một lớp thành một giao diện khác mà client mong đợi.**
- **Đóng vai trò như một "bộ chuyển đổi"**, giúp các lớp không tương thích có thể làm việc cùng nhau.

---

## **2. Khi nào sử dụng Adapter Pattern?**
✅ Khi cần **tích hợp một lớp hoặc thư viện có giao diện không tương thích** với hệ thống hiện tại.  
✅ Khi muốn **sử dụng lại mã nguồn cũ** mà không cần sửa đổi nó.  
✅ Khi làm việc với **các hệ thống bên thứ ba (third-party libraries, APIs)**.

📌 **Ví dụ thực tế:**  
🔹 **Bộ sạc điện thoại**: Cổng sạc Type-C nhưng bạn chỉ có dây Micro-USB → Cần Adapter.  
🔹 **Đọc file từ nhiều định dạng khác nhau**: JSON, XML, CSV cần adapter để thống nhất cách đọc.  
🔹 **Giao tiếp giữa các hệ thống cũ và mới**: Ứng dụng cũ dùng `LegacyLogger`, ứng dụng mới dùng `ModernLogger`.  
🔹 **Chuyển đổi đơn vị đo lường**: Hệ thống Mỹ (inch, pound) vs Hệ thống quốc tế (cm, kg).

---

## **3. Cấu trúc của Adapter Pattern**
Adapter Pattern có **4 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Target (interface)** | Định nghĩa giao diện mà Client mong đợi. |
| **Adaptee** | Lớp hiện có có giao diện không tương thích. |
| **Adapter** | Cầu nối giữa `Target` và `Adaptee`, giúp chúng làm việc cùng nhau. |
| **Client** | Sử dụng `Target` mà không biết về `Adaptee`. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Client chỉ tương tác với `Target` interface**, không cần biết về `Adaptee`.  
2️⃣ `Adapter` đóng vai trò trung gian, chuyển đổi dữ liệu từ `Target` sang `Adaptee`.  
3️⃣ `Adaptee` thực hiện chức năng gốc của nó mà không cần sửa đổi mã nguồn.

---

## **4. Triển khai Adapter Pattern trong Java**
### **Ví dụ: Bộ sạc điện thoại (Chuyển đổi cổng Type-C sang Micro-USB)**
Hệ thống hỗ trợ **sạc điện thoại qua Type-C**, nhưng **điện thoại chỉ hỗ trợ Micro-USB**.

---

### **Bước 1: Tạo `Target` (Giao diện sạc Type-C)**
```java
interface TypeCCharger {
    void chargeWithTypeC();
}
```
🔹 **Mọi bộ sạc Type-C đều phải triển khai `chargeWithTypeC()`.**

---

### **Bước 2: Tạo `Adaptee` (Thiết bị chỉ hỗ trợ Micro-USB)**
```java
class MicroUSBPhone {
    public void chargeWithMicroUSB() {
        System.out.println("🔌 Sạc bằng Micro-USB...");
    }
}
```
🔹 **Điện thoại cũ chỉ có thể sạc bằng Micro-USB.**

---

### **Bước 3: Tạo `Adapter` (Chuyển đổi Type-C thành Micro-USB)**
```java
class TypeCToMicroUSBAdapter implements TypeCCharger {
    private MicroUSBPhone phone;

    public TypeCToMicroUSBAdapter(MicroUSBPhone phone) {
        this.phone = phone;
    }

    @Override
    public void chargeWithTypeC() {
        System.out.println("🔄 Chuyển đổi từ Type-C sang Micro-USB...");
        phone.chargeWithMicroUSB();
    }
}
```
🔹 **Adapter nhận đầu vào là Type-C, nhưng thực tế gọi `chargeWithMicroUSB()`.**

---

### **Bước 4: Kiểm thử Adapter Pattern**
```java
public class AdapterPatternDemo {
    public static void main(String[] args) {
        // Tạo điện thoại chỉ hỗ trợ Micro-USB
        MicroUSBPhone phone = new MicroUSBPhone();

        // Sử dụng Adapter để sạc bằng Type-C
        TypeCCharger adapter = new TypeCToMicroUSBAdapter(phone);
        adapter.chargeWithTypeC();
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
🔄 Chuyển đổi từ Type-C sang Micro-USB...
🔌 Sạc bằng Micro-USB...
```
📌 **Nhận xét:**
- **Client chỉ cần gọi `chargeWithTypeC()`**, nhưng thực tế điện thoại vẫn sạc bằng Micro-USB.
- **Không cần sửa đổi mã nguồn của `MicroUSBPhone`**, giúp dễ bảo trì.

---

## **6. Hai loại Adapter Pattern**

### **1️⃣ Object Adapter (Dùng Composition - Ưu tiên)**
- `Adapter` **chứa một tham chiếu đến `Adaptee`**, gọi phương thức của `Adaptee`.
- **Ưu điểm**: Linh hoạt hơn vì có thể thay đổi `Adaptee` khi cần.

### **2️⃣ Class Adapter (Dùng Inheritance - Ít dùng)**
```java
class TypeCToMicroUSBClassAdapter extends MicroUSBPhone implements TypeCCharger {
    @Override
    public void chargeWithTypeC() {
        System.out.println("🔄 Chuyển đổi từ Type-C sang Micro-USB...");
        chargeWithMicroUSB();
    }
}
```
- **Dùng kế thừa (`extends`) thay vì Composition**.
- **Nhược điểm**: Cứng nhắc, vì Java không hỗ trợ đa kế thừa.

---

## **7. Ứng dụng thực tế của Adapter Pattern**
✅ **Bộ sạc điện thoại** 🔋 (Type-C → Micro-USB).  
✅ **Đọc nhiều định dạng file khác nhau** 📂 (JSON → XML → CSV).  
✅ **Giao tiếp với API bên thứ ba** 🌍 (REST → SOAP).  
✅ **Tương thích giữa các hệ thống cũ và mới** 🔄 (Legacy Code → Modern Code).  
✅ **Chuyển đổi đơn vị đo lường** 📏 (Mỹ → Quốc tế).

---

## **8. Ưu điểm & Nhược điểm của Adapter Pattern**
✅ **Ưu điểm:**  
✔️ **Không cần sửa mã nguồn của `Adaptee`**, giúp tích hợp dễ dàng.  
✔️ **Tái sử dụng mã nguồn cũ mà không làm gián đoạn hệ thống.**  
✔️ **Dễ mở rộng**, có thể tạo nhiều Adapter cho các loại `Adaptee` khác nhau.

❌ **Nhược điểm:**  
⚠️ **Có thể làm tăng độ phức tạp**, nếu có quá nhiều Adapter.  
⚠️ **Hiệu suất có thể bị ảnh hưởng**, vì thêm một lớp trung gian.  
⚠️ **Class Adapter ít linh hoạt**, vì bị ràng buộc với `Adaptee` do kế thừa.

---

## **9. Bài tập thực hành**
Hãy triển khai một **hệ thống đọc dữ liệu từ nhiều định dạng file (`FileReaderAdapter`)**, gồm:
1. **CSVReader**: Đọc file `.csv`.
2. **XMLReader**: Đọc file `.xml`.
3. **JSONReader**: Đọc file `.json`.

👉 **Mục tiêu**: Viết một `FileReaderAdapter` để `Client` có thể đọc tất cả định dạng file qua một giao diện chung. 🚀