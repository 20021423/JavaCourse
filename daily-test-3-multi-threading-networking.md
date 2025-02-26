# DAILY TEST 3: MULTI-THREADED PROGRAMMING, DATA FILES, AND NETWORK PROGRAMMING

## Câu hỏi trắc nghiệm

### 1. Trong Java, để tạo một luồng (thread) mới, bạn có thể:
A. Kế thừa từ lớp Thread
B. Triển khai giao diện Runnable
C. Sử dụng java.util.concurrent.Executor
D. Tất cả các phương án trên

**Đáp án: D**

*Giải thích:* Trong Java, có nhiều cách để tạo thread mới, bao gồm kế thừa từ lớp Thread, triển khai giao diện Runnable, và sử dụng các lớp trong gói java.util.concurrent như Executor để quản lý thread. Cả ba phương pháp đều có thể được sử dụng tùy thuộc vào nhu cầu cụ thể.

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 2. Vấn đề "deadlock" xảy ra khi:
A. Hai hoặc nhiều threads đang chờ đợi lẫn nhau để giải phóng tài nguyên
B. Một thread sử dụng quá nhiều CPU
C. Một thread bị chấm dứt bởi lỗi
D. Không đủ bộ nhớ để phân bổ cho thread mới

**Đáp án: A**

*Giải thích:* Deadlock xảy ra khi hai hoặc nhiều threads bị chặn vĩnh viễn, mỗi thread đang chờ đợi một tài nguyên đang được giữ bởi thread khác. Ví dụ: Thread A nắm giữ tài nguyên X và đang chờ tài nguyên Y, trong khi Thread B nắm giữ tài nguyên Y và đang chờ tài nguyên X.

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 3. Các biến trong Java được đánh dấu là "volatile" khi:
A. Chúng không thể thay đổi giá trị
B. Chúng được truy cập bởi nhiều threads và không lưu trữ trong bộ nhớ cache của thread
C. Chúng được lưu trữ trong bộ nhớ heap
D. Chúng chỉ có thể được truy cập bởi thread tạo ra nó

**Đáp án: B**

*Giải thích:* Từ khóa volatile trong Java đảm bảo rằng các thay đổi đối với biến được hiển thị ngay lập tức cho tất cả các threads. Khi một biến được đánh dấu là volatile, nó không được lưu trữ trong bộ nhớ cache của thread, và tất cả các thao tác đọc và ghi đều được thực hiện trực tiếp từ bộ nhớ chính.

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 4. Trong Python, cách nào sau đây được sử dụng để tạo thread:
A. Sử dụng module threading và tạo đối tượng Thread
B. Kế thừa từ lớp Process
C. Sử dụng hàm async
D. Tạo đối tượng từ lớp MultithreadedTask

**Đáp án: A**

*Giải thích:* Trong Python, thread được tạo bằng cách sử dụng module threading và tạo một đối tượng Thread. Có hai cách chính: truyền một hàm vào thread hoặc mở rộng lớp Thread và ghi đè phương thức run().

```python
import threading

def function():
    print("Thread running")

# Cách 1
thread = threading.Thread(target=function)
thread.start()

# Cách 2
class MyThread(threading.Thread):
    def run(self):
        print("Thread running")

thread = MyThread()
thread.start()
```

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 5. Khái niệm "Thread Pool" đề cập đến:
A. Một nhóm thread được tạo trước và sẵn sàng thực hiện các nhiệm vụ
B. Một thread đang ở trạng thái nghỉ
C. Một thread chạy với độ ưu tiên thấp
D. Một thread đang sử dụng quá nhiều tài nguyên

**Đáp án: A**

*Giải thích:* Thread Pool là một mẫu thiết kế trong đó một số lượng threads được tạo trước và được lưu trong một pool, sẵn sàng thực hiện các nhiệm vụ. Thay vì tạo thread mới cho mỗi nhiệm vụ, thread từ pool được sử dụng, thực hiện nhiệm vụ và sau đó quay trở lại pool. Điều này giảm chi phí tạo và hủy thread và cải thiện hiệu suất trong các ứng dụng cần xử lý nhiều nhiệm vụ ngắn.

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 6. Trong Java, lớp nào trong gói java.util.concurrent cung cấp hỗ trợ để tạo thread pool:
A. ThreadPool
B. ExecutorService
C. ThreadManager
D. ConcurrentPool

**Đáp án: B**

*Giải thích:* Trong Java, ExecutorService là một interface trong gói java.util.concurrent cung cấp các phương thức để quản lý việc chấm dứt và các phương thức có thể tạo Future để theo dõi tiến trình của một hoặc nhiều nhiệm vụ không đồng bộ. Executors là lớp tiện ích cung cấp các phương thức factory để tạo các loại ExecutorService khác nhau, bao gồm các thread pool.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

ExecutorService executor = Executors.newFixedThreadPool(5);
executor.submit(() -> {
    System.out.println("Task executing");
});
```

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 7. "Race condition" trong lập trình đa luồng là gì?
A. Khi hai hoặc nhiều threads cạnh tranh để hoàn thành nhiệm vụ của họ trước
B. Khi một thread chạy nhanh hơn các thread khác
C. Khi kết quả của một tính toán phụ thuộc vào thứ tự trong đó các threads được thực thi
D. Khi các threads chạy song song trên các CPU khác nhau

**Đáp án: C**

*Giải thích:* Race condition là một vấn đề trong lập trình đa luồng khi kết quả của một tính toán phụ thuộc vào thứ tự không thể dự đoán được trong đó các threads được thực thi. Nó xảy ra khi nhiều threads truy cập và sửa đổi dữ liệu được chia sẻ đồng thời, và kết quả cuối cùng phụ thuộc vào thời gian cụ thể của việc truy cập.

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 8. Trong Java, từ khóa "synchronized" được sử dụng để:
A. Tăng tốc độ thực thi của một phương thức
B. Ngăn nhiều threads thực thi một phương thức hoặc khối mã cùng một lúc
C. Cho phép một phương thức chạy trên nhiều CPU
D. Đảm bảo một phương thức luôn được thực thi bởi thread chính

**Đáp án: B**

*Giải thích:* Trong Java, từ khóa synchronized được sử dụng để tạo một critical section, ngăn nhiều threads thực thi một phương thức hoặc khối mã cùng một lúc. Khi một thread thực thi một phương thức synchronized, nó có được lock (khóa) của đối tượng và không thread nào khác có thể thực thi bất kỳ phương thức synchronized nào của đối tượng đó cho đến khi lock được giải phóng.

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 9. Trong Python, cơ chế nào sau đây không thể sử dụng để đồng bộ hóa các threads:
A. Lock
B. Semaphore
C. Event
D. GIL Bypasser

**Đáp án: D**

*Giải thích:* "GIL Bypasser" là phương án không tồn tại. Python cung cấp nhiều cơ chế đồng bộ hóa trong module threading, bao gồm Lock, RLock, Semaphore, BoundedSemaphore, Event và Condition. Global Interpreter Lock (GIL) là một cơ chế trong CPython ngăn nhiều threads thực thi mã Python đồng thời, nhưng không có cơ chế chính thức gọi là "GIL Bypasser" để bỏ qua GIL.

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 10. Trong lập trình đa luồng, "context switching" là:
A. Khi một thread đang chạy bị tạm dừng và thread khác được thực thi
B. Khi một thread thay đổi thuật toán của nó
C. Khi một thread thay đổi độ ưu tiên của nó
D. Khi một thread đang chạy đọc dữ liệu từ thread khác

**Đáp án: A**

*Giải thích:* Context switching là quá trình lưu trạng thái của một thread hoặc quá trình đang chạy để có thể tiếp tục thực thi nó sau, và sau đó nạp trạng thái của thread khác và thực thi nó. Đây là một phần quan trọng của multitasking và là một trong những chi phí liên quan đến việc chạy nhiều threads hoặc processes.

*Mảng kiến thức: Lập trình đa luồng (Multi-threaded Programming)*

### 11. Trong Java, đối tượng nào sau đây được sử dụng để đọc dữ liệu từ một tập tin văn bản:
A. FileWriter
B. BufferedReader
C. OutputStream
D. PrintWriter

**Đáp án: B**

*Giải thích:* Trong Java, BufferedReader là một lớp được sử dụng để đọc văn bản từ một stream đầu vào ký tự một cách hiệu quả. Nó đệm các ký tự để cung cấp hiệu suất đọc hiệu quả. BufferedReader có phương thức readLine() cho phép đọc một dòng văn bản tại một thời điểm.

```java
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 12. Trong Python, cách nào sau đây là cách đúng để mở một tập tin để ghi:
A. file = open("file.txt", "r")
B. file = open("file.txt", "w")
C. file = open("file.txt", "a")
D. file = open("file.txt", "x")

**Đáp án: B**

*Giải thích:* Trong Python, chế độ "w" được sử dụng để mở một tập tin để ghi. Nếu tập tin không tồn tại, nó sẽ được tạo. Nếu tập tin đã tồn tại, nội dung của nó sẽ bị ghi đè. Các chế độ khác là "r" (đọc), "a" (nối) và "x" (tạo mới, lỗi nếu tập tin đã tồn tại).

```python
with open("file.txt", "w") as file:
    file.write("Hello, World!")
```

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 13. Trong Java, đối tượng nào sau đây được sử dụng để đọc dữ liệu nhị phân từ một tập tin:
A. FileInputStream
B. BufferedReader
C. StringReader
D. CharArrayReader

**Đáp án: A**

*Giải thích:* Trong Java, FileInputStream được sử dụng để đọc dữ liệu nhị phân (binary) từ một tập tin. Nó là một lớp con của InputStream và có thể được sử dụng để đọc dữ liệu byte từ một tập tin.

```java
try (FileInputStream fis = new FileInputStream("file.bin")) {
    int data;
    while ((data = fis.read()) != -1) {
        // Xử lý dữ liệu
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 14. Trong Python, thư viện nào sau đây thường được sử dụng để làm việc với dữ liệu CSV:
A. numpy
B. pandas
C. matplotlib
D. scikit-learn

**Đáp án: B**

*Giải thích:* Thư viện pandas trong Python cung cấp các công cụ mạnh mẽ để làm việc với dữ liệu có cấu trúc, bao gồm dữ liệu CSV. Hàm read_csv() của pandas là một cách phổ biến để đọc dữ liệu CSV vào một DataFrame, cung cấp nhiều tùy chọn để xử lý các định dạng CSV khác nhau.

```python
import pandas as pd

# Đọc tập tin CSV
df = pd.read_csv("data.csv")

# Ghi DataFrame vào tập tin CSV
df.to_csv("output.csv", index=False)
```

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 15. Trong Java, lớp nào được sử dụng để làm việc với tập tin XML:
A. XMLReader
B. DocumentBuilder
C. SAXParser
D. Tất cả các phương án trên

**Đáp án: D**

*Giải thích:* Java cung cấp nhiều cách để làm việc với tập tin XML. DocumentBuilder (trong DOM API) được sử dụng để phân tích một tài liệu XML thành một cây DOM, SAXParser được sử dụng cho phân tích dựa trên sự kiện, và XMLReader là interface chính trong SAX API. Tất cả các lớp này đều có thể được sử dụng để làm việc với tập tin XML, tùy thuộc vào nhu cầu cụ thể.

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 16. RandomAccessFile trong Java được sử dụng để:
A. Tạo tập tin với nội dung ngẫu nhiên
B. Đọc và ghi tập tin theo kiểu tuần tự
C. Đọc và ghi tập tin tại vị trí bất kỳ
D. Mã hóa nội dung tập tin

**Đáp án: C**

*Giải thích:* RandomAccessFile trong Java cho phép đọc và ghi tại vị trí bất kỳ trong tập tin. Nó cung cấp phương thức seek() để di chuyển con trỏ tập tin đến vị trí cụ thể và các phương thức để đọc và ghi dữ liệu tại vị trí đó.

```java
try (RandomAccessFile raf = new RandomAccessFile("file.dat", "rw")) {
    // Di chuyển đến vị trí cụ thể
    raf.seek(100);
    // Ghi dữ liệu tại vị trí đó
    raf.writeInt(42);
    // Quay lại để đọc
    raf.seek(100);
    int value = raf.readInt();
} catch (IOException e) {
    e.printStackTrace();
}
```

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 17. Trong Python, phương thức nào sau đây được sử dụng để đọc toàn bộ nội dung của một tập tin vào một chuỗi:
A. readlines()
B. readline()
C. read()
D. readall()

**Đáp án: C**

*Giải thích:* Trong Python, phương thức read() được sử dụng để đọc toàn bộ nội dung của một tập tin vào một chuỗi. Phương thức readlines() đọc tất cả các dòng và trả về một danh sách các dòng, readline() đọc một dòng tại một thời điểm, và không có phương thức readall() trong đối tượng tập tin tiêu chuẩn của Python.

```python
with open("file.txt", "r") as file:
    content = file.read()  # Đọc toàn bộ tập tin
```

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 18. Trong Java, lớp nào sau đây được sử dụng để tuần tự hóa (serialize) đối tượng vào một tập tin:
A. FileOutputStream
B. ObjectOutputStream
C. DataOutputStream
D. PrintStream

**Đáp án: B**

*Giải thích:* Trong Java, ObjectOutputStream được sử dụng để tuần tự hóa các đối tượng vào một tập tin hoặc stream. Nó chuyển đổi các đối tượng Java thành chuỗi byte để có thể được lưu trữ hoặc truyền.

```java
try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("object.ser"))) {
    // Tuần tự hóa đối tượng
    oos.writeObject(myObject);
} catch (IOException e) {
    e.printStackTrace();
}
```

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 19. Trong Python, thư viện nào sau đây được sử dụng để làm việc với dữ liệu JSON:
A. jsonlib
B. simplejson
C. json
D. pyjson

**Đáp án: C**

*Giải thích:* Thư viện json là một phần của thư viện tiêu chuẩn Python và được sử dụng để mã hóa và giải mã dữ liệu JSON. Nó cung cấp các phương thức như json.dumps() để chuyển đổi đối tượng Python thành chuỗi JSON và json.loads() để phân tích chuỗi JSON thành đối tượng Python.

```python
import json

# Chuyển đổi từ Python sang JSON
data = {"name": "John", "age": 30}
json_str = json.dumps(data)

# Chuyển đổi từ JSON sang Python
parsed_data = json.loads(json_str)
```

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 20. Trong Java, lớp java.nio.file.Files cung cấp phương thức nào để đọc tất cả các dòng từ một tập tin:
A. readAllBytes()
B. readAllLines()
C. lines()
D. readLines()

**Đáp án: B**

*Giải thích:* Trong Java, phương thức readAllLines() của lớp java.nio.file.Files đọc tất cả các dòng từ một tập tin và trả về chúng dưới dạng một List<String>. Phương thức này đọc toàn bộ tập tin vào bộ nhớ, vì vậy không phù hợp cho các tập tin rất lớn.

```java
import java.nio.file.*;
import java.util.List;

try {
    List<String> lines = Files.readAllLines(Paths.get("file.txt"));
    for (String line : lines) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

*Mảng kiến thức: Làm việc với tập tin dữ liệu (Working with Data Files)*

### 21. Trong mô hình OSI, TCP hoạt động ở tầng nào:
A. Tầng Vật lý (Physical Layer)
B. Tầng Mạng (Network Layer)
C. Tầng Giao vận (Transport Layer)
D. Tầng Ứng dụng (Application Layer)

**Đáp án: C**

*Giải thích:* Trong mô hình OSI, TCP (Transmission Control Protocol) hoạt động ở tầng Giao vận (Transport Layer). Tầng này chịu trách nhiệm cho việc truyền dữ liệu đáng tin cậy giữa các điểm cuối. TCP cung cấp truyền dữ liệu theo trình tự, kiểm soát luồng, kiểm soát tắc nghẽn và phục hồi lỗi.

*Mảng kiến thức: Kỹ thuật làm việc trong môi trường mạng (Network Programming)*

### 22. Trong Java, lớp nào sau đây được sử dụng để thiết lập kết nối TCP với máy chủ:
A. ServerSocket
B. Socket
C. DatagramSocket
D. MulticastSocket

**Đáp án: B**

*Giải thích:* Trong Java, lớp Socket được sử dụng bởi client để thiết lập kết nối TCP với máy chủ. Nó tạo ra một socket và kết nối socket đó đến máy chủ được chỉ định bởi địa chỉ IP và cổng.

```java
try (Socket socket = new Socket("localhost", 8080)) {
    // Giao tiếp với máy chủ
    OutputStream output = socket.getOutputStream();
    // Gửi dữ liệu
} catch (IOException e) {
    e.printStackTrace();
}
```

*Mảng kiến thức: Kỹ thuật làm việc trong môi trường mạng (Network Programming)*

### 23. Trong Python, module nào sau đây được sử dụng để làm việc với socket:
A. network
B. socket
C. connection
D. net

**Đáp án: B**

*Giải thích:* Trong Python, module socket cung cấp truy cập vào giao diện socket Berkeley. Module này là một phần của thư viện tiêu chuẩn Python và cung cấp các công cụ để tạo các ứng dụng mạng cấp thấp.

```python
import socket

# Tạo socket TCP
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# Kết nối đến máy chủ
client_socket.connect(("localhost", 8080))
```

*Mảng kiến thức: Kỹ thuật làm việc trong môi trường mạng (Network Programming)*

### 24. Trong networking, "port" là:
A. Một loại thiết bị phần cứng
B. Một địa chỉ logic được sử dụng để xác định một ứng dụng cụ thể
C. Một giao thức truyền dữ liệu
D. Một loại tường lửa

**Đáp án: B**

*Giải thích:* Trong networking, port là một địa chỉ logic được sử dụng để xác định một ứng dụng hoặc quy trình cụ thể đang chạy trên một host trong mạng. Các port cho phép nhiều ứng dụng trên cùng một host giao tiếp thông qua mạng. Port được biểu diễn bằng một số 16-bit (0-65535).

*Mảng kiến thức: Kỹ thuật làm việc trong môi trường mạng (Network Programming)*

### 25. Trong Java, lớp nào sau đây được sử dụng để tạo máy chủ TCP:
A. Socket
B. ServerSocket
C. DatagramSocket
D. Protocol

**Đáp án: B**

*Giải thích:* Trong Java, lớp ServerSocket được sử dụng để tạo máy chủ TCP. Nó tạo ra một server socket, gắn nó với một cổng cụ thể, và sau đó lắng nghe các kết nối đến từ clients.

```java
try (ServerSocket serverSocket = new ServerSocket(8080)) {
    while (true) {
        Socket clientSocket = serverSocket.accept();
        // Xử lý kết nối từ client
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

*Mảng kiến thức: Kỹ thuật làm việc trong môi trường mạng (Network Programming)*

### 26. Trong Python, thư viện nào sau đây KHÔNG được sử dụng để làm việc với các yêu cầu HTTP:
A. requests
B. urllib
C. httplib2
D. socketserver

**Đáp án: D**

*Giải thích:* Trong Python, thư viện socketserver được sử dụng để tạo các máy chủ socket, không phải để làm việc trực tiếp với các yêu cầu HTTP. Thư viện requests, urllib, và httplib2 đều được sử dụng để làm việc với các yêu cầu HTTP.

```python
# Sử dụng thư viện requests
import requests
response = requests.get("https://www.example.com")

# Sử dụng thư viện urllib
import urllib.request
response = urllib.request.urlopen("https://www.example.com")
```

*Mảng kiến thức: Kỹ thuật làm việc trong môi trường mạng (Network Programming)*

### 27. Trong Java, lớp nào sau đây được sử dụng để thực hiện các yêu cầu HTTP:
A. HttpClient (từ java.net.http trong Java 11+)
B. HttpConnection
C. WebClient
D. NetworkClient

**Đáp án: A**

*Giải thích:* Trong Java 11 trở lên, lớp HttpClient từ gói java.net.http được sử dụng để thực hiện các yêu cầu HTTP. Nó cung cấp một API hiện đại và linh hoạt cho việc gửi các yêu cầu HTTP và xử lý các phản hồi.

```java
import java.net.http.*;
import java.net.URI;

HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create("https://www.example.com"))
        .build();
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
```

*Mảng kiến thức: Kỹ thuật làm việc trong môi trường mạng (Network Programming)*

### 28. Giao thức UDP khác với TCP ở chỗ:
A. UDP là hướng kết nối, TCP là không kết nối
B. UDP cung cấp truyền dữ liệu đáng tin cậy, TCP không
C. UDP là không kết nối và không đảm bảo việc phân phối gói tin, TCP là hướng kết nối và đáng tin cậy
D. UDP chỉ có thể truyền text, TCP có thể truyền nhiều loại dữ liệu

**Đáp án: C**