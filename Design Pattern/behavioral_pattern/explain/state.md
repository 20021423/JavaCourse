# **State Design Pattern trong Java**

## **1. Định nghĩa**
**State Pattern** là một mẫu thiết kế thuộc nhóm **Behavioral Patterns**, giúp một đối tượng **thay đổi hành vi** của mình khi trạng thái bên trong nó thay đổi, mà không làm ảnh hưởng đến các phần khác của chương trình.

👉 **Mô phỏng hệ thống có nhiều trạng thái khác nhau**, như máy bán hàng tự động, trò chơi, hoặc hệ thống đặt vé.

---

## **2. Khi nào sử dụng?**
✅ Khi một đối tượng có **nhiều trạng thái khác nhau** và hành vi của nó thay đổi theo từng trạng thái.  
✅ Khi bạn muốn **tránh sử dụng quá nhiều câu lệnh `if-else` hoặc `switch-case`** để xử lý trạng thái.  
✅ Khi hệ thống cần **mở rộng dễ dàng**, chỉ cần thêm trạng thái mới mà không cần sửa đổi mã nguồn hiện có.

📌 **Ví dụ thực tế:**  
🔹 **Máy ATM** 🏧  
→ Khi nhập sai mã PIN, máy sẽ chuyển sang trạng thái "bị khóa".  
🔹 **Máy bán hàng tự động** 🛒  
→ Khi người dùng nhét tiền vào, máy sẽ chuyển sang trạng thái "đã nhận tiền".  
🔹 **Trình phát nhạc** 🎵  
→ Khi nhấn "Play", trình phát chuyển từ trạng thái "Paused" sang "Playing".

---

## **3. Cấu trúc của State Pattern**
State Pattern có **3 thành phần chính**:

| Thành phần  | Vai trò  |
|-------------|---------|
| **State (interface)** | Định nghĩa hành vi chung cho tất cả các trạng thái |
| **ConcreteState** | Cài đặt hành vi cụ thể cho từng trạng thái |
| **Context** | Chứa trạng thái hiện tại và thay đổi hành vi dựa trên trạng thái đó |

📌 **Quy tắc hoạt động:**  
1️⃣ `Context` chứa tham chiếu đến trạng thái hiện tại.  
2️⃣ Khi trạng thái thay đổi, `Context` chuyển sang một `ConcreteState` khác.  
3️⃣ `ConcreteState` mới sẽ quyết định cách `Context` hoạt động.

---

## **4. Triển khai State Pattern trong Java**
### **Ví dụ: Máy bán hàng tự động**
Hệ thống có các trạng thái:
- `NoCoinState` (chưa có tiền)
- `HasCoinState` (đã nhận tiền)
- `DispensingState` (đang trả hàng)

### **Bước 1: Tạo Interface `State`**
```java
interface State {
    void insertCoin(VendingMachine machine);
    void pressButton(VendingMachine machine);
    void dispense(VendingMachine machine);
}
```
🔹 Định nghĩa các hành vi mà mọi trạng thái đều có.

---

### **Bước 2: Cài đặt các trạng thái cụ thể (`ConcreteState`)**
#### **Trạng thái 1: `NoCoinState` (Chưa có tiền)**
```java
class NoCoinState implements State {
    @Override
    public void insertCoin(VendingMachine machine) {
        System.out.println("💰 Tiền đã được đưa vào.");
        machine.setState(new HasCoinState());
    }

    @Override
    public void pressButton(VendingMachine machine) {
        System.out.println("⚠ Vui lòng đưa tiền vào trước.");
    }

    @Override
    public void dispense(VendingMachine machine) {
        System.out.println("⚠ Không thể trả hàng khi chưa nhận tiền.");
    }
}
```
🔹 Nếu chưa bỏ tiền, không thể nhấn nút hoặc lấy hàng.

#### **Trạng thái 2: `HasCoinState` (Đã nhận tiền)**
```java
class HasCoinState implements State {
    @Override
    public void insertCoin(VendingMachine machine) {
        System.out.println("⚠ Máy đã nhận tiền rồi.");
    }

    @Override
    public void pressButton(VendingMachine machine) {
        System.out.println("✅ Bạn đã nhấn nút chọn hàng.");
        machine.setState(new DispensingState());
    }

    @Override
    public void dispense(VendingMachine machine) {
        System.out.println("⚠ Hãy nhấn nút trước khi nhận hàng.");
    }
}
```
🔹 Khi nhấn nút, máy chuyển sang trạng thái "Đang trả hàng".

#### **Trạng thái 3: `DispensingState` (Đang trả hàng)**
```java
class DispensingState implements State {
    @Override
    public void insertCoin(VendingMachine machine) {
        System.out.println("⚠ Vui lòng đợi máy trả hàng trước khi đưa thêm tiền.");
    }

    @Override
    public void pressButton(VendingMachine machine) {
        System.out.println("⚠ Đang xử lý đơn hàng, vui lòng chờ...");
    }

    @Override
    public void dispense(VendingMachine machine) {
        System.out.println("🛒 Sản phẩm đã được trả.");
        machine.setState(new NoCoinState());
    }
}
```
🔹 Khi trả hàng xong, máy trở về trạng thái ban đầu.

---

### **Bước 3: Tạo lớp `Context` (Máy bán hàng)**
```java
class VendingMachine {
    private State currentState;

    public VendingMachine() {
        this.currentState = new NoCoinState();
    }

    public void setState(State state) {
        this.currentState = state;
    }

    public void insertCoin() {
        currentState.insertCoin(this);
    }

    public void pressButton() {
        currentState.pressButton(this);
    }

    public void dispense() {
        currentState.dispense(this);
    }
}
```
🔹 `VendingMachine` bắt đầu với trạng thái `NoCoinState`.  
🔹 Nó ủy quyền xử lý sự kiện (`insertCoin()`, `pressButton()`, `dispense()`) cho trạng thái hiện tại.

---

### **Bước 4: Kiểm thử State Pattern**
```java
public class StatePatternDemo {
    public static void main(String[] args) {
        VendingMachine vendingMachine = new VendingMachine();

        vendingMachine.insertCoin();
        vendingMachine.pressButton();
        vendingMachine.dispense();

        // Thử nhấn nút khi chưa bỏ tiền
        vendingMachine.pressButton();
    }
}
```

---

## **5. Kết quả khi chạy chương trình**
```
💰 Tiền đã được đưa vào.
✅ Bạn đã nhấn nút chọn hàng.
🛒 Sản phẩm đã được trả.
⚠ Vui lòng đưa tiền vào trước.
```
📌 **Nhận xét:**
- Khi nhét tiền, trạng thái chuyển sang `HasCoinState`.
- Khi nhấn nút, trạng thái chuyển sang `DispensingState`.
- Khi trả hàng xong, máy quay lại trạng thái `NoCoinState`.

---

## **6. Ứng dụng thực tế của State Pattern**
✅ **Hệ thống đặt vé máy bay** ✈  
→ Người dùng có thể ở trạng thái **"Chờ thanh toán" → "Đã thanh toán" → "Hoàn tất đặt vé"**.  
✅ **Trò chơi điện tử** 🎮  
→ Một nhân vật có thể ở trạng thái **"Chạy" → "Nhảy" → "Tấn công"**.  
✅ **Máy ATM** 🏧  
→ Máy có thể ở trạng thái **"Nhập PIN" → "Rút tiền" → "Giao dịch hoàn tất"**.

---

## **7. Ưu điểm & Nhược điểm của State Pattern**
✅ **Ưu điểm:**  
✔️ **Loại bỏ các câu lệnh `if-else` phức tạp** khi xử lý nhiều trạng thái.  
✔️ **Tách biệt logic từng trạng thái**, giúp mã dễ bảo trì và mở rộng.  
✔️ **Tuân theo nguyên tắc **Open/Closed** (SOLID), dễ dàng thêm trạng thái mới.

❌ **Nhược điểm:**  
⚠️ **Có thể làm tăng số lượng class**, gây khó khăn trong quản lý nếu hệ thống có quá nhiều trạng thái.  
⚠️ **Chuyển đổi trạng thái phức tạp** nếu không thiết kế cẩn thận.

---

## **8. Bài tập thực hành**
Hãy tự triển khai hệ thống **hệ thống đèn giao thông**, gồm các trạng thái:
1. **GreenLight** → Sau 10 giây, chuyển sang **YellowLight**.
2. **YellowLight** → Sau 3 giây, chuyển sang **RedLight**.
3. **RedLight** → Sau 10 giây, chuyển sang **GreenLight**.

Bạn có muốn tự làm trước hay mình sẽ hướng dẫn từng bước? 🚀