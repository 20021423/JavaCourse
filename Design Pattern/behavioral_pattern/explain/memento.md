# **Memento Design Pattern trong Java**

---

## **1. Memento Pattern là gì?**
**Memento Pattern** là một **mẫu thiết kế hành vi (Behavioral Pattern)** cho phép **lưu trữ trạng thái của một đối tượng** để có thể **khôi phục lại sau này** mà không vi phạm nguyên tắc **Encapsulation**.

💡 **Ý tưởng chính:**
- **Lưu trạng thái đối tượng tại một thời điểm** để có thể khôi phục sau này.
- **Giúp tính năng Undo/Redo** trong các ứng dụng.
- **Tách việc lưu trữ trạng thái ra khỏi đối tượng chính**, giúp dễ quản lý.

---

## **2. Khi nào sử dụng Memento Pattern?**
✅ Khi cần **lưu trạng thái** của một đối tượng và có thể **quay lại trạng thái trước đó**.  
✅ Khi muốn **triển khai tính năng Undo/Redo** trong ứng dụng.  
✅ Khi cần **tách biệt logic lưu trữ dữ liệu** khỏi lớp đối tượng chính.

📌 **Ví dụ thực tế:**  
🔹 **Trình soạn thảo văn bản**: Lưu trạng thái tài liệu để có thể hoàn tác (Undo).  
🔹 **Trò chơi (Game Save)**: Lưu tiến trình game để tiếp tục chơi sau.  
🔹 **Hệ thống quản lý phiên bản**: Lưu lại trạng thái trước khi cập nhật.

---

## **3. Cấu trúc của Memento Pattern**
Memento Pattern có **3 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|-------------|-----------|
| **Originator** | Đối tượng gốc, có trạng thái cần lưu, có thể tạo và khôi phục từ Memento. |
| **Memento** | Đối tượng lưu trữ trạng thái, giúp khôi phục khi cần. |
| **Caretaker** | Quản lý danh sách các Memento, cho phép quay lại trạng thái trước đó. |

📌 **Quy tắc hoạt động:**  
1️⃣ `Originator` tạo một `Memento` chứa trạng thái hiện tại.  
2️⃣ `Caretaker` lưu trữ `Memento`.  
3️⃣ Khi cần, `Originator` có thể khôi phục trạng thái từ `Memento`.

---

## **4. Triển khai Memento Pattern trong Java**
### **Ví dụ: Trình soạn thảo văn bản có Undo**
**Yêu cầu:**
- Người dùng có thể nhập văn bản.
- Lưu lại lịch sử nội dung (Undo).
- Khi nhấn Undo, văn bản quay về trạng thái trước đó.

---

### **Bước 1: Tạo lớp `Memento` để lưu trạng thái**
```java
class Memento {
    private final String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}
```
🔹 **`Memento` lưu trữ trạng thái của đối tượng** nhưng không cho phép sửa đổi (**immutable**).

---

### **Bước 2: Tạo lớp `Originator` (Trình soạn thảo)**
```java
class TextEditor {
    private String text = "";

    public void setText(String text) {
        System.out.println("✏ Cập nhật nội dung: " + text);
        this.text = text;
    }

    public String getText() {
        return text;
    }

    // Tạo một bản sao lưu trạng thái hiện tại
    public Memento save() {
        return new Memento(text);
    }

    // Khôi phục trạng thái từ Memento
    public void restore(Memento memento) {
        text = memento.getState();
        System.out.println("⏪ Khôi phục nội dung: " + text);
    }
}
```
🔹 **`TextEditor` có thể tạo và khôi phục từ `Memento` để hỗ trợ Undo**.

---

### **Bước 3: Tạo lớp `Caretaker` để quản lý lịch sử trạng thái**
```java
import java.util.Stack;

class History {
    private Stack<Memento> history = new Stack<>();

    public void saveState(Memento memento) {
        history.push(memento);
    }

    public Memento undo() {
        if (!history.isEmpty()) {
            return history.pop();
        }
        return null;
    }
}
```
🔹 **`History` dùng `Stack` để lưu trữ trạng thái**, hỗ trợ Undo.

---

### **Bước 4: Kiểm thử Memento Pattern**
```java
public class MementoPatternDemo {
    public static void main(String[] args) {
        TextEditor editor = new TextEditor();
        History history = new History();

        editor.setText("Hello, World!");
        history.saveState(editor.save()); // Lưu trạng thái

        editor.setText("Hello, Java!");
        history.saveState(editor.save()); // Lưu trạng thái

        editor.setText("Hello, Design Patterns!");

        // Thực hiện Undo
        editor.restore(history.undo()); // Quay lại "Hello, Java!"
        editor.restore(history.undo()); // Quay lại "Hello, World!"
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
✏ Cập nhật nội dung: Hello, World!
✏ Cập nhật nội dung: Hello, Java!
✏ Cập nhật nội dung: Hello, Design Patterns!
⏪ Khôi phục nội dung: Hello, Java!
⏪ Khôi phục nội dung: Hello, World!
```
📌 **Nhận xét:**
- Khi nhập văn bản, trạng thái được lưu vào `History`.
- Khi nhấn Undo, trình soạn thảo **quay lại trạng thái trước đó**.

---

## **6. Ứng dụng thực tế của Memento Pattern**
✅ **Undo/Redo trong trình soạn thảo văn bản** 📝  
✅ **Lưu checkpoint trong game** 🎮  
✅ **Hệ thống quản lý phiên bản** 🗄  
✅ **Ứng dụng chỉnh sửa ảnh (Photoshop, Figma)** 🎨

---

## **7. Ưu điểm & Nhược điểm của Memento Pattern**
✅ **Ưu điểm:**  
✔️ **Đảm bảo tính đóng gói (Encapsulation)**, vì trạng thái được lưu trữ mà không vi phạm bảo mật dữ liệu.  
✔️ **Dễ dàng khôi phục trạng thái trước đó**, giúp hỗ trợ Undo/Redo hiệu quả.  
✔️ **Tăng tính mở rộng**, có thể lưu nhiều trạng thái khác nhau.

❌ **Nhược điểm:**  
⚠️ **Tốn bộ nhớ** nếu lưu quá nhiều trạng thái.  
⚠️ **Không kiểm soát được số lượng trạng thái lưu trữ**, có thể gây tràn bộ nhớ nếu không giới hạn.

---

## **8. Bài tập thực hành**
Hãy triển khai một hệ thống **trò chơi có chức năng lưu checkpoint**, gồm các trạng thái:
1. **Checkpoint 1**: Nhân vật có 100 máu, 10 điểm kinh nghiệm.
2. **Checkpoint 2**: Nhân vật có 80 máu, 20 điểm kinh nghiệm.
3. **Checkpoint 3**: Nhân vật có 50 máu, 30 điểm kinh nghiệm.

👉 **Mục tiêu**: Khi nhân vật chết, có thể quay lại checkpoint gần nhất.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀