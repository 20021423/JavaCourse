# **Flyweight Design Pattern trong Java**

---

## **1. Flyweight Pattern là gì?**
**Flyweight Pattern** là một **mẫu thiết kế cấu trúc (Structural Pattern)** giúp **giảm thiểu việc sử dụng bộ nhớ bằng cách chia sẻ các đối tượng giống nhau** thay vì tạo mới chúng mỗi lần.

💡 **Ý tưởng chính:**
- **Lưu trữ và tái sử dụng các đối tượng có thể chia sẻ**, thay vì tạo mới liên tục.
- **Tối ưu hóa hiệu suất trong các hệ thống cần tạo ra hàng triệu đối tượng giống nhau**.

---

## **2. Khi nào sử dụng Flyweight Pattern?**
✅ Khi hệ thống cần **tạo và sử dụng số lượng lớn đối tượng giống nhau**.  
✅ Khi muốn **giảm sử dụng bộ nhớ** và **tăng hiệu suất** bằng cách chia sẻ dữ liệu giữa các đối tượng.  
✅ Khi có nhiều **đối tượng chỉ khác nhau một phần nhỏ** (thuộc tính có thể tách biệt).

📌 **Ví dụ thực tế:**  
🔹 **Trò chơi 2D/3D (Game Engine)** 🎮: Chia sẻ sprite nhân vật, texture cây cỏ, vật phẩm để tiết kiệm RAM.  
🔹 **Trình soạn thảo văn bản (Text Editor)** 📝: Chia sẻ font chữ, màu sắc thay vì tạo mới cho từng ký tự.  
🔹 **Hệ thống bản đồ (GIS, Google Maps)** 🗺: Tái sử dụng biểu tượng (icon) của địa điểm, tuyến đường.  
🔹 **Mạng xã hội (Social Media)** 🌐: Chia sẻ ảnh đại diện thay vì lưu trữ bản sao cho mỗi người dùng.

---

## **3. Cấu trúc của Flyweight Pattern**
Flyweight Pattern có **4 thành phần chính**:

| **Thành phần**  | **Vai trò** |
|--------------|-----------|
| **Flyweight (interface/abstract class)** | Định nghĩa phương thức chung cho tất cả đối tượng có thể chia sẻ. |
| **ConcreteFlyweight** | Cài đặt cụ thể của `Flyweight`, chứa trạng thái có thể chia sẻ. |
| **FlyweightFactory** | Quản lý danh sách `Flyweight`, tái sử dụng thay vì tạo mới. |
| **Client** | Sử dụng `FlyweightFactory` để lấy `Flyweight` thay vì tạo mới. |

📌 **Quy tắc hoạt động:**  
1️⃣ `FlyweightFactory` kiểm tra xem `Flyweight` đã tồn tại chưa, nếu có thì trả về đối tượng cũ.  
2️⃣ Nếu `Flyweight` chưa tồn tại, Factory tạo mới và lưu trữ để dùng lại.  
3️⃣ **Chỉ lưu trữ các trạng thái dùng chung**, còn trạng thái riêng biệt sẽ do client quản lý.

---

## **4. Triển khai Flyweight Pattern trong Java**
### **Ví dụ: Hệ thống vẽ hình (Drawing App)**
Hệ thống cần **vẽ hàng triệu hình tròn (`Circle`) với cùng màu sắc**, nhưng tọa độ khác nhau.
- **Trạng thái chia sẻ (Intrinsic State)**: `color` (màu sắc).
- **Trạng thái không chia sẻ (Extrinsic State)**: `x`, `y`, `radius` (tọa độ, kích thước).

---

### **Bước 1: Tạo `Flyweight` (Giao diện Shape)**
```java
interface Shape {
    void draw(int x, int y, int radius);
}
```
🔹 **Tất cả hình vẽ phải triển khai `draw(x, y, radius)`.**

---

### **Bước 2: Cài đặt `ConcreteFlyweight` (Hình tròn)**
```java
class Circle implements Shape {
    private final String color; // Trạng thái có thể chia sẻ (Intrinsic State)

    public Circle(String color) {
        this.color = color;
    }

    @Override
    public void draw(int x, int y, int radius) {
        System.out.println("🎨 Vẽ hình tròn [Màu: " + color + ", Tọa độ: (" + x + ", " + y + "), Bán kính: " + radius + "]");
    }
}
```
🔹 **`color` được lưu trữ trong `Circle`, nhưng `x, y, radius` chỉ được truyền khi vẽ.**

---

### **Bước 3: Tạo `FlyweightFactory` để quản lý các đối tượng Circle**
```java
import java.util.HashMap;
import java.util.Map;

class ShapeFactory {
    private static final Map<String, Shape> circleMap = new HashMap<>();

    public static Shape getCircle(String color) {
        Shape circle = circleMap.get(color);

        if (circle == null) {
            circle = new Circle(color);
            circleMap.put(color, circle);
            System.out.println("🆕 Tạo mới hình tròn màu " + color);
        } else {
            System.out.println("♻️ Tái sử dụng hình tròn màu " + color);
        }

        return circle;
    }
}
```
🔹 **`ShapeFactory` kiểm tra xem Circle có tồn tại không. Nếu có, tái sử dụng nó thay vì tạo mới.**

---

### **Bước 4: Kiểm thử Flyweight Pattern**
```java
public class FlyweightPatternDemo {
    private static final String[] colors = {"Đỏ", "Xanh", "Vàng", "Đen", "Trắng"};

    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            String color = colors[i % colors.length]; // Chọn màu ngẫu nhiên từ danh sách
            Shape circle = ShapeFactory.getCircle(color);
            circle.draw(randomNumber(), randomNumber(), 10);
        }
    }

    private static int randomNumber() {
        return (int) (Math.random() * 100);
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
🆕 Tạo mới hình tròn màu Đỏ
🎨 Vẽ hình tròn [Màu: Đỏ, Tọa độ: (23, 45), Bán kính: 10]
🆕 Tạo mới hình tròn màu Xanh
🎨 Vẽ hình tròn [Màu: Xanh, Tọa độ: (78, 12), Bán kính: 10]
🆕 Tạo mới hình tròn màu Vàng
🎨 Vẽ hình tròn [Màu: Vàng, Tọa độ: (35, 67), Bán kính: 10]
♻️ Tái sử dụng hình tròn màu Đỏ
🎨 Vẽ hình tròn [Màu: Đỏ, Tọa độ: (19, 34), Bán kính: 10]
♻️ Tái sử dụng hình tròn màu Xanh
🎨 Vẽ hình tròn [Màu: Xanh, Tọa độ: (90, 76), Bán kính: 10]
...
```
📌 **Nhận xét:**
- **Chỉ tạo hình tròn một lần cho mỗi màu**, sau đó **tái sử dụng để tiết kiệm bộ nhớ**.
- **Hiệu suất cải thiện đáng kể nếu có hàng triệu hình vẽ giống nhau**.

---

## **6. Ứng dụng thực tế của Flyweight Pattern**
✅ **Game Engine** 🎮 (Tái sử dụng sprite, texture, vật phẩm).  
✅ **Trình soạn thảo văn bản** 📝 (Chia sẻ font chữ, màu sắc).  
✅ **Bản đồ (GIS, Google Maps)** 🗺 (Biểu tượng địa điểm, tuyến đường).  
✅ **Hệ thống mạng xã hội** 🌐 (Avatar người dùng, biểu tượng cảm xúc).  
✅ **Ứng dụng đồ họa (Photoshop, Figma)** 🎨 (Các hình vẽ lặp lại).

---

## **7. Ưu điểm & Nhược điểm của Flyweight Pattern**
✅ **Ưu điểm:**  
✔️ **Giảm sử dụng bộ nhớ**, đặc biệt trong hệ thống có nhiều đối tượng giống nhau.  
✔️ **Tăng hiệu suất**, vì không phải tạo mới đối tượng liên tục.  
✔️ **Dễ bảo trì**, vì dữ liệu dùng chung được quản lý tập trung.

❌ **Nhược điểm:**  
⚠️ **Có thể làm tăng độ phức tạp**, cần tách biệt trạng thái chia sẻ và không chia sẻ.  
⚠️ **Không phải lúc nào cũng hiệu quả**, nếu đối tượng có quá nhiều trạng thái khác nhau.

---

## **8. Bài tập thực hành**
Hãy triển khai một **hệ thống quản lý biểu tượng bản đồ (`MapIconFactory`)**, gồm:
1. **Icon chia sẻ (Intrinsic State)**: Loại địa điểm (`Nhà hàng`, `Khách sạn`, `Cửa hàng`).
2. **Vị trí riêng biệt (Extrinsic State)**: `latitude`, `longitude`.

👉 **Mục tiêu**: Dùng Flyweight để tiết kiệm bộ nhớ khi hiển thị hàng nghìn điểm trên bản đồ. 🚀