# **Strategy Design Pattern trong Java**

## **1. Strategy Pattern là gì?**
**Strategy Pattern** là một **mẫu thiết kế hành vi (Behavioral Pattern)**, cho phép thay đổi thuật toán trong khi chạy mà **không cần sửa đổi mã nguồn** của lớp sử dụng thuật toán đó.

💡 **Ý tưởng chính:**
- **Tách thuật toán** ra khỏi lớp chính.
- **Định nghĩa một nhóm thuật toán** (Strategies) có thể hoán đổi cho nhau.
- **Cho phép đối tượng sử dụng một Strategy linh hoạt** mà không cần thay đổi mã nguồn.

---

## **2. Khi nào sử dụng Strategy Pattern?**
✅ Khi có **nhiều cách khác nhau để thực hiện một hành động** (ví dụ: nhiều cách sắp xếp dữ liệu).  
✅ Khi muốn **loại bỏ các câu lệnh `if-else` hoặc `switch-case` phức tạp**.  
✅ Khi cần **tăng khả năng mở rộng**, dễ dàng thêm thuật toán mới mà không làm thay đổi lớp hiện có.

📌 **Ví dụ thực tế:**  
🔹 **Thuật toán sắp xếp**: QuickSort, MergeSort, BubbleSort có thể thay đổi linh hoạt.  
🔹 **Chương trình nén dữ liệu**: Nén bằng ZIP, RAR, GZIP mà không cần sửa đổi mã gốc.  
🔹 **Xác thực người dùng**: Đăng nhập bằng **Google, Facebook, Email** có thể thay đổi dễ dàng.

---

## **3. Cấu trúc của Strategy Pattern**
Strategy Pattern có **3 thành phần chính**:

| Thành phần | Vai trò |
|------------|---------|
| **Strategy (interface)** | Định nghĩa phương thức chung cho tất cả các thuật toán. |
| **ConcreteStrategy** | Cài đặt cụ thể của từng thuật toán. |
| **Context** | Chứa Strategy và có thể thay đổi nó linh hoạt. |

📌 **Quy tắc hoạt động:**  
1️⃣ `Context` chứa một `Strategy`.  
2️⃣ `Context` gọi `executeStrategy()`, nhưng thực tế sẽ thực thi một `ConcreteStrategy` cụ thể.  
3️⃣ Khi cần thay đổi thuật toán, chỉ cần thay `Strategy` mà không cần sửa đổi `Context`.

---

## **4. Triển khai Strategy Pattern trong Java**
### **Ví dụ: Chương trình tính toán chi phí thanh toán**
**Yêu cầu:** Hệ thống hỗ trợ **3 phương thức thanh toán**:
- **Thanh toán bằng PayPal**
- **Thanh toán bằng Thẻ tín dụng**
- **Thanh toán bằng Ví điện tử (E-Wallet)**

---

### **Bước 1: Tạo interface `PaymentStrategy` (Strategy)**
```java
interface PaymentStrategy {
    void pay(int amount);
}
```
🔹 **Mọi phương thức thanh toán phải triển khai phương thức `pay()`.**

---

### **Bước 2: Cài đặt các chiến lược thanh toán (`ConcreteStrategy`)**
#### **Thanh toán bằng PayPal**
```java
class PayPalPayment implements PaymentStrategy {
    private String email;

    public PayPalPayment(String email) {
        this.email = email;
    }

    @Override
    public void pay(int amount) {
        System.out.println("💰 Thanh toán " + amount + " bằng PayPal (Email: " + email + ")");
    }
}
```

#### **Thanh toán bằng Thẻ tín dụng**
```java
class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;

    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    @Override
    public void pay(int amount) {
        System.out.println("💳 Thanh toán " + amount + " bằng Thẻ tín dụng (Số thẻ: " + cardNumber + ")");
    }
}
```

#### **Thanh toán bằng Ví điện tử**
```java
class EWalletPayment implements PaymentStrategy {
    private String phoneNumber;

    public EWalletPayment(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }

    @Override
    public void pay(int amount) {
        System.out.println("📱 Thanh toán " + amount + " bằng Ví điện tử (SĐT: " + phoneNumber + ")");
    }
}
```

---

### **Bước 3: Tạo lớp `ShoppingCart` (Context)**
```java
class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(int amount) {
        if (paymentStrategy == null) {
            System.out.println("⚠ Vui lòng chọn phương thức thanh toán!");
        } else {
            paymentStrategy.pay(amount);
        }
    }
}
```
🔹 `ShoppingCart` chứa một **Strategy**, có thể thay đổi linh hoạt.

---

### **Bước 4: Kiểm thử Strategy Pattern**
```java
public class StrategyPatternDemo {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        // Thanh toán bằng PayPal
        cart.setPaymentStrategy(new PayPalPayment("user@example.com"));
        cart.checkout(100);

        // Đổi sang thanh toán bằng thẻ tín dụng
        cart.setPaymentStrategy(new CreditCardPayment("1234-5678-9876-5432"));
        cart.checkout(250);

        // Đổi sang thanh toán bằng ví điện tử
        cart.setPaymentStrategy(new EWalletPayment("0987654321"));
        cart.checkout(50);
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
💰 Thanh toán 100 bằng PayPal (Email: user@example.com)
💳 Thanh toán 250 bằng Thẻ tín dụng (Số thẻ: 1234-5678-9876-5432)
📱 Thanh toán 50 bằng Ví điện tử (SĐT: 0987654321)
```
📌 **Nhận xét:**
- `ShoppingCart` có thể thay đổi phương thức thanh toán mà không cần thay đổi mã gốc.
- **Không cần `if-else`, mỗi phương thức thanh toán đều độc lập và dễ mở rộng.**

---

## **6. Ứng dụng thực tế của Strategy Pattern**
✅ **Hệ thống nén file**: ZIP, RAR, GZIP, 7z.  
✅ **Thuật toán tìm kiếm**: Tìm kiếm tuần tự, nhị phân, heuristic.  
✅ **Xác thực người dùng**: Google, Facebook, OAuth, Username/Password.  
✅ **Trình biên dịch**: Tối ưu hóa code bằng nhiều thuật toán khác nhau.

---

## **7. Ưu điểm & Nhược điểm của Strategy Pattern**
✅ **Ưu điểm:**  
✔️ **Loại bỏ các câu lệnh `if-else` dài dòng**, giúp mã sạch hơn.  
✔️ **Tăng tính linh hoạt**, có thể thêm thuật toán mới mà không cần sửa mã cũ.  
✔️ **Tuân theo nguyên tắc Open/Closed** (SOLID), dễ mở rộng.

❌ **Nhược điểm:**  
⚠️ **Tạo nhiều class**, có thể làm tăng độ phức tạp của hệ thống.  
⚠️ **Context phải biết về tất cả các Strategies**, có thể gây khó khăn trong quản lý.

---

## **8. Bài tập thực hành**
Hãy triển khai một hệ thống **thuật toán sắp xếp**, gồm các chiến lược:
1. **Bubble Sort**
2. **Quick Sort**
3. **Merge Sort**  
   👉 **Lớp `SortingContext`** có thể thay đổi thuật toán sắp xếp linh hoạt.

Bạn có muốn tự làm trước, hay mình sẽ hướng dẫn từng bước? 🚀