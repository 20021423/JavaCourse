## **Observer Design Pattern trong Java**

### **1. Observer Pattern là gì?**
Observer Pattern là một **mẫu thiết kế hành vi (Behavioral Pattern)**, giúp một đối tượng (**Subject**) có thể **thông báo** đến nhiều đối tượng khác (**Observers**) khi có sự thay đổi.

💡 **Mô hình: Một-Nhiều**
- **Một Subject** có thể có **nhiều Observer** đăng ký nhận thông báo.
- Khi **Subject thay đổi**, nó **tự động thông báo** đến tất cả các Observer đã đăng ký.

---

### **2. Khi nào sử dụng Observer Pattern?**
✅ Khi một đối tượng thay đổi trạng thái, **cần cập nhật nhiều đối tượng khác mà không cần kết nối trực tiếp**.
✅ Khi muốn **loại bỏ việc kiểm tra liên tục** (polling) từ Observer, giúp tiết kiệm tài nguyên.
✅ Khi muốn **tăng tính linh hoạt**, để có thể thêm hoặc xóa Observer mà không ảnh hưởng đến Subject.

📌 **Ví dụ thực tế**:
🔹 **Hệ thống thông báo sự kiện** (khi admin đăng bài mới, tất cả người dùng đều nhận thông báo).
🔹 **Mô hình Publisher-Subscriber** (báo chí, blog, YouTube Subscriptions).
🔹 **Sàn giao dịch chứng khoán** (khi giá cổ phiếu thay đổi, tất cả nhà đầu tư sẽ được cập nhật).

---

### **3. Cấu trúc của Observer Pattern**
Observer Pattern có **3 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Observer (interface)** | Định nghĩa phương thức `update()` để nhận thông báo từ Subject. |
| **ConcreteObserver** | Hiện thực cụ thể của Observer, nhận và xử lý thông báo từ Subject. |
| **Subject (interface)** | Quản lý danh sách Observer, cho phép đăng ký/hủy đăng ký và thông báo. |
| **ConcreteSubject** | Cài đặt Subject, chứa dữ liệu và thông báo Observer khi có thay đổi. |

---

### **4. Triển khai Observer Pattern trong Java**
#### **Bước 1: Tạo interface `Observer`**
```java
interface Observer {
    void update(String message);
}
```
🔹 **Mỗi Observer phải có phương thức `update()` để nhận thông báo.**

---

#### **Bước 2: Tạo interface `Subject`**
```java
import java.util.ArrayList;
import java.util.List;

interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers(String message);
}
```
🔹 **Subject chứa danh sách Observer và gửi thông báo khi cần.**

---

#### **Bước 3: Cài đặt `ConcreteSubject`**
```java
class NewsChannel implements Subject {
    private List<Observer> observers = new ArrayList<>();

    @Override
    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void publishNews(String news) {
        System.out.println("📢 Tin tức mới: " + news);
        notifyObservers(news);
    }
}
```
🔹 `NewsChannel` là **Subject**, có thể đăng ký/hủy đăng ký Observer và gửi thông báo.

---

#### **Bước 4: Cài đặt `ConcreteObserver`**
```java
class Subscriber implements Observer {
    private String name;

    public Subscriber(String name) {
        this.name = name;
    }

    @Override
    public void update(String message) {
        System.out.println("📩 " + name + " nhận được thông báo: " + message);
    }
}
```
🔹 `Subscriber` là **Observer**, khi có tin tức mới, nó sẽ hiển thị thông báo.

---

#### **Bước 5: Kiểm thử Observer Pattern**
```java
public class ObserverPatternDemo {
    public static void main(String[] args) {
        NewsChannel newsChannel = new NewsChannel();

        Observer user1 = new Subscriber("Nguyễn Văn A");
        Observer user2 = new Subscriber("Trần Thị B");

        newsChannel.addObserver(user1);
        newsChannel.addObserver(user2);

        // Đăng tin mới
        newsChannel.publishNews("🔥 Java 21 chính thức ra mắt!");

        // Một người hủy đăng ký nhận tin
        newsChannel.removeObserver(user1);

        // Đăng tin tiếp theo
        newsChannel.publishNews("🚀 Spring Boot 3.0 có tính năng mới!");
    }
}
```

---

### **5. Output khi chạy chương trình**
```
📢 Tin tức mới: 🔥 Java 21 chính thức ra mắt!
📩 Nguyễn Văn A nhận được thông báo: 🔥 Java 21 chính thức ra mắt!
📩 Trần Thị B nhận được thông báo: 🔥 Java 21 chính thức ra mắt!

📢 Tin tức mới: 🚀 Spring Boot 3.0 có tính năng mới!
📩 Trần Thị B nhận được thông báo: 🚀 Spring Boot 3.0 có tính năng mới!
```
📌 **Nhận xét:**
- Khi tin tức mới được đăng, tất cả `Observer` đều nhận được thông báo.
- Khi `Nguyễn Văn A` hủy đăng ký, anh ta **không nhận thông báo nữa** khi có tin tức mới.

---

### **6. Ứng dụng thực tế của Observer Pattern**
✅ **Hệ thống thông báo sự kiện** trong ứng dụng di động.
✅ **Sàn giao dịch chứng khoán**, cập nhật giá cổ phiếu theo thời gian thực.
✅ **Hệ thống IoT**, khi cảm biến phát hiện thay đổi, nó sẽ gửi tín hiệu đến các thiết bị khác.
✅ **Mạng xã hội (Facebook, Twitter, YouTube)**, khi bạn follow ai đó, bạn sẽ nhận được thông báo khi họ đăng bài mới.

---

### **7. Ưu điểm & Nhược điểm của Observer Pattern**
✅ **Ưu điểm:**
✔️ **Tăng tính linh hoạt**, vì `Observers` và `Subject` không phụ thuộc trực tiếp vào nhau.
✔️ **Mở rộng dễ dàng**, có thể thêm `Observer` mới mà không ảnh hưởng đến `Subject`.
✔️ **Giúp hệ thống hoạt động theo sự kiện**, giảm việc kiểm tra thủ công.

❌ **Nhược điểm:**
⚠️ **Khó kiểm soát nếu có quá nhiều Observers**, gây hiệu suất kém.
⚠️ **Risk of memory leaks**, nếu không quản lý tốt, `Observer` có thể không được hủy đúng cách.

---

### **8. Bài tập thực hành**
Hãy tự triển khai một hệ thống **giá cổ phiếu**, trong đó:
1. Có một **StockMarket** (Subject) chứa giá của một cổ phiếu.
2. Có nhiều nhà đầu tư (**Investor**, Observer) theo dõi giá cổ phiếu.
3. Khi giá cổ phiếu thay đổi, tất cả Investors sẽ nhận được thông báo.

Bạn có thể tự làm thử, hoặc mình có thể hướng dẫn từng bước nếu bạn gặp khó khăn! 🚀