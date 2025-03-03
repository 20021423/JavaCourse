# **Decorator Design Pattern trong Java**

---

## **1. Decorator Pattern là gì?**
**Decorator Pattern** là một **mẫu thiết kế cấu trúc (Structural Pattern)** cho phép **mở rộng chức năng của một đối tượng động (runtime) mà không làm thay đổi mã nguồn của nó**.

💡 **Ý tưởng chính:**
- **Bọc (wrap) đối tượng gốc bên trong một lớp khác** để mở rộng hoặc thay đổi hành vi của nó.
- **Không làm thay đổi mã nguồn của lớp gốc**, giúp dễ dàng mở rộng hệ thống mà không vi phạm **nguyên tắc Open/Closed (SOLID)**.

---

## **2. Khi nào sử dụng Decorator Pattern?**
✅ Khi cần **mở rộng chức năng của một đối tượng mà không thay đổi lớp gốc**.  
✅ Khi muốn **thêm tính năng theo cách linh hoạt**, không bị ràng buộc vào kế thừa (Inheritance).  
✅ Khi muốn **kết hợp nhiều hành vi bổ sung (Multiple Wrappers)** một cách dễ dàng.

📌 **Ví dụ thực tế:**  
🔹 **Hệ thống đồ uống (Coffee Shop)**: Bắt đầu với một cà phê đơn giản, sau đó thêm đường, sữa, caramel.  
🔹 **Hệ thống UI (Graphical User Interface)**: Một `TextBox` có thể được thêm `Border`, `Shadow`, `Scrollbar`.  
🔹 **Bộ mã hóa dữ liệu (Data Encryption)**: Mã hóa dữ liệu bằng nhiều lớp bảo mật khác nhau.  
🔹 **Hệ thống Logger**: Ghi log ra file, console, hoặc cơ sở dữ liệu mà không sửa đổi lớp Logger gốc.

---

## **3. Cấu trúc của Decorator Pattern**
Decorator Pattern có **4 thành phần chính**:

| **Thành phần** | **Vai trò** |
|--------------|------------|
| **Component (interface)** | Định nghĩa phương thức chung cho cả đối tượng gốc và decorator. |
| **ConcreteComponent** | Cài đặt mặc định của `Component`, có thể được mở rộng. |
| **Decorator (abstract class)** | Lớp bọc (`Wrapper`) chứa một `Component`, có thể thêm hành vi mới. |
| **ConcreteDecorator** | Mở rộng `Decorator`, thêm hành vi bổ sung. |

📌 **Quy tắc hoạt động:**  
1️⃣ `ConcreteComponent` là đối tượng gốc, chứa logic mặc định.  
2️⃣ `Decorator` bọc `ConcreteComponent`, mở rộng hoặc thay đổi hành vi.  
3️⃣ **Client có thể sử dụng `ConcreteComponent` hoặc `Decorator` một cách giống nhau**, không cần biết chi tiết bên trong.

---

## **4. Triển khai Decorator Pattern trong Java**
### **Ví dụ: Hệ thống đặt hàng cà phê**
Hệ thống hỗ trợ **tạo một tách cà phê** với **các thành phần bổ sung tùy chọn**:
- **Loại cơ bản**: `SimpleCoffee`.
- **Bổ sung thêm**: `Milk`, `Sugar`, `Caramel`.

---

### **Bước 1: Tạo `Component` (Coffee)**
```java
interface Coffee {
    String getDescription();
    double getCost();
}
```
🔹 **Mọi loại cà phê phải có mô tả (`getDescription()`) và giá tiền (`getCost()`).**

---

### **Bước 2: Cài đặt `ConcreteComponent` (Cà phê cơ bản)**
```java
class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Cà phê đen";
    }

    @Override
    public double getCost() {
        return 20.0;
    }
}
```
🔹 **`SimpleCoffee` là một loại cà phê đơn giản, chưa có bổ sung.**

---

### **Bước 3: Tạo `Decorator` (Lớp bọc chung)**
```java
abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription();
    }

    @Override
    public double getCost() {
        return coffee.getCost();
    }
}
```
🔹 **`CoffeeDecorator` bọc `Coffee`, giúp mở rộng hành vi mà không cần sửa đổi `SimpleCoffee`.**

---

### **Bước 4: Cài đặt `ConcreteDecorator` (Thêm thành phần vào cà phê)**
#### **Thêm sữa (`Milk`)**
```java
class Milk extends CoffeeDecorator {
    public Milk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Sữa";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 5.0;
    }
}
```

#### **Thêm đường (`Sugar`)**
```java
class Sugar extends CoffeeDecorator {
    public Sugar(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Đường";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 2.0;
    }
}
```

#### **Thêm caramel (`Caramel`)**
```java
class Caramel extends CoffeeDecorator {
    public Caramel(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Caramel";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 7.0;
    }
}
```
🔹 **Mỗi `Decorator` mở rộng `CoffeeDecorator`, giúp thêm hương vị mà không ảnh hưởng `SimpleCoffee`.**

---

### **Bước 5: Kiểm thử Decorator Pattern**
```java
public class DecoratorPatternDemo {
    public static void main(String[] args) {
        // Cà phê đen
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " - Giá: " + coffee.getCost() + "K");

        // Cà phê có sữa
        Coffee milkCoffee = new Milk(coffee);
        System.out.println(milkCoffee.getDescription() + " - Giá: " + milkCoffee.getCost() + "K");

        // Cà phê có sữa + đường
        Coffee milkSugarCoffee = new Sugar(milkCoffee);
        System.out.println(milkSugarCoffee.getDescription() + " - Giá: " + milkSugarCoffee.getCost() + "K");

        // Cà phê có sữa + đường + caramel
        Coffee specialCoffee = new Caramel(milkSugarCoffee);
        System.out.println(specialCoffee.getDescription() + " - Giá: " + specialCoffee.getCost() + "K");
    }
}
```

---

## **6. Kết quả khi chạy chương trình**
```
Cà phê đen - Giá: 20.0K
Cà phê đen, Sữa - Giá: 25.0K
Cà phê đen, Sữa, Đường - Giá: 27.0K
Cà phê đen, Sữa, Đường, Caramel - Giá: 34.0K
```
📌 **Nhận xét:**
- **Dễ dàng thêm nhiều thành phần khác nhau mà không sửa đổi mã nguồn cũ**.
- **Mỗi thành phần có thể được kết hợp linh hoạt**, giúp tạo nhiều loại cà phê khác nhau.

---

## **7. Ứng dụng thực tế của Decorator Pattern**
✅ **Hệ thống quán cà phê (Coffee Shop)** ☕ (Thêm đường, sữa, caramel).  
✅ **Giao diện UI (Graphical User Interface)** 🎨 (Thêm Border, Scrollbar, Shadow).  
✅ **Hệ thống Logger** 📝 (Ghi log ra file, console, database).  
✅ **Hệ thống mã hóa dữ liệu** 🔒 (Mã hóa bằng AES, RSA, GZIP).

---

## **8. Ưu điểm & Nhược điểm của Decorator Pattern**
✅ **Ưu điểm:**  
✔️ **Linh hoạt**, có thể kết hợp nhiều decorator để mở rộng chức năng.  
✔️ **Tuân theo nguyên tắc Open/Closed (SOLID)**, dễ mở rộng mà không sửa mã nguồn cũ.  
✔️ **Thay thế được kế thừa**, giúp giảm số lượng class con.

❌ **Nhược điểm:**  
⚠️ **Tạo nhiều class**, có thể làm tăng độ phức tạp.  
⚠️ **Gọi phương thức nhiều lần**, có thể làm giảm hiệu suất.

---

## **9. Bài tập thực hành**
Hãy triển khai một **hệ thống Logger (`LoggerDecorator`)**, gồm:
1. **Ghi log ra console (`ConsoleLogger`)**.
2. **Ghi log ra file (`FileLogger`)**.
3. **Ghi log với timestamp (`TimestampLogger`)**.

👉 **Mục tiêu**: Kết hợp các decorator để ghi log linh hoạt. 🚀