## ✅ **1. Khởi tạo dự án Spring Boot**
---
### 🔹 **1.1. Sử dụng Spring Initializr**
1. Truy cập **[Spring Initializr](https://start.spring.io/)**.
2. Chọn cấu hình:
   - **Project:** Maven
   - **Language:** Java
   - **Spring Boot Version:** 3.x.x (mới nhất)
   - **Dependencies:**
     - **Spring Web** (Xây dựng REST API, Controller)
     - **Thymeleaf** (Render HTML)
     - **Spring Boot DevTools** (Reload nhanh khi code)
     - **Lombok** (Giúp giảm code Java)
     - **Jackson Databind** (Để ánh xạ JSON)
     - **Spring Data JPA** (Chuẩn bị sẵn nếu cần sử dụng DB)
     - **H2 Database** (Nếu muốn dùng DB nội bộ thay JSON sau này)

3. Nhấn **Generate** và tải về.

---

## ✅ **2. Cấu trúc dự án chuẩn**
```
src/main/java/com/example/demo
    ├── controller/         # Controller xử lý request
    │   ├── UserController.java
    ├── model/              # Model dữ liệu
    │   ├── User.java
    ├── repository/         # Repository (nếu dùng JPA sau này)
    │   ├── UserRepository.java
    ├── service/            # Service để xử lý dữ liệu
    │   ├── UserService.java
    ├── storage/            # Lưu trữ dữ liệu JSON
    │   ├── data.json
    ├── DemoApplication.java  # File chạy ứng dụng
src/main/resources
    ├── static/css/         # File CSS
    │   ├── styles.css
    ├── templates/          # Giao diện Thymeleaf
    │   ├── index.html
```

---

## ✅ **3. Cấu hình `pom.xml`**
📌 **Thêm Gson và Jackson để xử lý JSON**
```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.14.1</version>
</dependency>
```
📌 **Thêm Spring Data JPA để có thể mở rộng sau này**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```
📌 **Thêm H2 Database nếu sau này muốn dùng database thay vì JSON**
```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

---

## ✅ **4. Tạo Model `User.java`**
📌 **Ánh xạ dữ liệu từ JSON + Chuẩn bị sẵn cho JPA**  

```java
package com.example.demo.model;

import com.fasterxml.jackson.annotation.JsonProperty;
import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Getter
@Setter
@NoArgsConstructor
@Entity // Nếu muốn dùng JPA sau này
@Table(name = "users") // Chỉ định tên bảng nếu sử dụng DB
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Tạo ID tự động nếu dùng DB
    private Long id;

    @JsonProperty("first_name")
    private String firstName;

    @JsonProperty("last_name")
    private String lastName;

    private String email;
    private String avatar;

    /**
     * Trả về họ và tên đầy đủ của User
     */
    public String getFullName() {
        return firstName + " " + lastName;
    }

    /**
     * Trả về email đã được ẩn đi
     */
    public String getMaskedEmail() {
        return email.replaceAll("(^.)(.*)(@.*$)", "$1***$3");
    }
}
```

---

## ✅ **5. Lưu trữ dữ liệu trong `data.json`**
📌 **Tạo file `src/main/java/com/example/demo/storage/data.json`**
```json
[
  {
    "first_name": "George",
    "last_name": "Bluth",
    "email": "george.bluth@reqres.in",
    "avatar": "george.jpg"
  },
  {
    "first_name": "Janet",
    "last_name": "Weaver",
    "email": "janet.weaver@reqres.in",
    "avatar": "janet.jpg"
  }
]
```

---

## ✅ **6. Tạo Service `UserService.java`**
📌 **Dùng `ObjectMapper` để đọc JSON, không dùng `@Autowired`, thay bằng Constructor Injection.**  

```java
package com.example.demo.service;

import com.example.demo.model.User;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.core.type.TypeReference;
import org.springframework.stereotype.Service;

import java.io.File;
import java.util.List;

@Service
public class UserService {
    
    private final ObjectMapper objectMapper;
    private static final String DATA_FILE = "src/main/java/com/example/demo/storage/data.json";

    public UserService(ObjectMapper objectMapper) {
        this.objectMapper = objectMapper;
    }

    public List<User> getUsers() {
        try {
            return objectMapper.readValue(new File(DATA_FILE), new TypeReference<List<User>>() {});
        } catch (Exception e) {
            e.printStackTrace();
            return List.of();
        }
    }
}
```

---

## ✅ **7. Tạo Controller `UserController.java`**
📌 **Sử dụng Constructor Injection, không dùng `@Autowired`**

```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.List;

@Controller
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/")
    public String getUsers(Model model) {
        List<User> users = userService.getUsers();
        model.addAttribute("users", users);
        return "index";
    }
}
```

---

## ✅ **8. Cập nhật giao diện Thymeleaf `index.html`**
📌 **Thêm `<meta name="viewport">` để hỗ trợ responsive**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>ReqRes Users</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" th:href="@{/css/styles.css}">
</head>
<body>
    <h1>Hello ReqRes users!</h1>
    <div class="container">
        <div class="user-card" th:each="user : ${users}">
            <div class="user-name" th:text="${user.fullName}"></div>
            <img th:src="@{/images/{avatar}(avatar=${user.avatar})}" alt="User Avatar">
            <div class="email" th:text="${user.maskedEmail}"></div>
        </div>
    </div>
</body>
</html>
```

---

## ✅ **9. Chạy ứng dụng**
📌 **Mở terminal và chạy:**
```sh
mvn spring-boot:run
```
📌 **Truy cập trình duyệt tại:**
```
http://localhost:8080/
```

---
