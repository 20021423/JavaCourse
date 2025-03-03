# **Command Design Pattern trong Java**

---

## **1. Command Pattern là gì?**
**Command Pattern** là một **mẫu thiết kế hành vi (Behavioral Pattern)** giúp **biến các yêu cầu (commands) thành đối tượng** để có thể lưu trữ, xếp hàng (queue), hoặc hoàn tác (undo).

💡 **Ý tưởng chính:**
- **Tách biệt người gửi lệnh và người thực thi lệnh**, giúp giảm sự phụ thuộc giữa chúng.
- **Lưu trữ các lệnh để có thể thực thi lại hoặc hoàn tác (undo).**
- **Dễ dàng mở rộng với các lệnh mới mà không thay đổi mã nguồn của hệ thống.**

---

## **2. Khi nào sử dụng Command Pattern?**
✅ Khi cần **tách rời đối tượng gọi lệnh và đối tượng thực thi lệnh**, giúp dễ mở rộng.  
✅ Khi cần **thực hiện Undo/Redo**, vì có thể lưu trữ lệnh để quay lại trạng thái trước đó.  
✅ Khi cần **hỗ trợ macro (chuỗi lệnh)**, ví dụ như một tổ hợp phím thực hiện nhiều thao tác.

📌 **Ví dụ thực tế:**  
🔹 **Tính năng Undo/Redo trong trình soạn thảo** (Ctrl+Z, Ctrl+Y).  
🔹 **Điều khiển thiết bị thông minh (Smart Home)** (bật/tắt đèn, máy lạnh, TV).  
🔹 **Hệ thống đặt hàng (E-commerce)** (đặt hàng, hủy đơn hàng).  
🔹 **Macro trong game hoặc phần mềm** (thực hiện chuỗi lệnh liên tiếp).

---

## **3. Cấu trúc của Command Pattern**
Command Pattern có **4 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Command (interface)** | Định nghĩa phương thức thực thi lệnh (`execute()`) và có thể có `undo()`. |
| **ConcreteCommand** | Cài đặt cụ thể của `Command`, thực thi hành động cụ thể trên Receiver. |
| **Receiver** | Đối tượng thực hiện hành động thực tế. |
| **Invoker** | Lưu trữ và gọi các `Command`, có thể hỗ trợ Undo/Redo. |

📌 **Quy tắc hoạt động:**  
1️⃣ `Invoker` gọi `execute()` của `Command`.  
2️⃣ `Command` gọi hành động trên `Receiver`.  
3️⃣ `Receiver` thực hiện hành động thực tế.  
4️⃣ Nếu hỗ trợ `Undo`, `Invoker` có thể gọi `undo()`.

---

## **4. Triển khai Command Pattern trong Java**
### **Ví dụ: Điều khiển thiết bị điện thông minh (Smart Home)**
Hệ thống hỗ trợ **bật/tắt đèn** bằng cách gửi lệnh đến `Light`.

---

### **Bước 1: Tạo interface `Command`**
```java
interface Command {
    void execute();
    void undo();
}
```
🔹 **Mọi lệnh phải có phương thức `execute()` để thực hiện lệnh và `undo()` để hoàn tác.**

---

### **Bước 2: Tạo lớp `Receiver` (Thiết bị `Light`)**
```java
class Light {
    public void turnOn() {
        System.out.println("💡 Đèn bật!");
    }

    public void turnOff() {
        System.out.println("💡 Đèn tắt!");
    }
}
```
🔹 **`Light` thực hiện hành động thực tế khi được lệnh bật/tắt.**

---

### **Bước 3: Cài đặt `ConcreteCommand` (Lệnh `LightOnCommand` và `LightOffCommand`)**
#### **Lệnh bật đèn**
```java
class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }

    @Override
    public void undo() {
        light.turnOff();
    }
}
```

#### **Lệnh tắt đèn**
```java
class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOff();
    }

    @Override
    public void undo() {
        light.turnOn();
    }
}
```
🔹 **Mỗi lệnh đều có thể thực thi (`execute()`) và hoàn tác (`undo()`).**

---

### **Bước 4: Tạo lớp `Invoker` (Điều khiển từ xa)**
```java
class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }

    public void pressUndo() {
        command.undo();
    }
}
```
🔹 **`RemoteControl` lưu lệnh hiện tại và gọi `execute()` khi nhấn nút.**

---

### **Bước 5: Kiểm thử Command Pattern**
```java
public class CommandPatternDemo {
    public static void main(String[] args) {
        Light light = new Light(); // Receiver
        RemoteControl remote = new RemoteControl(); // Invoker

        Command lightOn = new LightOnCommand(light);
        Command lightOff = new LightOffCommand(light);

        // Bật đèn
        remote.setCommand(lightOn);
        remote.pressButton();  // 💡 Đèn bật!

        // Hoàn tác (Tắt đèn)
        remote.pressUndo();    // 💡 Đèn tắt!

        // Tắt đèn
        remote.setCommand(lightOff);
        remote.pressButton();  // 💡 Đèn tắt!

        // Hoàn tác (Bật đèn)
        remote.pressUndo();    // 💡 Đèn bật!
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
💡 Đèn bật!
💡 Đèn tắt!
💡 Đèn tắt!
💡 Đèn bật!
```
📌 **Nhận xét:**
- `RemoteControl` có thể thực hiện và hoàn tác lệnh **mà không cần biết chi tiết `Light` hoạt động như thế nào**.
- **Dễ dàng thêm lệnh mới (TV, Quạt) mà không sửa mã nguồn hiện có.**

---

## **6. Ứng dụng thực tế của Command Pattern**
✅ **Điều khiển thiết bị thông minh** (Alexa, Google Home).  
✅ **Hệ thống Undo/Redo** (Word, Photoshop).  
✅ **Hệ thống giao dịch tài chính** (Đặt lệnh, Hủy lệnh).  
✅ **Macro trong phần mềm & game** (Tự động hóa chuỗi lệnh).

---

## **7. Ưu điểm & Nhược điểm của Command Pattern**
✅ **Ưu điểm:**  
✔️ **Tách biệt đối tượng gọi lệnh và thực thi lệnh**, giúp dễ mở rộng.  
✔️ **Hỗ trợ Undo/Redo**, giúp cải thiện trải nghiệm người dùng.  
✔️ **Có thể xếp hàng (Queue) lệnh**, giúp hỗ trợ xử lý bất đồng bộ.

❌ **Nhược điểm:**  
⚠️ **Tốn bộ nhớ**, vì mỗi hành động phải tạo một đối tượng `Command` riêng.  
⚠️ **Có thể làm tăng độ phức tạp**, nếu có quá nhiều loại lệnh cần xử lý.

---

## **8. Bài tập thực hành**
Hãy triển khai một hệ thống **điều khiển quạt**, gồm:
1. **Quạt (`Fan`)** có thể bật/tắt.
2. **Lệnh bật/tắt quạt (`FanOnCommand`, `FanOffCommand`)**.
3. **Điều khiển từ xa (`RemoteControl`)** có thể điều khiển cả quạt và đèn.

👉 **Mục tiêu**: Dùng cùng một `RemoteControl` để bật/tắt cả đèn và quạt.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀