# **Prototype Design Pattern trong Java**

---

## **1. Prototype Pattern là gì?**
**Prototype Pattern** là một **mẫu thiết kế sáng tạo (Creational Pattern)** giúp **tạo đối tượng mới bằng cách sao chép (clone) một đối tượng đã có**, thay vì khởi tạo từ đầu.

💡 **Ý tưởng chính:**
- **Tránh việc tạo đối tượng mới từ đầu**, đặc biệt khi chi phí khởi tạo cao.
- **Sao chép đối tượng nhanh hơn và linh hoạt hơn**, thay vì dùng `new`.
- **Dễ dàng tạo các biến thể của một đối tượng mà không cần thay đổi mã nguồn.**

---

## **2. Khi nào sử dụng Prototype Pattern?**
✅ Khi việc **tạo đối tượng mới từ đầu tốn kém tài nguyên** (ví dụ: kết nối DB, đọc file).  
✅ Khi cần **sao chép nhiều đối tượng có cùng cấu trúc nhưng khác dữ liệu**.  
✅ Khi muốn **tạo đối tượng mà không phụ thuộc vào lớp cụ thể**, giúp code linh hoạt hơn.

📌 **Ví dụ thực tế:**  
🔹 **Nhân bản tài liệu (MS Word, Google Docs)**: Khi tạo bản sao tài liệu, ta chỉ cần copy nội dung thay vì tạo mới.  
🔹 **Game (Nhân bản nhân vật, kẻ địch)**: Khi cần tạo hàng trăm kẻ địch giống nhau nhưng có chỉ số khác nhau.  
🔹 **Thiết kế đồ họa (Figma, Photoshop)**: Sao chép một button với cùng thuộc tính nhưng khác nội dung.  
🔹 **Cấu hình Database Connection**: Dùng một mẫu cấu hình và tạo các bản sao cho nhiều server khác nhau.

---

## **3. Cấu trúc của Prototype Pattern**
Prototype Pattern có **3 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Prototype (interface)** | Định nghĩa phương thức `clone()` để sao chép đối tượng. |
| **ConcretePrototype** | Cài đặt cụ thể của `Prototype`, thực hiện việc sao chép. |
| **Client** | Yêu cầu sao chép một đối tượng bằng cách gọi `clone()`. |

📌 **Quy tắc hoạt động:**  
1️⃣ `ConcretePrototype` thực hiện phương thức `clone()`.  
2️⃣ `Client` gọi `clone()` trên một đối tượng để tạo bản sao.  
3️⃣ Có thể **tùy chỉnh dữ liệu** của bản sao mà không ảnh hưởng đến bản gốc.

---

## **4. Triển khai Prototype Pattern trong Java**
### **Ví dụ: Nhân bản tài liệu (`Document`)**
Hệ thống hỗ trợ **tạo bản sao của tài liệu**, với các thuộc tính:
- **`title`**: Tiêu đề.
- **`content`**: Nội dung.
- **`author`**: Tác giả.

---

### **Bước 1: Tạo interface `Prototype`**
```java
interface Prototype {
    Prototype clone();
}
```
🔹 **Mọi lớp cần nhân bản phải triển khai phương thức `clone()`.**

---

### **Bước 2: Cài đặt `ConcretePrototype` (`Document`)**
```java
class Document implements Prototype {
    private String title;
    private String content;
    private String author;

    public Document(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }

    // Triển khai phương thức clone()
    @Override
    public Document clone() {
        return new Document(this.title, this.content, this.author);
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public void showInfo() {
        System.out.println("📄 Document: " + title + " | 🖋 Author: " + author);
        System.out.println("Content: " + content);
    }
}
```
🔹 **Khi gọi `clone()`, một `Document` mới được tạo với cùng dữ liệu**.  
🔹 **Có thể thay đổi dữ liệu của bản sao mà không ảnh hưởng đến bản gốc.**

---

### **Bước 3: Kiểm thử Prototype Pattern**
```java
public class PrototypePatternDemo {
    public static void main(String[] args) {
        // Tạo tài liệu gốc
        Document doc1 = new Document("Design Patterns", "Prototype Pattern Example", "John Doe");
        doc1.showInfo();

        // Nhân bản tài liệu
        Document doc2 = doc1.clone();
        doc2.setTitle("Cloned Document");
        
        System.out.println("\n🔁 Sau khi nhân bản:");
        doc1.showInfo();
        doc2.showInfo();
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
📄 Document: Design Patterns | 🖋 Author: John Doe
Content: Prototype Pattern Example

🔁 Sau khi nhân bản:
📄 Document: Design Patterns | 🖋 Author: John Doe
Content: Prototype Pattern Example

📄 Document: Cloned Document | 🖋 Author: John Doe
Content: Prototype Pattern Example
```
📌 **Nhận xét:**
- **Bản gốc và bản sao có dữ liệu giống nhau ban đầu**.
- **Có thể thay đổi tiêu đề bản sao (`doc2.setTitle()`) mà không ảnh hưởng đến bản gốc**.

---

## **6. Deep Copy vs Shallow Copy trong Prototype Pattern**
Prototype Pattern có **2 cách sao chép**:

| **Loại Copy**  | **Ý nghĩa** | **Khi nào dùng?** |
|--------------|-----------|------------------|
| **Shallow Copy** | Sao chép địa chỉ của đối tượng tham chiếu (chỉ nhân bản giá trị gốc) | Khi dữ liệu là immutable hoặc không thay đổi. |
| **Deep Copy** | Tạo bản sao hoàn toàn mới của mọi thành phần, kể cả đối tượng tham chiếu bên trong | Khi đối tượng chứa nhiều tham chiếu (List, Map, Object). |

---

### **Bổ sung Deep Copy cho Prototype**
#### **Bước 1: Thêm thuộc tính `List<String> tags`**
```java
import java.util.ArrayList;
import java.util.List;

class Document implements Prototype {
    private String title;
    private String content;
    private String author;
    private List<String> tags;

    public Document(String title, String content, String author, List<String> tags) {
        this.title = title;
        this.content = content;
        this.author = author;
        this.tags = new ArrayList<>(tags); // Deep Copy
    }

    @Override
    public Document clone() {
        return new Document(this.title, this.content, this.author, new ArrayList<>(this.tags));
    }

    public void addTag(String tag) {
        this.tags.add(tag);
    }

    public void showInfo() {
        System.out.println("📄 Document: " + title + " | 🖋 Author: " + author + " | Tags: " + tags);
    }
}
```

#### **Bước 2: Kiểm thử Deep Copy**
```java
public class PrototypePatternDemo {
    public static void main(String[] args) {
        List<String> tags = new ArrayList<>();
        tags.add("Design Patterns");

        Document doc1 = new Document("Design Patterns", "Prototype Pattern Example", "John Doe", tags);
        doc1.showInfo();

        // Nhân bản tài liệu
        Document doc2 = doc1.clone();
        doc2.addTag("Java");

        System.out.println("\n🔁 Sau khi nhân bản:");
        doc1.showInfo();
        doc2.showInfo();
    }
}
```
📌 **Nhận xét:**
- `doc2.addTag("Java")` chỉ thay đổi bản sao, không ảnh hưởng đến bản gốc (`doc1`).
- Nếu dùng **shallow copy**, `tags` của cả hai sẽ bị thay đổi cùng nhau.

---

## **7. Ứng dụng thực tế của Prototype Pattern**
✅ **Sao chép tài liệu (MS Word, Google Docs)** 📄.  
✅ **Tạo nhân vật hoặc quái vật trong game** 🎮.  
✅ **Cấu hình mẫu cho Database Connection** 🗄.  
✅ **Nhân bản UI Component (Button, Icon, Layout)** 🎨.

---

## **8. Ưu điểm & Nhược điểm của Prototype Pattern**
✅ **Ưu điểm:**  
✔️ **Nhanh hơn so với tạo mới từ đầu**, đặc biệt khi đối tượng có chi phí khởi tạo cao.  
✔️ **Giảm phụ thuộc vào lớp cụ thể**, giúp dễ mở rộng.  
✔️ **Hỗ trợ sao chép đối tượng phức tạp**, không cần nhiều constructor.

❌ **Nhược điểm:**  
⚠️ **Có thể phức tạp nếu đối tượng có nhiều tham chiếu**, cần xử lý **Deep Copy** đúng cách.  
⚠️ **Dễ xảy ra lỗi nếu sao chép không đúng cách** (ví dụ: chỉ sao chép tham chiếu thay vì dữ liệu).

---

## **9. Bài tập thực hành**
Hãy triển khai một hệ thống **nhân bản nhân vật trong game (`CharacterPrototype`)**, gồm:
1. **Tên (`name`)**.
2. **Vũ khí (`weapon`)**.
3. **Kỹ năng (`skills`) (Danh sách List)**.

👉 **Mục tiêu**: Khi nhân bản nhân vật, có thể chỉnh sửa vũ khí mà không ảnh hưởng bản gốc. 🚀