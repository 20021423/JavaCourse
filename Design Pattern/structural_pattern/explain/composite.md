# **Composite Design Pattern trong Java**

---

## **1. Composite Pattern là gì?**
**Composite Pattern** là một **mẫu thiết kế cấu trúc (Structural Pattern)** giúp **biểu diễn hệ thống theo cấu trúc cây**, trong đó **các thành phần đơn lẻ và nhóm thành phần có thể được xử lý theo cùng một cách**.

💡 **Ý tưởng chính:**
- **Nhóm các đối tượng lại thành một cấu trúc cây**, trong đó **các đối tượng đơn lẻ (Leaf) và nhóm đối tượng (Composite) có thể được xử lý như nhau**.
- **Client có thể thao tác trên từng phần tử hoặc cả nhóm mà không cần biết chi tiết bên trong**.

---

## **2. Khi nào sử dụng Composite Pattern?**
✅ Khi cần **biểu diễn một hệ thống có cấu trúc cây**, với **các đối tượng đơn lẻ và nhóm đối tượng**.  
✅ Khi muốn **xử lý các đối tượng riêng lẻ và nhóm đối tượng theo cùng một cách**.  
✅ Khi hệ thống có **nhiều cấp bậc (hierarchical structure)** như **hệ thống file, menu UI, tổ chức công ty**.

📌 **Ví dụ thực tế:**  
🔹 **Hệ thống thư mục & file (File System)**: Thư mục có thể chứa nhiều thư mục con hoặc file.  
🔹 **Tổ chức công ty (Company Structure)**: CEO → Quản lý → Nhân viên.  
🔹 **Giao diện đồ họa (GUI Components)**: `Button`, `Panel`, `TextBox` có thể chứa các thành phần khác.  
🔹 **Hệ thống menu (Menu System)**: Menu chính có thể chứa nhiều menu con và mục menu riêng lẻ.

---

## **3. Cấu trúc của Composite Pattern**
Composite Pattern có **4 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Component (interface/abstract class)** | Định nghĩa phương thức chung cho cả `Leaf` và `Composite`. |
| **Leaf (Đối tượng đơn lẻ)** | Đối tượng không thể chứa thành phần khác, thực hiện hành vi cụ thể. |
| **Composite (Nhóm đối tượng)** | Có thể chứa nhiều `Leaf` hoặc `Composite` khác. |
| **Client** | Tương tác với `Component` mà không cần biết đó là `Leaf` hay `Composite`. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Client chỉ làm việc với `Component`**, không cần biết chi tiết về `Leaf` hay `Composite`.  
2️⃣ **`Composite` có thể chứa `Leaf` hoặc `Composite` khác**, tạo thành một cấu trúc cây.  
3️⃣ **Gọi phương thức trên `Composite` sẽ áp dụng cho tất cả phần tử bên trong**.

---

## **4. Triển khai Composite Pattern trong Java**
### **Ví dụ: Hệ thống quản lý thư mục & file**
Hệ thống hỗ trợ **tổ chức thư mục**, trong đó:
- **File (Leaf)**: Đối tượng đơn lẻ.
- **Folder (Composite)**: Có thể chứa `File` hoặc `Folder` khác.

---

### **Bước 1: Tạo interface `Component` (FileSystemItem)**
```java
interface FileSystemItem {
    void showDetails();
}
```
🔹 **Mọi `File` và `Folder` đều phải có phương thức `showDetails()`.**

---

### **Bước 2: Cài đặt `Leaf` (File)**
```java
class File implements FileSystemItem {
    private String name;
    
    public File(String name) {
        this.name = name;
    }

    @Override
    public void showDetails() {
        System.out.println("📄 File: " + name);
    }
}
```
🔹 **File là một đối tượng đơn lẻ, không thể chứa phần tử khác.**

---

### **Bước 3: Cài đặt `Composite` (Folder)**
```java
import java.util.ArrayList;
import java.util.List;

class Folder implements FileSystemItem {
    private String name;
    private List<FileSystemItem> items = new ArrayList<>();

    public Folder(String name) {
        this.name = name;
    }

    public void addItem(FileSystemItem item) {
        items.add(item);
    }

    @Override
    public void showDetails() {
        System.out.println("📁 Folder: " + name);
        for (FileSystemItem item : items) {
            item.showDetails();
        }
    }
}
```
🔹 **Folder có thể chứa `File` hoặc `Folder` khác, tạo thành cấu trúc cây.**

---

### **Bước 4: Kiểm thử Composite Pattern**
```java
public class CompositePatternDemo {
    public static void main(String[] args) {
        // Tạo file
        File file1 = new File("document.txt");
        File file2 = new File("photo.jpg");
        File file3 = new File("music.mp3");

        // Tạo thư mục
        Folder folder1 = new Folder("My Documents");
        folder1.addItem(file1);
        folder1.addItem(file2);

        Folder folder2 = new Folder("Media");
        folder2.addItem(file3);

        // Thư mục chính chứa các thư mục con
        Folder mainFolder = new Folder("Main Folder");
        mainFolder.addItem(folder1);
        mainFolder.addItem(folder2);

        // Hiển thị cấu trúc thư mục
        mainFolder.showDetails();
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
📁 Folder: Main Folder
📁 Folder: My Documents
📄 File: document.txt
📄 File: photo.jpg
📁 Folder: Media
📄 File: music.mp3
```
📌 **Nhận xét:**
- **Client chỉ gọi `showDetails()` trên `Main Folder`**, nhưng mọi file & thư mục con đều được hiển thị.
- **Dễ dàng mở rộng**, có thể thêm nhiều file hoặc thư mục mà không cần sửa đổi mã nguồn.

---

## **6. Ứng dụng thực tế của Composite Pattern**
✅ **Hệ thống quản lý file & thư mục** 📂 (Windows Explorer, Linux File System).  
✅ **Cấu trúc tổ chức công ty** 🏢 (CEO → Quản lý → Nhân viên).  
✅ **Giao diện đồ họa (GUI Components)** 🎨 (`Panel` chứa `Button`, `TextBox`).  
✅ **Hệ thống menu UI** 📑 (Menu chính chứa menu con và mục menu).

---

## **7. Ưu điểm & Nhược điểm của Composite Pattern**
✅ **Ưu điểm:**  
✔️ **Dễ dàng mở rộng**, có thể thêm `Leaf` hoặc `Composite` mà không ảnh hưởng đến code cũ.  
✔️ **Tính nhất quán**, mọi phần tử (`File`, `Folder`) được xử lý theo cùng một cách.  
✔️ **Dễ bảo trì**, giúp giảm số lượng điều kiện (`if-else`) khi xử lý nhiều loại đối tượng.

❌ **Nhược điểm:**  
⚠️ **Có thể làm tăng độ phức tạp**, vì phải xử lý nhiều cấp độ lồng nhau.  
⚠️ **Có thể dư thừa**, nếu hệ thống không thực sự cần xử lý theo cấu trúc cây.

---

## **8. Bài tập thực hành**
Hãy triển khai một **hệ thống tổ chức công ty (`CompanyStructure`)**, gồm:
1. **CEO (`Leaf`)**.
2. **Quản lý (`Composite`)** có thể chứa nhiều nhân viên hoặc quản lý khác.
3. **Nhân viên (`Leaf`)**.

👉 **Mục tiêu**: Hiển thị toàn bộ sơ đồ công ty bằng cách gọi `showDetails()` trên CEO. 🚀