# **Interpreter Design Pattern trong Java**

---

## **1. Interpreter Pattern là gì?**
**Interpreter Pattern** là một **mẫu thiết kế hành vi (Behavioral Pattern)** dùng để **đánh giá và xử lý ngôn ngữ hoặc biểu thức**.

💡 **Ý tưởng chính:**
- **Định nghĩa một ngữ pháp (Grammar)** cho một ngôn ngữ hoặc tập hợp quy tắc.
- **Xây dựng một trình thông dịch (Interpreter)** để xử lý các biểu thức dựa trên ngữ pháp đó.

---

## **2. Khi nào sử dụng Interpreter Pattern?**
✅ Khi cần **xây dựng và xử lý một ngôn ngữ đặc biệt** (DSL - Domain-Specific Language).  
✅ Khi phải **biên dịch hoặc đánh giá biểu thức toán học hoặc logic**.  
✅ Khi hệ thống có **cấu trúc dữ liệu dạng cây**, trong đó mỗi nút là một phần của biểu thức.

📌 **Ví dụ thực tế:**  
🔹 **Máy tính bỏ túi**: Xử lý biểu thức toán học như `5 + 3 * 2`.  
🔹 **SQL Parser**: Phân tích và thực thi truy vấn SQL.  
🔹 **Biểu thức Boolean**: Kiểm tra điều kiện trong rule engine (`AND`, `OR`, `NOT`).  
🔹 **Regular Expression (Regex)**: Kiểm tra chuỗi khớp với mẫu đã định nghĩa.

---

## **3. Cấu trúc của Interpreter Pattern**
Interpreter Pattern có **4 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **AbstractExpression (interface)** | Định nghĩa phương thức `interpret()` cho tất cả biểu thức. |
| **TerminalExpression** | Xử lý các biểu thức đơn giản, không thể phân tách. |
| **NonTerminalExpression** | Kết hợp nhiều `Expression` để tạo thành biểu thức phức tạp. |
| **Context** | Lưu trữ thông tin cần thiết để đánh giá biểu thức. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Tách biểu thức thành các phần nhỏ (TerminalExpression, NonTerminalExpression)**.  
2️⃣ **Tạo cây biểu thức**, trong đó mỗi nút là một `Expression`.  
3️⃣ **Gọi `interpret()` trên cây**, mỗi phần sẽ được xử lý theo ngữ nghĩa riêng.

---

## **4. Triển khai Interpreter Pattern trong Java**
### **Ví dụ: Bộ tính toán đơn giản (`Expression Evaluator`)**
**Yêu cầu:**
- Xây dựng trình thông dịch để tính toán biểu thức đơn giản gồm **các số nguyên, phép cộng (`+`) và phép trừ (`-`)**.
- Biểu thức: `"5 + 3 - 2"` → **Kết quả: `6`**.

---

### **Bước 1: Tạo interface `Expression`**
```java
interface Expression {
    int interpret();
}
```
🔹 **Mỗi biểu thức phải có phương thức `interpret()` để tính giá trị.**

---

### **Bước 2: Cài đặt `TerminalExpression` (Biểu thức số nguyên)**
```java
class NumberExpression implements Expression {
    private int number;

    public NumberExpression(int number) {
        this.number = number;
    }

    @Override
    public int interpret() {
        return number;
    }
}
```
🔹 **`NumberExpression` đại diện cho các số nguyên trong biểu thức.**

---

### **Bước 3: Cài đặt `NonTerminalExpression` (Biểu thức cộng và trừ)**
#### **Phép cộng (`AdditionExpression`)**
```java
class AdditionExpression implements Expression {
    private Expression left;
    private Expression right;

    public AdditionExpression(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int interpret() {
        return left.interpret() + right.interpret();
    }
}
```

#### **Phép trừ (`SubtractionExpression`)**
```java
class SubtractionExpression implements Expression {
    private Expression left;
    private Expression right;

    public SubtractionExpression(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int interpret() {
        return left.interpret() - right.interpret();
    }
}
```
🔹 **`AdditionExpression` và `SubtractionExpression` kết hợp hai biểu thức để thực hiện phép toán.**

---

### **Bước 4: Kiểm thử Interpreter Pattern**
```java
public class InterpreterPatternDemo {
    public static void main(String[] args) {
        // Biểu thức: 5 + 3 - 2
        Expression num1 = new NumberExpression(5);
        Expression num2 = new NumberExpression(3);
        Expression num3 = new NumberExpression(2);

        // 5 + 3
        Expression add = new AdditionExpression(num1, num2);
        // (5 + 3) - 2
        Expression result = new SubtractionExpression(add, num3);

        // Kết quả cuối cùng
        System.out.println("Kết quả: " + result.interpret()); // Output: 6
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
Kết quả: 6
```
📌 **Nhận xét:**
- **Biểu thức được chia thành cây toán học (`Expression Tree`)**.
- **Dễ dàng mở rộng**, chỉ cần thêm `MultiplicationExpression`, `DivisionExpression` nếu cần.

---

## **6. Ứng dụng thực tế của Interpreter Pattern**
✅ **Máy tính bỏ túi** 🧮 (xử lý biểu thức toán học).  
✅ **Trình phân tích cú pháp SQL** 📊 (chuyển đổi và thực thi truy vấn SQL).  
✅ **Xử lý biểu thức Boolean** 🔍 (`AND`, `OR`, `NOT`).  
✅ **Công cụ kiểm tra cú pháp (Regex Engine)** 📝.

---

## **7. Ưu điểm & Nhược điểm của Interpreter Pattern**
✅ **Ưu điểm:**  
✔️ **Dễ mở rộng**, chỉ cần thêm lớp `Expression` mới để hỗ trợ toán tử mới.  
✔️ **Mô hình hóa ngữ pháp tự nhiên**, giúp xử lý dễ dàng các DSL (Domain-Specific Language).  
✔️ **Tách biệt logic đánh giá biểu thức**, giúp mã nguồn dễ bảo trì.

❌ **Nhược điểm:**  
⚠️ **Không phù hợp với các ngôn ngữ phức tạp**, vì mã nguồn sẽ rất cồng kềnh.  
⚠️ **Hiệu suất kém** nếu xử lý biểu thức lớn, vì tạo nhiều đối tượng trong cây cú pháp.  
⚠️ **Khó tối ưu**, vì mỗi toán tử là một đối tượng riêng biệt.

---

## **8. Bài tập thực hành**
Hãy triển khai một **bộ xử lý biểu thức Boolean**, hỗ trợ các toán tử:
1. **AND (`&&`)**
2. **OR (`||`)**
3. **NOT (`!`)**

👉 **Mục tiêu**: Xử lý biểu thức như **`true && false || !false`** và trả về kết quả đúng/sai.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀