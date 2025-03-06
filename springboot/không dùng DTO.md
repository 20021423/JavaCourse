Dưới đây là một ví dụ “production‐ready” hoàn chỉnh cho dự án quản lý người dùng với các chức năng tìm kiếm, xoá, “tym”,… và tính năng giỏ hàng (giỏ hàng ở đây là danh sách những người dùng được thêm vào) – sử dụng Spring Boot, Spring Data JPA (với PostgreSQL), Lombok, Thymeleaf và tích hợp việc seed dữ liệu từ file JSON. Trong phiên bản này chúng ta không dùng DTO mà làm việc trực tiếp với entity.

Các điểm nổi bật trong ví dụ này:

1. **Entity User**  
   - Được thiết kế theo chuẩn JPA, với các trường như *id, email, firstName, lastName, avatar, username, password, role, likeCount*.
   - Sử dụng các annotation của Jackson (@JsonProperty) để map các trường từ file JSON (ví dụ: “first_name”, “last_name”) sang các trường trong Java (firstName, lastName).

2. **Repository UserRepository**  
   - Mở rộng từ JpaRepository, có thêm method tìm kiếm theo tên.

3. **Service UserService**  
   - Thực hiện các nghiệp vụ cơ bản (lấy danh sách, tìm kiếm, like, xoá…).
   - Trong business logic, ta làm việc trực tiếp với entity User.

4. **Seed dữ liệu ban đầu từ file JSON**  
   - Sử dụng CommandLineRunner để đọc file JSON (với @JsonProperty) và lưu vào PostgreSQL nếu DB chưa có dữ liệu.

5. **Giỏ hàng**  
   - Lớp Cart chứa danh sách User (entity) được thêm vào giỏ.
   - CartController quản lý thêm – xoá – xem giỏ hàng thông qua HttpSession.

6. **Thymeleaf Templates & CSS**  
   - Template “user/list.html” hiển thị danh sách người dùng (có nút “Add to Cart”).
   - Template “cart/view.html” hiển thị giỏ hàng.
   - File CSS được tách riêng trong thư mục static.

Dưới đây là code chi tiết từng file:

---

### 1. pom.xml

```xml
<!-- pom.xml -->
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>cart-demo</artifactId>
  <version>1.0.0</version>
  <name>Cart Demo</name>
  <packaging>jar</packaging>

  <properties>
    <java.version>17</java.version>
    <spring.boot.version>3.0.2</spring.boot.version>
  </properties>

  <dependencies>
    <!-- Spring Boot Starter Web (bao gồm cả Thymeleaf) -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Data JPA -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- PostgreSQL Driver -->
    <dependency>
      <groupId>org.postgresql</groupId>
      <artifactId>postgresql</artifactId>
      <scope>runtime</scope>
    </dependency>

    <!-- Thymeleaf -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <!-- Lombok -->
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <optional>true</optional>
    </dependency>

    <!-- Jackson Databind -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
    </dependency>
    
  </dependencies>

  <build>
    <plugins>
      <!-- Spring Boot Maven Plugin -->
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

---

### 2. application.properties

```properties
# File: src/main/resources/application.properties

# Server port
server.port=8080

# Tắt cache của Thymeleaf (cho mục đích phát triển)
spring.thymeleaf.cache=false

# Cấu hình datasource cho PostgreSQL
spring.datasource.url=jdbc:postgresql://localhost:5432/cart_demo_db
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# Chọn nguồn dữ liệu: "db" hoặc "json" (ở đây ta dùng "db" vì dữ liệu được lưu vào PostgreSQL)
app.datasource-type=db
```

---

### 3. CartDemoApplication.java  
(Chứa CommandLineRunner để seed dữ liệu từ file JSON nếu DB trống)

```java
// File: src/main/java/com/example/demo/CartDemoApplication.java
package com.example.demo;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import java.io.File;
import java.util.List;

@SpringBootApplication
public class CartDemoApplication {
  public static void main(String[] args) {
    SpringApplication.run(CartDemoApplication.class, args);
  }

  // Seed dữ liệu từ file JSON nếu DB trống
  @Bean
  public CommandLineRunner loadData(UserRepository userRepository) {
    return args -> {
      if (userRepository.count() == 0) {
        ObjectMapper mapper = new ObjectMapper();
        try {
          File file = new File("src/main/resources/data/users.json");
          List<User> users = mapper.readValue(file, new TypeReference<List<User>>() {});
          // Thiết lập các trường bổ sung cho việc đăng nhập, “tym”,…
          users.forEach(user -> {
            // Ví dụ: dùng email làm username, password mặc định "123", role mặc định "USER"
            user.setUsername(user.getEmail());
            user.setPassword("123");
            user.setRole("USER");
            user.setLikeCount(0);
          });
          userRepository.saveAll(users);
        } catch (Exception e) {
          e.printStackTrace();
        }
      }
    };
  }
}
```

---

### 4. Entity – User.java  
(Sử dụng JPA, kèm @JsonProperty để map các trường từ file JSON)

```java
// File: src/main/java/com/example/demo/model/User.java
package com.example.demo.model;

import com.fasterxml.jackson.annotation.JsonProperty;
import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "users")
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class User {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  private String email;

  // Sử dụng @JsonProperty để map "first_name" từ JSON sang firstName của Java
  @JsonProperty("first_name")
  @Column(name = "first_name")
  private String firstName;

  @JsonProperty("last_name")
  @Column(name = "last_name")
  private String lastName;

  // Map trực tiếp trường "avatar"
  @JsonProperty("avatar")
  private String avatar;

  // Các trường phục vụ đăng nhập và phân quyền
  private String username;
  private String password;
  private String role;

  // Số lượt “tym”
  private int likeCount;
}
```

---

### 5. Model – Cart.java  
(Lớp lưu giỏ hàng dưới dạng danh sách các User)

```java
// File: src/main/java/com/example/demo/model/Cart.java
package com.example.demo.model;

import lombok.Data;
import lombok.NoArgsConstructor;

import java.util.ArrayList;
import java.util.List;

@Data
@NoArgsConstructor
public class Cart {
  // Danh sách người dùng được thêm vào giỏ
  private List<User> items = new ArrayList<>();

  public void addItem(User user) {
    boolean exists = items.stream().anyMatch(u -> u.getId().equals(user.getId()));
    if (!exists) {
      items.add(user);
    }
  }

  public void removeItem(Long id) {
    items.removeIf(u -> u.getId().equals(id));
  }

  public int getTotalItems() {
    return items.size();
  }
}
```

---

### 6. Repository – UserRepository.java  
(Sử dụng Spring Data JPA cho PostgreSQL)

```java
// File: src/main/java/com/example/demo/repository/UserRepository.java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;
import java.util.Optional;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
  Optional<User> findByUsername(String username);
  
  // Tìm kiếm theo tên (ghép firstName và lastName) không phân biệt chữ hoa chữ thường
  List<User> findByFirstNameContainingIgnoreCaseOrLastNameContainingIgnoreCase(String firstName, String lastName);
}
```

---

### 7. Service – UserService.java và UserServiceImpl.java  
(Sử dụng entity User trực tiếp trong các thao tác nghiệp vụ)

```java
// File: src/main/java/com/example/demo/service/UserService.java
package com.example.demo.service;

import com.example.demo.model.User;

import java.util.List;

public interface UserService {
  List<User> getAllUsers();
  User getUserById(Long id);
  List<User> search(String keyword);
  User likeUser(Long id);
  void deleteUser(Long id);
  User findByUsername(String username);
}
```

```java
// File: src/main/java/com/example/demo/service/impl/UserServiceImpl.java
package com.example.demo.service.impl;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import com.example.demo.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
@RequiredArgsConstructor
public class UserServiceImpl implements UserService {

  private final UserRepository userRepository;

  @Override
  public List<User> getAllUsers() {
    return userRepository.findAll();
  }

  @Override
  public User getUserById(Long id) {
    return userRepository.findById(id).orElse(null);
  }

  @Override
  public List<User> search(String keyword) {
    if (keyword == null || keyword.isBlank()) {
      return getAllUsers();
    }
    // Tìm kiếm theo firstName hoặc lastName
    return userRepository.findByFirstNameContainingIgnoreCaseOrLastNameContainingIgnoreCase(keyword, keyword);
  }

  @Override
  public User likeUser(Long id) {
    User user = getUserById(id);
    if (user != null) {
      user.setLikeCount(user.getLikeCount() + 1);
      return userRepository.save(user);
    }
    return null;
  }

  @Override
  public void deleteUser(Long id) {
    userRepository.deleteById(id);
  }

  @Override
  public User findByUsername(String username) {
    return userRepository.findByUsername(username).orElse(null);
  }
}
```

---

### 8. Controller

#### 8.1 UserController.java  
(Hiển thị danh sách người dùng, tìm kiếm, xoá, “tym”)

```java
// File: src/main/java/com/example/demo/controller/UserController.java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@Controller
@RequestMapping("/users")
@RequiredArgsConstructor
public class UserController {

  private final UserService userService;

  // Hiển thị danh sách người dùng với khả năng tìm kiếm
  @GetMapping
  public String listUsers(@RequestParam(value = "search", required = false) String search, Model model) {
    List<User> users = (search == null || search.isBlank())
        ? userService.getAllUsers()
        : userService.search(search);
    model.addAttribute("users", users);
    model.addAttribute("search", search);
    return "user/list";
  }

  // "Tym" người dùng
  @GetMapping("/like/{id}")
  public String likeUser(@PathVariable("id") Long id) {
    userService.likeUser(id);
    return "redirect:/users";
  }

  // Xoá người dùng
  @GetMapping("/delete/{id}")
  public String deleteUser(@PathVariable("id") Long id) {
    userService.deleteUser(id);
    return "redirect:/users";
  }
}
```

#### 8.2 CartController.java  
(Quản lý giỏ hàng sử dụng HttpSession)

```java
// File: src/main/java/com/example/demo/controller/CartController.java
package com.example.demo.controller;

import com.example.demo.model.Cart;
import com.example.demo.model.User;
import com.example.demo.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import jakarta.servlet.http.HttpSession;

@Controller
@RequestMapping("/cart")
@RequiredArgsConstructor
public class CartController {

  private final UserService userService;

  // Xem giỏ hàng
  @GetMapping
  public String viewCart(HttpSession session, Model model) {
    Cart cart = (Cart) session.getAttribute("cart");
    if (cart == null) {
      cart = new Cart();
      session.setAttribute("cart", cart);
    }
    model.addAttribute("cart", cart);
    return "cart/view";
  }

  // Thêm người dùng vào giỏ hàng theo id
  @GetMapping("/add/{id}")
  public String addToCart(@PathVariable("id") Long id, HttpSession session) {
    User user = userService.getUserById(id);
    if (user != null) {
      Cart cart = (Cart) session.getAttribute("cart");
      if (cart == null) {
        cart = new Cart();
      }
      cart.addItem(user);
      session.setAttribute("cart", cart);
    }
    return "redirect:/cart";
  }

  // Xoá người dùng khỏi giỏ hàng
  @GetMapping("/remove/{id}")
  public String removeFromCart(@PathVariable("id") Long id, HttpSession session) {
    Cart cart = (Cart) session.getAttribute("cart");
    if (cart != null) {
      cart.removeItem(id);
      session.setAttribute("cart", cart);
    }
    return "redirect:/cart";
  }
}
```

---

### 9. Thymeleaf Templates

#### 9.1 Danh sách người dùng – list.html

```html
<!-- File: src/main/resources/templates/user/list.html -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <title>List of Users</title>
  <link rel="stylesheet" th:href="@{/css/style.css}" />
</head>
<body>
  <h1>List of Users</h1>
  <form th:action="@{/users}" method="get">
    <input type="text" name="search" th:value="${search}" placeholder="Search by name..." />
    <button type="submit">Search</button>
  </form>
  <div class="user-container">
    <div th:each="user : ${users}" class="user-card">
      <img th:src="${user.avatar}" alt="Avatar" />
      <h3 th:text="${user.firstName} + ' ' + ${user.lastName}">Name</h3>
      <p th:text="${user.email}">Email</p>
      <p>Tym: <span th:text="${user.likeCount}">0</span></p>
      <a th:href="@{/users/like/{id}(id=${user.id})}">Tym</a> |
      <a th:href="@{/users/delete/{id}(id=${user.id})}">Remove</a> |
      <!-- Nút thêm vào giỏ hàng -->
      <a th:href="@{/cart/add/{id}(id=${user.id})}">Add to Cart</a>
    </div>
  </div>
  <a href="/cart">View Cart</a>
</body>
</html>
```

#### 9.2 Giỏ hàng – view.html

```html
<!-- File: src/main/resources/templates/cart/view.html -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <title>Your Cart</title>
  <link rel="stylesheet" th:href="@{/css/style.css}" />
</head>
<body>
  <h1>Your Cart</h1>
  <div th:if="${#lists.isEmpty(cart.items)}">
    <p>Your cart is empty.</p>
  </div>
  <div th:if="${!#lists.isEmpty(cart.items)}">
    <ul>
      <li th:each="user : ${cart.items}">
        <img th:src="${user.avatar}" alt="Avatar" width="50" />
        <span th:text="${user.firstName} + ' ' + ${user.lastName}">Name</span> -
        <span th:text="${user.email}">Email</span>
        <a th:href="@{/cart/remove/{id}(id=${user.id})}">Remove</a>
      </li>
    </ul>
    <p>Total items: <span th:text="${#lists.size(cart.items)}">0</span></p>
  </div>
  <a href="/users">Back to User List</a>
</body>
</html>
```

---

### 10. File JSON  
(Đặt tại: src/main/resources/data/users.json; dùng định dạng ban đầu với “first_name”, “last_name” và “avatar”)

```json
[
  {
    "id": 1,
    "email": "george.bluth@reqres.in",
    "first_name": "George",
    "last_name": "Bluth",
    "avatar": "https://reqres.in/img/faces/1-image.jpg"
  },
  {
    "id": 2,
    "email": "janet.weaver@reqres.in",
    "first_name": "Janet",
    "last_name": "Weaver",
    "avatar": "https://reqres.in/img/faces/2-image.jpg"
  },
  {
    "id": 3,
    "email": "emma.wong@reqres.in",
    "first_name": "Emma",
    "last_name": "Wong",
    "avatar": "https://reqres.in/img/faces/3-image.jpg"
  },
  {
    "id": 4,
    "email": "eve.holt@reqres.in",
    "first_name": "Eve",
    "last_name": "Holt",
    "avatar": "https://reqres.in/img/faces/4-image.jpg"
  },
  {
    "id": 5,
    "email": "charles.morris@reqres.in",
    "first_name": "Charles",
    "last_name": "Morris",
    "avatar": "https://reqres.in/img/faces/5-image.jpg"
  },
  {
    "id": 6,
    "email": "tracey.ramos@reqres.in",
    "first_name": "Tracey",
    "last_name": "Ramos",
    "avatar": "https://reqres.in/img/faces/6-image.jpg"
  },
  {
    "id": 7,
    "email": "michael.lawson@reqres.in",
    "first_name": "Michael",
    "last_name": "Lawson",
    "avatar": "https://reqres.in/img/faces/7-image.jpg"
  },
  {
    "id": 8,
    "email": "lindsay.ferguson@reqres.in",
    "first_name": "Lindsay",
    "last_name": "Ferguson",
    "avatar": "https://reqres.in/img/faces/8-image.jpg"
  },
  {
    "id": 9,
    "email": "tobias.funke@reqres.in",
    "first_name": "Tobias",
    "last_name": "Funke",
    "avatar": "https://reqres.in/img/faces/9-image.jpg"
  },
  {
    "id": 10,
    "email": "byron.fields@reqres.in",
    "first_name": "Byron",
    "last_name": "Fields",
    "avatar": "https://reqres.in/img/faces/10-image.jpg"
  },
  {
    "id": 11,
    "email": "george.edwards@reqres.in",
    "first_name": "George",
    "last_name": "Edwards",
    "avatar": "https://reqres.in/img/faces/11-image.jpg"
  }
]
```

---

### 11. File CSS  
(Đặt tại: src/main/resources/static/css/style.css)

```css
/* File: src/main/resources/static/css/style.css */
body {
  font-family: Arial, sans-serif;
  margin: 20px;
}

h1 {
  text-align: center;
}

.user-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.user-card {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
  width: 200px;
}

.user-card img {
  width: 100px;
  height: 100px;
  object-fit: cover;
  border-radius: 50%;
}

ul {
  list-style: none;
  padding: 0;
}

ul li {
  margin: 10px 0;
}

a {
  text-decoration: none;
  color: blue;
}

a:hover {
  text-decoration: underline;
}
```

---

### Hướng dẫn chạy ứng dụng

1. **Build & Chạy ứng dụng:**  
   - Đảm bảo PostgreSQL đã chạy và database “cart_demo_db” được tạo.  
   - Trong terminal, tại thư mục dự án, chạy:
     ```bash
     mvn clean spring-boot:run
     ```
   - Hoặc chạy class **CartDemoApplication** từ IDE.

2. **Kiểm tra chức năng:**  
   - Truy cập [http://localhost:8080/users](http://localhost:8080/users) để xem danh sách người dùng (được lấy từ DB – dữ liệu được seed từ file JSON nếu DB trống).  
   - Sử dụng form tìm kiếm, bấm “Tym”, “Remove” và “Add to Cart”.  
   - Truy cập [http://localhost:8080/cart](http://localhost:8080/cart) để xem giỏ hàng (danh sách người dùng đã được thêm vào session) và xoá nếu cần.

---

Với phiên bản này:
- Chúng ta sử dụng JPA (với PostgreSQL) để lưu trữ dữ liệu người dùng.
- Entity User dùng @JsonProperty để map các trường từ file JSON.
- Tất cả các tầng (Service, Controller, View) làm việc trực tiếp với entity User (không dùng DTO).
- Tính năng giỏ hàng được triển khai qua HttpSession.

Hy vọng ví dụ này đáp ứng yêu cầu của bạn và giúp bạn hiểu được cách kết hợp JPA, seed dữ liệu từ JSON có sử dụng @JsonProperty, và sử dụng Session cho giỏ hàng trong một dự án Spring Boot. Nếu có thắc mắc hay cần bổ sung, bạn cứ trao đổi thêm nhé!