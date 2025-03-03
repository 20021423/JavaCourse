# **Singleton Design Pattern trong Java**

---

## **1. Singleton Pattern là gì?**
**Singleton Pattern** là một **mẫu thiết kế sáng tạo (Creational Pattern)** đảm bảo rằng **một lớp chỉ có một thể hiện (instance) duy nhất và cung cấp một cách để truy cập nó**.

💡 **Ý tưởng chính:**
- **Giới hạn số lượng thể hiện (instance) của một lớp** chỉ có **một** trong toàn bộ chương trình.
- **Cung cấp một phương thức truy cập thể hiện duy nhất** từ bất kỳ đâu trong chương trình.

---

## **2. Khi nào sử dụng Singleton Pattern?**
✅ Khi cần **đảm bảo chỉ có một thể hiện duy nhất** của một lớp trong toàn bộ ứng dụng.  
✅ Khi cần **một điểm truy cập toàn cục** đến thể hiện của lớp.  
✅ Khi **tiết kiệm tài nguyên**, tránh tạo nhiều đối tượng không cần thiết.

📌 **Ví dụ thực tế:**  
🔹 **Kết nối cơ sở dữ liệu**: Mỗi ứng dụng chỉ cần một kết nối DB duy nhất.  
🔹 **Bộ nhớ cache**: Tránh tạo lại dữ liệu nhiều lần không cần thiết.  
🔹 **Trình quản lý tài nguyên (Resource Manager)**: Quản lý bộ nhớ, log, thiết bị in ấn.  
🔹 **Logger (Ghi log)**: Mọi phần của chương trình đều ghi log vào cùng một file.

---

## **3. Cách triển khai Singleton Pattern**
Có **nhiều cách** để triển khai Singleton trong Java, dưới đây là các cách phổ biến:

---

### **Cách 1: Eager Initialization (Khởi tạo sẵn)**
```java
class SingletonEager {
    private static final SingletonEager INSTANCE = new SingletonEager();

    private SingletonEager() {
        System.out.println("⚡ SingletonEager instance được tạo!");
    }

    public static SingletonEager getInstance() {
        return INSTANCE;
    }
}
```
✅ **Ưu điểm**: Dễ triển khai, thread-safe do JVM đảm bảo.  
❌ **Nhược điểm**: Đối tượng được tạo ngay cả khi không sử dụng, gây lãng phí tài nguyên.

---

### **Cách 2: Lazy Initialization (Chỉ khởi tạo khi cần)**
```java
class SingletonLazy {
    private static SingletonLazy instance;

    private SingletonLazy() {
        System.out.println("🕐 SingletonLazy instance được tạo!");
    }

    public static SingletonLazy getInstance() {
        if (instance == null) {
            instance = new SingletonLazy();
        }
        return instance;
    }
}
```
✅ **Ưu điểm**: Chỉ tạo khi cần, tiết kiệm tài nguyên.  
❌ **Nhược điểm**: **Không thread-safe**, nếu nhiều thread gọi `getInstance()` cùng lúc, có thể tạo nhiều thể hiện.

---

### **Cách 3: Thread-safe Singleton (Synchronized Method)**
```java
class SingletonThreadSafe {
    private static SingletonThreadSafe instance;

    private SingletonThreadSafe() {
        System.out.println("🔒 SingletonThreadSafe instance được tạo!");
    }

    public static synchronized SingletonThreadSafe getInstance() {
        if (instance == null) {
            instance = new SingletonThreadSafe();
        }
        return instance;
    }
}
```
✅ **Ưu điểm**: **Thread-safe**, đảm bảo chỉ có một thể hiện duy nhất.  
❌ **Nhược điểm**: **Hiệu suất thấp**, vì `synchronized` làm chậm hiệu năng khi nhiều thread gọi `getInstance()`.

---

### **Cách 4: Double-Checked Locking (Hiệu suất tốt hơn)**
```java
class SingletonDoubleChecked {
    private static volatile SingletonDoubleChecked instance;

    private SingletonDoubleChecked() {
        System.out.println("💡 SingletonDoubleChecked instance được tạo!");
    }

    public static SingletonDoubleChecked getInstance() {
        if (instance == null) {
            synchronized (SingletonDoubleChecked.class) {
                if (instance == null) {
                    instance = new SingletonDoubleChecked();
                }
            }
        }
        return instance;
    }
}
```
✅ **Ưu điểm**: **Thread-safe và hiệu suất cao**, vì chỉ đồng bộ khi cần thiết.  
❌ **Nhược điểm**: Cần hiểu rõ về `volatile` và `synchronized` để tránh lỗi.

---

### **Cách 5: Bill Pugh Singleton (Best Practice)**
```java
class SingletonBillPugh {
    private SingletonBillPugh() {
        System.out.println("🎯 SingletonBillPugh instance được tạo!");
    }

    private static class SingletonHelper {
        private static final SingletonBillPugh INSTANCE = new SingletonBillPugh();
    }

    public static SingletonBillPugh getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```
✅ **Ưu điểm**: **Thread-safe, hiệu suất cao, lazy initialization** mà không cần `synchronized`.  
✅ **Được khuyên dùng trong thực tế**.

---

## **4. Kiểm thử Singleton Pattern**
```java
public class SingletonPatternDemo {
    public static void main(String[] args) {
        // Eager Singleton
        SingletonEager eager = SingletonEager.getInstance();
        SingletonEager eager2 = SingletonEager.getInstance();
        System.out.println("Eager Singleton: " + (eager == eager2)); // true

        // Lazy Singleton
        SingletonLazy lazy = SingletonLazy.getInstance();
        SingletonLazy lazy2 = SingletonLazy.getInstance();
        System.out.println("Lazy Singleton: " + (lazy == lazy2)); // true

        // Thread-safe Singleton
        SingletonThreadSafe threadSafe = SingletonThreadSafe.getInstance();
        SingletonThreadSafe threadSafe2 = SingletonThreadSafe.getInstance();
        System.out.println("Thread-safe Singleton: " + (threadSafe == threadSafe2)); // true

        // Double-Checked Locking
        SingletonDoubleChecked doubleChecked = SingletonDoubleChecked.getInstance();
        SingletonDoubleChecked doubleChecked2 = SingletonDoubleChecked.getInstance();
        System.out.println("Double-Checked Singleton: " + (doubleChecked == doubleChecked2)); // true

        // Bill Pugh Singleton
        SingletonBillPugh billPugh = SingletonBillPugh.getInstance();
        SingletonBillPugh billPugh2 = SingletonBillPugh.getInstance();
        System.out.println("Bill Pugh Singleton: " + (billPugh == billPugh2)); // true
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
⚡ SingletonEager instance được tạo!
Eager Singleton: true

🕐 SingletonLazy instance được tạo!
Lazy Singleton: true

🔒 SingletonThreadSafe instance được tạo!
Thread-safe Singleton: true

💡 SingletonDoubleChecked instance được tạo!
Double-Checked Singleton: true

🎯 SingletonBillPugh instance được tạo!
Bill Pugh Singleton: true
```
📌 **Nhận xét:**
- Mỗi phương thức đều đảm bảo **chỉ tạo một thể hiện duy nhất**.
- **Bill Pugh Singleton** là lựa chọn tối ưu nhất trong thực tế.

---

## **6. Ứng dụng thực tế của Singleton Pattern**
✅ **Kết nối cơ sở dữ liệu (Database Connection Pooling)** 📂.  
✅ **Ghi log hệ thống (Logger)** 📜.  
✅ **Tải cấu hình hệ thống (Configuration Manager)** ⚙️.  
✅ **Quản lý bộ nhớ cache (Cache Manager)** 🚀.  
✅ **Điều khiển tài nguyên phần cứng (Printer Spooler, Thread Pool)** 🖨.

---

## **7. Ưu điểm & Nhược điểm của Singleton Pattern**
✅ **Ưu điểm:**  
✔️ **Đảm bảo chỉ có một thể hiện duy nhất**, tránh xung đột dữ liệu.  
✔️ **Cung cấp một điểm truy cập toàn cục**, giúp dễ quản lý tài nguyên.  
✔️ **Tiết kiệm bộ nhớ**, tránh tạo đối tượng dư thừa.

❌ **Nhược điểm:**  
⚠️ **Có thể gây vấn đề trong môi trường đa luồng**, nếu không triển khai đúng cách.  
⚠️ **Gây khó khăn khi viết Unit Test**, vì Singleton giữ trạng thái tĩnh.  
⚠️ **Làm tăng sự phụ thuộc giữa các thành phần**, nếu sử dụng không đúng cách.

---

## **8. Bài tập thực hành**
Hãy triển khai một **hệ thống ghi log duy nhất (`Logger`)**, có thể:
1. **Ghi log thông tin (INFO), cảnh báo (WARNING), lỗi (ERROR)**.
2. **Chỉ có một instance duy nhất trong toàn bộ ứng dụng**.

👉 **Mục tiêu**: Mọi phần trong ứng dụng đều ghi log vào cùng một file.

Bạn có muốn tự làm thử hay mình sẽ hướng dẫn từng bước? 🚀