# Khóa học Java – Tài liệu Chi Tiết và Thực Chiến

*Chào mừng bạn đến với khóa học Java.* Tài liệu này cung cấp kiến thức chi tiết, dễ hiểu và mang tính thực tiễn cao, bao gồm cả ví dụ minh họa và bài tập thực hành. Nội dung được tổ chức theo các chủ đề chính của khóa học, từ **Design Patterns**, **Lập trình hướng đối tượng (OOP) & SOLID**, **Lập trình đa luồng, xử lý file, lập trình mạng**, đến **Spring Framework**, **Thymeleaf**, **Cơ sở dữ liệu trong Spring**, và kết thúc bằng một **Dự án cuối khóa** tích hợp. Mỗi phần đều có giải thích lý thuyết, ví dụ minh họa, bài tập thực hành và phần ôn tập (quiz) để giúp bạn củng cố kiến thức.

---

## 1. Design Patterns

### Tầm quan trọng của Design Patterns trong lập trình

**Design Pattern (Mẫu thiết kế)** là những giải pháp đã được tối ưu hóa và kiểm chứng, dùng để giải quyết các vấn đề phổ biến trong thiết kế phần mềm hướng đối tượng ([Design Patterns là gì?  Tổng hợp 23 mẫu Design Pattern](https://viblo.asia/p/design-patterns-la-gi-tai-sao-no-lai-la-tro-thu-dac-luc-cua-developers-tong-hop-23-mau-design-pattern-GrLZDBQV5k0#:~:text=,C%C3%A1c%20v%E1%BA%A5n%20%C4%91%E1%BB%81%20m%C3%A0)) ([Giới thiệu Design Patterns - GP Coder (Lập trình Java)](https://gpcoder.com/4164-gioi-thieu-design-patterns/#:~:text=N%C3%B3%20cung%20c%E1%BA%A5p%20cho%20b%E1%BA%A1n,ph%C3%A1p%20trong%20l%E1%BA%ADp%20tr%C3%ACnh%20OOP)). Nói cách khác, design pattern cung cấp cho lập trình viên một công thức hoặc mô hình chung để xử lý những tình huống lặp đi lặp lại, giúp tránh phải “phát minh lại bánh xe” mỗi khi gặp vấn đề tương tự. Việc sử dụng design patterns mang lại nhiều lợi ích quan trọng:

- **Tối ưu và tiết kiệm thời gian:** Vì là giải pháp chuẩn đã được định nghĩa, design patterns giúp giải quyết vấn đề nhanh hơn, tiết kiệm thời gian thiết kế và triển khai ([Java Design Patterns - Example Tutorial | DigitalOcean](https://www.digitalocean.com/community/tutorials/java-design-patterns-example-tutorial#:~:text=Some%20of%20the%20benefits%20of,using%20design%20patterns%20are)). Bạn không phải tự nghĩ ra cách giải quyết mới cho một vấn đề quen thuộc, thay vào đó áp dụng mô hình đã có sẵn.

- **Tái sử dụng và bảo trì dễ dàng:** Design patterns khuyến khích **tái sử dụng mã** (reusability), làm cho code dễ mở rộng và bảo trì hơn ([Giới thiệu Design Patterns - GP Coder (Lập trình Java)](https://gpcoder.com/4164-gioi-thieu-design-patterns/#:~:text=Design%20Pattern%20gi%C3%BAp%20b%E1%BA%A1n%20t%C3%A1i,v%C3%A0%20d%E1%BA%BD%20d%C3%A0ng%20m%E1%BB%9F%20r%E1%BB%99ng)) ([Java Design Patterns - Example Tutorial | DigitalOcean](https://www.digitalocean.com/community/tutorials/java-design-patterns-example-tutorial#:~:text=design%20pattern,the%20team%20understand%20it%20easily)). Code được thiết kế theo mẫu chuẩn sẽ rõ ràng về mục đích, giúp các thành viên trong nhóm hiểu và chỉnh sửa dễ dàng. Nhờ đó, tổng chi phí và công sức bảo trì ứng dụng cũng giảm.

- **Giao tiếp và hiểu biết chung:** Nhiều lập trình viên đã quen thuộc với các design pattern thông dụng. Do đó, sử dụng design pattern giúp **giao tiếp** ý tưởng thiết kế với đồng nghiệp nhanh hơn – mọi người có thể hình dung giải pháp qua tên pattern (ví dụ: “dùng Singleton cho lớp cấu hình” hoặc “áp dụng Strategy cho phần thuật toán”) ([architecture - Why do we need design patterns - Stack Overflow](https://stackoverflow.com/questions/2323947/why-do-we-need-design-patterns#:~:text=Design%20Patterns%20provide%20easy%20to,interface%20to%20solve%20a%20problem)). Code tuân theo design pattern cũng dễ hiểu và dễ debug hơn đối với người khác.

Tóm lại, design patterns là công cụ đắc lực giúp tạo ra phần mềm **mạnh mẽ, linh hoạt và dễ bảo trì**, là “kim chỉ nam” để giải quyết các vấn đề lập trình một cách tối ưu ([Giới thiệu Design Patterns - GP Coder (Lập trình Java)](https://gpcoder.com/4164-gioi-thieu-design-patterns/#:~:text=Design%20Pattern%20gi%C3%BAp%20b%E1%BA%A1n%20t%C3%A1i,v%C3%A0%20d%E1%BA%BD%20d%C3%A0ng%20m%E1%BB%9F%20r%E1%BB%99ng)).

### Các Design Patterns phổ biến và ứng dụng trong Java

Có nhiều design pattern đã được định nghĩa (tiêu biểu là 23 mẫu trong cuốn *“Gang of Four”*). Chúng thường được phân thành 3 nhóm chính ([Design Patterns là gì?  Tổng hợp 23 mẫu Design Pattern](https://viblo.asia/p/design-patterns-la-gi-tai-sao-no-lai-la-tro-thu-dac-luc-cua-developers-tong-hop-23-mau-design-pattern-GrLZDBQV5k0#:~:text=1,t%E1%BA%A1o)) ([Design Patterns là gì?  Tổng hợp 23 mẫu Design Pattern](https://viblo.asia/p/design-patterns-la-gi-tai-sao-no-lai-la-tro-thu-dac-luc-cua-developers-tong-hop-23-mau-design-pattern-GrLZDBQV5k0#:~:text=3,vi)):

- **Creational Patterns (Nhóm khởi tạo):** Cung cấp giải pháp tạo đối tượng một cách linh hoạt, ẩn đi logic khởi tạo phức tạp. Ví dụ phổ biến:  
  - *Singleton*: Đảm bảo một lớp chỉ có **một instance duy nhất** trong suốt chương trình và cung cấp một điểm truy cập toàn cục tới nó. Ứng dụng: lớp quản lý cấu hình hoặc kết nối cơ sở dữ liệu thường được thiết kế dưới dạng Singleton để tránh lãng phí tài nguyên.  
  - *Factory Method*: Cung cấp một giao diện để tạo đối tượng, cho phép lớp con quyết định instantiation. Java áp dụng Factory pattern trong nhiều API – ví dụ `Calendar.getInstance()` trả về một subclass cụ thể của `Calendar` tùy môi trường.  
  - *Builder*: Tách việc xây dựng một đối tượng phức tạp thành nhiều bước nhỏ. Thường dùng khi cần tạo đối tượng với nhiều thuộc tính (ví dụ `StringBuilder` trong Java để xây dựng chuỗi).  

- **Structural Patterns (Nhóm cấu trúc):** Nhóm này quan tâm đến cấu trúc và mối quan hệ giữa các đối tượng, giúp xây dựng hệ thống linh hoạt và dễ mở rộng ([Design Patterns là gì?  Tổng hợp 23 mẫu Design Pattern](https://viblo.asia/p/design-patterns-la-gi-tai-sao-no-lai-la-tro-thu-dac-luc-cua-developers-tong-hop-23-mau-design-pattern-GrLZDBQV5k0#:~:text=2,tr%C3%BAc)). Ví dụ tiêu biểu:  
  - *Adapter*: Chuyển đổi giao diện của một lớp thành giao diện khác mà người dùng mong đợi. Ứng dụng: `java.util.Arrays#asList()` đóng vai trò adapter chuyển mảng thành `List`.  
  - *Decorator*: Bổ sung chức năng cho đối tượng một cách linh hoạt thông qua việc “bọc” đối tượng ban đầu. Java I/O sử dụng mẫu Decorator rộng rãi – ví dụ lớp `BufferedReader` bọc quanh `FileReader` để thêm chức năng buffer khi đọc dữ liệu.  
  - *Proxy*: Đại diện cho một đối tượng khác để điều khiển truy cập hoặc bổ sung hành vi. Trong Java, dynamic proxy được dùng (java.lang.reflect.Proxy) để tạo proxy cho các interface, ví dụ trong Hibernate hoặc Spring AOP để thêm logic giao dịch, bảo mật.

- **Behavioral Patterns (Nhóm hành vi):** Giải quyết cách các đối tượng tương tác và phân trách nhiệm cho nhau, tập trung vào hành vi của hệ thống ([Design Patterns là gì?  Tổng hợp 23 mẫu Design Pattern](https://viblo.asia/p/design-patterns-la-gi-tai-sao-no-lai-la-tro-thu-dac-luc-cua-developers-tong-hop-23-mau-design-pattern-GrLZDBQV5k0#:~:text=Nh%C3%B3m%20Behavioral%20Pattern%20g%E1%BB%93m%2011,m%E1%BA%ABu)). Ví dụ:  
  - *Observer*: Định nghĩa mối quan hệ phụ thuộc một-nhiều giữa các đối tượng, khi một đối tượng thay đổi trạng thái, các đối tượng phụ thuộc sẽ được thông báo và cập nhật. Java cung cấp sẵn `java.util.Observable` (tuy hiện đã deprecated) và mô hình lắng nghe sự kiện trong Swing cũng là một ví dụ Observer pattern (các `ActionListener` lắng nghe sự kiện từ nút bấm,...).  
  - *Strategy*: Cho phép thay đổi thuật toán thực thi tại runtime bằng cách tách rời thuật toán khỏi ngữ cảnh sử dụng. Ứng dụng: trong Java, interface `Comparator` dùng để thay đổi thuật toán so sánh khi sắp xếp (Collections.sort với comparator cụ thể chính là áp dụng Strategy).  
  - *Command*: Đóng gói một yêu cầu thành đối tượng, cho phép tham số hóa các tác vụ và hỗ trợ undo/redo. Ví dụ: trong Swing, mỗi hành động người dùng (như nhấn nút) có thể được đóng gói thành Command, tách phần phát sinh sự kiện và xử lý logic.

*Cách ứng dụng trong Java:* Các pattern trên không phụ thuộc ngôn ngữ, nhưng Java có nhiều **thư viện và framework** tận dụng hoặc cung cấp sẵn implementation của các pattern. Hiểu biết về design patterns giúp bạn sử dụng các thư viện hiệu quả hơn và thiết kế **mã nguồn “sạch”** (clean code) theo nguyên tắc đã được kiểm chứng.

### Ví dụ thực tế về Design Pattern

**Ví dụ:** Mô phỏng **Singleton Pattern** trong Java. Giả sử chúng ta cần một lớp quản lý cấu hình ứng dụng sao cho toàn bộ chương trình chỉ sử dụng một bản duy nhất của lớp này (để nhất quán dữ liệu cấu hình và tiết kiệm bộ nhớ). Ta có thể triển khai Singleton như sau:

```java
public class AppConfig {
    private static AppConfig instance;      // instance duy nhất (static)
    private Properties configProperties;    // ví dụ: chứa các cài đặt cấu hình

    // Constructor để private để không cho phép tạo đối tượng bên ngoài
    private AppConfig() {
        configProperties = new Properties();
        // ... load cấu hình từ file hoặc thiết lập mặc định
    }

    // Phương thức static để lấy instance duy nhất, tạo mới nếu chưa tồn tại
    public static AppConfig getInstance() {
        if (instance == null) {            // chưa có instance thì khởi tạo
            instance = new AppConfig();
        }
        return instance;
    }

    // Các phương thức khác để truy xuất cấu hình
    public String getSetting(String key) {
        return configProperties.getProperty(key);
    }
    // ... setter tương ứng nếu cần
}
```

Trong đoạn code trên, lớp `AppConfig` có một **biến tĩnh `instance`** và một constructor private. Phương thức `getInstance()` sẽ kiểm tra nếu `instance` chưa tồn tại thì tạo mới (lazy initialization). Nhờ đó, bất kỳ đâu trong chương trình khi gọi `AppConfig.getInstance()` cũng nhận được cùng một đối tượng cấu hình duy nhất. Mẫu thiết kế Singleton này đảm bảo **toàn cục** chỉ có một bản cấu hình chung.

*Lưu ý:* Singleton cơ bản như trên chưa xử lý vấn đề đa luồng. Trong môi trường đa luồng, ta cần bổ sung cơ chế đồng bộ (ví dụ dùng `synchronized`) hoặc dùng kỹ thuật *double-checked locking* để tránh tạo nhiều instance cùng lúc. Tuy nhiên, ví dụ trên đủ để minh họa ý tưởng Singleton.

### Bài tập thực hành Design Patterns

- **Bài 1: Factory Pattern đơn giản.** Xây dựng một **factory** để tạo các đối tượng hình học. Trước tiên, tạo interface `Shape` với phương thức `draw()`. Tạo các lớp triển khai `Circle`, `Square`, `Triangle` (mỗi lớp in ra thông báo khác nhau trong `draw()`, ví dụ: “Drawing Circle”). Sau đó, xây dựng lớp `ShapeFactory` với một phương thức tĩnh `getShape(String type)` trả về đối tượng `Shape` tương ứng với tham số type (ví dụ: “circle” -> trả về `new Circle()`). *Yêu cầu:* Sử dụng factory để tạo đối tượng thay vì dùng từ khóa `new` trực tiếp trong hàm `main`. Hãy viết chương trình `main` yêu cầu người dùng nhập loại hình học và sử dụng `ShapeFactory` để tạo đối tượng rồi gọi `draw()`.

- **Bài 2: Strategy Pattern thực tế.** Giả sử bạn đang viết chương trình tính **giá cước vận chuyển** cho một công ty. Công ty có nhiều chiến lược tính phí khác nhau (theo khối lượng, theo khoảng cách, phí cố định,...). Hãy thiết kế **Strategy Pattern** cho bài toán này: Tạo interface `ShippingStrategy` với phương thức `calculateFee(Order order)`. Triển khai một vài chiến lược cụ thể, ví dụ `WeightBasedStrategy`, `DistanceBasedStrategy`. Sau đó, tạo class `ShippingCalculator` sử dụng `ShippingStrategy` (có thể thông qua một setter hoặc constructor để chọn strategy). Viết code minh họa việc chuyển đổi qua lại các chiến lược tính phí cho một đơn hàng và in ra kết quả tính phí tương ứng từng chiến lược.

### Ôn tập và kiểm tra nhanh

- **Câu hỏi 1:** Design Pattern là gì và mục đích của việc sử dụng chúng trong lập trình hướng đối tượng? Nêu 2 lợi ích chính của Design Pattern đối với chất lượng mã nguồn ([Giới thiệu Design Patterns - GP Coder (Lập trình Java)](https://gpcoder.com/4164-gioi-thieu-design-patterns/#:~:text=N%C3%B3%20cung%20c%E1%BA%A5p%20cho%20b%E1%BA%A1n,ph%C3%A1p%20trong%20l%E1%BA%ADp%20tr%C3%ACnh%20OOP)) ([Java Design Patterns - Example Tutorial | DigitalOcean](https://www.digitalocean.com/community/tutorials/java-design-patterns-example-tutorial#:~:text=design%20pattern,the%20team%20understand%20it%20easily)).  
- **Câu hỏi 2:** Hãy kể tên **ba** mẫu design pattern thuộc nhóm **Creational** và cho biết ngắn gọn công dụng của từng mẫu.  
- **Câu hỏi 3:** Trong các design pattern đã học, pattern nào đảm bảo một lớp chỉ có một đối tượng duy nhất? Pattern nào cho phép thay đổi thuật toán lúc chạy? Pattern nào giúp mở rộng chức năng của đối tượng một cách linh hoạt? (Chỉ cần nêu tên tương ứng).

---

## 2. Lập trình Hướng Đối Tượng (OOP) và SOLID

### Các khái niệm quan trọng trong OOP

Lập trình hướng đối tượng (**OOP - Object Oriented Programming**) dựa trên bốn tính chất cơ bản sau đây:

- **Đóng gói (Encapsulation):** Đóng gói là kỹ thuật gộp dữ liệu và các phương thức thao tác trên dữ liệu đó vào cùng một đơn vị (class). Chi tiết triển khai bên trong được che giấu, chỉ cung cấp một **giao diện công khai** để tương tác. Nhờ đóng gói, đối tượng kiểm soát được việc truy cập/sửa đổi trạng thái nội tại, tránh những thay đổi ngoài ý muốn. *Ví dụ:* Trong Java, ta sử dụng các **modifier** như `private` cho biến thành viên và cung cấp các phương thức `getter/setter` công khai để truy xuất, đảm bảo bên ngoài không tác động trực tiếp dữ liệu bên trong.

- **Kế thừa (Inheritance):** Kế thừa cho phép một lớp **con (subclass)** tái sử dụng thuộc tính và phương thức của một lớp **cha (superclass)**. Lớp con có thể mở rộng hoặc thay đổi một phần hành vi của lớp cha. Kế thừa giúp **tái sử dụng mã** và tạo mối quan hệ *“IS-A”* (lớp con **là một** dạng của lớp cha). *Ví dụ:* Lớp `Animal` là cha của các lớp `Dog`, `Cat`. `Dog` kế thừa `Animal` nên có sẵn các thuộc tính chung như `age`, `weight` và phương thức như `eat()`, `sleep()`, ngoài ra `Dog` có thể có thêm phương thức đặc thù như `bark()`.

- **Đa hình (Polymorphism):** Đa hình nghĩa đen là “nhiều hình dạng”. Trong OOP, đa hình cho phép cùng một lời gọi phương thức nhưng hành vi cụ thể có thể khác nhau tùy thuộc đối tượng thực tế. Hai cơ chế đa hình phổ biến trong Java: **đa hình lúc chạy (runtime polymorphism)** thông qua **ghi đè phương thức (method overriding)** và **đa hình lúc biên dịch (compile-time polymorphism)** thông qua **nạp chồng (method overloading)**. *Ví dụ:* Nếu `Cat` và `Dog` đều kế thừa `Animal` và override phương thức `makeSound()`, thì khi ta viết code: `Animal pet = new Dog(); pet.makeSound();` kết quả sẽ gọi phiên bản `makeSound()` của `Dog` (sủa) thay vì của `Animal`. Điều này cho phép code tổng quát (kiểu `Animal`) nhưng hành vi cụ thể tùy đối tượng gắn vào.

- **Trừu tượng (Abstraction):** Trừu tượng hóa tức là **ẩn bớt chi tiết cài đặt**, chỉ tập trung vào những tính năng cốt lõi. Trong Java, tính trừu tượng thể hiện qua **abstract class** và **interface**. Chúng định nghĩa một giao diện hoặc bộ hành vi chung mà các lớp cụ thể phải tuân theo, nhưng không quan tâm chi tiết thực hiện. *Ví dụ:* Ta có interface `Shape` định nghĩa phương thức `draw()`. Các lớp `Circle`, `Rectangle` triển khai phương thức này với cách vẽ riêng. Code bên ngoài khi gọi `shape.draw()` không cần biết đó là hình tròn hay chữ nhật, và cách vẽ cụ thể ra sao – đây chính là trừu tượng, tập trung vào **ý nghĩa hành động** (“vẽ hình”) hơn là chi tiết.

Những tính chất trên kết hợp với nhau giúp OOP mô hình hóa thế giới thực rõ ràng và linh hoạt. Trong Java, hiểu vững các khái niệm này là nền tảng để thiết kế lớp, hệ thống class hiệu quả.

### Nguyên tắc SOLID và cách áp dụng trong Java

SOLID là tập hợp 5 nguyên tắc **thiết kế hướng đối tượng** giúp **mã nguồn dễ hiểu, linh hoạt và dễ bảo trì hơn** ([SOLID là gì? Giải thích dễ hiểu 5 SOLID principles (kèm ví dụ)](https://itviec.com/blog/solid-la-gi/#:~:text=SOLID%20l%C3%A0%20g%C3%AC%20v%C3%A0%20v%C3%AC,%C4%91%E1%BB%81u%20mu%E1%BB%91n%20khi%20vi%E1%BA%BFt%20code)) ([SOLID là gì? Giải thích dễ hiểu 5 SOLID principles (kèm ví dụ)](https://itviec.com/blog/solid-la-gi/#:~:text=C%C3%A1c%20SOLID%20principles%20c%C3%B3%20th%E1%BB%83,d%E1%BB%A5ng%20c%C3%A1c%20ph%C6%B0%C6%A1ng%20ph%C3%A1p%20Agile)). SOLID là viết tắt của các nguyên tắc sau:

1. **Single Responsibility Principle (SRP) – Nguyên tắc trách nhiệm đơn nhất:** Mỗi lớp chỉ nên có **một trách nhiệm duy nhất**, hay một lý do để thay đổi ([SOLID: Năm nguyên tắc cơ bản của thiết kế lớp trong Java](https://codegym.cc/vi/groups/posts/vi.232.solid-nam-nguyen-tac-co-ban-cua-thiet-ke-lop-trong-java#:~:text=Nguy%C3%AAn%20t%E1%BA%AFc%20n%C3%A0y%20n%C3%B3i%20r%E1%BA%B1ng,c%C6%A1%20s%E1%BB%9F%20d%E1%BB%AF%20li%E1%BB%87u%20v%C3%A0)). Nói cách khác, một class chỉ nên tập trung làm một việc. Áp dụng: Khi thiết kế, nếu thấy một lớp thực hiện quá nhiều nhiệm vụ (ví dụ vừa tính toán logic, vừa đọc/ghi file, vừa hiển thị giao diện), nên tách nó thành các lớp nhỏ hơn, mỗi lớp đảm nhận một chức năng. Điều này giúp mã dễ bảo trì: thay đổi trong chức năng này không ảnh hưởng lớp khác. *Ví dụ:* Lớp `OrderProcessor` chỉ lo xử lý đơn hàng, còn việc gửi email xác nhận nên tách sang lớp `EmailService`. Nhờ SRP, nếu cần sửa logic gửi email, ta chỉ chỉnh `EmailService` mà không đụng tới xử lý đơn hàng ([SOLID: Năm nguyên tắc cơ bản của thiết kế lớp trong Java](https://codegym.cc/vi/groups/posts/vi.232.solid-nam-nguyen-tac-co-ban-cua-thiet-ke-lop-trong-java#:~:text=Nguy%C3%AAn%20t%E1%BA%AFc%20n%C3%A0y%20n%C3%B3i%20r%E1%BA%B1ng,N%E1%BA%BFu)).

2. **Open/Closed Principle (OCP) – Nguyên tắc Mở/Đóng:** *“Mở để mở rộng, đóng để chỉnh sửa.”* Nghĩa là một module, lớp nên cho phép mở rộng thêm chức năng **mà không cần sửa đổi bên trong nó** ([SOLID: Năm nguyên tắc cơ bản của thiết kế lớp trong Java](https://codegym.cc/vi/groups/posts/vi.232.solid-nam-nguyen-tac-co-ban-cua-thiet-ke-lop-trong-java#:~:text=Nguy%C3%AAn%20t%E1%BA%AFc%20%C4%90%C3%B3ng%20M%E1%BB%9F%20)). Áp dụng: Sử dụng **kế thừa, đa hình** hoặc các interface để khi cần chức năng mới, ta **mở rộng** bằng cách tạo lớp dẫn xuất hoặc implement interface thay vì sửa mã nguồn có sẵn. *Ví dụ:* Một chương trình vẽ hình ban đầu hỗ trợ `Circle` và `Square`. Nếu tuân thủ OCP, lớp `ShapeDrawer` không chứa mã phụ thuộc cứng vào loại hình cụ thể, mà sử dụng một interface chung. Khi cần thêm `Triangle`, ta chỉ việc tạo lớp mới kế thừa interface `Shape` mà không phải sửa lớp `ShapeDrawer`.  

3. **Liskov Substitution Principle (LSP) – Nguyên tắc thay thế Liskov:** *“Các lớp con có thể thay thế lớp cha mà không làm thay đổi tính đúng đắn của chương trình.”* Hiểu đơn giản: **Ở bất kỳ đâu dùng đối tượng lớp cha, có thể thay bằng đối tượng lớp con mà chương trình vẫn hoạt động bình thường** ([SOLID: Năm nguyên tắc cơ bản của thiết kế lớp trong Java](https://codegym.cc/vi/groups/posts/vi.232.solid-nam-nguyen-tac-co-ban-cua-thiet-ke-lop-trong-java#:~:text=%C4%90%C3%A2y%20l%C3%A0%20m%E1%BB%99t%20bi%E1%BA%BFn%20th%E1%BB%83,s%E1%BB%9F%20ph%E1%BA%A3i%20ghi%20%C4%91%C3%A8%20c%C3%A1c)). Áp dụng: Đảm bảo lớp con **tuân thủ hợp đồng** (contract) mà lớp cha đưa ra, không **phá vỡ các giả định**. Tránh thiết kế kế thừa sai lệch. *Ví dụ kinh điển:* Lớp `Square` kế thừa `Rectangle` nhưng nếu cả hai có setter `setWidth`, `setHeight` thì `Square` sẽ vi phạm LSP (vì `square.setWidth(5)` ngầm thay đổi cả chiều cao – hành vi khác với `Rectangle`). Cách sửa: có thể thiết kế lại để `Square` không kế thừa `Rectangle` hoặc điều chỉnh hợp đồng của `Rectangle`. Nói chung, mọi **phương thức công khai** của lớp cha khi thực thi bởi lớp con phải giữ nguyên các tính chất kỳ vọng (như không thay đổi nghĩa của kết quả, không phát sinh lỗi bất ngờ).

4. **Interface Segregation Principle (ISP) – Nguyên tắc phân tách giao diện:** Không nên ép một lớp triển khai những interface có nhiều phương thức mà lớp đó **không dùng đến**. Thay vào đó, nên tách thành các **interface nhỏ, chuyên biệt** theo từng nhóm chức năng. Áp dụng: Thiết kế nhiều interface hẹp, mỗi interface tập trung vào một nhiệm vụ. *Ví dụ:* Thay vì có một interface lớn `Machine` với các phương thức `print()`, `scan()`, `fax()` cho mọi máy văn phòng, ta nên tách thành `Printer`, `Scanner`, `Fax` riêng. Một máy in đa năng có thể implement cả 3 interface, nhưng một máy in thường chỉ cần implement `Printer` mà không bị yêu cầu thừa các phương thức không liên quan.

5. **Dependency Inversion Principle (DIP) – Nguyên tắc đảo ngược phụ thuộc:** Các module cấp cao không nên phụ thuộc trực tiếp vào module cấp thấp; cả hai nên phụ thuộc vào các **trừu tượng (abstract)**. Đồng thời, các chi tiết cụ thể nên phụ thuộc ngược lại vào trừu tượng, chứ không phải ngược lại. Hiểu một cách đơn giản: **Thay vì lớp A tạo hoặc gọi trực tiếp lớp B cụ thể, A nên tương tác thông qua interface hoặc lớp trừu tượng B**. Áp dụng: Sử dụng interface để tách rời sự phụ thuộc giữa các lớp. *Ví dụ:* Một ứng dụng có lớp `NotificationService` gửi thông báo. Nếu lớp này gọi trực tiếp `EmailSender` hoặc `SMSSender` thì phụ thuộc cụ thể. Theo DIP, ta định nghĩa interface `MessageSender` với phương thức `send(msg)`. `NotificationService` chỉ biết đến `MessageSender` (trừu tượng), còn chi tiết gửi qua email hay SMS sẽ do các lớp cụ thể implement interface này đảm nhận. Trong Java và đặc biệt là Spring Framework, DIP được thực hiện rộng rãi thông qua cơ chế **Dependency Injection** – đối tượng cần phụ thuộc sẽ được truyền (inject) vào từ bên ngoài thay vì tự nó khởi tạo.

Áp dụng SOLID trong Java giúp thiết kế lớp rõ ràng, tránh được nhiều vấn đề tiềm ẩn và dễ dàng bảo trì mở rộng. Khi viết code, bạn nên thường xuyên tự hỏi: **Lớp này đã đơn nhiệm chưa (SRP)?** Thay đổi yêu cầu mới có cần sửa lớp cũ không hay chỉ cần thêm lớp mới (OCP)? Lớp con của mình có thực sự thay thế được lớp cha ở mọi nơi không (LSP)? Các interface có quá lớn không (ISP)? Và các module của mình có đang phụ thuộc qua lại hợp lý chưa (DIP)? Việc tuân thủ SOLID sẽ dần trở nên tự nhiên hơn khi bạn luyện tập thiết kế và refactor code thường xuyên.

### Bài tập thực hành OOP và SOLID

- **Bài 1: Thiết kế lớp OOP.** Xây dựng một lớp `Student` để quản lý sinh viên với các thuộc tính đóng gói (ví dụ: `id`, `name`, `email`). Thêm các phương thức getter/setter tương ứng. Tiếp đó, tạo lớp `GraduateStudent` kế thừa `Student`, bổ sung thuộc tính `thesisTitle` (tiêu đề luận văn) và override phương thức hiển thị thông tin để bao gồm cả thông tin luận văn (đa hình). Tạo một lớp `University` chứa danh sách sinh viên (`List<Student>`) và phương thức `addStudent(Student s)`. **Yêu cầu:** Thể hiện tính kế thừa (GraduateStudent *là* Student), đóng gói (thuộc tính private, getter/setter), đa hình (dùng một biến `Student` tham chiếu đối tượng `GraduateStudent`), và trừu tượng (nếu cần, có thể cho `Student` là abstract, tuy nhiên trong bài này không bắt buộc). Viết code minh họa việc thêm cả `Student` thường và `GraduateStudent` vào danh sách và duyệt in thông tin (sẽ gọi phương thức đã override tương ứng đối tượng).

- **Bài 2: Áp dụng SOLID refactor code.** Cho đoạn code giả định sau (mô tả bằng lời): *“Một lớp `ReportGenerator` có nhiệm vụ lấy dữ liệu từ DB rồi sinh báo cáo và gửi email báo cáo đó cho quản lý.”* Rõ ràng lớp này đang làm 3 việc: (1) truy xuất dữ liệu, (2) tạo báo cáo, (3) gửi email. Hãy đề xuất cách tách lớp theo nguyên tắc SRP. Sau khi tách, xác định xem lớp nào nên phụ thuộc vào interface nào để thỏa DIP. Gợi ý: Tạo các interface như `DataSource` (để lấy dữ liệu, có thể có implement như `DatabaseSource`, `FileSource`), interface `ReportFormatter`, interface `Notifier` (với implement `EmailNotifier`). `ReportGenerator` sẽ sử dụng các interface này (tiêm qua constructor hoặc setter) thay vì phụ thuộc lớp cụ thể. Bài tập này yêu cầu bạn viết ra thiết kế các lớp và interface (có thể dưới dạng code hoặc sơ đồ) và giải thích cách SOLID được áp dụng: SRP (mỗi lớp một nhiệm vụ), OCP (có thể mở rộng loại nguồn dữ liệu mới, loại thông báo mới mà không sửa `ReportGenerator`), LSP (các nguồn dữ liệu thay thế cho nhau), ISP (mỗi interface gọn mục đích) và DIP (ReportGenerator phụ thuộc abstraction).

### Ôn tập và kiểm tra nhanh

- **Câu hỏi 1:** Hãy nêu tên bốn tính chất cơ bản của lập trình hướng đối tượng và giải thích ngắn gọn từng tính chất.  
- **Câu hỏi 2:** Nguyên tắc **Single Responsibility** phát biểu như thế nào? Tại sao tuân thủ nguyên tắc này giúp code dễ bảo trì hơn?  
- **Câu hỏi 3:** Một lớp `Printer` vừa có phương thức `print()` để in ấn, vừa có phương thức `scan()` để quét tài liệu. Điều này vi phạm nguyên tắc SOLID nào? Bạn sẽ chỉnh thiết kế ra sao để tuân thủ nguyên tắc đó?  
- **Câu hỏi 4:** Dependency Inversion Principle khác gì so với việc “đảo ngược điều kiện” hay “đảo ngược vòng lặp”? (Gợi ý: DIP không liên quan đến logic if/else, mà nói về phụ thuộc giữa các mô-đun).  
- **Câu hỏi 5:** Liskov Substitution Principle yêu cầu gì khi thiết kế quan hệ kế thừa giữa hai lớp?

---

## 3. Lập trình Đa Luồng, Xử lý File và Mạng

### Đa luồng (Multithreading) trong Java

**Đa luồng** cho phép một chương trình thực thi nhiều phần (nhiều luồng) **song song** thay vì tuần tự. Thông thường, một chương trình Java khởi chạy sẽ chạy trên **một luồng chính (main thread)** thực thi các lệnh tuần tự. Với multithreading, chúng ta có thể tạo thêm các luồng phụ để thực hiện đồng thời nhiều tác vụ, giúp tận dụng tốt hơn sức mạnh của bộ xử lý đa nhân và tăng hiệu suất ứng dụng ([Multithreading in Java - Everything You MUST Know | DigitalOcean](https://www.digitalocean.com/community/tutorials/multithreading-in-java#:~:text=When%20we%20run%20a%20real,application%20is%20use%20utilize%20multithreading)).

- **Khái niệm:** Mỗi **thread (luồng)** là một luồng thực thi độc lập trong tiến trình Java. Các luồng có thể chạy **đồng thời (concurrently)**, nghĩa là trong cùng một khoảng thời gian, CPU có thể chuyển qua lại thực thi các luồng, tạo hiệu ứng chúng chạy song song. Nếu máy có nhiều nhân CPU, các luồng thậm chí có thể thực sự chạy cùng lúc trên các nhân khác nhau.

- **Tạo luồng trong Java:** Java cung cấp hai cách chính để tạo luồng: (1) **Kế thừa lớp `Thread`** và override phương thức `run()`, sau đó gọi `start()` trên đối tượng Thread để nó chạy song song. (2) **Triển khai `Runnable`** (hoặc `Callable` cho trường hợp có kết quả trả về), implement phương thức `run()`, rồi tạo `Thread` mới với đối tượng Runnable đó. Ví dụ cơ bản với kế thừa Thread:  

  ```java
  class MyThread extends Thread {
      public void run() {
          System.out.println("Hello from thread " + this.getName());
      }
  }
  // Sử dụng
  MyThread t = new MyThread();
  t.start();  // không gọi trực tiếp run() mà gọi start() để JVM tạo luồng mới thực thi run()
  ```
  Hoặc dùng Runnable:  
  ```java
  class MyTask implements Runnable {
      public void run() {
          // code thực hiện nhiệm vụ
      }
  }
  // ...
  Thread t = new Thread(new MyTask());
  t.start();
  ```
  Từ Java 8, có thể sử dụng lambda cho Runnable để tạo luồng nhanh chóng: `new Thread(() -> { /* code */ }).start();`.

- **Chia sẻ dữ liệu và đồng bộ:** Các luồng trong cùng một tiến trình Java **chia sẻ vùng nhớ** (biến static, đối tượng trên heap). Điều này thuận lợi để các luồng tương tác, nhưng cũng gây ra vấn đề *“điều kiện đua”* nếu nhiều luồng cùng truy cập/sửa một dữ liệu. Java cung cấp cơ chế **đồng bộ hóa (synchronization)**, ví dụ từ khóa `synchronized` hoặc các khóa (Lock) trong `java.util.concurrent.locks`, để đảm bảo tại mỗi thời điểm chỉ một luồng truy cập khối code quan trọng (critical section). Ngoài ra còn có các công cụ cao cấp như **Executor Service** (quản lý pool các luồng), **atomic variables**, **Concurrent Collections** giúp lập trình đa luồng an toàn và tiện lợi hơn.

- **Ứng dụng của đa luồng:** Đa luồng hữu ích khi chương trình có nhiều công việc có thể làm độc lập hoặc chờ đợi (I/O, tính toán song song). Ví dụ: một máy chủ web Java có thể tạo một luồng mới cho mỗi yêu cầu đến, cho phép xử lý nhiều request đồng thời. Hoặc trong ứng dụng desktop, một luồng nền có thể tải dữ liệu từ Internet trong khi luồng giao diện vẫn hoạt động mượt mà.

### Xử lý file (File handling) trong Java

Xử lý tập tin là một phần quan trọng của bất kỳ ứng dụng nào. Java cung cấp nhiều cách để tạo, đọc, cập nhật và xóa file ([Java Files - W3Schools](https://www.w3schools.com/java/java_files.asp#:~:text=File%20handling%20is%20an%20important,reading%2C%20updating%2C%20and%20deleting%20files)). Các thao tác với file thường sử dụng các lớp trong gói `java.io` (các lớp cổ điển) hoặc `java.nio.file` (NIO mới hơn).

- **Đọc dữ liệu từ file:** Có nhiều cách, ví dụ sử dụng `FileReader` kết hợp với `BufferedReader` để đọc text theo dòng:

  ```java
  BufferedReader reader = new BufferedReader(new FileReader("data.txt"));
  String line;
  while ((line = reader.readLine()) != null) {
      System.out.println(line);
  }
  reader.close();
  ```
  Đoạn code trên mở file "data.txt" và đọc từng dòng cho đến hết file, in mỗi dòng ra màn hình. Sử dụng `BufferedReader` giúp đọc hiệu quả hơn nhờ cơ chế đệm. Lưu ý đóng file sau khi xong (ở ví dụ dùng `close()`). Từ Java 7, ta có thể dùng try-with-resources để tự động đóng:  
  ```java
  try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
      // đọc và xử lý
  }
  ``` 

- **Ghi dữ liệu ra file:** Tương tự, có thể dùng `FileWriter` và/hoặc `BufferedWriter`. Ví dụ:  
  ```java
  BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"));
  writer.write("Hello File\n");
  writer.write("This is a new line.\n");
  writer.close();
  ```
  Lệnh trên tạo (hoặc ghi đè) file "output.txt" và ghi hai dòng text vào đó.

- **Làm việc với file và thư mục:** Lớp `File` (java.io.File) cho phép tương tác với thuộc tính file/thư mục (tên, đường dẫn, kích thước, tạo thư mục, xóa file...). Từ Java 7 trở đi, API NIO2 (trong `java.nio.file`) cung cấp lớp tiện ích `Files` và `Paths` với nhiều phương thức tĩnh hữu ích, ví dụ: `Files.readAllLines(Paths.get("data.txt"))` để đọc toàn bộ file vào List<String>, hoặc `Files.copy`, `Files.move` để sao chép/di chuyển file.

- **Xử lý ngoại lệ:** Các thao tác I/O thường dễ phát sinh lỗi (file không tồn tại, quyền truy cập bị từ chối, lỗi đọc/ghi...). Java yêu cầu xử lý ngoại lệ (checked exceptions) cho hầu hết các phương thức I/O. Do vậy, khi đọc/ghi file, cần đặt trong khối try-catch và xử lý `IOException` phù hợp (ví dụ in thông báo lỗi, hoặc ném ngoại lệ lên cho lớp trên xử lý).

### Lập trình mạng trong Java (sockets, HTTP requests, ...)

Java tích hợp sẵn các thư viện lập trình mạng mạnh mẽ trong gói `java.net`, cho phép tạo các ứng dụng **client-server** và thực hiện các **HTTP request**.

- **Lập trình Socket (TCP/IP):** Sockets là cơ chế giao tiếp hai chiều qua mạng giữa hai chương trình (thường một bên là server, một bên là client). Java hỗ trợ lập trình socket qua lớp `ServerSocket` (dành cho server) và `Socket` (dành cho client) ([Java Socket Server Examples (TCP/IP)](https://www.codejava.net/java-se/networking/java-socket-server-examples-tcp-ip#:~:text=1)). Mô hình hoạt động cơ bản:  
  - *Server:* Tạo đối tượng `ServerSocket` lắng nghe trên một cổng (port) nhất định. Gọi `accept()` để chờ client kết nối. Khi có một client kết nối, `accept()` trả về một `Socket` đại diện cho kết nối tới client đó. Server có thể dùng `InputStream` của socket để **đọc dữ liệu** từ client và `OutputStream` để **gửi dữ liệu** cho client ([Java Socket Server Examples (TCP/IP)](https://www.codejava.net/java-se/networking/java-socket-server-examples-tcp-ip#:~:text=1,to%20a%20specific%20port%20number)). Mỗi kết nối client-server thường được xử lý trên một luồng riêng biệt để phục vụ nhiều client đồng thời ([Java Socket Server Examples (TCP/IP)](https://www.codejava.net/java-se/networking/java-socket-server-examples-tcp-ip#:~:text=The%20steps%203%20and%204,the%20server%20and%20the%20client)).  
  - *Client:* Tạo `Socket` kết nối tới địa chỉ IP và cổng của server. Sau khi kết nối, sử dụng luồng vào/ra của socket để giao tiếp (gửi yêu cầu, nhận phản hồi).  
  *Ví dụ:* Viết một server trên cổng 5000, nhận chuỗi ký tự từ client và trả lại chuỗi in hoa. Client kết nối đến server gửi chuỗi “Hello”, server sẽ đáp “HELLO”.

- **Giao thức HTTP và URL:** Để thực hiện các request HTTP (như GET/POST tới một web server), Java cung cấp lớp `HttpURLConnection` (trước Java 11) và **HTTP Client API** (từ Java 11 trở lên có `java.net.http.HttpClient`). Với `HttpURLConnection`, ta có thể tạo một URL, mở kết nối, thiết lập phương thức (GET, POST...), gửi request và đọc response.  
  *Ví dụ:*  
  ```java
  URL url = new URL("https://api.github.com");
  HttpURLConnection con = (HttpURLConnection) url.openConnection();
  con.setRequestMethod("GET");
  int status = con.getResponseCode();
  BufferedReader in = new BufferedReader(new InputStreamReader(con.getInputStream()));
  String inputLine;
  StringBuilder content = new StringBuilder();
  while ((inputLine = in.readLine()) != null) {
      content.append(inputLine);
  }
  in.close();
  con.disconnect();
  System.out.println("Response: " + content);
  ```  
  Đoạn code trên gửi một GET request đến GitHub API và in ra phản hồi (thông thường là chuỗi JSON).  

- **Thư viện tiện ích:** Thay vì dùng `HttpURLConnection` thô sơ, lập trình viên thường dùng các thư viện như **Apache HttpClient**, **OkHttp** hoặc sử dụng trực tiếp Spring WebClient (nếu đang dùng Spring) để gửi HTTP một cách tiện lợi hơn (quản lý kết nối, xây dựng request phức tạp, v.v.).

- **UDP socket:** Ngoài TCP (kết nối hướng trạng thái), Java cũng hỗ trợ UDP (giao thức không kết nối) qua các lớp `DatagramSocket`, `DatagramPacket`. UDP phù hợp cho ứng dụng cần gửi thông điệp ngắn gọn, không yêu cầu đảm bảo tuyệt đối (ví dụ gửi log, trò chơi online...).

Tóm lại, Java có đầy đủ công cụ để xây dựng ứng dụng mạng từ thấp (sockets) đến cao (HTTP client, web services). Khi viết ứng dụng mạng, lưu ý đến các vấn đề đồng bộ, đa luồng (vì I/O mạng thường chậm, nên dùng luồng riêng để tránh khóa ứng dụng), và bảo mật (SSL, mã hóa nếu cần thiết).

### Bài tập thực hành về đa luồng, file và mạng

- **Bài 1: Đa luồng cơ bản.** Viết một chương trình Java tạo ra hai luồng chạy song song. Luồng thứ nhất in ra các số từ 1 đến 10, mỗi số cách nhau 300ms. Luồng thứ hai in ra các chữ cái từ A tới J, mỗi chữ cách nhau 500ms. Sử dụng hai cách tạo thread khác nhau cho hai luồng (một luồng dùng extends Thread, luồng kia dùng implements Runnable). Chạy chương trình và quan sát sự xen kẽ ngẫu nhiên của các số và chữ cái trên console, chứng minh hai luồng thực sự chạy đồng thời.

- **Bài 2: Đọc/ghi file CSV.** Cho một file “students.csv” chứa danh sách sinh viên với định dạng: mỗi dòng gồm `id,name,email`. Hãy viết chương trình Java thực hiện: (a) Đọc toàn bộ file CSV, lưu thông tin vào một `List<Student>` (với `Student` là lớp model có các trường tương ứng). (b) In ra thống kê số sinh viên. (c) Thêm một sinh viên mới vào danh sách và ghi cập nhật trở lại file CSV. Yêu cầu sử dụng `BufferedReader` để đọc và `BufferedWriter` để ghi. Xử lý các trường hợp file không tồn tại hoặc lỗi I/O bằng cách in thông báo phù hợp.

- **Bài 3: Ứng dụng client-server đơn giản.** Xây dựng một ứng dụng client-server dùng socket TCP: Server lắng nghe trên một cổng (ví dụ 6000). Mỗi khi có client kết nối, server tạo luồng mới để giao tiếp với client. Giả sử giao thức đơn giản: client gửi một chuỗi văn bản, server trả lại chuỗi đó viết hoa. Thực hiện các bước:
  1. Viết chương trình server: dùng `ServerSocket`, lặp vô hạn chờ accept kết nối; khi accept, tạo Thread mới để xử lý. Trong luồng xử lý: nhận dữ liệu từ client (dùng `BufferedReader` đọc từ `socket.getInputStream()`), chuyển nội dung thành viết hoa rồi gửi trả (dùng `PrintWriter` ghi ra `socket.getOutputStream()`), sau đó đóng socket.
  2. Viết chương trình client: kết nối tới server qua `Socket`, gửi một thông điệp do người dùng nhập, nhận phản hồi từ server và in ra. Cho phép người dùng gửi nhiều thông điệp cho đến khi gõ lệnh “exit”. Đảm bảo đóng kết nối khi kết thúc.
  3. Chạy thử server rồi chạy nhiều client (có thể trên cùng máy với các cửa sổ terminal khác nhau), kiểm tra các client đều nhận được phản hồi đúng từ server.

- **Bài 4 (Nâng cao, tùy chọn):** Sử dụng Java HTTP client để gọi đến một API công khai. Ví dụ: Gọi API của OpenWeatherMap để lấy thông tin thời tiết theo thành phố (sử dụng API key cá nhân, nếu có). Phân tích JSON nhận được (có thể sử dụng thư viện JSON đơn giản hoặc xử lý chuỗi) và hiển thị nhiệt độ, độ ẩm,... trên console. Bài tập này giúp thực hành gửi HTTP request và xử lý dữ liệu nhận được từ Internet.

### Ôn tập và kiểm tra nhanh

- **Câu hỏi 1:** Đa luồng là gì? Hãy nêu **hai cách** tạo một luồng mới trong Java và sự khác biệt giữa chúng.  
- **Câu hỏi 2:** Tại sao cần có cơ chế đồng bộ (synchronized) khi lập trình đa luồng? Mô tả ngắn gọn vấn đề gì xảy ra nếu hai luồng cùng ghi dữ liệu vào một biến toàn cục đồng thời.  
- **Câu hỏi 3:** Để đọc dữ liệu văn bản từ một file một cách hiệu quả, bạn sẽ sử dụng lớp nào? Hãy viết mẫu 2 dòng code (giả lập) để đọc một dòng văn bản từ file trong Java.  
- **Câu hỏi 4:** Lớp `File` trong Java có công dụng gì? Nêu một vài thao tác có thể làm với `File` (ví dụ: kiểm tra file có tồn tại không, tạo thư mục,...).  
- **Câu hỏi 5:** Sockets trong Java dùng để làm gì? Phân biệt vai trò của `ServerSocket` và `Socket`.  
- **Câu hỏi 6:** Giả sử bạn muốn gửi một yêu cầu HTTP GET tới API của Github để lấy thông tin người dùng, bạn sẽ sử dụng những lớp nào trong Java (kể tên ít nhất một cách tiếp cận).

---

## 4. Spring Framework

### Tổng quan về Spring Framework

**Spring Framework** là một framework Java mạnh mẽ, hỗ trợ xây dựng ứng dụng doanh nghiệp một cách toàn diện. Spring cung cấp một loạt giải pháp từ quản lý **bean** và **Dependency Injection** (Spring Core), lập trình web (Spring MVC), truy xuất cơ sở dữ liệu (Spring Data), bảo mật (Spring Security) cho đến xử lý giao dịch, v.v. Mục tiêu cốt lõi của Spring là giúp phát triển ứng dụng Java dễ dàng hơn bằng cách **đơn giản hóa cấu hình** và thúc đẩy các thực hành thiết kế tốt (như IoC, DIP). 

Đặc trưng nổi bật của Spring Framework gồm:

- **Inversion of Control (IoC) Container:** Spring có một **IoC container** để quản lý vòng đời các *bean* – là các đối tượng trong ứng dụng. Container chịu trách nhiệm khởi tạo các đối tượng, kết nối (inject) các phụ thuộc giữa chúng và quản lý vòng đời của chúng ([Tổng quan về Spring Bean](https://viblo.asia/p/tong-quan-ve-spring-bean-WR5JRbZ0JGv#:~:text=,XML%29%20ho%E1%BA%B7c%20c%C3%A1c%20annotations)). Nhờ IoC, các đối tượng trong Spring **không tự tạo phụ thuộc của mình**, thay vào đó, chúng khai báo những gì chúng cần, còn việc cung cấp các đối tượng phụ thuộc sẽ do container đảm nhiệm ([Tổng quan về Spring Bean](https://viblo.asia/p/tong-quan-ve-spring-bean-WR5JRbZ0JGv#:~:text=C%C3%B3%20th%E1%BB%83%20hi%E1%BB%83u%20m%E1%BB%99t%20c%C3%A1ch,dependencies%20v%C3%A0o%20c%C3%A1c%20Ioc%20Container)). Điều này giúp tách rời module, tăng tính linh hoạt và dễ kiểm thử (vì có thể dùng mock cho các phụ thuộc).

- **Dependency Injection (DI):** Là cơ chế cụ thể để thực hiện IoC – container sẽ **tiêm (inject)** các phụ thuộc vào đối tượng khi tạo ra nó ([Tổng quan về Spring Bean](https://viblo.asia/p/tong-quan-ve-spring-bean-WR5JRbZ0JGv#:~:text=,XML%29%20ho%E1%BA%B7c%20c%C3%A1c%20annotations)). Các phụ thuộc có thể được tiêm qua constructor, setter hoặc trực tiếp vào trường (field) với annotation. Lập trình viên chỉ cần đánh dấu (ví dụ dùng `@Autowired` trên thuộc tính hoặc constructor) và khai báo bean (bằng `@Component`, `@Bean` trong cấu hình), Spring sẽ tự động nối chúng với nhau. DI giúp code của bạn tuân thủ DIP (phụ thuộc vào interface, container đưa vào implementation cụ thể) và giảm sự phụ thuộc chặt chẽ giữa các class.

- **Quản lý cấu hình bằng Annotation:** Spring trước đây yêu cầu cấu hình XML khá phức tạp. Ngày nay, Spring Boot và Spring Framework cho phép dùng **Java annotation** để cấu hình dễ dàng hơn. Ví dụ: `@Component` hoặc `@Service` để đánh dấu class là bean, `@Controller` cho controller tầng web, `@Autowired` để yêu cầu DI, `@Configuration` cho lớp cấu hình, `@Bean` cho phương thức tạo bean. Nhờ annotation, ta có cấu hình dạng “zero XML”, trực quan ngay trong mã nguồn.

- **Module hóa và tích hợp:** Spring được thiết kế module hóa – bạn có thể chỉ dùng Spring Core (IoC container) độc lập, hoặc tích hợp thêm Spring MVC, Spring Data, v.v. tùy nhu cầu. Spring Boot (một dự án con của Spring) thậm chí giúp **tự động cấu hình** nhiều thành phần, giúp bạn nhanh chóng tạo ứng dụng chạy được với rất ít cấu hình thủ công.

Tóm lại, Spring Framework giống như **bộ khung xương** cho ứng dụng Java, cung cấp sẵn nhiều thành phần hữu ích. Bằng cách tuân thủ các nguyên tắc của Spring, bạn tập trung vào **logic nghiệp vụ**, còn những thứ như quản lý đối tượng, kết nối dữ liệu, bảo mật... đã có Spring hỗ trợ.

### Các khái niệm quan trọng trong Spring (Dependency Injection, Bean, Annotation, ...)

- **Bean:** Trong ngữ cảnh Spring, một *bean* là một đối tượng được khởi tạo, quản lý bởi Spring IoC container ([Tổng quan về Spring Bean](https://viblo.asia/p/tong-quan-ve-spring-bean-WR5JRbZ0JGv#:~:text=In%20Spring%2C%20the%20objects%20that,by%20a%20Spring%20IoC%20container)) ([Tổng quan về Spring Bean](https://viblo.asia/p/tong-quan-ve-spring-bean-WR5JRbZ0JGv#:~:text=In%20Spring%2C%20the%20objects%20that,by%20a%20Spring%20IoC%20container)). Những đối tượng cấu thành ứng dụng (service, repository, controller, ...) sau khi khai báo cho Spring sẽ trở thành bean do Spring quản lý. Bạn có thể tưởng tượng container Spring như một nhà máy, khi khởi động sẽ tạo ra các bean (theo cấu hình/annotation), sau đó khi ứng dụng chạy, bất cứ nơi nào cần dùng bean, Spring sẽ cung cấp đúng instance đã quản lý (thường theo scope singleton mặc định). Bean có thể được định nghĩa bằng annotation (`@Component`, `@Service`, `@Controller`, `@Repository` – các biến thể của component) hoặc bằng khai báo trong file cấu hình (Java config với `@Bean` methods, hoặc XML config truyền thống).

- **Dependency Injection (DI):** Như đã đề cập, DI là cơ chế **tiêm phụ thuộc**. Có hai kiểu chính: **Constructor Injection** (Spring gọi constructor của bean và truyền sẵn các tham số phụ thuộc), **Setter/Field Injection** (Spring gọi setter hoặc gán giá trị trực tiếp vào thuộc tính đánh dấu `@Autowired`). Ví dụ:  
  ```java
  @Component
  class NotificationService {
      @Autowired
      private MessageSender sender; // MessageSender là interface, có EmailSender implement chẳng hạn

      public void sendAlert(String msg) {
          sender.send(msg); // sử dụng phụ thuộc đã được Spring inject
      }
  }
  ```  
  Ở đây `NotificationService` cần một `MessageSender`. Nhờ `@Autowired`, Spring sẽ tìm một bean phù hợp (ví dụ có `@Component class EmailSender implements MessageSender`) và tự động gán vào thuộc tính `sender`. Lập trình viên không cần viết `notificationService.setSender(new EmailSender())`; Spring lo việc đó. DI giúp **giảm kết dính** giữa `NotificationService` và `EmailSender` cụ thể – ta có thể thay bằng `SmsSender` dễ dàng chỉ bằng đổi cấu hình, mà không cần sửa code NotificationService.

- **Inversion of Control (IoC) Container:** Chính là **Spring Container**, nơi chứa và quản lý các bean như đã nói. IoC container chịu trách nhiệm đọc cấu hình (annotation hoặc XML), khởi tạo bean theo thứ tự phù hợp (đáp ứng dependency), quản lý scope (ví dụ singleton: mỗi bean chỉ có một instance, hoặc prototype: tạo mới mỗi lần yêu cầu, vv). Lập trình viên ít khi tương tác trực tiếp với IoC container, nhưng hiểu rằng container là “bộ não” đằng sau việc kết nối mọi thứ. Trong ứng dụng Spring Boot, container khởi động khi ứng dụng chạy (nhờ `SpringApplication.run`), và giữ các bean sẵn sàng để inject bất cứ lúc nào.

- **Các annotation quan trọng:** Spring cung cấp rất nhiều annotation, sau đây là một số cái chính thường dùng:
  - `@Component`: Đánh dấu một class là bean thông thường.
  - `@Service`: Cũng là @Component (chức năng tương tự) nhưng dùng ngữ nghĩa cho tầng service (business logic).
  - `@Repository`: @Component cho tầng truy xuất dữ liệu (DAO/repository), Spring Data dùng để tự động tạo bean cho interface repository.
  - `@Controller`: @Component cho tầng web MVC controller. (Nếu là REST API, dùng `@RestController`).
  - `@Autowired`: Yêu cầu Spring tiêm phụ thuộc vào trường hoặc constructor. (Với constructor hiện nay Spring Boot khuyến khích **constructor injection** không cần @Autowired nếu class có một constructor duy nhất).
  - `@Configuration`: Đánh dấu một class là lớp cấu hình, chứa các định nghĩa bean.
  - `@Bean`: Đặt trên một phương thức trong @Configuration, để khai báo một bean (phương thức trả về kiểu đối tượng cần quản lý, và Spring sẽ gọi nó để lấy bean).
  - `@Qualifier`: Dùng kèm @Autowired khi có nhiều bean cùng loại, để chỉ định tên bean cụ thể cần inject.
  - `@Scope`: Để chỉ định scope của bean (singleton, prototype,...).
  - Ngoài ra, trong Spring MVC/Spring Boot Web còn có các annotation như `@RequestMapping`/`@GetMapping`/`@PostMapping` (cho các URL của controller), `@PathVariable`, `@RequestParam`, `@ModelAttribute`, v.v. (sẽ nói thêm ở phần web).

- **Quản lý cấu hình & hồ sơ (Profile):** Spring cho phép chia cấu hình theo môi trường (dev, prod) bằng `@Profile`, sử dụng file `application.properties` hoặc `application.yml` cho các thông số (như cấu hình database, cổng server...). Spring Boot tự động load những cấu hình này và map vào các bean (thông qua annotation `@Value` hoặc cấu trúc `@ConfigurationProperties`).

Tóm lại, hiểu các khái niệm trên giúp bạn làm chủ Spring. Khi viết code Spring, bạn chủ yếu viết các class mang annotation phù hợp và logic nghiệp vụ, **Spring sẽ lo dây nối** chúng lại với nhau. Cách tiếp cận này khác biệt so với lập trình Java SE truyền thống (nơi bạn tự `new` đối tượng), nhưng mang lại nhiều lợi ích về kiến trúc và test.

### Xây dựng ứng dụng Spring cơ bản

Để hình dung cách các thành phần Spring hoạt động cùng nhau, ta sẽ xây dựng một **ứng dụng Spring Boot cơ bản** (một ứng dụng console hoặc web đơn giản).

**Bước 1: Tạo dự án Spring Boot** – dễ nhất là dùng Spring Initializr (start.spring.io) chọn **Spring Web** (nếu làm ứng dụng web) hoặc chỉ **Spring Context** (nếu console). Spring Boot sẽ tạo sẵn cấu trúc dự án Maven/Gradle với mọi thư viện cần thiết.

**Bước 2: Viết mã nguồn chính** – Điểm bắt đầu thường là class có `@SpringBootApplication` và phương thức `main`. Annotation này bao gồm luôn @Configuration, @EnableAutoConfiguration, @ComponentScan, giúp tự động cấu hình và quét các bean. Ví dụ:

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

Khi chạy `SpringApplication.run`, Spring Boot sẽ khởi tạo IoC container, tạo các bean dựa trên annotation quét được.

**Bước 3: Định nghĩa các bean và logic:**

- *Ví dụ:* Xây dựng một dịch vụ tính toán thuế đơn giản. Tạo interface `TaxCalculator` với phương thức `double calculateTax(double income)`. Tạo hai triển khai: `BasicTaxCalculator` (tính thuế kiểu cơ bản 10%) và `AdvancedTaxCalculator` (tính thuế luỹ tiến phức tạp hơn). Đánh dấu cả hai lớp `@Component` (hoặc `@Service`).  
- Tạo một lớp dịch vụ sử dụng nó: `@Service class SalaryService` với thuộc tính `@Autowired private TaxCalculator taxCalc;`. Giả sử ta muốn dùng `BasicTaxCalculator` mặc định. Có thể dùng thêm `@Primary` trên bean BasicTaxCalculator để được ưu tiên inject. Trong `SalaryService` có phương thức `double getNetIncome(double gross)` trả về `gross - taxCalc.calculateTax(gross)`.  
- Viết một `@Component class CommandLineAppRunner implements CommandLineRunner` (Spring Boot cho phép chạy code sau khi khởi động ứng dụng thông qua CommandLineRunner). Implement phương thức `run(String... args)`: trong đó dùng `salaryService.getNetIncome(1000)` và in kết quả. Đánh dấu `@Autowired` `SalaryService` trong lớp này.

**Bước 4: Chạy ứng dụng:** Chạy `DemoApplication.main()`. Kết quả Spring Boot sẽ khởi chạy, tạo bean cho `BasicTaxCalculator`, `AdvancedTaxCalculator`, thấy `SalaryService` cần `TaxCalculator` nên inject `BasicTaxCalculator` (vì nó có @Primary hoặc đơn giản vì có một bean phù hợp). Sau đó `CommandLineAppRunner.run` thực thi, sử dụng `SalaryService` (cũng đã được inject) để tính toán và in kết quả ra console, rồi kết thúc. 

Ứng dụng trên minh họa một app Spring đơn giản chạy ở chế độ console. Thông thường, chúng ta hay viết ứng dụng web với Spring MVC – khi đó thay vì CommandLineRunner, ta có các controller và chạy một server. Dù là console hay web, **cách Spring quản lý bean và DI đều tương tự.**

### Bài tập thực hành với Spring

- **Bài 1: Spring DI căn bản.** Tạo một dự án Spring Boot (có thể dùng Maven, Spring Initializr). Định nghĩa một interface `GreetingService` với phương thức `getGreeting(String name)`. Tạo hai implement `EnglishGreetingService` (trả về "Hello, <name>") và `VietnameseGreetingService` (trả về "Xin chào, <name>"). Đánh dấu cả hai lớp với `@Service` (hoặc `@Component`). Trong lớp main (có @SpringBootApplication), sử dụng `ApplicationContext` để lấy một bean `GreetingService` rồi gọi `getGreeting("John")` và in ra màn hình. Lưu ý: nếu có hai bean cùng implement `GreetingService`, cần dùng `@Primary` hoặc dùng `@Qualifier` khi lấy bean. Thử cấu hình để sử dụng `VietnameseGreetingService` làm bean chính và kiểm tra kết quả.

- **Bài 2: Xây dựng ứng dụng web “Hello Spring”.** Sử dụng Spring Boot Web, tạo một controller đơn giản. Bước làm: thêm dependency spring-boot-starter-web; tạo lớp `HomeController` với annotation `@RestController`. Tạo phương thức `@GetMapping("/hello") public String hello()` trả về một chuỗi JSON hoặc text, ví dụ `"Hello Spring"`. Chạy ứng dụng và mở trình duyệt truy cập `http://localhost:8080/hello` để xem thông điệp. Sau đó, bổ sung một service layer: tạo `GreetingService` như bài 1, controller sẽ gọi service để lấy message thay vì trả về trực tiếp. Mục tiêu: Hiểu luồng hoạt động MVC: request -> controller -> service -> trả về response.

- **Bài 3: Hiểu cơ chế IoC Container.** (Bài suy nghĩ, không cần code): Trong một ứng dụng Spring, làm thế nào để một bean biết được các phụ thuộc của nó? Hãy mô tả quá trình Spring tạo bean `A` có phụ thuộc `B`: Spring phát hiện `B` là @Component và tạo trước, sau đó khi tạo `A` nó thấy `@Autowired B` nên tìm trong container bean kiểu `B` để gán. Nếu chưa có sẽ báo lỗi. Ngoài ra, thử tìm hiểu khái niệm **Bean Scope**: khi nào nên dùng scope prototype thay vì singleton? Thử viết một ví dụ nhỏ có bean A singleton chứa bean B prototype, xem mỗi lần lấy A thì B có mới không.

### Ôn tập và kiểm tra nhanh

- **Câu hỏi 1:** Spring Framework nhằm mục đích gì trong phát triển ứng dụng Java? Hãy liệt kê 2 lợi ích mà Spring IoC container đem lại cho kiến trúc ứng dụng.  
- **Câu hỏi 2:** Giải thích ngắn gọn khái niệm **Dependency Injection** và cách Spring thực hiện điều này thông qua annotation `@Autowired`.  
- **Câu hỏi 3:** Sự khác nhau giữa các annotation `@Component`, `@Service` và `@Repository` (nếu có) là gì?  
- **Câu hỏi 4:** Nếu một class A cần sử dụng class B và C (là các bean khác) thông qua DI, hãy viết ví dụ khai báo các field với annotation phù hợp trong class A.  
- **Câu hỏi 5:** Trong file cấu hình `application.properties` của Spring Boot, bạn sẽ cấu hình gì? Cho ví dụ một vài cấu hình thông dụng (như cổng server, cấu hình database).

---

## 5. Thymeleaf và Spring Web

### Sử dụng Thymeleaf để xây dựng ứng dụng Web với Spring

Trong các ứng dụng web MVC (Model-View-Controller) dùng Spring, **Thymeleaf** thường được dùng làm **template engine** để xây dựng giao diện động trên tầng View. Thymeleaf cho phép chúng ta viết trang HTML có gắn kèm các biểu thức đặc biệt để hiển thị dữ liệu từ server một cách thuận tiện.

**Thymeleaf là gì?** Đó là một thư viện Java cho phép tạo các file HTML động. Điểm đặc biệt của Thymeleaf là nó cho phép sử dụng **cú pháp gần gũi với HTML tự nhiên** – nghĩa là file template Thymeleaf chính là file HTML (có thể mở trong trình duyệt bình thường để xem khung giao diện tĩnh), và các chỉ thị động được chèn dưới dạng **thuộc tính HTML có tiền tố `th:`**. Khi chạy ứng dụng, Spring + Thymeleaf sẽ xử lý các thuộc tính này để sinh ra nội dung HTML hoàn chỉnh gửi về cho trình duyệt. Thymeleaf có thể thay thế hoàn toàn vai trò của JSP trong Spring MVC ([Tìm hiểu Thymeleaf và sử dụng Thymeleaf với Spring](https://viblo.asia/p/tim-hieu-thymeleaf-va-su-dung-thymeleaf-voi-spring-Ljy5VMp3lra#:~:text=Thymeleaf%20c%C3%B3%20th%E1%BB%83%20s%E1%BB%AD%20d%E1%BB%A5ng,0)).

**Tích hợp Thymeleaf với Spring Boot:** Spring Boot Starter Web đi kèm với Thymeleaf nếu thêm dependency `spring-boot-starter-thymeleaf`. Mặc định, các file template `.html` đặt trong thư mục `src/main/resources/templates` sẽ được tự động nạp. Controller trong Spring khi trả về tên view (chuỗi tên template) sẽ được Thymeleaf xử lý để tìm file tương ứng và render.

### Tạo giao diện động với Thymeleaf

Một số tính năng chính và cú pháp của Thymeleaf:

- **Hiển thị biến (th:text):** Để hiển thị nội dung của một biến model (biến được controller đưa vào model), ta dùng thuộc tính `th:text`.  
  *Ví dụ:* Trong controller, ta thêm `model.addAttribute("message", "Xin chào, Thymeleaf");` và trả về view "home". Trong file `home.html`:  
  ```html
  <h1 th:text="${message}">[Thông báo]</h1>
  ```  
  Khi render, Thymeleaf sẽ thay thế nội dung thẻ `<h1>` bằng giá trị của `message` từ model. Nếu `message = "Xin chào, Thymeleaf"`, kết quả HTML gửi ra sẽ là `<h1>Xin chào, Thymeleaf</h1>`. Thuộc tính `th:text` sẽ **ghi đè nội dung** thẻ (nên giữa `<h1>` ban đầu có nội dung placeholder chỉ để xem khi mở file trực tiếp, sẽ bị thay thế).

- **Thiết lập thuộc tính (th:src, th:href, ...):** Thymeleaf cho phép đặt giá trị cho thuộc tính thẻ HTML một cách động.  
  *Ví dụ:*  
  ```html
  <img th:src="@{/images/logo.png}" alt="Logo">
  <a th:href="@{http://example.com/page?id=${itemId}}">Chi tiết</a>
  ```  
  Ở đây, `th:src="@{/images/logo.png}"` sẽ được Thymeleaf chuyển thành `src="/images/logo.png"` (cú pháp `${...}` để truy xuất biến model, còn `@{...}` để tạo URL, có thể bao gồm path của ứng dụng). Tương tự cho th:href – trong ví dụ trên, nếu `itemId = 5`, link sẽ trở thành `<a href="http://example.com/page?id=5">Chi tiết</a>`.

- **Lặp với danh sách (th:each):** Để hiển thị danh sách dữ liệu (ví dụ bảng danh sách người dùng), ta dùng thuộc tính `th:each` trên thẻ HTML lặp.  
  *Ví dụ:*  
  ```html
  <table>
    <tr>
      <th>ID</th><th>Name</th><th>Email</th>
    </tr>
    <tr th:each="stu : ${students}">
      <td th:text="${stu.id}">1</td>
      <td th:text="${stu.name}">Name</td>
      <td th:text="${stu.email}">email@example.com</td>
    </tr>
  </table>
  ```  
  Nếu model có `students` là List<Student>, Thymeleaf sẽ lặp tạo một `<tr>` cho mỗi phần tử. Bên trong, `${stu.id}` lấy trường id của đối tượng hiện tại, tương tự cho name và email. Kết quả là một bảng HTML hoàn chỉnh liệt kê sinh viên.

- **Cấu trúc điều kiện (th:if, th:unless):** Cho phép ẩn/hiện một thành phần dựa trên điều kiện.  
  *Ví dụ:*  
  ```html
  <p th:if="${user != null}">Welcome, <span th:text="${user.username}">user</span>!</p>
  <p th:unless="${user != null}">Welcome, Guest!</p>
  ```  
  Nếu biến `user` khác null, sẽ hiện đoạn chào mừng người dùng, ngược lại hiện "Welcome, Guest!". (th:unless là phủ định của th:if).

- **Fragment và Layout:** Thymeleaf còn hỗ trợ tách giao diện thành các phần (fragment) và tái sử dụng (như header, footer chung nhiều trang) với `th:fragment` và `th:replace`/`th:insert`. Điều này giúp tạo layout trang web hiệu quả (ví dụ có một template base chứa khung chung, các trang cụ thể chèn nội dung vào).

**Sử dụng Thymeleaf trong Spring MVC controller:**  
Khi viết controller (annotation `@Controller`), thay vì dùng `@RestController` (trả JSON), ta dùng phương thức trả về tên view (string). Ví dụ:

```java
@Controller
public class StudentController {
    @GetMapping("/students")
    public String listStudents(Model model) {
        List<Student> students = studentService.findAll();
        model.addAttribute("students", students);
        return "students"; // trả về template "students.html"
    }
}
```

Ở đây, Spring MVC sẽ tìm file `students.html` trong `resources/templates`. Thymeleaf render file này với biến `${students}` đã được đưa vào model. Template `students.html` có thể như ví dụ bảng ở trên để hiển thị danh sách.

**Form và gửi dữ liệu:** Thymeleaf hỗ trợ tốt cho tạo form HTML và binding dữ liệu với model object (sử dụng `th:object` và `th:field`). Ví dụ form thêm sinh viên:

```html
<form th:action="@{/students}" th:object="${studentForm}" method="post">
    <p>Name: <input type="text" th:field="*{name}" /></p>
    <p>Email: <input type="email" th:field="*{email}" /></p>
    <button type="submit">Add Student</button>
</form>
```

Controller tương ứng sẽ có phương thức `@PostMapping("/students")` nhận đối tượng `@ModelAttribute Student studentForm` để lấy dữ liệu người dùng nhập. Thymeleaf tự động mapping các field name với thuộc tính của đối tượng.

### Ví dụ minh họa + bài tập thực hành

**Ví dụ:** Xây dựng giao diện hiển thị danh sách sản phẩm và chi tiết sản phẩm dùng Thymeleaf.

- Trang danh sách (template `products.html`): hiển thị bảng các sản phẩm với cột Tên, Giá, nút "Xem chi tiết". Sử dụng `th:each` để liệt kê sản phẩm từ biến model `products` (List<Product>). Nút "Xem chi tiết" có link `th:href="@{/product/{id}(id=${prod.id})}"` dẫn đến URL chi tiết.

- Trang chi tiết (template `product_detail.html`): hiển thị thông tin chi tiết của một sản phẩm (tên, giá, mô tả). Sử dụng các th:text để điền dữ liệu từ biến model `product`. Có nút/quay lại về danh sách (th:href về `/products`).

- Controller: 
  ```java
  @Controller
  public class ProductController {
      @Autowired ProductService productService;

      @GetMapping("/products")
      public String list(Model model) {
          model.addAttribute("products", productService.getAll());
          return "products";
      }

      @GetMapping("/product/{id}")
      public String detail(@PathVariable("id") Long id, Model model) {
          Product prod = productService.getById(id);
          model.addAttribute("product", prod);
          return "product_detail";
      }
  }
  ```
  Service `ProductService` trả về danh sách hoặc sản phẩm theo id (phần service/DAO này chỉ cần mô phỏng, có thể giả lập dữ liệu).

Khi chạy, truy cập `http://localhost:8080/products` sẽ xem được danh sách sản phẩm, nhấp vào link chi tiết sẽ gọi `/product/{id}` và Thymeleaf render trang chi tiết tương ứng.

**Bài tập thực hành:**

- **Bài 1: Tạo trang web giới thiệu bản thân.** Sử dụng Spring Boot + Thymeleaf, tạo một trang đơn giản hiển thị thông tin cá nhân. Viết một controller với mapping “/about” trả về view "about.html". Trong template, sử dụng ít nhất 3 biến model (ví dụ: tên, nghề nghiệp, danh sách sở thích). Controller đưa các thông tin này vào Model. Template sử dụng `th:text` để hiển thị tên, nghề nghiệp và dùng `th:each` để liệt kê danh sách sở thích (mỗi sở thích trong một `<li>`).

- **Bài 2: Form đăng ký người dùng.** Tạo một form HTML cho phép người dùng nhập thông tin đăng ký (tên, email, mật khẩu). Sử dụng Thymeleaf form binding: Tạo class `UserForm` với các trường trên. Controller cung cấp model `userForm` cho GET `/register` để hiển thị form. Khi submit (POST `/register`), controller nhận dữ liệu vào một đối tượng `UserForm` (dùng `@ModelAttribute`). Kiểm tra và in ra màn hình (có thể in ra console hoặc chuyển hướng sang trang thành công). Yêu cầu: Sử dụng `th:object` và `th:field` trong form Thymeleaf để binding dữ liệu.

- **Bài 3: Template layout.** Tạo một layout chung cho các trang web của bạn (ví dụ chứa header với menu và footer cố định). Sử dụng Thymeleaf fragment: tạo file `layout.html` chứa cấu trúc HTML cơ bản và khai báo một fragment `content`. Các trang khác (vd `home.html`, `contact.html`) sẽ `th:insert` nội dung của chúng vào fragment content của layout. Kiểm tra rằng thay đổi ở layout (như sửa footer) sẽ tự động áp dụng cho tất cả trang.

### Ôn tập và kiểm tra nhanh

- **Câu hỏi 1:** Thymeleaf dùng để làm gì trong ứng dụng Spring MVC? Tại sao ta cần Thymeleaf thay vì chỉ trả về dữ liệu JSON?  
- **Câu hỏi 2:** Cú pháp `${...}` và `*{...}` trong Thymeleaf khác nhau như thế nào? (Gợi ý: `${...}` truy cập biến trong model, còn `*{...}` dùng trong bối cảnh đối tượng được thiết lập bởi th:object).  
- **Câu hỏi 3:** Để hiển thị danh sách các đối tượng `Book` trên giao diện bằng Thymeleaf, bạn sẽ dùng thuộc tính gì để lặp qua danh sách? Viết một đoạn mã mẫu cho việc này.  
- **Câu hỏi 4:** Làm sao để tạo một link động trong Thymeleaf trỏ đến URL `/product/5` (giả sử 5 là một biến id)? Gợi ý cú pháp `@{...}`.  
- **Câu hỏi 5:** Nếu muốn tái sử dụng một header HTML cho nhiều trang, Thymeleaf cung cấp cơ chế gì? Hãy nêu tên và cách sử dụng cơ bản.

---

## 6. Làm việc với Database trong Spring

### Kết nối cơ sở dữ liệu trong Spring (JPA, Hibernate, ...)

Spring hỗ trợ mạnh mẽ việc tương tác với **cơ sở dữ liệu** thông qua các module như **Spring JDBC**, **Spring ORM** và đặc biệt là **Spring Data JPA**. Mục tiêu là đơn giản hóa việc kết nối, truy vấn và thao tác dữ liệu bằng cách tận dụng các ORM (Object-Relational Mapping) framework như **Hibernate**.

- **JPA và Hibernate:** *JPA* (Java Persistence API) là một đặc tả (specification) của Java định nghĩa cách quản lý dữ liệu quan hệ bằng đối tượng Java. *Hibernate* là một implementation nổi tiếng của JPA (ngoài ra còn EclipseLink, OpenJPA...). Spring Data JPA là một library của Spring giúp tích hợp JPA một cách thuận tiện, giảm bớt boilerplate code.

- **Cấu hình kết nối DB trong Spring Boot:** Với Spring Boot, cấu hình database chủ yếu qua `application.properties` (hoặc `application.yml`). Bạn cần cung cấp các thông số như:
  ```properties
  spring.datasource.url=jdbc:mysql://localhost:3306/mydb
  spring.datasource.username=root
  spring.datasource.password=1234
  spring.jpa.hibernate.ddl-auto=update
  spring.jpa.show-sql=true
  ```
  Dòng trên thiết lập URL JDBC, user, password cho datasource (ví dụ MySQL). `ddl-auto=update` cho phép Hibernate tự động tạo/ cập nhật schema dựa trên entity (dùng cho phát triển, không nên dùng trên production), `show-sql=true` để in câu SQL ra console cho dễ debug. Spring Boot tự động tạo DataSource và EntityManager dựa trên cấu hình này.

- **Entity (Thực thể):** Là lớp Java ánh xạ với bảng trong database. Đánh dấu bằng annotation `@Entity` và mỗi entity cần một **primary key** với `@Id` (thường kết hợp `@GeneratedValue` để auto-increment). Mỗi field ánh xạ một cột (có thể dùng `@Column` để tùy chỉnh tên cột, độ dài,... nếu cần). 
  *Ví dụ:*  
  ```java
  @Entity
  public class Student {
      @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
      private Long id;
      private String name;
      private String email;
      // getters, setters, constructors...
  }
  ```
  Đoạn trên định nghĩa entity `Student` ứng với bảng `student` (mặc định tên bảng = tên lớp, có thể dùng @Table để chỉ định tên khác). Trường `id` là khóa chính, tự động tăng (IDENTITY phù hợp MySQL). Các trường name, email ánh xạ cột cùng tên.

- **Repository (Spring Data JPA):** Thay vì viết DAO lớp xử lý SQL, Spring Data JPA cho phép chúng ta chỉ cần khai báo một interface *repository* kế thừa `JpaRepository<Entity, KeyType>`, Spring sẽ tự động tạo bean và cung cấp các phương thức CRUD cơ bản. 
  *Ví dụ:*  
  ```java
  public interface StudentRepository extends JpaRepository<Student, Long> {
      // có thể định nghĩa thêm query method nếu cần, ví dụ:
      List<Student> findByNameContaining(String keyword);
  }
  ```
  Khi chạy, Spring Data JPA sẽ tạo một implementation cho `StudentRepository`. Bạn có thể `@Autowired` repository này ở service hoặc controller để sử dụng. Các phương thức có sẵn như `save()`, `findById()`, `findAll()`, `deleteById()`, v.v. Ngoài ra, method `findByNameContaining(String)` theo quy ước đặt tên sẽ được Spring parse thành câu query tìm các Student có name chứa chuỗi keyword.

- **Service layer (tuỳ chọn):** Thông thường, ta có một lớp Service nằm giữa Controller và Repository để viết các logic nghiệp vụ phức tạp hoặc giao dịch. Nhưng với CRUD đơn giản, controller có thể gọi repository trực tiếp. Service cũng giúp đóng gói việc xử lý transaction (Spring @Transactional), nhưng Spring Data JPA các phương thức repository mặc định đã được quản lý giao dịch.

### Thao tác CRUD trong Spring

Với Spring Data JPA, các thao tác CRUD (Create-Read-Update-Delete) trở nên khá đơn giản:

- **Create (Tạo mới):** Sử dụng phương thức `save(entity)` của repository. Nếu entity chưa có Id (null), repository sẽ gọi `INSERT` DB. *Ví dụ:* 
  ```java
  Student s = new Student();
  s.setName("John");
  s.setEmail("john@example.com");
  studentRepo.save(s);
  ```
  Sau lệnh trên, đối tượng `s` sẽ được gắn Id (do JPA trả về).

- **Read (Đọc/Xem):** 
  - Lấy một đối tượng theo Id: `findById(id)` trả về `Optional<Student>` (ta có thể dùng `orElse(null)` hoặc `orElseThrow` tùy ý để xử lý khi không tìm thấy). 
  - Lấy tất cả: `findAll()` trả về `List<Student>`. 
  - Lấy theo điều kiện: dùng các phương thức query derivation như `findByEmail(String email)`, `findByNameContaining(String name)`, hoặc dùng @Query tùy chỉnh nếu cần query phức tạp.

- **Update (Cập nhật):** Thao tác update thực chất cũng dùng `save()`. Nếu entity có Id khác null và tồn tại trong DB, `save()` sẽ thực hiện `UPDATE`. Các bước thường: dùng repository lấy entity ra, thay đổi một vài trường, rồi gọi save. (Hoặc nếu chắc chắn entity tồn tại, có thể tạo đối tượng với Id đó rồi save, Hibernate sẽ hiểu là update).

- **Delete (Xóa):** Gọi `deleteById(id)` để xóa theo khóa chính, hoặc `delete(entity)` nếu có entity. Ngoài ra có thể có method `deleteAll()` để xóa tất cả. Mặc định, xóa một entity liên quan có cascade cần chú ý cấu hình trong quan hệ (cascade = REMOVE, etc. – khi có quan hệ giữa các bảng).

**Ví dụ:** Xây dựng tính năng CRUD cho `Student`:
- *Create:* Form Thymeleaf "Thêm sinh viên" gửi dữ liệu lên `/students` (POST). Controller nhận `Student` từ form và gọi `studentRepo.save(student)`, sau đó redirect về danh sách.
- *Read:* Trang danh sách sinh viên, controller dùng `studentRepo.findAll()` đưa vào model, Thymeleaf hiển thị bảng.
- *Update:* Trang chỉnh sửa (tương tự form thêm nhưng điền sẵn thông tin), gửi PUT (hoặc POST) đến `/students/{id}`. Controller nhận dữ liệu, kiểm tra entity tồn tại (`findById`), cập nhật và save. Redirect về danh sách.
- *Delete:* Nút "Xóa" trên mỗi dòng danh sách gửi request DELETE (hoặc GET đơn giản) đến `/students/delete/{id}`. Controller gọi `studentRepo.deleteById(id)` rồi redirect.

Lưu ý: Nếu dùng phương thức HTTP PUT/DELETE, cần xử lý trong Spring Boot (mặc định Thymeleaf form chỉ hỗ trợ GET/POST, nhưng Spring cung cấp `_method` hidden field để giả lập PUT/DELETE).

### Xây dựng ứng dụng CRUD hoàn chỉnh

Kết hợp tất cả: Ta có thể tạo một ứng dụng web Spring Boot quản lý sinh viên (hoặc **bất kỳ đối tượng nào** như Sản phẩm, Công việc, v.v.) với các thành phần:

- **Entity + Repository:** Mô tả đối tượng và kết nối DB.
- **Service (tùy chọn):** Xử lý logic (nếu logic đơn giản có thể bỏ qua).
- **Controller:** Xử lý các request web (list, view form, xử lý form submit).
- **Thymeleaf Templates:** Giao diện cho danh sách, form thêm/sửa, v.v.

**Ví dụ đề xuất:** Ứng dụng quản lý **Thư viện Sách** (CRUD sách):
- Entity `Book` (id, title, author, publishedDate, ...).
- Repository `BookRepository` extends JpaRepository<Book, Long>.
- Controller `BookController` với các handler:
  - GET `/books` – hiển thị danh sách (view "book_list.html").
  - GET `/books/new` – hiển thị form thêm mới (view "book_form.html" dùng `th:object="${book}"`).
  - POST `/books` – xử lý lưu sách mới (nhận `Book` từ form, save, redirect `/books`).
  - GET `/books/{id}/edit` – hiển thị form chỉnh sửa (model đổ dữ liệu sách hiện tại).
  - POST `/books/{id}` – xử lý cập nhật sách (nhận `Book`, có id, save, redirect `/books`).
  - GET `/books/{id}/delete` – xóa sách rồi redirect về danh sách.
- Templates Thymeleaf tương ứng:
  - `book_list.html` liệt kê sách, có link "New Book", link "Edit" từng sách, nút "Delete".
  - `book_form.html` chứa form chung cho thêm và sửa (có thể dùng cùng template, tùy biến tiêu đề form nếu sách đã có id thì là "Edit Book", không có thì "Add Book").

- Navigation: Đảm bảo sau các thao tác thì chuyển hướng (redirect) hợp lý để tránh lặp lại form (Post/Redirect/Get pattern).

**Thực hiện trên H2 Database:** Để đơn giản, có thể dùng **H2 (database nhúng)** để không cần cài đặt. Chỉ cần cấu hình `spring.datasource.url=jdbc:h2:mem:testdb` (bộ nhớ) hoặc đường dẫn file. H2 sẽ tự tạo DB khi chạy ứng dụng.

**Kiểm thử:** Chạy ứng dụng, truy cập chức năng thêm, sửa, xóa xem dữ liệu lưu vào DB và hiển thị đúng không. Nếu cấu hình `show-sql=true`, có thể xem câu SQL trong log để hiểu Spring Data JPA đang làm gì.

### Bài tập thực hành

- **Bài 1: Ứng dụng Quản lý Nhiệm vụ (Task Manager).** Xây dựng một ứng dụng Spring Boot nhỏ quản lý công việc (Task):
  - Entity `Task` (id, title, description, dueDate, status).
  - Repository `TaskRepository`.
  - Controller `TaskController` với các chức năng CRUD tương tự như trên (liệt kê tasks, thêm task, đánh dấu hoàn thành (status done), xóa task).
  - Giao diện Thymeleaf cho danh sách tasks (hiển thị trạng thái, quá hạn hay chưa dựa trên dueDate), form thêm/sửa task.
  - Thực hành: Thêm một chức năng lọc tasks (ví dụ: xem chỉ tasks chưa hoàn thành hoặc tasks quá hạn).

- **Bài 2: Tích hợp xác thực cơ bản (tuỳ chọn nâng cao).** Thêm một bảng `User` (id, username, password) và cho phép người dùng đăng ký, đăng nhập để quản lý task của riêng mình. Sử dụng Spring Security (nâng cao, nếu học tới) hoặc tự quản lý session để hạn chế chức năng CRUD chỉ khi đăng nhập. (Bài này mở rộng nhiều kiến thức ngoài, làm nếu muốn tìm hiểu thêm).

- **Bài 3: Xuất dữ liệu ra CSV/Excel (tuỳ chọn).** Viết chức năng xuất toàn bộ danh sách Task hoặc Book (từ ví dụ trên) ra file CSV. Gợi ý: Sử dụng thư viện OpenCSV hoặc Apache POI (xuất Excel) – thao tác này có thể làm ở Service, rồi controller trả về `ResponseEntity<byte[]>` với header Content-Disposition để tải file. (Nội dung nâng cao, không bắt buộc cho khóa cơ bản, nhưng thử thách dành cho ai muốn thực chiến hơn).

### Ôn tập và kiểm tra nhanh

- **Câu hỏi 1:** JPA là gì? Hibernate là gì? Mối quan hệ giữa chúng ra sao trong ứng dụng Spring? ([Spring Boot application with CRUD operations using Spring Data ...](https://medium.com/@chandantechie/spring-boot-application-with-crud-operations-using-spring-data-jpa-and-mysql-23c8019660b1#:~:text=,and%20MySQL%20involves%20several%20steps))  
- **Câu hỏi 2:** Khi định nghĩa một entity trong JPA, annotation nào được dùng để đánh dấu: (a) Lớp entity, (b) Khóa chính, (c) Cột tương ứng của một thuộc tính?  
- **Câu hỏi 3:** Spring Data JPA giúp ích gì cho việc viết CRUD? Bạn cần viết gì để có một repository cơ bản thao tác với bảng `Employee`?  
- **Câu hỏi 4:** Giả sử bạn có đối tượng `Product p` đã có Id, muốn cập nhật giá trị của nó trong DB. Bạn sẽ dùng phương thức nào của JpaRepository? Nếu `p` chưa có trong DB (Id null), phương thức đó sẽ làm gì?  
- **Câu hỏi 5:** Nêu các bước chính để tạo một chức năng “Xóa” bản ghi trong ứng dụng Spring MVC dùng Spring Data JPA (từ giao diện Thymeleaf đến thao tác repository).  
- **Câu hỏi 6:** Công dụng của thuộc tính `spring.jpa.show-sql=true` trong cấu hình là gì? Khi nào hữu ích cho lập trình viên?

---

## 7. Dự án Cuối Khóa

Đến đây, bạn đã được học qua hầu hết các kiến thức quan trọng: từ tư duy thiết kế (OOP, SOLID, Design Patterns) đến các kỹ thuật lập trình Java căn bản (đa luồng, I/O, mạng) và phát triển ứng dụng web với Spring (bao gồm Spring MVC, Thymeleaf, Spring Data JPA). **Dự án cuối khóa** sẽ là cơ hội để bạn vận dụng tổng hợp những kiến thức này, xây dựng một ứng dụng hoàn chỉnh và trải nghiệm quy trình phát triển phần mềm thực tế.

### Đề bài dự án

Hãy xây dựng một **hệ thống quản lý web** (web management system) cho một chủ đề mà bạn quan tâm, sử dụng Spring Framework và các design patterns phù hợp. Ví dụ đề tài: *Quản lý thư viện sách, Quản lý khóa học trực tuyến, Quản lý cửa hàng, Quản lý công việc cá nhân,...* Ứng dụng của bạn nên là một **ứng dụng web** cho phép thực hiện các chức năng CRUD trên một vài thực thể chính và có giao diện người dùng để tương tác.

**Yêu cầu chung:**

- Sử dụng **Spring Boot** để phát triển ứng dụng web (bao gồm Spring MVC + Thymeleaf cho giao diện, Spring Data JPA cho database).
- Áp dụng **ít nhất 2 design pattern** trong thiết kế hệ thống. (Ví dụ: sử dụng Singleton cho một lớp config hoặc kết nối, Factory Method cho logic tạo đối tượng phức tạp, Strategy cho một chức năng có nhiều phương án xử lý, etc. Bạn nên giải thích trong tài liệu báo cáo pattern nào được dùng và ở đâu).
- Tuân thủ các nguyên tắc **OOP và SOLID** trong cấu trúc code. Ví dụ: tách các tầng Controller, Service, Repository rõ ràng; các lớp/service mỗi cái một nhiệm vụ; dùng interface cho service/repository nếu cần để tiện mở rộng và test (DIP); tránh lặp code v.v.
- Giao diện web xây dựng với **Thymeleaf** cho phép người dùng thực hiện các chức năng quản lý (thêm, xem, sửa, xóa dữ liệu). Giao diện không cần quá phức tạp, nhưng tối thiểu có trang danh sách và form thêm/sửa cho mỗi chức năng chính.
- Sử dụng **cơ sở dữ liệu quan hệ** (H2, MySQL, PostgreSQL tùy bạn chọn). Nếu có thể, seed sẵn một ít dữ liệu để demo.
- **Đa luồng:** Nếu phù hợp với đề tài, hãy thể hiện việc sử dụng đa luồng. (Ví dụ: một chức năng gửi email thông báo có thể chạy ở luồng riêng, hoặc một tác vụ nền cập nhật trạng thái,...). Nếu dự án của bạn không có chỗ tự nhiên cho đa luồng, bạn có thể viết một logic nhỏ chạy đa luồng trong service cho biết bạn biết cách sử dụng (vd: tính toán gì đó song song).
- **Xử lý file hoặc gọi mạng:** Tương tự đa luồng, nếu có tình huống sử dụng I/O hoặc call HTTP thì tốt. Không bắt buộc, nhưng ví dụ nếu làm quản lý thư viện, có thể có chức năng import/export dữ liệu từ file CSV; hoặc gọi một API ngoài để bổ sung dữ liệu (vd lấy bìa sách từ một URL). Hãy sáng tạo nếu có thể.

### Các bước triển khai dự án

1. **Lập kế hoạch:** Chọn chủ đề cụ thể cho hệ thống quản lý. Xác định các **entity** chính (ví dụ: với quản lý thư viện có `Book`, `Author`, `User`, `Loan`...). Lên danh sách các chức năng sẽ có (CRUD trên các entity, các trang giao diện cần thiết). Vạch ra kiến trúc tổng quan: sẽ có mấy module, mỗi module phụ trách gì.

2. **Thiết kế cơ sở dữ liệu:** Vẽ sơ đồ các bảng và mối quan hệ (nếu có nhiều bảng liên quan). Từ đó định nghĩa các class Entity tương ứng trong Java. Áp dụng **design pattern** nếu có chỗ: ví dụ, có thể dùng **Builder pattern** nếu entity có nhiều thuộc tính cần xây dựng, hoặc **Factory** nếu việc tạo đối tượng có logic phức tạp (hoặc dựa trên tham số). Đảm bảo khóa chính, khóa ngoại (trong entity dùng @OneToMany, @ManyToOne nếu có quan hệ).

3. **Thiết kế tầng service và áp dụng SOLID:** Xác định các **service** lớp sẽ viết để xử lý nghiệp vụ. Thường mỗi entity chính có một service tương ứng (vd: `BookService`, `UserService`...). Những logic như “mượn sách” có thể nằm trong một service kết hợp (vd `LibraryService` thực hiện việc gắn Book với User, kiểm tra số lượng...). Tại đây, nhìn lại SOLID:
   - SRP: Mỗi service một nhiệm vụ.
   - OCP: Thiết kế các phương thức mở rộng dễ (vd thay vì viết cứng nếu...else, có thể dùng Strategy pattern cho các biến thể nghiệp vụ).
   - LSP: Nếu dùng interface + implementation cho service, đảm bảo implement thay thế tốt.
   - ISP: Nếu có interface quá lớn, xem xét tách.
   - DIP: Sử dụng interface cho tầng service/repo và Spring DI để inject (mặc dù Spring cho phép inject implementation thẳng, việc dùng interface giúp decouple tốt hơn).

4. **Viết code khung dự án:**
   - Tạo project Spring Boot, thêm các dependency (spring-boot-starter-web, spring-boot-starter-thymeleaf, spring-boot-starter-data-jpa, và driver DB).
   - Tạo các package: `entity`, `repository`, `service`, `controller`, `config` (nếu cần cấu hình riêng), `util` (nếu có lớp tiện ích).
   - Tạo các Entity class, Repository interface.
   - Tạo các Service class (có thể đánh dấu @Service, và dùng @Transactional nếu cần khi thao tác db nhiều bước).
   - Tạo các Controller class cho các chức năng/webpages.

5. **Xây dựng giao diện (Thymeleaf templates):** Thiết kế các trang HTML trong thư mục templates:
   - Template layout chung (nếu dùng).
   - Trang chủ hoặc menu điều hướng.
   - Trang danh sách cho mỗi đối tượng quản lý (liệt kê records, có nút thêm mới, sửa, xóa).
   - Trang form cho thêm/sửa.
   - (Trang đăng nhập nếu có quản lý người dùng).
   Mỗi trang Thymeleaf nên kiểm tra hoạt động đúng với controller tương ứng.

6. **Kiểm thử và hoàn thiện:** Chạy ứng dụng, thử các chức năng. Debug lỗi nếu có. Kiểm tra các pattern đã áp dụng có hoạt động như mong muốn. Dọn dẹp code: xóa code thừa, thêm comment JavaDoc nếu có thể. Đảm bảo tuân thủ các best practice đã học (ví dụ: không viết truy vấn logic trong controller, không lạm dụng static, etc.)

7. **Triển khai dự án lên GitHub:** Tạo một repository GitHub (nếu chưa có). Đẩy toàn bộ mã nguồn dự án lên. Đảm bảo không commit các thông tin nhạy cảm (ví dụ mật khẩu DB – có thể dùng H2 để không lo tài khoản, hoặc dùng file `.gitignore` để bỏ qua nếu có). Viết file README.md giới thiệu ngắn gọn về dự án (chủ đề, các chức năng, cách chạy dự án, tài khoản demo nếu có).

8. **Viết tài liệu và tối ưu mã nguồn:** Soạn thảo một báo cáo ngắn hoặc bổ sung vào README:
   - Mô tả kiến trúc hệ thống, các thành phần.
   - Liệt kê các design pattern đã sử dụng, giải thích chúng được dùng ở đâu, giải quyết vấn đề gì.
   - Đánh giá tuân thủ SOLID: code của bạn đã áp dụng những nguyên tắc nào, có đoạn nào chưa tối ưu không?
   - Hướng dẫn chạy thử, ảnh chụp màn hình kết quả (nếu có giao diện).
   - (Nếu được) phân tích những gì có thể cải thiện hoặc mở rộng thêm nếu có thời gian.

### Gợi ý một số design pattern trong dự án

- **Singleton**: Lớp quản lý kết nối bên ngoài, hoặc lớp config. Tuy nhiên, trong Spring, các bean mặc định là singleton, nên bạn có thể xem mỗi bean như một singleton do container quản lý. Nếu có class tiện ích không trạng thái, có thể dùng singleton kiểu Spring bean hoặc enum singleton.
- **Factory**: Nếu ứng dụng của bạn cần tạo các đối tượng dựa trên tham số hoặc loại, có thể viết một Factory. Ví dụ: ứng dụng quản lý cửa hàng có thể có Factory tạo đối tượng `Notification` khác nhau (EmailNotification, SMSNotification) tùy kênh gửi.
- **Strategy**: Khi có nhiều cách thức thực hiện một chức năng. Ví dụ: tính giá ship hàng (nhanh, thường), tính giảm giá (theo phần trăm, theo tiền cố định) – áp dụng Strategy pattern để dễ mở rộng thêm cách tính mới.
- **Observer**: Nếu có tình huống cần thông báo sự kiện. Ví dụ: sau khi người dùng đăng ký tài khoản thành công (sự kiện), ta muốn gửi email chào mừng – có thể thiết kế Event + Observer để tách rời việc gửi email ra khỏi logic đăng ký.
- **Template Method**: Nếu có các quy trình nghiệp vụ chung nhưng một vài bước cụ thể khác nhau giữa các trường hợp. Ví dụ: quy trình xử lý đơn hàng có thể có bước tính thuế khác nhau ở các bang -> có thể dùng template method và override bước tính thuế.
- **Decorator**: Nếu muốn bổ sung tính năng cho đối tượng động. Ít gặp trong ứng dụng business, nhưng có thể sử dụng cho trang trí giao diện hoặc kết quả (ví dụ, wrapper một dịch vụ để log thêm thông tin mỗi khi gọi).
- **MVC**: Bản thân kiến trúc Spring MVC đã là một pattern (Model-View-Controller) – bạn đang áp dụng rồi.
- **DAO**: Pattern Data Access Object chính là tầng Repository bạn dùng thông qua Spring Data.

Hãy nhớ, quan trọng nhất là code **đúng chức năng** và **sạch**. Design pattern chỉ là công cụ hỗ trợ, đừng cố gượng ép nếu không phù hợp. Chỉ dùng khi nó làm thiết kế của bạn rõ ràng và linh hoạt hơn.

### Triển khai dự án lên GitHub

Đưa dự án lên GitHub không chỉ để nộp bài mà còn để tập thói quen dùng hệ thống quản lý phiên bản:

- Khởi tạo kho (repository) Git cho thư mục dự án (`git init`), tạo commit đầu tiên với toàn bộ mã nguồn.
- Tạo tài khoản GitHub (nếu chưa có), tạo repository mới (có thể đặt là `java-final-project` chẳng hạn, để public cho dễ chia sẻ).
- Liên kết repo local với GitHub remote: `git remote add origin <repo-url>`.
- Push mã nguồn: `git push -u origin master` (hoặc main tùy git init).
- Kiểm tra trên GitHub xem mã nguồn đã xuất hiện đầy đủ chưa. Cập nhật README.md trực tiếp trên web hoặc qua commit đều được.

Khi đẩy lên GitHub, chú ý không đưa thông tin nhạy cảm. Nếu dự án dùng MySQL với mật khẩu thật, có thể sử dụng file `.properties` riêng hoặc biến môi trường và không commit chúng (sử dụng `.gitignore`). Giải pháp đơn giản: dùng H2 database (nhúng, không cần mật khẩu) trong cấu hình để mọi người có thể chạy dễ dàng.

### Hướng dẫn viết tài liệu và tối ưu mã nguồn

Khi hoàn thành, bạn nên dành thời gian **refactor và tối ưu** mã nguồn:

- Đặt tên biến, tên hàm rõ nghĩa, tuân theo coding convention Java (camelCase, PascalCase cho class, Hằng số viết HOA, v.v.).
- Xóa bỏ những đoạn mã không dùng đến, comment code thừa.
- Kiểm tra format code (thụt đầu dòng, khoảng trắng) cho nhất quán, dễ đọc.
- Đảm bảo phân chia package, lớp hợp lý, tránh lớp quá dài. Nếu một hàm quá dài hoặc phức tạp, xem xét tách nhỏ.
- Kiểm tra các chỗ có thể xảy ra lỗi (null, ngoại lệ) và xử lý chúng (try-catch, hoặc validate input).
- Thử nghĩ vài tình huống mở rộng: nếu sau này yêu cầu thay đổi A hoặc thêm tính năng B, code bạn có dễ chỉnh sửa không? Điều này giúp đánh giá tính đóng mở (OCP) và linh hoạt của thiết kế. Nếu thấy chỗ nào cứng nhắc, cân nhắc cải thiện (nhưng tránh “quá sớm tối ưu” – chỉ sửa khi cần thiết).
- Chạy kiểm thử lần cuối, có thể nhờ người khác dùng thử nếu được, xem có lỗi hoặc UI bất tiện nào không.

Cuối cùng, hoàn thiện **tài liệu báo cáo** (có thể dưới dạng Markdown trong README hoặc file PDF gửi kèm tùy yêu cầu khóa học). Trong tài liệu, trình bày:

- Mục tiêu dự án, mô tả bài toán.
- Các chức năng đã thực hiện.
- Hướng dẫn chạy (cách build, run, tài khoản demo,...).
- Ảnh chụp màn hình giao diện (nếu là ứng dụng web, chụp một vài trang chính).
- Kiến trúc: liệt kê các module (Controller, Service, Repository, Entity), cơ sở dữ liệu (liệt kê bảng).
- Các kỹ thuật đã áp dụng: Spring Boot, Thymeleaf, JPA, etc., và những design pattern, nguyên tắc thiết kế đã sử dụng (đưa vào để thể hiện bạn có áp dụng lý thuyết).
- Kết luận: học được gì, nếu làm thêm sẽ làm gì,...

Việc viết tài liệu giúp bạn hệ thống lại những gì đã làm và cũng là phần **giải thích cho người xem/giáo viên** hiểu được dự án của bạn đã đáp ứng yêu cầu ra sao.

Chúc mừng bạn đã hoàn thành khóa học Java! Dự án cuối khóa này không chỉ là bài kiểm tra kiến thức, mà quan trọng hơn, nó là sản phẩm đầu tay thể hiện kỹ năng của bạn. Hãy làm hết sức, nhưng cũng đừng quá căng thẳng – qua mỗi bước, bạn sẽ học thêm điều mới. Chúc bạn triển khai dự án thành công và có một sản phẩm hoàn chỉnh để tự hào chia sẻ!
