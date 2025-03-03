# **Chain of Responsibility Design Pattern trong Java**

---

## **1. Chain of Responsibility Pattern là gì?**
**Chain of Responsibility Pattern** là một **mẫu thiết kế hành vi (Behavioral Pattern)** cho phép **gửi yêu cầu qua một chuỗi các đối tượng xử lý (handlers)** cho đến khi có một handler xử lý được yêu cầu.

💡 **Ý tưởng chính:**
- **Tách rời người gửi yêu cầu và người xử lý yêu cầu**, giúp hệ thống linh hoạt hơn.
- **Mỗi handler quyết định có xử lý yêu cầu không**, hoặc chuyển tiếp cho handler tiếp theo.

---

## **2. Khi nào sử dụng Chain of Responsibility Pattern?**
✅ Khi có **nhiều đối tượng có thể xử lý yêu cầu**, nhưng không biết đối tượng nào sẽ xử lý.  
✅ Khi muốn **giảm sự phụ thuộc giữa người gửi yêu cầu và người xử lý**.  
✅ Khi cần **một hệ thống linh hoạt**, dễ mở rộng, có thể thêm/bớt handler mà không ảnh hưởng đến hệ thống.

📌 **Ví dụ thực tế:**  
🔹 **Hệ thống xử lý yêu cầu hỗ trợ khách hàng** (Level 1 → Level 2 → Level 3).  
🔹 **Bộ lọc đăng nhập và bảo mật** (Xác thực → Kiểm tra quyền → Ghi log).  
🔹 **Duyệt đơn hàng** (Nhân viên → Trưởng phòng → Giám đốc).  
🔹 **Tường lửa (Firewall) kiểm tra gói tin** (Spam → Virus → Chính sách mạng).

---

## **3. Cấu trúc của Chain of Responsibility Pattern**
Chain of Responsibility Pattern có **3 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Handler (interface/abstract class)** | Định nghĩa phương thức xử lý yêu cầu và thiết lập handler tiếp theo. |
| **ConcreteHandler** | Cài đặt cụ thể, quyết định có xử lý yêu cầu hay chuyển tiếp nó. |
| **Client** | Gửi yêu cầu đến handler đầu tiên trong chuỗi. |

📌 **Quy tắc hoạt động:**  
1️⃣ `Client` gửi yêu cầu đến handler đầu tiên.  
2️⃣ Handler kiểm tra xem có xử lý được không.  
3️⃣ Nếu xử lý được, nó hoàn tất yêu cầu; nếu không, chuyển yêu cầu đến handler tiếp theo.  
4️⃣ Quá trình tiếp tục cho đến khi có một handler xử lý được hoặc hết chuỗi.

---

## **4. Triển khai Chain of Responsibility Pattern trong Java**
### **Ví dụ: Hệ thống phê duyệt đơn hàng theo cấp bậc**
Hệ thống có **3 cấp xử lý**:
- **Nhân viên (`Employee`)** có thể duyệt đơn hàng dưới **$1000**.
- **Trưởng phòng (`Manager`)** có thể duyệt đơn hàng dưới **$5000**.
- **Giám đốc (`Director`)** có thể duyệt đơn hàng **mọi giá trị**.

---

### **Bước 1: Tạo abstract class `Approver` (Handler)**
```java
abstract class Approver {
    protected Approver nextApprover; // Handler tiếp theo

    public void setNextApprover(Approver nextApprover) {
        this.nextApprover = nextApprover;
    }

    public abstract void processRequest(PurchaseRequest request);
}
```
🔹 **Mỗi `Approver` có một handler tiếp theo (`nextApprover`) để chuyển yêu cầu nếu cần.**

---

### **Bước 2: Tạo lớp `PurchaseRequest` (Yêu cầu mua hàng)**
```java
class PurchaseRequest {
    private int amount;

    public PurchaseRequest(int amount) {
        this.amount = amount;
    }

    public int getAmount() {
        return amount;
    }
}
```
🔹 **`PurchaseRequest` chứa số tiền cần duyệt.**

---

### **Bước 3: Cài đặt các `ConcreteHandler` (Các cấp duyệt đơn hàng)**
#### **Nhân viên (`Employee`)**
```java
class Employee extends Approver {
    @Override
    public void processRequest(PurchaseRequest request) {
        if (request.getAmount() < 1000) {
            System.out.println("✅ Nhân viên đã duyệt đơn hàng $" + request.getAmount());
        } else if (nextApprover != null) {
            nextApprover.processRequest(request);
        }
    }
}
```
🔹 **Nhân viên có thể duyệt đơn hàng dưới $1000, nếu lớn hơn thì chuyển tiếp.**

#### **Trưởng phòng (`Manager`)**
```java
class Manager extends Approver {
    @Override
    public void processRequest(PurchaseRequest request) {
        if (request.getAmount() < 5000) {
            System.out.println("✅ Trưởng phòng đã duyệt đơn hàng $" + request.getAmount());
        } else if (nextApprover != null) {
            nextApprover.processRequest(request);
        }
    }
}
```
🔹 **Trưởng phòng có thể duyệt đơn hàng dưới $5000, nếu lớn hơn thì chuyển tiếp.**

#### **Giám đốc (`Director`)**
```java
class Director extends Approver {
    @Override
    public void processRequest(PurchaseRequest request) {
        System.out.println("✅ Giám đốc đã duyệt đơn hàng $" + request.getAmount());
    }
}
```
🔹 **Giám đốc có thể duyệt mọi đơn hàng.**

---

### **Bước 4: Kiểm thử Chain of Responsibility Pattern**
```java
public class ChainOfResponsibilityDemo {
    public static void main(String[] args) {
        Approver employee = new Employee();
        Approver manager = new Manager();
        Approver director = new Director();

        // Thiết lập chuỗi xử lý
        employee.setNextApprover(manager);
        manager.setNextApprover(director);

        // Tạo yêu cầu mua hàng
        PurchaseRequest request1 = new PurchaseRequest(500);
        PurchaseRequest request2 = new PurchaseRequest(3000);
        PurchaseRequest request3 = new PurchaseRequest(10000);

        // Gửi yêu cầu đến handler đầu tiên (Nhân viên)
        employee.processRequest(request1);  // Nhân viên duyệt
        employee.processRequest(request2);  // Trưởng phòng duyệt
        employee.processRequest(request3);  // Giám đốc duyệt
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
✅ Nhân viên đã duyệt đơn hàng $500
✅ Trưởng phòng đã duyệt đơn hàng $3000
✅ Giám đốc đã duyệt đơn hàng $10000
```
📌 **Nhận xét:**
- **Yêu cầu nhỏ được xử lý bởi handler đầu tiên (`Employee`).**
- **Yêu cầu lớn hơn được chuyển đến handler tiếp theo (`Manager`, `Director`).**
- **Dễ dàng thêm một cấp phê duyệt mới mà không ảnh hưởng đến mã nguồn hiện có.**

---

## **6. Ứng dụng thực tế của Chain of Responsibility Pattern**
✅ **Xử lý yêu cầu hỗ trợ khách hàng** 🛠 (Chăm sóc khách hàng → Kỹ thuật → Quản lý).  
✅ **Quy trình phê duyệt tài liệu** 📄 (Nhân viên → Trưởng phòng → Giám đốc).  
✅ **Middleware kiểm tra bảo mật** 🔒 (Xác thực → Phân quyền → Ghi log).  
✅ **Hệ thống xử lý đơn hàng** 🛒 (Tính phí → Kiểm tra kho → Xác nhận đơn hàng).

---

## **7. Ưu điểm & Nhược điểm của Chain of Responsibility Pattern**
✅ **Ưu điểm:**  
✔️ **Giảm sự phụ thuộc giữa Client và Handler**, giúp mã nguồn dễ mở rộng.  
✔️ **Dễ thêm hoặc sửa handler mới**, không làm ảnh hưởng đến hệ thống.  
✔️ **Dễ kiểm soát luồng xử lý**, có thể thêm điều kiện dừng xử lý.

❌ **Nhược điểm:**  
⚠️ **Có thể không có handler nào xử lý yêu cầu**, cần đảm bảo có handler cuối cùng.  
⚠️ **Có thể làm tăng độ phức tạp**, nếu chuỗi xử lý quá dài.

---

## **8. Bài tập thực hành**
Hãy triển khai một hệ thống **kiểm duyệt nội dung**, gồm:
1. **Kiểm tra ngôn từ thô tục (Profanity Filter)**.
2. **Kiểm tra nội dung quảng cáo (Spam Filter)**.
3. **Kiểm tra bản quyền (Copyright Filter)**.

👉 **Mục tiêu**: Nội dung phải vượt qua tất cả bộ lọc trước khi được đăng.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀