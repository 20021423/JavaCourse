# **Iterator Design Pattern trong Java**

---

## **1. Iterator Pattern là gì?**
**Iterator Pattern** là một **mẫu thiết kế hành vi (Behavioral Pattern)** giúp **duyệt qua các phần tử của một tập hợp (collection) mà không cần biết cấu trúc bên trong của nó**.

💡 **Ý tưởng chính:**
- **Tách biệt logic duyệt phần tử ra khỏi tập hợp**, giúp code dễ mở rộng.
- **Cung cấp một giao diện chung để duyệt qua nhiều loại tập hợp khác nhau**.

---

## **2. Khi nào sử dụng Iterator Pattern?**
✅ Khi cần **duyệt qua một tập hợp mà không làm lộ chi tiết bên trong**.  
✅ Khi muốn **cung cấp nhiều cách duyệt khác nhau** cho cùng một tập hợp.  
✅ Khi cần **duyệt qua tập hợp mà không cần dùng `for`, `while` phức tạp**.

📌 **Ví dụ thực tế:**  
🔹 **Duyệt danh sách người dùng trong hệ thống**.  
🔹 **Duyệt qua các phần tử trong một cây nhị phân (Binary Tree Traversal)**.  
🔹 **Duyệt qua tập hợp các bài hát trong danh sách phát (Playlist)**.  
🔹 **Duyệt qua trang trong tài liệu (PDF Viewer, eBook Reader)**.

---

## **3. Cấu trúc của Iterator Pattern**
Iterator Pattern có **4 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Iterator (interface)** | Định nghĩa phương thức để duyệt qua tập hợp. |
| **ConcreteIterator** | Cài đặt cụ thể của `Iterator`, chứa logic duyệt phần tử. |
| **Aggregate (interface)** | Định nghĩa phương thức để lấy `Iterator`. |
| **ConcreteAggregate** | Cài đặt cụ thể của tập hợp, lưu trữ danh sách phần tử. |

📌 **Quy tắc hoạt động:**  
1️⃣ `ConcreteAggregate` chứa danh sách phần tử.  
2️⃣ `ConcreteIterator` duyệt qua danh sách mà không cần làm lộ cấu trúc bên trong.  
3️⃣ `Client` chỉ cần gọi `hasNext()` và `next()` để duyệt qua các phần tử.

---

## **4. Triển khai Iterator Pattern trong Java**
### **Ví dụ: Duyệt danh sách tên sinh viên**
Hệ thống hỗ trợ **duyệt danh sách sinh viên**, nhưng **không làm lộ cấu trúc dữ liệu bên trong**.

---

### **Bước 1: Tạo interface `Iterator`**
```java
interface Iterator {
    boolean hasNext();
    Object next();
}
```
🔹 **`Iterator` định nghĩa hai phương thức chính**:
- `hasNext()`: Kiểm tra xem còn phần tử nào không.
- `next()`: Lấy phần tử tiếp theo trong danh sách.

---

### **Bước 2: Cài đặt `ConcreteIterator` (`StudentIterator`)**
```java
class StudentIterator implements Iterator {
    private String[] students;
    private int index = 0;

    public StudentIterator(String[] students) {
        this.students = students;
    }

    @Override
    public boolean hasNext() {
        return index < students.length;
    }

    @Override
    public Object next() {
        if (this.hasNext()) {
            return students[index++];
        }
        return null;
    }
}
```
🔹 **`StudentIterator` duyệt qua mảng sinh viên từng bước, không lộ chi tiết bên trong.**

---

### **Bước 3: Tạo interface `Aggregate` (`StudentCollection`)**
```java
interface StudentCollection {
    Iterator createIterator();
}
```
🔹 **`StudentCollection` định nghĩa phương thức `createIterator()` để lấy `Iterator`.**

---

### **Bước 4: Cài đặt `ConcreteAggregate` (`StudentList`)**
```java
class StudentList implements StudentCollection {
    private String[] students;

    public StudentList(String[] students) {
        this.students = students;
    }

    @Override
    public Iterator createIterator() {
        return new StudentIterator(students);
    }
}
```
🔹 **`StudentList` chứa danh sách sinh viên và tạo `StudentIterator`.**

---

### **Bước 5: Kiểm thử Iterator Pattern**
```java
public class IteratorPatternDemo {
    public static void main(String[] args) {
        String[] studentNames = {"Alice", "Bob", "Charlie", "David"};

        StudentCollection studentList = new StudentList(studentNames);
        Iterator iterator = studentList.createIterator();

        System.out.println("📌 Danh sách sinh viên:");
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
📌 Danh sách sinh viên:
Alice
Bob
Charlie
David
```
📌 **Nhận xét:**
- **`StudentIterator` giúp duyệt danh sách mà không cần biết dữ liệu lưu trữ như thế nào.**
- **Có thể dễ dàng thay đổi cách lưu trữ (ArrayList, LinkedList) mà không ảnh hưởng đến `Iterator`.**

---

## **6. Ứng dụng thực tế của Iterator Pattern**
✅ **Duyệt danh sách sinh viên, nhân viên, sản phẩm** 🏫  
✅ **Duyệt qua tập hợp các phần tử trong cây nhị phân (Tree Traversal)** 🌲  
✅ **Duyệt qua danh sách phát trong ứng dụng âm nhạc (Playlist Iterator)** 🎵  
✅ **Duyệt qua trang trong tài liệu (PDF Viewer, eBook Reader)** 📖

---

## **7. Ưu điểm & Nhược điểm của Iterator Pattern**
✅ **Ưu điểm:**  
✔️ **Ẩn chi tiết cấu trúc dữ liệu**, giúp dễ thay đổi mà không ảnh hưởng đến Client.  
✔️ **Dễ dàng mở rộng**, có thể tạo nhiều loại `Iterator` khác nhau.  
✔️ **Tuân theo nguyên tắc Single Responsibility (SRP) và Open/Closed (OCP)** trong SOLID.

❌ **Nhược điểm:**  
⚠️ **Tốn bộ nhớ**, vì phải tạo đối tượng `Iterator` riêng biệt.  
⚠️ **Không thể sửa đổi danh sách khi đang duyệt**, có thể gây lỗi `ConcurrentModificationException`.

---

## **8. Bài tập thực hành**
Hãy triển khai một hệ thống **duyệt qua danh sách bài hát trong Playlist**, gồm:
1. **Bài hát (Song)**: Chứa tên và ca sĩ.
2. **Playlist**: Chứa danh sách các bài hát.
3. **PlaylistIterator**: Giúp duyệt từng bài hát một.

👉 **Mục tiêu**: Duyệt qua danh sách phát mà không cần biết dữ liệu lưu trữ bên trong.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀