Dưới đây là code hoàn chỉnh **UML Sequence Diagram** mô tả quy trình **end-to-end** của một ứng dụng web cơ bản sử dụng Spring Boot, Spring Data JPA, Spring Security, Hibernate, PostgreSQL, MVC Pattern, Thymeleaf và Layered Architecture, với chức năng login, tạo JWT hoặc Session, truy vấn dữ liệu từ database và hiển thị lên View sử dụng Thymeleaf.

### 🎯 Quy trình cụ thể được minh họa gồm:

- Người dùng đăng nhập
- Xác thực thông tin và cấp JWT Token hoặc Session
- Truy cập dữ liệu bảo mật bằng JWT hoặc Session
- Trả kết quả hiển thị trên View bằng Thymeleaf

Dưới đây là toàn bộ mã PlantUML:

```mermaid
sequenceDiagram
    actor Client
    participant Browser
    participant Controller
    participant SecurityService
    participant UserService
    participant UserRepository
    participant DB
    participant ThymeleafView

    Note right of Client: User initiates login
    Client ->> +Controller: POST /login (username, password)
    Controller ->> +SecurityService: Authenticate user

    SecurityService ->> +UserRepository: Find user by username
    UserRepository ->> DB: SELECT * FROM users WHERE username=?
    DB -->> UserRepository: Return User Data
    UserRepository -->> -SecurityService: User Entity

    SecurityService ->> SecurityService: Verify password

    alt Invalid username/password
        SecurityService -->> Controller: Authentication Failed
        Controller -->> Client: 401 Unauthorized
    else Successful authentication
        SecurityService -->> -Controller: Authentication Success

        Controller ->> SecurityService: Generate JWT/Session
        SecurityService -->> Controller: Return JWT/Session ID
        Controller -->> Client: 200 OK (JWT/Session Cookie)
    end

    Note right of Client: User accesses protected resource
    Client ->> +Controller: GET /protected/resource (JWT/Session Cookie)
    Controller ->> +SecurityService: Validate JWT/Session

    alt Invalid or expired token
        SecurityService -->> Controller: Authentication Failed
        Controller -->> Client: 401 Unauthorized
    else Token is valid
        SecurityService -->> Controller: Authentication Success
        Controller ->> +UserService: Fetch protected data
        UserService ->> +UserRepository: Query database
        UserRepository ->> DB: SELECT * FROM protected_table
        DB -->> UserRepository: Return data
        UserRepository -->> UserService: Return entity list
        UserService -->> Controller: Return DTO
        Controller -->> ThymeleafView: Render HTML
        ThymeleafView -->> Client: Display HTML Page
    end


```

---

### Giải thích các bước chi tiết trong Sequence Diagram:

### ✅ **Bước 1: Đăng nhập**
- Người dùng gửi thông tin đăng nhập (`username, password`) lên API.
- Controller tiếp nhận và gọi Security Service để xác thực.

### Step-by-step code tương ứng (Spring Controller):
```java
@PostMapping("/login")
public ResponseEntity<?> authenticate(@RequestBody LoginDto dto) {
    Authentication authentication = authenticationManager.authenticate(
        new UsernamePasswordAuthenticationToken(dto.getUsername(), dto.getPassword())
    );
    SecurityContextHolder.getContext().setAuthentication(authentication);
    String jwtToken = jwtTokenProvider.generateToken(authentication);
    return ResponseEntity.ok().header(HttpHeaders.SET_COOKIE, jwtCookie).body(jwtToken);
}
```

---

### Bước xác thực (Security Service):
- Tìm kiếm người dùng từ repository.
- So khớp mật khẩu với password encoder.

### Step-by-step code tương ứng (Security Service):
```java
public Authentication authenticate(String username, String password) {
    UserDetails userDetails = userDetailsService.loadUserByUsername(username);
    if (!passwordEncoder.matches(password, userDetails.getPassword())) {
        throw new BadCredentialsException("Sai mật khẩu");
    }
    return new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
}
```

---

### Truy cập tài nguyên bảo mật:
- Client gửi yêu cầu GET kèm JWT token hoặc session cookie.
- Spring Security kiểm tra token/session hợp lệ.
- Controller gọi service và repository để lấy dữ liệu từ PostgreSQL.
- Dữ liệu trả về controller được render ra view bằng Thymeleaf.

### Step-by-step code tương ứng (protected controller):
```java
@GetMapping("/dashboard")
public String dashboard(Model model, Authentication authentication) {
    List<Data> dataList = protectedService.getData(authentication.getName());
    model.addAttribute("data", data);
    return "dashboard"; // Thymeleaf
}
```

### Thymeleaf template tương ứng (dashboard.html):
```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
  <h1>Protected Dashboard</h1>
  <ul>
    <li th:each="item : ${items}" th:text="${item.name}">Item</li>
  </ul>
</body>
</html>
```

---

### Tổng kết Pipeline và các bước được thể hiện trong Sequence Diagram trên:

1. **Client gửi Request** đến Server.
2. **Controller**: Tiếp nhận request và gọi các service tương ứng.
3. **Security Layer**: Xác thực & tạo JWT Token hoặc Session.
4. **Service Layer**: Xử lý nghiệp vụ.
5. **Repository Layer**: Truy vấn CSDL sử dụng Hibernate + JPA.
6. **Database (PostgreSQL)**: Xử lý lưu trữ và trả dữ liệu.
7. Kết quả trả về thông qua các layer (Repository → Service → Controller).
8. Controller gửi dữ liệu lên View (Thymeleaf).
9. Client nhận kết quả và hiển thị giao diện người dùng.

---

Bây giờ bạn có thể sử dụng mã UML này để trực quan hóa dễ dàng quy trình phát triển ứng dụng web, rất hữu ích trong việc học tập, phát triển ứng dụng và trình bày dự án.