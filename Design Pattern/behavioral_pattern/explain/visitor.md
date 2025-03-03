# **Visitor Design Pattern trong Java**

---

## **1. Visitor Pattern là gì?**
**Visitor Pattern** là một **mẫu thiết kế hành vi (Behavioral Pattern)** cho phép thêm các thao tác mới vào một nhóm đối tượng mà **không làm thay đổi mã nguồn của các lớp đó**.

💡 **Ý tưởng chính:**
- **Tách riêng thuật toán xử lý ra khỏi các lớp đối tượng**, giúp dễ mở rộng mà không làm thay đổi cấu trúc lớp.
- **Dùng một "Visitor" để thực hiện thao tác trên nhiều đối tượng khác nhau**, giúp tránh lặp code.

---

## **2. Khi nào sử dụng Visitor Pattern?**
✅ Khi cần **thêm chức năng mới cho một nhóm lớp** mà không muốn sửa đổi mã nguồn của chúng.  
✅ Khi có **nhiều đối tượng khác nhau nhưng cần xử lý tương tự**, giúp tránh trùng lặp mã.  
✅ Khi cần **áp dụng nguyên tắc Open/Closed** (SOLID), để mở rộng chức năng mà không sửa lớp gốc.

📌 **Ví dụ thực tế:**  
🔹 **Hệ thống thuế**: Mỗi loại sản phẩm (điện tử, thực phẩm, quần áo) có công thức tính thuế khác nhau.  
🔹 **Trình biên dịch**: Một trình biên dịch có thể duyệt qua các thành phần mã nguồn như biến, vòng lặp, biểu thức, câu lệnh.  
🔹 **Hệ thống đồ họa**: Vẽ hình tròn, hình vuông, tam giác nhưng không cần sửa đổi lớp của các hình này.

---

## **3. Cấu trúc của Visitor Pattern**
Visitor Pattern có **4 thành phần chính**:

| **Thành phần**   | **Vai trò** |
|----------------|------------|
| **Visitor (interface)** | Định nghĩa các phương thức thao tác trên các lớp khác nhau. |
| **ConcreteVisitor** | Cài đặt cụ thể của Visitor, thực hiện xử lý trên từng loại đối tượng. |
| **Element (interface)** | Định nghĩa phương thức `accept(Visitor v)` để nhận Visitor. |
| **ConcreteElement** | Các lớp cụ thể thực hiện phương thức `accept()` để gọi Visitor. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Visitor có các phương thức tương ứng với từng loại Element**.  
2️⃣ **Element gọi `accept(Visitor v)`, truyền chính nó vào Visitor**.  
3️⃣ **Visitor xử lý Element mà không cần sửa đổi mã nguồn của Element**.

---

## **4. Triển khai Visitor Pattern trong Java**
### **Ví dụ: Hệ thống tính thuế cho sản phẩm**
Hệ thống hỗ trợ **3 loại sản phẩm**:
- **Sản phẩm điện tử (`Electronics`)**
- **Thực phẩm (`Food`)**
- **Quần áo (`Clothing`)**

---

### **Bước 1: Tạo interface `Visitor`**
```java
interface Visitor {
    void visit(Electronics electronics);
    void visit(Food food);
    void visit(Clothing clothing);
}
```
🔹 **`Visitor` định nghĩa các phương thức xử lý cho từng loại sản phẩm.**

---

### **Bước 2: Tạo interface `Element` (Product)**
```java
interface Product {
    void accept(Visitor visitor);
}
```
🔹 **Mỗi sản phẩm phải có phương thức `accept()` để nhận Visitor.**

---

### **Bước 3: Tạo các lớp `ConcreteElement` (Các loại sản phẩm)**

#### **Sản phẩm điện tử**
```java
class Electronics implements Product {
    private double price;

    public Electronics(double price) {
        this.price = price;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

#### **Thực phẩm**
```java
class Food implements Product {
    private double price;

    public Food(double price) {
        this.price = price;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

#### **Quần áo**
```java
class Clothing implements Product {
    private double price;

    public Clothing(double price) {
        this.price = price;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

🔹 **Mỗi sản phẩm có phương thức `accept(Visitor v)`, giúp Visitor xử lý mà không cần thay đổi mã nguồn của lớp.**

---

### **Bước 4: Tạo `ConcreteVisitor` để tính thuế**
```java
class TaxVisitor implements Visitor {
    @Override
    public void visit(Electronics electronics) {
        double tax = electronics.getPrice() * 0.18;
        System.out.println("💻 Thuế sản phẩm điện tử: $" + tax);
    }

    @Override
    public void visit(Food food) {
        double tax = food.getPrice() * 0.05;
        System.out.println("🍎 Thuế thực phẩm: $" + tax);
    }

    @Override
    public void visit(Clothing clothing) {
        double tax = clothing.getPrice() * 0.12;
        System.out.println("👕 Thuế quần áo: $" + tax);
    }
}
```
🔹 **Mỗi loại sản phẩm có công thức tính thuế khác nhau, nhưng mã nguồn của `Product` không thay đổi.**

---

### **Bước 5: Kiểm thử Visitor Pattern**
```java
import java.util.ArrayList;
import java.util.List;

public class VisitorPatternDemo {
    public static void main(String[] args) {
        List<Product> products = new ArrayList<>();
        products.add(new Electronics(1000));
        products.add(new Food(200));
        products.add(new Clothing(500));

        Visitor taxCalculator = new TaxVisitor();

        for (Product product : products) {
            product.accept(taxCalculator);
        }
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
💻 Thuế sản phẩm điện tử: $180.0
🍎 Thuế thực phẩm: $10.0
👕 Thuế quần áo: $60.0
```
📌 **Nhận xét:**
- **Dễ dàng thêm loại sản phẩm mới** mà không cần sửa đổi `Visitor`.
- **Tách biệt hoàn toàn thuật toán tính thuế ra khỏi các lớp sản phẩm**.
- **Nếu muốn thêm tính năng giảm giá, chỉ cần tạo một Visitor khác (DiscountVisitor).**

---

## **6. Ứng dụng thực tế của Visitor Pattern**
✅ **Hệ thống tính thuế**: Mỗi loại sản phẩm có công thức tính thuế riêng.  
✅ **Trình biên dịch**: Kiểm tra cú pháp, tối ưu hóa mã, tạo mã máy mà không cần thay đổi mã gốc.  
✅ **Hệ thống kiểm tra an toàn**: Duyệt qua các tệp tin để phát hiện virus.  
✅ **Hệ thống đồ họa**: Xuất hình ảnh dưới nhiều định dạng khác nhau (PNG, JPG, SVG).

---

## **7. Ưu điểm & Nhược điểm của Visitor Pattern**
✅ **Ưu điểm:**  
✔️ **Dễ dàng thêm chức năng mới mà không sửa mã nguồn của lớp cũ**.  
✔️ **Tăng tính mở rộng**, giúp tuân theo nguyên tắc Open/Closed (SOLID).  
✔️ **Tách biệt thuật toán khỏi dữ liệu**, giúp mã nguồn sạch hơn.

❌ **Nhược điểm:**  
⚠️ **Khó mở rộng nếu có quá nhiều loại đối tượng**, vì phải sửa Visitor mỗi khi thêm lớp mới.  
⚠️ **Các lớp phải có phương thức `accept()`**, có thể không phù hợp với tất cả hệ thống.

---

## **8. Bài tập thực hành**
Hãy triển khai một hệ thống **kiểm tra bảo mật tập tin**, gồm các loại tập tin:
1. **File hình ảnh (ImageFile)**
2. **File văn bản (TextFile)**
3. **File thực thi (ExecutableFile)**

👉 **Mục tiêu**: Kiểm tra virus trong từng loại file mà không cần thay đổi mã nguồn của lớp file.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀