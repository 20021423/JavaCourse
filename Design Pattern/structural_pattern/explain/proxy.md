# **Proxy Design Pattern trong Java**

---

## **1. Proxy Pattern là gì?**
**Proxy Pattern** là một **mẫu thiết kế cấu trúc (Structural Pattern)** cho phép **tạo một lớp thay thế (Proxy) để kiểm soát việc truy cập đến một đối tượng thực sự (Real Object)**.

💡 **Ý tưởng chính:**
- **Proxy đóng vai trò trung gian** giữa client và đối tượng thực, giúp kiểm soát quyền truy cập, tối ưu hiệu suất, hoặc ghi log trước khi gọi đối tượng gốc.
- **Client không tương tác trực tiếp với đối tượng thật**, mà thông qua `Proxy`.

---

## **2. Khi nào sử dụng Proxy Pattern?**
✅ Khi cần **kiểm soát quyền truy cập** đến một đối tượng (bảo mật, xác thực).  
✅ Khi đối tượng thực sự **tốn tài nguyên để khởi tạo**, cần `Lazy Loading`.  
✅ Khi cần **ghi log, cache, hoặc kiểm tra đầu vào trước khi gọi đối tượng thật**.  
✅ Khi đối tượng thực sự **nằm trên một máy khác** (Remote Proxy, RMI).

📌 **Ví dụ thực tế:**  
🔹 **Hệ thống bảo mật** 🔐: Proxy kiểm tra quyền truy cập trước khi gọi API nội bộ.  
🔹 **Tải hình ảnh từ Internet** 🌐: Proxy chỉ tải hình khi thực sự cần thiết.  
🔹 **Cơ sở dữ liệu (Database Connection Pooling)** 🗄: Proxy quản lý số lượng kết nối đến DB.  
🔹 **Mạng xã hội (Instagram, Facebook API)** 📸: Proxy kiểm tra token trước khi lấy dữ liệu từ API.

---

## **3. Cấu trúc của Proxy Pattern**
Proxy Pattern có **3 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Subject (interface/abstract class)** | Định nghĩa phương thức chung cho Proxy và Real Object. |
| **RealSubject** | Đối tượng thật, thực hiện công việc thực tế. |
| **Proxy** | Kiểm soát truy cập vào `RealSubject`, có thể cache, kiểm tra quyền, ghi log. |

📌 **Quy tắc hoạt động:**  
1️⃣ **Client không gọi trực tiếp `RealSubject`**, mà thông qua `Proxy`.  
2️⃣ `Proxy` có thể **tạo `RealSubject` khi cần (Lazy Loading)**, hoặc **chặn truy cập nếu không hợp lệ**.  
3️⃣ Nếu mọi thứ hợp lệ, `Proxy` chuyển tiếp yêu cầu đến `RealSubject`.

---

## **4. Triển khai Proxy Pattern trong Java**
### **Ví dụ: Hệ thống tải ảnh từ Internet**
Hệ thống cần tải ảnh từ URL, nhưng để tối ưu hiệu suất:
- **Ảnh chỉ được tải khi thực sự cần thiết** → **Lazy Loading Proxy**.
- **Nếu ảnh đã tải, Proxy sử dụng lại** thay vì tải lại từ Internet.

---

### **Bước 1: Tạo interface `Image` (Subject)**
```java
interface Image {
    void display();
}
```
🔹 **Mọi hình ảnh đều có phương thức `display()`.**

---

### **Bước 2: Cài đặt `RealSubject` (Hình ảnh thực tế)**
```java
class RealImage implements Image {
    private String filename;

    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk(); // Giả lập tải ảnh từ ổ cứng hoặc internet
    }

    private void loadFromDisk() {
        System.out.println("🖼 Đang tải ảnh từ ổ cứng: " + filename);
    }

    @Override
    public void display() {
        System.out.println("📸 Hiển thị ảnh: " + filename);
    }
}
```
🔹 **`RealImage` thực sự tải ảnh khi được khởi tạo (`loadFromDisk()`).**

---

### **Bước 3: Tạo `Proxy` (Ảnh có cơ chế tải lười)**
```java
class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;

    public ProxyImage(String filename) {
        this.filename = filename;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename); // Tải ảnh khi cần
        }
        realImage.display(); // Hiển thị ảnh (có thể dùng lại nếu đã tải)
    }
}
```
🔹 **`ProxyImage` không tải ảnh ngay khi được tạo, chỉ tải khi gọi `display()`.**

---

### **Bước 4: Kiểm thử Proxy Pattern**
```java
public class ProxyPatternDemo {
    public static void main(String[] args) {
        Image image1 = new ProxyImage("photo1.jpg");
        Image image2 = new ProxyImage("photo2.jpg");

        // Lần đầu gọi display() -> ảnh sẽ được tải
        System.out.println("\n📌 Hiển thị ảnh 1 lần đầu:");
        image1.display();

        // Lần thứ hai gọi display() -> ảnh đã tải từ trước, không tải lại
        System.out.println("\n📌 Hiển thị ảnh 1 lần thứ hai:");
        image1.display();

        // Hiển thị ảnh khác
        System.out.println("\n📌 Hiển thị ảnh 2:");
        image2.display();
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
📌 Hiển thị ảnh 1 lần đầu:
🖼 Đang tải ảnh từ ổ cứng: photo1.jpg
📸 Hiển thị ảnh: photo1.jpg

📌 Hiển thị ảnh 1 lần thứ hai:
📸 Hiển thị ảnh: photo1.jpg

📌 Hiển thị ảnh 2:
🖼 Đang tải ảnh từ ổ cứng: photo2.jpg
📸 Hiển thị ảnh: photo2.jpg
```
📌 **Nhận xét:**
- **Ảnh chỉ được tải một lần duy nhất**, sau đó `ProxyImage` sử dụng lại mà không tải lại từ ổ cứng.
- **Cải thiện hiệu suất** trong trường hợp có hàng ngàn ảnh cần hiển thị.

---

## **6. Các loại Proxy Pattern phổ biến**
### **1️⃣ Virtual Proxy (Lazy Loading)**
- **Tạo đối tượng thật chỉ khi cần thiết** (như ví dụ tải ảnh trên).

### **2️⃣ Protection Proxy (Bảo mật)**
- **Chặn truy cập nếu không có quyền**, thường dùng trong hệ thống bảo mật.
```java
class SecureProxy implements Image {
    private RealImage realImage;
    private String filename;
    private boolean isAuthenticated;

    public SecureProxy(String filename, boolean isAuthenticated) {
        this.filename = filename;
        this.isAuthenticated = isAuthenticated;
    }

    @Override
    public void display() {
        if (!isAuthenticated) {
            System.out.println("🚫 Truy cập bị từ chối!");
            return;
        }
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}
```
🔹 **Chỉ hiển thị ảnh nếu người dùng đã đăng nhập (`isAuthenticated == true`).**

### **3️⃣ Remote Proxy (Gọi API từ xa)**
- **Đại diện cho đối tượng nằm trên máy khác (RMI, Web Service, REST API)**.

---

## **7. Ứng dụng thực tế của Proxy Pattern**
✅ **Lazy Loading** 🕐 (Tải ảnh, tải video khi cần).  
✅ **Bảo mật (Security Proxy)** 🔐 (Kiểm tra quyền truy cập API, Database).  
✅ **Ghi log hoặc theo dõi hệ thống (Logging, Monitoring)** 📊.  
✅ **Tối ưu kết nối mạng (Remote Proxy)** 🌍 (Gọi API, RMI, gRPC).  
✅ **Quản lý kết nối Database (Database Proxy)** 🗄 (Hạn chế số lượng kết nối).

---

## **8. Ưu điểm & Nhược điểm của Proxy Pattern**
✅ **Ưu điểm:**  
✔️ **Giảm tải tài nguyên**, chỉ tạo đối tượng khi cần thiết.  
✔️ **Cải thiện bảo mật**, kiểm soát truy cập trước khi gọi API hoặc Database.  
✔️ **Tối ưu kết nối mạng**, giúp gọi API từ xa mà không ảnh hưởng đến client.

❌ **Nhược điểm:**  
⚠️ **Có thể làm giảm hiệu suất**, nếu Proxy thêm quá nhiều bước xử lý.  
⚠️ **Tăng độ phức tạp**, vì cần thêm một lớp trung gian.

---

## **9. Bài tập thực hành**
Hãy triển khai một **hệ thống Proxy bảo vệ truy cập tài liệu (`DocumentProxy`)**, gồm:
1. **Chặn truy cập nếu không có quyền (`isAuthenticated == false`)**.
2. **Chỉ tải tài liệu từ server nếu được phép truy cập**.

👉 **Mục tiêu**: Kiểm soát truy cập vào tài liệu PDF trong hệ thống bảo mật. 🚀