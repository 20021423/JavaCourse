# **Mediator Design Pattern trong Java**

---

## **1. Mediator Pattern là gì?**
**Mediator Pattern** là một **mẫu thiết kế hành vi (Behavioral Pattern)** giúp giảm sự phụ thuộc trực tiếp giữa các đối tượng bằng cách **đưa một đối tượng trung gian (Mediator) làm trung tâm giao tiếp**.

💡 **Ý tưởng chính:**
- **Tách biệt các đối tượng giao tiếp với nhau**, tránh liên kết chặt chẽ (**tight coupling**).
- **Tất cả giao tiếp giữa các đối tượng đều thông qua một lớp trung gian (`Mediator`)**.

---

## **2. Khi nào sử dụng Mediator Pattern?**
✅ Khi có **nhiều đối tượng cần giao tiếp với nhau** nhưng muốn **giảm sự phụ thuộc giữa chúng**.  
✅ Khi một hệ thống có **nhiều mối quan hệ phức tạp**, dễ gây khó khăn trong bảo trì.  
✅ Khi cần **tăng tính linh hoạt**, giúp thay đổi cách giao tiếp mà không cần sửa đổi nhiều lớp.

📌 **Ví dụ thực tế:**  
🔹 **Hệ thống chat (Chat Room)**: Người dùng không cần gửi tin nhắn trực tiếp, mà thông qua một máy chủ trung gian.  
🔹 **Điều khiển giao thông (Air Traffic Control)**: Các máy bay không liên lạc trực tiếp mà thông qua trung tâm kiểm soát không lưu.  
🔹 **Giao tiếp giữa các thành phần UI**: Trong một ứng dụng GUI, khi một nút bấm thay đổi trạng thái, các thành phần khác có thể phản ứng mà không cần liên kết trực tiếp.

---

## **3. Cấu trúc của Mediator Pattern**
Mediator Pattern có **4 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Mediator (interface)** | Định nghĩa phương thức để các đối tượng giao tiếp với nhau. |
| **ConcreteMediator** | Cài đặt cụ thể của Mediator, quản lý và điều phối giao tiếp giữa các đối tượng. |
| **Colleague (interface)** | Đối tượng tham gia giao tiếp, không giao tiếp trực tiếp mà thông qua Mediator. |
| **ConcreteColleague** | Cài đặt cụ thể của Colleague, khi cần giao tiếp thì gọi Mediator. |

📌 **Quy tắc hoạt động:**  
1️⃣ `ConcreteColleague` không giao tiếp trực tiếp với nhau, mà gửi yêu cầu đến `Mediator`.  
2️⃣ `ConcreteMediator` xử lý yêu cầu và chuyển tiếp thông điệp đến `Colleague` phù hợp.

---

## **4. Triển khai Mediator Pattern trong Java**
### **Ví dụ: Hệ thống chat giữa người dùng**
Hệ thống có **nhiều người dùng (User)**, nhưng **không gửi tin nhắn trực tiếp** cho nhau, mà thông qua **ChatRoom (Mediator)**.

---

### **Bước 1: Tạo interface `Mediator`**
```java
interface ChatMediator {
    void sendMessage(String message, User user);
    void addUser(User user);
}
```
🔹 **`ChatMediator` quản lý giao tiếp giữa các User**.

---

### **Bước 2: Cài đặt `ConcreteMediator` (`ChatRoom`)**
```java
import java.util.ArrayList;
import java.util.List;

class ChatRoom implements ChatMediator {
    private List<User> users = new ArrayList<>();

    @Override
    public void addUser(User user) {
        users.add(user);
    }

    @Override
    public void sendMessage(String message, User sender) {
        for (User user : users) {
            // Không gửi tin nhắn cho chính người gửi
            if (user != sender) {
                user.receiveMessage(message);
            }
        }
    }
}
```
🔹 **`ChatRoom` đóng vai trò trung gian**, nhận tin nhắn từ một User và gửi nó đến tất cả các User khác.

---

### **Bước 3: Tạo lớp `Colleague` (`User`)**
```java
abstract class User {
    protected ChatMediator mediator;
    protected String name;

    public User(ChatMediator mediator, String name) {
        this.mediator = mediator;
        this.name = name;
    }

    public abstract void sendMessage(String message);
    public abstract void receiveMessage(String message);
}
```
🔹 **Mỗi `User` có thể gửi và nhận tin nhắn thông qua `ChatMediator`**.

---

### **Bước 4: Cài đặt `ConcreteColleague` (`ChatUser`)**
```java
class ChatUser extends User {
    public ChatUser(ChatMediator mediator, String name) {
        super(mediator, name);
    }

    @Override
    public void sendMessage(String message) {
        System.out.println("📤 " + name + " gửi: " + message);
        mediator.sendMessage(message, this);
    }

    @Override
    public void receiveMessage(String message) {
        System.out.println("📩 " + name + " nhận: " + message);
    }
}
```
🔹 **`ChatUser` không gửi tin nhắn trực tiếp**, mà thông qua `ChatRoom`.

---

### **Bước 5: Kiểm thử Mediator Pattern**
```java
public class MediatorPatternDemo {
    public static void main(String[] args) {
        ChatMediator chatRoom = new ChatRoom();

        User user1 = new ChatUser(chatRoom, "Alice");
        User user2 = new ChatUser(chatRoom, "Bob");
        User user3 = new ChatUser(chatRoom, "Charlie");

        chatRoom.addUser(user1);
        chatRoom.addUser(user2);
        chatRoom.addUser(user3);

        user1.sendMessage("Xin chào mọi người!");
        user3.sendMessage("Chào Alice!");
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
📤 Alice gửi: Xin chào mọi người!
📩 Bob nhận: Xin chào mọi người!
📩 Charlie nhận: Xin chào mọi người!

📤 Charlie gửi: Chào Alice!
📩 Alice nhận: Chào Alice!
📩 Bob nhận: Chào Alice!
```
📌 **Nhận xét:**
- Khi **Alice gửi tin nhắn**, `ChatRoom` phân phối tin nhắn đến **Bob và Charlie**.
- Khi **Charlie gửi tin nhắn**, `ChatRoom` gửi tin đến **Alice và Bob**.
- **Không có kết nối trực tiếp giữa Alice, Bob và Charlie**, giúp hệ thống dễ mở rộng.

---

## **6. Ứng dụng thực tế của Mediator Pattern**
✅ **Hệ thống chat (Slack, Discord, Teams)** 🗨  
✅ **Điều khiển giao thông hàng không (Air Traffic Control)** ✈  
✅ **Trung tâm báo động trong tòa nhà** 🚨  
✅ **Giao tiếp giữa các thành phần UI trong ứng dụng** 🖥

---

## **7. Ưu điểm & Nhược điểm của Mediator Pattern**
✅ **Ưu điểm:**  
✔️ **Giảm sự phụ thuộc giữa các đối tượng**, giúp mã dễ bảo trì.  
✔️ **Tăng tính mở rộng**, dễ dàng thêm `Colleague` mới mà không ảnh hưởng đến hệ thống.  
✔️ **Đơn giản hóa mối quan hệ**, thay vì nhiều kết nối phức tạp, chỉ có một kết nối với `Mediator`.

❌ **Nhược điểm:**  
⚠️ **Mediator có thể trở thành điểm tập trung**, nếu quá phức tạp thì khó bảo trì.  
⚠️ **Có thể làm giảm hiệu suất**, vì mọi giao tiếp đều phải thông qua `Mediator`.

---

## **8. Bài tập thực hành**
Hãy triển khai một hệ thống **trung tâm kiểm soát không lưu (Air Traffic Control)**, gồm:
1. **Máy bay (Aircraft)** có thể gửi yêu cầu hạ cánh/cất cánh.
2. **Trung tâm điều khiển (AirTrafficControl)** điều phối các máy bay, tránh va chạm.

👉 **Mục tiêu**: Máy bay không liên lạc trực tiếp mà thông qua trung tâm kiểm soát.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀