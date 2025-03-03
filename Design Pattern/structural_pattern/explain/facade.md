# **Facade Design Pattern trong Java**

---

## **1. Facade Pattern là gì?**
**Facade Pattern** là một **mẫu thiết kế cấu trúc (Structural Pattern)** giúp **đơn giản hóa một hệ thống phức tạp bằng cách cung cấp một giao diện duy nhất (Facade)** để client dễ dàng sử dụng.

💡 **Ý tưởng chính:**
- **Ẩn đi sự phức tạp của hệ thống**, chỉ cung cấp một giao diện đơn giản để người dùng tương tác.
- **Giảm sự phụ thuộc giữa client và các thành phần bên trong hệ thống**, giúp dễ bảo trì hơn.

---

## **2. Khi nào sử dụng Facade Pattern?**
✅ Khi hệ thống có **quá nhiều thành phần phức tạp**, cần một giao diện đơn giản để tương tác.  
✅ Khi muốn **giảm sự phụ thuộc** giữa client và hệ thống, giúp code dễ bảo trì.  
✅ Khi muốn **tích hợp hệ thống cũ vào hệ thống mới**, mà không cần thay đổi mã nguồn bên trong.

📌 **Ví dụ thực tế:**  
🔹 **Hệ thống đặt hàng (E-commerce System)**: Facade giúp khách hàng đặt hàng mà không cần biết chi tiết về thanh toán, kho hàng, vận chuyển.  
🔹 **Hệ thống đăng nhập (Authentication System)**: Client chỉ gọi `login()`, mà không cần quan tâm tới xác thực mật khẩu, tạo token, ghi log.  
🔹 **Hệ thống đa phương tiện (Multimedia System)**: Người dùng chỉ cần gọi `playMovie()`, mà không cần biết cách giải mã video, âm thanh.  
🔹 **Giao diện điều khiển xe hơi (Car System)**: Khi nhấn "Start", Facade xử lý động cơ, nhiên liệu, hệ thống điện thay vì người dùng phải khởi động từng phần.

---

## **3. Cấu trúc của Facade Pattern**
Facade Pattern có **3 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Subsystem (Hệ thống con)** | Các lớp có logic phức tạp, thực hiện công việc cụ thể. |
| **Facade (Lớp giao diện đơn giản)** | Cung cấp phương thức đơn giản để client sử dụng, gọi các hệ thống con bên trong. |
| **Client** | Tương tác với hệ thống qua `Facade`, không cần biết chi tiết bên trong. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Client chỉ gọi phương thức của `Facade`**, không cần biết hệ thống hoạt động như thế nào.  
2️⃣ `Facade` gọi các `Subsystem` để thực hiện công việc.  
3️⃣ `Subsystem` thực hiện công việc phức tạp, nhưng client không cần quan tâm đến điều đó.

---

## **4. Triển khai Facade Pattern trong Java**
### **Ví dụ: Hệ thống đặt hàng trực tuyến (Order Processing System)**
Hệ thống gồm các bước:
- **Xác minh kho hàng (`Inventory`)**
- **Xử lý thanh toán (`Payment`)**
- **Giao hàng (`Shipping`)**

---

### **Bước 1: Tạo các `Subsystem` (Hệ thống con)**
#### **Hệ thống kiểm tra kho hàng**
```java
class Inventory {
    public boolean checkStock(String product) {
        System.out.println("📦 Kiểm tra kho hàng cho sản phẩm: " + product);
        return true; // Giả sử sản phẩm luôn có sẵn
    }
}
```

#### **Hệ thống thanh toán**
```java
class Payment {
    public boolean processPayment(String account, double amount) {
        System.out.println("💳 Xử lý thanh toán " + amount + "K từ tài khoản " + account);
        return true; // Giả sử thanh toán luôn thành công
    }
}
```

#### **Hệ thống giao hàng**
```java
class Shipping {
    public void shipProduct(String product) {
        System.out.println("🚚 Giao hàng: " + product);
    }
}
```
🔹 **Các hệ thống con có thể phức tạp hơn trong thực tế, nhưng client không cần biết điều đó.**

---

### **Bước 2: Tạo `Facade` (Lớp giao diện đơn giản)**
```java
class OrderFacade {
    private Inventory inventory;
    private Payment payment;
    private Shipping shipping;

    public OrderFacade() {
        this.inventory = new Inventory();
        this.payment = new Payment();
        this.shipping = new Shipping();
    }

    public void placeOrder(String product, String account, double amount) {
        System.out.println("\n🛒 Đặt hàng: " + product);
        
        if (inventory.checkStock(product)) {
            if (payment.processPayment(account, amount)) {
                shipping.shipProduct(product);
                System.out.println("✅ Đặt hàng thành công!");
            } else {
                System.out.println("❌ Thanh toán thất bại.");
            }
        } else {
            System.out.println("❌ Sản phẩm không có sẵn.");
        }
    }
}
```
🔹 **`OrderFacade` cung cấp phương thức `placeOrder()`**, giúp client đặt hàng chỉ với một lệnh.

---

### **Bước 3: Kiểm thử Facade Pattern**
```java
public class FacadePatternDemo {
    public static void main(String[] args) {
        OrderFacade orderFacade = new OrderFacade();

        // Đặt hàng 1 chiếc iPhone
        orderFacade.placeOrder("iPhone 15", "123-456-789", 25000);
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
🛒 Đặt hàng: iPhone 15
📦 Kiểm tra kho hàng cho sản phẩm: iPhone 15
💳 Xử lý thanh toán 25000K từ tài khoản 123-456-789
🚚 Giao hàng: iPhone 15
✅ Đặt hàng thành công!
```
📌 **Nhận xét:**
- **Client chỉ cần gọi `placeOrder()`**, không cần biết về `Inventory`, `Payment`, `Shipping`.
- **Dễ dàng mở rộng hệ thống**, có thể thêm tính năng `EmailNotification` mà không ảnh hưởng đến client.

---

## **6. Ứng dụng thực tế của Facade Pattern**
✅ **Hệ thống đặt hàng (E-commerce System)** 🛍 (Kiểm tra kho, thanh toán, giao hàng).  
✅ **Hệ thống đăng nhập (Authentication System)** 🔐 (Kiểm tra mật khẩu, tạo token, ghi log).  
✅ **Hệ thống đa phương tiện (Multimedia System)** 🎬 (Phát video, giải mã âm thanh, xử lý hình ảnh).  
✅ **Tích hợp API bên thứ ba** 🌍 (Một API gọi nhiều API nhỏ bên trong).

---

## **7. Ưu điểm & Nhược điểm của Facade Pattern**
✅ **Ưu điểm:**  
✔️ **Đơn giản hóa hệ thống**, giúp client dễ sử dụng.  
✔️ **Giảm sự phụ thuộc**, nếu hệ thống con thay đổi, chỉ cần cập nhật Facade.  
✔️ **Dễ bảo trì**, vì client chỉ tương tác với Facade, không can thiệp vào hệ thống con.

❌ **Nhược điểm:**  
⚠️ **Có thể làm mất đi một số tính linh hoạt**, nếu Facade ẩn quá nhiều chi tiết quan trọng.  
⚠️ **Có thể trở thành điểm nghẽn (Bottleneck)**, nếu tất cả yêu cầu phải đi qua Facade.

---

## **8. Bài tập thực hành**
Hãy triển khai một **hệ thống đăng nhập (`LoginFacade`)**, gồm:
1. **Kiểm tra tên đăng nhập/mật khẩu (`UserAuthentication`)**.
2. **Tạo token phiên đăng nhập (`SessionManager`)**.
3. **Ghi log đăng nhập (`Logger`)**.

👉 **Mục tiêu**: Client chỉ cần gọi `login(username, password)` thay vì xử lý từng bước. 🚀