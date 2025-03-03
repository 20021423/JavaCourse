# **Builder Design Pattern trong Java**

---

## **1. Builder Pattern là gì?**
**Builder Pattern** là một **mẫu thiết kế sáng tạo (Creational Pattern)** giúp **tạo đối tượng phức tạp** bằng cách **xây dựng từng phần nhỏ**, thay vì truyền một danh sách tham số dài vào constructor.

💡 **Ý tưởng chính:**
- **Tránh việc truyền quá nhiều tham số vào constructor** (Constructor Hell).
- **Tách biệt quá trình xây dựng đối tượng** khỏi lớp đối tượng chính, giúp code dễ đọc hơn.
- **Hỗ trợ tạo đối tượng với nhiều cấu hình khác nhau** mà không cần nhiều constructor.

---

## **2. Khi nào sử dụng Builder Pattern?**
✅ Khi đối tượng có **quá nhiều tham số cần khởi tạo**.  
✅ Khi cần **tạo đối tượng theo nhiều cấu hình khác nhau**.  
✅ Khi muốn **code dễ đọc hơn**, không cần nhớ thứ tự tham số trong constructor.

📌 **Ví dụ thực tế:**  
🔹 **Tạo một tài khoản người dùng**: Có thể có hoặc không có Email, SĐT, Địa chỉ.  
🔹 **Xây dựng một chiếc xe hơi**: Có thể chọn động cơ, màu sắc, số ghế tùy ý.  
🔹 **Cấu hình một báo cáo**: Có thể thêm tiêu đề, biểu đồ, dữ liệu thống kê, v.v.  
🔹 **Xây dựng một chuỗi SQL Query**: Có thể chọn bảng, điều kiện lọc, thứ tự sắp xếp.

---

## **3. Cấu trúc của Builder Pattern**
Builder Pattern có **4 thành phần chính**:

| **Thành phần** | **Vai trò** |
|--------------|------------|
| **Product** | Đối tượng phức tạp cần xây dựng. |
| **Builder (interface)** | Định nghĩa các phương thức để thiết lập từng phần của `Product`. |
| **ConcreteBuilder** | Cài đặt cụ thể của `Builder`, giúp tạo `Product` theo từng bước. |
| **Director (Tùy chọn)** | Điều phối quá trình xây dựng `Product` theo một thứ tự xác định. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Client không tạo đối tượng trực tiếp**, mà gọi `Builder`.  
2️⃣ **Builder tạo từng phần của đối tượng**, giúp tùy chỉnh linh hoạt.  
3️⃣ **Cuối cùng, `build()` trả về đối tượng hoàn chỉnh**.

---

## **4. Triển khai Builder Pattern trong Java**
### **Ví dụ: Xây dựng một tài khoản người dùng (`User`)**
Hệ thống hỗ trợ **tạo tài khoản người dùng**, với các thông tin **tùy chọn**:
- **Bắt buộc**: `name`, `email`.
- **Tùy chọn**: `phone`, `address`, `age`.

---

### **Bước 1: Tạo lớp `User` (Product)**
```java
class User {
    private final String name;  // Bắt buộc
    private final String email; // Bắt buộc
    private final String phone; // Tùy chọn
    private final String address; // Tùy chọn
    private final int age; // Tùy chọn

    // Constructor riêng tư, chỉ có thể tạo bằng Builder
    private User(UserBuilder builder) {
        this.name = builder.name;
        this.email = builder.email;
        this.phone = builder.phone;
        this.address = builder.address;
        this.age = builder.age;
    }

    @Override
    public String toString() {
        return "User{name='" + name + "', email='" + email + "', phone='" + phone +
               "', address='" + address + "', age=" + age + "}";
    }

    // Getter (nếu cần)
    public String getName() { return name; }
    public String getEmail() { return email; }
    public String getPhone() { return phone; }
    public String getAddress() { return address; }
    public int getAge() { return age; }

    // Nested static class (Builder)
    public static class UserBuilder {
        private final String name;  
        private final String email;
        private String phone = ""; 
        private String address = ""; 
        private int age = 0; 

        // Constructor bắt buộc name và email
        public UserBuilder(String name, String email) {
            this.name = name;
            this.email = email;
        }

        // Phương thức thiết lập giá trị tùy chọn
        public UserBuilder setPhone(String phone) {
            this.phone = phone;
            return this;
        }

        public UserBuilder setAddress(String address) {
            this.address = address;
            return this;
        }

        public UserBuilder setAge(int age) {
            this.age = age;
            return this;
        }

        // Phương thức xây dựng đối tượng User
        public User build() {
            return new User(this);
        }
    }
}
```
🔹 **`User` có constructor `private`**, chỉ có thể khởi tạo thông qua `UserBuilder`.  
🔹 **Mọi phương thức trong `UserBuilder` đều trả về `this`**, giúp gọi liên tiếp (`method chaining`).

---

### **Bước 2: Kiểm thử Builder Pattern**
```java
public class BuilderPatternDemo {
    public static void main(String[] args) {
        // Tạo User chỉ với thông tin bắt buộc
        User user1 = new User.UserBuilder("Alice", "alice@example.com")
                             .build();

        // Tạo User với thông tin đầy đủ
        User user2 = new User.UserBuilder("Bob", "bob@example.com")
                             .setPhone("0123456789")
                             .setAddress("123 Đường ABC")
                             .setAge(25)
                             .build();

        System.out.println(user1);
        System.out.println(user2);
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
User{name='Alice', email='alice@example.com', phone='', address='', age=0}
User{name='Bob', email='bob@example.com', phone='0123456789', address='123 Đường ABC', age=25}
```
📌 **Nhận xét:**
- **Không cần nhớ thứ tự tham số** khi khởi tạo `User`.
- **Dễ dàng mở rộng**, nếu cần thêm trường mới, chỉ cần cập nhật `UserBuilder`.
- **Không cần nhiều constructor quá tải (constructor overloading)**.

---

## **6. Ứng dụng thực tế của Builder Pattern**
✅ **Tạo tài khoản người dùng** 🏫 (Có thể có hoặc không có số điện thoại, địa chỉ, tuổi).  
✅ **Cấu hình xe hơi** 🚗 (Chọn động cơ, màu sắc, số ghế).  
✅ **Tạo báo cáo** 📊 (Tiêu đề, dữ liệu, biểu đồ, footer).  
✅ **Xây dựng chuỗi SQL Query** 📜 (SELECT, WHERE, ORDER BY, GROUP BY).

---

## **7. Ưu điểm & Nhược điểm của Builder Pattern**
✅ **Ưu điểm:**  
✔️ **Dễ đọc và dễ hiểu**, loại bỏ constructor có quá nhiều tham số.  
✔️ **Linh hoạt**, có thể tạo đối tượng với các cấu hình khác nhau.  
✔️ **Dễ mở rộng**, thêm thuộc tính mới mà không ảnh hưởng đến mã cũ.

❌ **Nhược điểm:**  
⚠️ **Tạo nhiều class (Builder)**, có thể làm tăng độ phức tạp nếu đối tượng đơn giản.  
⚠️ **Hiệu suất thấp hơn so với constructor thông thường**, vì tạo thêm `Builder` object.

---

## **8. Bài tập thực hành**
Hãy triển khai một **hệ thống tạo xe hơi (`CarBuilder`)**, gồm:
1. **Bắt buộc**: `brand`, `model`.
2. **Tùy chọn**: `color`, `engineType`, `gps`.

👉 **Mục tiêu**: Dùng `CarBuilder` để tạo xe hơi mà không cần constructor quá tải.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀