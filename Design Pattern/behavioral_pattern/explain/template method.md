# **Template Method Design Pattern trong Java**

---

## **1. Template Method Pattern là gì?**
**Template Method Pattern** là một **mẫu thiết kế hành vi (Behavioral Pattern)**, cho phép định nghĩa **bộ khung (template) của một thuật toán**, nhưng **cho phép các bước cụ thể được triển khai bởi các lớp con**.

💡 **Ý tưởng chính:**
- **Lớp cha xác định luồng thực thi tổng quát**.
- **Các lớp con chỉ cần cung cấp chi tiết cho các bước cụ thể**.
- **Tránh việc trùng lặp mã**, giúp dễ bảo trì và mở rộng.

---

## **2. Khi nào sử dụng Template Method Pattern?**
✅ Khi có **một thuật toán cố định**, nhưng các bước trong thuật toán có thể thay đổi.  
✅ Khi muốn **tránh trùng lặp mã** bằng cách định nghĩa chung một bộ khung thuật toán.  
✅ Khi cần **áp dụng nguyên tắc Open/Closed**, giúp dễ mở rộng mà không thay đổi mã nguồn của lớp cha.

📌 **Ví dụ thực tế:**  
🔹 **Các bước pha trà/cà phê**: Dù là trà hay cà phê, chúng đều có các bước chung như **đun nước, pha, rót vào cốc, thêm hương liệu**.  
🔹 **Hệ thống thanh toán**: Dù là PayPal, Visa hay Bitcoin, các bước chung gồm **xác thực, trừ tiền, thông báo**.  
🔹 **Trình biên dịch**: Bất kỳ ngôn ngữ nào cũng cần các bước chung như **phân tích cú pháp, biên dịch, tối ưu hóa, sinh mã máy**.

---

## **3. Cấu trúc của Template Method Pattern**
Template Method Pattern có **3 thành phần chính**:

| **Thành phần** | **Vai trò** |
|---------------|------------|
| **AbstractClass (Lớp trừu tượng)** | Xác định thuật toán tổng quát, cung cấp phương thức `templateMethod()`. |
| **ConcreteClass (Lớp con)** | Cài đặt cụ thể cho các bước trừu tượng. |
| **Client (Người dùng)** | Gọi phương thức `templateMethod()` để thực thi thuật toán. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Lớp cha định nghĩa bộ khung** của thuật toán bằng một phương thức `final templateMethod()`.  
2️⃣ **Một số bước trong thuật toán là trừu tượng**, được các lớp con triển khai.  
3️⃣ **Client chỉ cần gọi `templateMethod()`**, lớp con tự động thực thi thuật toán đúng cách.

---

## **4. Triển khai Template Method Pattern trong Java**
### **Ví dụ: Pha Trà và Pha Cà Phê**
Hệ thống hỗ trợ **2 loại đồ uống**:
- **Trà (`Tea`)**
- **Cà phê (`Coffee`)**

Cả hai đều có các bước chung:
1. Đun nước
2. Pha đồ uống
3. Rót vào cốc
4. Thêm hương liệu (tùy chỉnh)

---

### **Bước 1: Tạo lớp trừu tượng `BeverageTemplate`**
```java
abstract class BeverageTemplate {
    
    // Template Method (final để không bị override)
    public final void prepareBeverage() {
        boilWater();
        brew();
        pourInCup();
        if (wantCondiments()) { // Hook method
            addCondiments();
        }
    }

    // Các bước cố định
    private void boilWater() {
        System.out.println("🔥 Đun sôi nước...");
    }

    private void pourInCup() {
        System.out.println("☕ Rót đồ uống vào cốc...");
    }

    // Các bước thay đổi tùy từng loại đồ uống
    protected abstract void brew();
    protected abstract void addCondiments();

    // Hook method - có thể override nếu cần
    protected boolean wantCondiments() {
        return true;
    }
}
```
🔹 **`templateMethod()` đã xác định luồng chung**: đun nước → pha → rót → thêm hương liệu.  
🔹 **`brew()` và `addCondiments()` là phương thức trừu tượng**, phải được cài đặt bởi lớp con.  
🔹 **Hook method (`wantCondiments()`) giúp tùy chỉnh có thêm hương liệu hay không.**

---

### **Bước 2: Cài đặt lớp `Tea`**
```java
class Tea extends BeverageTemplate {
    @Override
    protected void brew() {
        System.out.println("🍵 Ngâm trà trong nước nóng...");
    }

    @Override
    protected void addCondiments() {
        System.out.println("🍋 Thêm chanh và mật ong...");
    }
}
```
🔹 `Tea` triển khai các bước **pha trà và thêm chanh, mật ong**.

---

### **Bước 3: Cài đặt lớp `Coffee`**
```java
class Coffee extends BeverageTemplate {
    @Override
    protected void brew() {
        System.out.println("☕ Pha cà phê bằng bộ lọc...");
    }

    @Override
    protected void addCondiments() {
        System.out.println("🥛 Thêm đường và sữa...");
    }

    @Override
    protected boolean wantCondiments() {
        return true; // Có thể thay đổi theo sở thích
    }
}
```
🔹 `Coffee` triển khai các bước **pha cà phê và thêm đường, sữa**.  
🔹 **`wantCondiments()` có thể được override nếu người dùng không muốn thêm gì.**

---

### **Bước 4: Kiểm thử Template Method Pattern**
```java
public class TemplateMethodPatternDemo {
    public static void main(String[] args) {
        System.out.println("📌 Pha trà:");
        BeverageTemplate tea = new Tea();
        tea.prepareBeverage();

        System.out.println("\n📌 Pha cà phê:");
        BeverageTemplate coffee = new Coffee();
        coffee.prepareBeverage();
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
📌 Pha trà:
🔥 Đun sôi nước...
🍵 Ngâm trà trong nước nóng...
☕ Rót đồ uống vào cốc...
🍋 Thêm chanh và mật ong...

📌 Pha cà phê:
🔥 Đun sôi nước...
☕ Pha cà phê bằng bộ lọc...
☕ Rót đồ uống vào cốc...
🥛 Thêm đường và sữa...
```
📌 **Nhận xét:**
- Cả hai đều tuân theo **bộ khung chung**, nhưng **các bước chi tiết do từng lớp con quyết định**.
- **Dễ dàng thêm đồ uống mới**, chỉ cần tạo một lớp mới kế thừa `BeverageTemplate`.

---

## **6. Ứng dụng thực tế của Template Method Pattern**
✅ **Hệ thống thanh toán**: Mỗi loại thanh toán (PayPal, Visa, Bitcoin) có thể có các bước xác thực khác nhau.  
✅ **Trình biên dịch ngôn ngữ lập trình**: Mọi ngôn ngữ đều cần phân tích cú pháp, tối ưu hóa, sinh mã máy.  
✅ **Quy trình sản xuất**: Mọi dây chuyền sản xuất đều có chung các bước như kiểm tra nguyên liệu, chế biến, đóng gói.

---

## **7. Ưu điểm & Nhược điểm của Template Method Pattern**
✅ **Ưu điểm:**  
✔️ **Loại bỏ trùng lặp mã**, giúp code sạch hơn.  
✔️ **Dễ mở rộng**, chỉ cần thêm lớp con để thay đổi hành vi.  
✔️ **Tuân theo nguyên tắc Open/Closed** (SOLID), giúp dễ bảo trì.

❌ **Nhược điểm:**  
⚠️ **Bắt buộc sử dụng kế thừa**, không linh hoạt bằng Strategy Pattern.  
⚠️ **Khó đọc nếu có quá nhiều bước trong thuật toán**.

---

## **8. Bài tập thực hành**
Hãy triển khai một hệ thống **quản lý quy trình đăng ký tài khoản**, gồm các bước:
1. **Nhập thông tin**
2. **Xác thực email/SĐT**
3. **Duyệt tài khoản (Admin duyệt hoặc tự động duyệt)**
4. **Kích hoạt tài khoản**

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀