Dưới đây là một ví dụ “code chuẩn” từ đầu đến cuối cho dự án quản lý người dùng có các chức năng tìm kiếm, xoá, “tym” kèm tính năng giỏ hàng (là danh sách những người dùng được thêm vào) – sử dụng Spring Boot, Spring Data JPA (PostgreSQL), Lombok, Spring Security (với JWT cơ bản), Thymeleaf và áp dụng một số nguyên tắc OOP, SOLID cũng như Factory Method Pattern để lựa chọn nguồn dữ liệu (DB hoặc JSON).  
Trong phiên bản này, chúng ta **không sử dụng DTO** mà làm việc trực tiếp với entity User. Đồng thời, để mapping file JSON (với các key “first_name”, “last_name”, “avatar”) với các thuộc tính của entity, ta sử dụng annotation **@JsonProperty**.

Các file dưới đây được trình bày chi tiết:

---

## 1. Cấu trúc dự án

```
cart-demo
 └─ src
     ├─ main
     │   ├─ java
     │   │   └─ com/example/demo
     │   │       ├─ CartDemoApplication.java
     │   │       ├─ config
     │   │       │   └─ SecurityConfig.java
     │   │       ├─ controller
     │   │       │   ├─ AuthController.java
     │   │       │   ├─ UserController.java
     │   │       │   └─ CartController.java         // Controller giỏ hàng
     │   │       ├─ datasource
     │   │       │   ├─ UserDataSource.java
     │   │       │   ├─ JsonUserDataSource.java
     │   │       │   ├─ DbUserDataSource.java
     │   │       │   └─ UserDataSourceFactory.java
     │   │       ├─ model
     │   │       │   ├─ User.java
     │   │       │   └─ Cart.java                    // Model giỏ hàng
     │   │       ├─ repository
     │   │       │   └─ UserRepository.java
     │   │       ├─ security
     │   │       │   ├─ JwtUtils.java
     │   │       │   └─ JwtAuthenticationFilter.java
     │   │       └─ service
     │   │           ├─ UserService.java
     │   │           └─ impl
     │   │               └─ UserServiceImpl.java
     │   └─ resources
     │       ├─ application.properties
     │       ├─ data
     │       │   └─ users.json
     │       ├─ static
     │       │   └─ css
     │       │       └─ style.css
     │       └─ templates
     │           ├─ user
     │           │   ├─ list.html
     │           │   └─ create.html
     │           └─ cart
     │               └─ view.html
     └─ pom.xml
```

---

## 2. File pom.xml

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

    <!-- Spring Security -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <!-- JWT (jjwt) -->
    <dependency>
      <groupId>io.jsonwebtoken</groupId>
      <artifactId>jjwt-api</artifactId>
      <version>0.11.5</version>
    </dependency>
    <dependency>
      <groupId>io.jsonwebtoken</groupId>
      <artifactId>jjwt-impl</artifactId>
      <version>0.11.5</version>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>io.jsonwebtoken</groupId>
      <artifactId>jjwt-jackson</artifactId>
      <version>0.11.5</version>
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

    <!-- Jackson Databind để xử lý JSON -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
    </dependency>
    
    <!-- Validation (nếu cần) -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
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

## 3. application.properties

```properties
# File: src/main/resources/application.properties

# Chọn nguồn dữ liệu: "db" hoặc "json"
app.datasource-type=db

# Cấu hình PostgreSQL
spring.datasource.url=jdbc:postgresql://localhost:5432/demo_db
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# Thymeleaf
spring.thymeleaf.cache=false

# Cấu hình JWT
app.jwt.secret=MY_SUPER_SECRET_KEY
app.jwt.expiration=3600000

server.port=8080
```

---

## 4. CartDemoApplication.java  
(Với CommandLineRunner để seed dữ liệu từ file JSON nếu DB trống)

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
          // Thiết lập các trường bổ sung
          users.forEach(user -> {
            // Ghép first_name và last_name thành name (bạn có thể điều chỉnh logic tùy ý)
            user.setName(user.getFirstName() + " " + user.getLastName());
            // Dùng email làm username, password mặc định "123", role mặc định "USER"
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

## 5. Model

### 5.1 User.java  
(Sử dụng JPA, Lombok và @JsonProperty để map file JSON)

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

  // Trường dùng để hiển thị tên đầy đủ (sẽ được gán từ firstName + lastName khi seed dữ liệu)
  private String name;

  private String email;

  // Sử dụng @JsonProperty để map "first_name" trong JSON sang firstName của Java
  @JsonProperty("first_name")
  @Column(name = "first_name")
  private String firstName;

  // Map "last_name" trong JSON
  @JsonProperty("last_name")
  @Column(name = "last_name")
  private String lastName;

  // Map "avatar" trong JSON sang avatarUrl
  @JsonProperty("avatar")
  @Column(name = "avatar_url")
  private String avatarUrl;

  // Các trường phục vụ đăng nhập
  private String username;
  private String password;
  private String role;

  // Số lượt "tym"
  private int likeCount;
}
```

### 5.2 Cart.java  
(Lưu giỏ hàng dưới dạng danh sách các User – chúng ta sử dụng entity trực tiếp)

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

## 6. Repository

### UserRepository.java  
(Sử dụng Spring Data JPA)

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
  
  // Tìm kiếm theo tên (không phân biệt chữ hoa chữ thường)
  List<User> findByNameContainingIgnoreCase(String name);
}
```

---

## 7. Datasource & Factory Method Pattern

### 7.1 UserDataSource.java  
(Định nghĩa các phương thức cần thiết)

```java
// File: src/main/java/com/example/demo/datasource/UserDataSource.java
package com.example.demo.datasource;

import com.example.demo.model.User;
import java.util.List;
import java.util.Optional;

public interface UserDataSource {
  List<User> getAllUsers();
  List<User> searchUsers(String keyword);
  User saveUser(User user);
  Optional<User> findById(Long id);
  void deleteById(Long id);
  Optional<User> findByUsername(String username);
}
```

### 7.2 JsonUserDataSource.java  
(Đọc dữ liệu từ file JSON)

```java
// File: src/main/java/com/example/demo/datasource/JsonUserDataSource.java
package com.example.demo.datasource;

import com.example.demo.model.User;
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.extern.slf4j.Slf4j;

import java.io.File;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

@Slf4j
public class JsonUserDataSource implements UserDataSource {

  private final List<User> inMemoryUsers = new ArrayList<>();

  public JsonUserDataSource() {
    loadUsersFromJson();
  }

  private void loadUsersFromJson() {
    try {
      File jsonFile = new File("src/main/resources/data/users.json");
      if (jsonFile.exists()) {
        ObjectMapper mapper = new ObjectMapper();
        List<User> users = mapper.readValue(jsonFile, new TypeReference<List<User>>() {});
        // Ở đây không cần mapping phức tạp vì entity đã có @JsonProperty để map dữ liệu
        inMemoryUsers.addAll(users);
      }
    } catch (Exception e) {
      log.error("Error loading JSON: ", e);
    }
  }

  @Override
  public List<User> getAllUsers() {
    return inMemoryUsers;
  }

  @Override
  public List<User> searchUsers(String keyword) {
    if (keyword == null || keyword.isBlank()) return getAllUsers();
    return inMemoryUsers.stream()
        .filter(u -> u.getName() != null && u.getName().toLowerCase().contains(keyword.toLowerCase()))
        .toList();
  }

  @Override
  public User saveUser(User user) {
    if (user.getId() == null) {
      user.setId((long) (inMemoryUsers.size() + 1));
      inMemoryUsers.add(user);
    } else {
      for (int i = 0; i < inMemoryUsers.size(); i++) {
        if (inMemoryUsers.get(i).getId().equals(user.getId())) {
          inMemoryUsers.set(i, user);
          break;
        }
      }
    }
    return user;
  }

  @Override
  public Optional<User> findById(Long id) {
    return inMemoryUsers.stream().filter(u -> u.getId().equals(id)).findFirst();
  }

  @Override
  public void deleteById(Long id) {
    inMemoryUsers.removeIf(u -> u.getId().equals(id));
  }

  @Override
  public Optional<User> findByUsername(String username) {
    return inMemoryUsers.stream().filter(u -> username.equals(u.getUsername())).findFirst();
  }
}
```

### 7.3 DbUserDataSource.java  
(Truy xuất dữ liệu qua JPA)

```java
// File: src/main/java/com/example/demo/datasource/DbUserDataSource.java
package com.example.demo.datasource;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import lombok.RequiredArgsConstructor;

import java.util.List;
import java.util.Optional;

@RequiredArgsConstructor
public class DbUserDataSource implements UserDataSource {

  private final UserRepository userRepository;

  @Override
  public List<User> getAllUsers() {
    return userRepository.findAll();
  }

  @Override
  public List<User> searchUsers(String keyword) {
    return userRepository.findByNameContainingIgnoreCase(keyword);
  }

  @Override
  public User saveUser(User user) {
    return userRepository.save(user);
  }

  @Override
  public Optional<User> findById(Long id) {
    return userRepository.findById(id);
  }

  @Override
  public void deleteById(Long id) {
    userRepository.deleteById(id);
  }

  @Override
  public Optional<User> findByUsername(String username) {
    return userRepository.findByUsername(username);
  }
}
```

### 7.4 UserDataSourceFactory.java  
(Chọn nguồn dữ liệu dựa trên cấu hình)

```java
// File: src/main/java/com/example/demo/datasource/UserDataSourceFactory.java
package com.example.demo.datasource;

import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class UserDataSourceFactory {

  @Value("${app.datasource-type}")
  private String dataSourceType;

  private final UserRepository userRepository;

  public UserDataSourceFactory(UserRepository userRepository) {
    this.userRepository = userRepository;
  }

  @Bean
  public UserDataSource userDataSource() {
    if ("db".equalsIgnoreCase(dataSourceType)) {
      return new DbUserDataSource(userRepository);
    } else {
      return new JsonUserDataSource();
    }
  }
}
```

---

## 8. Service Layer

### 8.1 UserService.java  
(Sử dụng entity User trực tiếp)

```java
// File: src/main/java/com/example/demo/service/UserService.java
package com.example.demo.service;

import com.example.demo.model.User;
import java.util.List;

public interface UserService {
  User register(User user);
  User findById(Long id);
  List<User> getAllUsers();
  List<User> search(String keyword);
  User likeUser(Long id);
  void deleteUser(Long id);
  User findByUsername(String username);
}
```

### 8.2 UserServiceImpl.java

```java
// File: src/main/java/com/example/demo/service/impl/UserServiceImpl.java
package com.example.demo.service.impl;

import com.example.demo.datasource.UserDataSource;
import com.example.demo.model.User;
import com.example.demo.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
@RequiredArgsConstructor
public class UserServiceImpl implements UserService {

  private final UserDataSource userDataSource;

  @Override
  public User register(User user) {
    if (user.getRole() == null) {
      user.setRole("USER");
    }
    if (user.getLikeCount() == 0) {
      user.setLikeCount(0);
    }
    return userDataSource.saveUser(user);
  }

  @Override
  public User findById(Long id) {
    return userDataSource.findById(id).orElse(null);
  }

  @Override
  public List<User> getAllUsers() {
    return userDataSource.getAllUsers();
  }

  @Override
  public List<User> search(String keyword) {
    return userDataSource.searchUsers(keyword);
  }

  @Override
  public User likeUser(Long id) {
    User user = findById(id);
    if (user != null) {
      user.setLikeCount(user.getLikeCount() + 1);
      return userDataSource.saveUser(user);
    }
    return null;
  }

  @Override
  public void deleteUser(Long id) {
    userDataSource.deleteById(id);
  }

  @Override
  public User findByUsername(String username) {
    return userDataSource.findByUsername(username).orElse(null);
  }
}
```

---

## 9. Security

### 9.1 JwtUtils.java

```java
// File: src/main/java/com/example/demo/security/JwtUtils.java
package com.example.demo.security;

import io.jsonwebtoken.*;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class JwtUtils {

  @Value("${app.jwt.secret}")
  private String jwtSecret;

  @Value("${app.jwt.expiration}")
  private long jwtExpirationMs;

  public String generateToken(String username) {
    return Jwts.builder()
        .setSubject(username)
        .setIssuedAt(new Date())
        .setExpiration(new Date(System.currentTimeMillis() + jwtExpirationMs))
        .signWith(SignatureAlgorithm.HS256, jwtSecret)
        .compact();
  }

  public String getUsernameFromToken(String token) {
    return Jwts.parser()
        .setSigningKey(jwtSecret)
        .parseClaimsJws(token)
        .getBody()
        .getSubject();
  }

  public boolean validateToken(String token) {
    try {
      Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(token);
      return true;
    } catch (JwtException ex) {
      // Log lỗi nếu cần
    }
    return false;
  }
}
```

### 9.2 JwtAuthenticationFilter.java

```java
// File: src/main/java/com/example/demo/security/JwtAuthenticationFilter.java
package com.example.demo.security;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpHeaders;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.filter.OncePerRequestFilter;

import java.io.IOException;
import java.util.Collections;

@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

  @Autowired
  private JwtUtils jwtUtils;

  @Autowired
  private UserService userService;

  @Override
  protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
      throws ServletException, IOException {
    try {
      String jwt = parseJwt(request);
      if (jwt != null && jwtUtils.validateToken(jwt)) {
        String username = jwtUtils.getUsernameFromToken(jwt);
        User user = userService.findByUsername(username);
        if (user != null) {
          UsernamePasswordAuthenticationToken authToken =
              new UsernamePasswordAuthenticationToken(user, null, Collections.emptyList());
          authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
          SecurityContextHolder.getContext().setAuthentication(authToken);
        }
      }
    } catch (Exception e) {
      // Log lỗi nếu cần
    }
    filterChain.doFilter(request, response);
  }

  private String parseJwt(HttpServletRequest request) {
    String headerAuth = request.getHeader(HttpHeaders.AUTHORIZATION);
    if (StringUtils.hasText(headerAuth) && headerAuth.startsWith("Bearer ")) {
      return headerAuth.substring(7);
    }
    return null;
  }
}
```

### 9.3 SecurityConfig.java

```java
// File: src/main/java/com/example/demo/config/SecurityConfig.java
package com.example.demo.config;

import com.example.demo.security.JwtAuthenticationFilter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
public class SecurityConfig {

  private final JwtAuthenticationFilter jwtAuthenticationFilter;

  public SecurityConfig(JwtAuthenticationFilter jwtAuthenticationFilter) {
    this.jwtAuthenticationFilter = jwtAuthenticationFilter;
  }

  @Bean
  public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
        .csrf(csrf -> csrf.disable())
        .authorizeHttpRequests(auth -> {
          // Cho phép truy cập công khai vào các đường dẫn sau:
          auth.requestMatchers("/", "/auth/**", "/css/**").permitAll();
          auth.anyRequest().authenticated();
        })
        .formLogin(Customizer.withDefaults());

    http.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);
    return http.build();
  }

  @Bean
  public AuthenticationManager authenticationManager(
      AuthenticationConfiguration config) throws Exception {
    return config.getAuthenticationManager();
  }
}
```

---

## 10. Controller

### 10.1 AuthController.java  
(Đăng ký và đăng nhập – sử dụng entity User trực tiếp)

```java
// File: src/main/java/com/example/demo/controller/AuthController.java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.security.JwtUtils;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.*;
import org.springframework.security.core.Authentication;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/auth")
public class AuthController {

  @Autowired
  private AuthenticationManager authenticationManager;

  @Autowired
  private JwtUtils jwtUtils;

  @Autowired
  private UserService userService;

  // Đăng ký người dùng
  @PostMapping("/register")
  public ResponseEntity<?> register(@RequestBody User user) {
    // Ở đây nên thêm validate và mã hóa password trong production
    User registered = userService.register(user);
    return ResponseEntity.ok(registered);
  }

  // Đăng nhập – trả về JWT
  @PostMapping("/login")
  public ResponseEntity<?> login(@RequestBody User loginRequest) {
    try {
      Authentication authentication = authenticationManager.authenticate(
          new UsernamePasswordAuthenticationToken(loginRequest.getUsername(), loginRequest.getPassword())
      );
      String jwt = jwtUtils.generateToken(loginRequest.getUsername());
      return ResponseEntity.ok(jwt);
    } catch (BadCredentialsException e) {
      return ResponseEntity.status(401).body("Invalid credentials");
    }
  }
}
```

### 10.2 UserController.java  
(Quản lý người dùng: hiển thị danh sách, tìm kiếm, “tym”, xoá – làm việc trực tiếp với entity User)

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

  // Hiển thị danh sách người dùng (có tìm kiếm)
  @GetMapping
  public String listUsers(@RequestParam(value = "search", required = false) String search, Model model) {
    List<User> users = (search == null || search.isBlank())
        ? userService.getAllUsers()
        : userService.search(search);
    model.addAttribute("users", users);
    model.addAttribute("search", search);
    return "user/list";
  }

  // Hiển thị form tạo người dùng
  @GetMapping("/create")
  public String showCreateForm(Model model) {
    model.addAttribute("user", new User());
    return "user/create";
  }

  // Xử lý tạo người dùng
  @PostMapping("/create")
  public String createUser(@ModelAttribute("user") User user) {
    userService.register(user);
    return "redirect:/users";
  }

  // Xoá người dùng
  @GetMapping("/delete/{id}")
  public String deleteUser(@PathVariable Long id) {
    userService.deleteUser(id);
    return "redirect:/users";
  }

  // “Tym” người dùng
  @GetMapping("/like/{id}")
  public String likeUser(@PathVariable Long id) {
    userService.likeUser(id);
    return "redirect:/users";
  }
}
```

### 10.3 CartController.java  
(Quản lý giỏ hàng – thêm/xoá người dùng vào giỏ, lưu vào HttpSession)

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
@RequiredArgsConstructor
@RequestMapping("/cart")
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

  // Thêm người dùng vào giỏ hàng (dựa theo id)
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

## 11. Thymeleaf Templates

### 11.1 User List – list.html

```html
<!-- File: src/main/resources/templates/user/list.html -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <title>Danh sách người dùng</title>
  <link rel="stylesheet" th:href="@{/css/style.css}" />
</head>
<body>
  <h1>Danh sách người dùng</h1>
  <form th:action="@{/users}" method="get">
    <input type="text" name="search" th:value="${search}" placeholder="Search by name..." />
    <button type="submit">Search</button>
  </form>
  <div class="user-container">
    <div th:each="user : ${users}" class="user-card">
      <img th:src="${user.avatarUrl}" alt="Avatar" />
      <h3 th:text="${user.name}">Name</h3>
      <p th:text="${user.email}">Email</p>
      <p>Tym: <span th:text="${user.likeCount}">0</span></p>
      <a th:href="@{/users/like/{id}(id=${user.id})}">Tym</a> |
      <a th:href="@{/users/delete/{id}(id=${user.id})}">Remove</a> |
      <!-- Nút thêm vào giỏ hàng -->
      <a th:href="@{/cart/add/{id}(id=${user.id})}">Add to Cart</a>
    </div>
  </div>
  <a th:href="@{/users/create}">Create new user</a> |
  <a th:href="@{/cart}">View Cart</a>
</body>
</html>
```

### 11.2 User Create – create.html

```html
<!-- File: src/main/resources/templates/user/create.html -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <title>Create User</title>
  <link rel="stylesheet" th:href="@{/css/style.css}" />
</head>
<body>
  <h1>Create New User</h1>
  <form th:action="@{/users/create}" method="post">
    <label>Name:</label>
    <input type="text" name="name" th:value="${user.name}" /><br/>
    
    <label>Username:</label>
    <input type="text" name="username" th:value="${user.username}" /><br/>
    
    <label>Password:</label>
    <input type="password" name="password" th:value="${user.password}" /><br/>
    
    <label>Email:</label>
    <input type="email" name="email" th:value="${user.email}" /><br/>
    
    <label>Avatar URL:</label>
    <input type="text" name="avatarUrl" th:value="${user.avatarUrl}" /><br/>
    
    <button type="submit">Save</button>
  </form>
</body>
</html>
```

### 11.3 Cart View – view.html

```html
<!-- File: src/main/resources/templates/cart/view.html -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <title>Giỏ hàng của bạn</title>
  <link rel="stylesheet" th:href="@{/css/style.css}" />
</head>
<body>
  <h1>Giỏ hàng của bạn</h1>
  <div th:if="${#lists.isEmpty(cart.items)}">
    <p>Giỏ hàng trống.</p>
  </div>
  <div th:if="${!#lists.isEmpty(cart.items)}">
    <ul>
      <li th:each="user : ${cart.items}">
        <img th:src="${user.avatarUrl}" alt="Avatar" width="50" />
        <span th:text="${user.name}">User Name</span> -
        <span th:text="${user.email}">Email</span>
        <a th:href="@{/cart/remove/{id}(id=${user.id})}">Remove</a>
      </li>
    </ul>
    <p>Tổng số người trong giỏ hàng: <span th:text="${cart.totalItems}">0</span></p>
  </div>
  <a href="/users">Quay lại danh sách người dùng</a>
</body>
</html>
```

---

## 12. File JSON  
(Đặt tại: src/main/resources/data/users.json; sử dụng định dạng ban đầu với "first_name", "last_name", "avatar")

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

## 13. File CSS  
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
  margin-top: 20px;
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

a {
  text-decoration: none;
  color: blue;
}

a:hover {
  text-decoration: underline;
}

ul {
  list-style: none;
  padding: 0;
}

ul li {
  margin: 10px 0;
}
```

---

## 14. Hướng dẫn chạy ứng dụng

1. **Build & Chạy:**
   - Đảm bảo PostgreSQL đang chạy và database “demo_db” đã được tạo.
   - Từ thư mục dự án, chạy lệnh:
     ```bash
     mvn clean spring-boot:run
     ```
   - Hoặc chạy class **CartDemoApplication** từ IDE.

2. **Kiểm tra các chức năng:**
   - Truy cập [http://localhost:8080/users](http://localhost:8080/users) để xem danh sách người dùng (dữ liệu sẽ được seed từ file JSON nếu DB trống).
   - Sử dụng form tìm kiếm, bấm “Tym”, “Remove” và “Add to Cart” cho từng người dùng.
   - Truy cập [http://localhost:8080/cart](http://localhost:8080/cart) để xem giỏ hàng (danh sách người dùng đã được thêm vào session) và xoá nếu cần.
   - Các API đăng ký và đăng nhập có thể được truy cập qua các endpoint `/auth/register` và `/auth/login` (sử dụng JSON body khi test qua Postman).

---

## 15. Kết luận

Phiên bản này giữ nguyên các thành phần khác (Repository, Service, Controller, Security, Thymeleaf) và sử dụng entity User trực tiếp (không dùng DTO).  
Các điểm chính:
- **Entity User** sử dụng JPA, Lombok và @JsonProperty để mapping dữ liệu JSON.
- **Seed dữ liệu** từ file JSON vào PostgreSQL nếu DB trống (sử dụng CommandLineRunner).
- **UserRepository** cung cấp hàm tìm kiếm theo tên.
- **UserService** và **UserController** làm việc trực tiếp với entity User.
- **Giỏ hàng** được quản lý qua HttpSession với model Cart chứa danh sách User.
- **Security** được tích hợp với JWT qua các lớp JwtUtils, JwtAuthenticationFilter và SecurityConfig.
- **Giao diện** sử dụng Thymeleaf và CSS tách riêng.

Hy vọng ví dụ này đã đáp ứng yêu cầu của bạn (không dùng DTO) mà vẫn giữ các tính năng nâng cao của dự án. Nếu có thắc mắc hoặc cần bổ sung thêm, bạn cứ trao đổi nhé!