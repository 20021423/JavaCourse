Dưới đây là một ví dụ “code chuẩn” từ đầu đến cuối cho dự án ban đầu (quản lý người dùng với các chức năng tìm kiếm, xoá, “tym”,…) có tích hợp thêm giỏ hàng dùng session (giỏ hàng ở đây là danh sách những người dùng được “thêm vào giỏ”). Ví dụ này sử dụng Spring Boot, Spring Data JPA, Lombok, Spring Security (với JWT đơn giản – bạn có thể nâng cấp nếu cần), Thymeleaf và áp dụng các nguyên tắc OOP, SOLID, DTO, cũng như Factory Method Pattern để lựa chọn nguồn dữ liệu (DB hoặc JSON).

Chú ý:  
- Trong ví dụ này, chúng ta sẽ dùng file JSON để load dữ liệu người dùng (với cấu trúc mẫu) nhưng bạn có thể chuyển sang DB bằng cách thay đổi cấu hình.  
- Giỏ hàng ở đây là “giỏ người dùng” (tức là danh sách UserDTO được thêm vào giỏ).  
- Mã được viết chi tiết, kèm chú thích cho mỗi file.

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
     │   │       ├─ dto
     │   │       │   ├─ UserDTO.java
     │   │       │   └─ UserMapper.java
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
    <!-- Spring Boot Starter Web -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    
    <!-- Spring Data JPA -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- PostgreSQL Driver (nếu dùng DB) -->
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
    
    <!-- Jackson để xử lý JSON -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
    </dependency>
    
    <!-- Validation -->
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

## 3. File application.properties

```properties
# File: src/main/resources/application.properties

# Loại nguồn dữ liệu: "db" hoặc "json"
app.datasource-type=json

# Database config (chỉ dùng khi app.datasource-type=db)
spring.datasource.url=jdbc:postgresql://localhost:5432/demo_db
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# Thymeleaf
spring.thymeleaf.cache=false

# JWT config
app.jwt.secret=MY_SUPER_SECRET_KEY
app.jwt.expiration=3600000

server.port=8080
```

---

## 4. Lớp chạy chính: CartDemoApplication.java

```java
// File: src/main/java/com/example/demo/CartDemoApplication.java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CartDemoApplication {
  public static void main(String[] args) {
    SpringApplication.run(CartDemoApplication.class, args);
  }
}
```

---

## 5. Model

### 5.1 User.java

```java
// File: src/main/java/com/example/demo/model/User.java
package com.example.demo.model;

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

  // Họ và tên (ví dụ: "George Bluth")
  private String name;

  private String email;

  // URL ảnh đại diện
  private String avatarUrl;

  // Các trường dùng cho đăng nhập
  private String username;
  private String password;
  private String role;

  // Số lượt “tym”
  private int likeCount;
}
```

### 5.2 Cart.java  
(Đây là model cho giỏ hàng, lưu danh sách UserDTO đã được chọn)

```java
// File: src/main/java/com/example/demo/model/Cart.java
package com.example.demo.model;

import com.example.demo.dto.UserDTO;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.util.ArrayList;
import java.util.List;

@Data
@NoArgsConstructor
public class Cart {
  // Danh sách người dùng (UserDTO) đã được thêm vào "giỏ hàng"
  private List<UserDTO> items = new ArrayList<>();

  // Thêm 1 người dùng vào giỏ (nếu chưa có)
  public void addItem(UserDTO user) {
    boolean exists = items.stream().anyMatch(u -> u.getId().equals(user.getId()));
    if (!exists) {
      items.add(user);
    }
  }

  // Xoá người dùng khỏi giỏ
  public void removeItem(Long userId) {
    items.removeIf(u -> u.getId().equals(userId));
  }

  // Tổng số người trong giỏ
  public int getTotalItems() {
    return items.size();
  }
}
```

---

## 6. DTO và Mapper

### 6.1 UserDTO.java

```java
// File: src/main/java/com/example/demo/dto/UserDTO.java
package com.example.demo.dto;

import lombok.*;

@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class UserDTO {
  private Long id;
  private String name;
  private String email;
  private String avatarUrl;
  private int likeCount;
}
```

### 6.2 UserMapper.java

```java
// File: src/main/java/com/example/demo/dto/UserMapper.java
package com.example.demo.dto;

import com.example.demo.model.User;

public class UserMapper {

  // Chuyển đổi entity User sang DTO
  public static UserDTO toDTO(User user) {
    if (user == null) return null;
    return UserDTO.builder()
        .id(user.getId())
        .name(user.getName())
        .email(user.getEmail())
        .avatarUrl(user.getAvatarUrl())
        .likeCount(user.getLikeCount())
        .build();
  }

  // Chuyển đổi DTO sang entity User
  public static User toEntity(UserDTO dto) {
    if (dto == null) return null;
    return User.builder()
        .id(dto.getId())
        .name(dto.getName())
        .email(dto.getEmail())
        .avatarUrl(dto.getAvatarUrl())
        .likeCount(dto.getLikeCount())
        .build();
  }
}
```

---

## 7. Repository

### 7.1 UserRepository.java

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
  // Tìm kiếm theo username (cho đăng nhập)
  Optional<User> findByUsername(String username);

  // Tìm kiếm theo tên (không phân biệt chữ hoa chữ thường)
  List<User> findByNameContainingIgnoreCase(String name);
}
```

---

## 8. Datasource & Factory Method Pattern

### 8.1 UserDataSource.java  
(Định nghĩa các phương thức cần thiết để lấy dữ liệu)

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

### 8.2 JsonUserDataSource.java  
(Đọc dữ liệu từ file JSON, sử dụng Lombok cho DTO nội bộ)

```java
// File: src/main/java/com/example/demo/datasource/JsonUserDataSource.java
package com.example.demo.datasource;

import com.example.demo.model.User;
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;
import lombok.extern.slf4j.Slf4j;

import java.io.File;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

@Slf4j
public class JsonUserDataSource implements UserDataSource {

  private final List<User> inMemoryUsers = new ArrayList<>();

  // DTO để map dữ liệu JSON
  @Data
  @NoArgsConstructor
  @AllArgsConstructor
  public static class JsonUserDTO {
    private Long id;
    private String email;
    private String first_name;
    private String last_name;
    private String avatar;
  }

  public JsonUserDataSource() {
    loadUsersFromJson();
  }

  private void loadUsersFromJson() {
    try {
      File jsonFile = new File("src/main/resources/data/users.json");
      if (jsonFile.exists()) {
        ObjectMapper mapper = new ObjectMapper();
        List<JsonUserDTO> dtos = mapper.readValue(jsonFile, new TypeReference<List<JsonUserDTO>>(){});
        dtos.forEach(dto -> {
          User user = User.builder()
              .id(dto.getId())
              .name(dto.getFirst_name() + " " + dto.getLast_name())
              .email(dto.getEmail())
              .avatarUrl(dto.getAvatar())
              // Dùng email làm username, password mặc định "123"
              .username(dto.getEmail())
              .password("123")
              .role("USER")
              .likeCount(0)
              .build();
          inMemoryUsers.add(user);
        });
      } else {
        log.warn("File JSON không tồn tại: {}", jsonFile.getAbsolutePath());
      }
    } catch (Exception e) {
      log.error("Lỗi khi load JSON: ", e);
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

### 8.3 DbUserDataSource.java  
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

### 8.4 UserDataSourceFactory.java  
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

## 9. Service Layer

### 9.1 UserService.java  
(Sử dụng DTO)

```java
// File: src/main/java/com/example/demo/service/UserService.java
package com.example.demo.service;

import com.example.demo.dto.UserDTO;
import java.util.List;

public interface UserService {
  UserDTO register(UserDTO userDTO);
  UserDTO findById(Long id);
  List<UserDTO> getAllUsers();
  List<UserDTO> search(String keyword);
  UserDTO likeUser(Long id);
  void deleteUser(Long id);
  UserDTO findByUsername(String username);
}
```

### 9.2 UserServiceImpl.java

```java
// File: src/main/java/com/example/demo/service/impl/UserServiceImpl.java
package com.example.demo.service.impl;

import com.example.demo.datasource.UserDataSource;
import com.example.demo.dto.UserDTO;
import com.example.demo.dto.UserMapper;
import com.example.demo.model.User;
import com.example.demo.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.stream.Collectors;

@Service
@RequiredArgsConstructor
public class UserServiceImpl implements UserService {

  private final UserDataSource userDataSource;

  @Override
  public UserDTO register(UserDTO userDTO) {
    // Chuyển DTO sang entity, thiết lập giá trị mặc định nếu cần
    User user = UserMapper.toEntity(userDTO);
    if (user.getRole() == null) {
      user.setRole("USER");
    }
    if (user.getLikeCount() == 0) {
      user.setLikeCount(0);
    }
    User saved = userDataSource.saveUser(user);
    return UserMapper.toDTO(saved);
  }

  @Override
  public UserDTO findById(Long id) {
    User user = userDataSource.findById(id).orElse(null);
    return UserMapper.toDTO(user);
  }

  @Override
  public List<UserDTO> getAllUsers() {
    return userDataSource.getAllUsers().stream()
        .map(UserMapper::toDTO)
        .collect(Collectors.toList());
  }

  @Override
  public List<UserDTO> search(String keyword) {
    return userDataSource.searchUsers(keyword).stream()
        .map(UserMapper::toDTO)
        .collect(Collectors.toList());
  }

  @Override
  public UserDTO likeUser(Long id) {
    User user = userDataSource.findById(id).orElse(null);
    if (user != null) {
      user.setLikeCount(user.getLikeCount() + 1);
      user = userDataSource.saveUser(user);
    }
    return UserMapper.toDTO(user);
  }

  @Override
  public void deleteUser(Long id) {
    userDataSource.deleteById(id);
  }

  @Override
  public UserDTO findByUsername(String username) {
    User user = userDataSource.findByUsername(username).orElse(null);
    return UserMapper.toDTO(user);
  }
}
```

---

## 10. Security (JWT và Filter)

### 10.1 JwtUtils.java

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

### 10.2 JwtAuthenticationFilter.java

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

### 10.3 SecurityConfig.java

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

## 11. Controller

### 11.1 AuthController.java  
(Đăng ký và đăng nhập – sử dụng entity cho đăng nhập)

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
    // Ở đây nên thêm validate và mã hoá password trong production
    User registered = userService.register(
        // Chuyển entity sang DTO trước khi truyền cho service
        com.example.demo.dto.UserMapper.toDTO(user)
    );
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

### 11.2 UserController.java  
(Quản lý người dùng: list, tạo, xoá, “tym”)

```java
// File: src/main/java/com/example/demo/controller/UserController.java
package com.example.demo.controller;

import com.example.demo.dto.UserDTO;
import com.example.demo.service.UserService;
import com.example.demo.dto.UserMapper;
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
    List<UserDTO> users = (search == null || search.isBlank())
        ? userService.getAllUsers()
        : userService.search(search);
    model.addAttribute("users", users);
    model.addAttribute("search", search);
    return "user/list";
  }

  // Hiển thị form tạo người dùng
  @GetMapping("/create")
  public String showCreateForm(Model model) {
    model.addAttribute("user", new UserDTO());
    return "user/create";
  }

  // Xử lý tạo người dùng
  @PostMapping("/create")
  public String createUser(@ModelAttribute("user") UserDTO userDTO) {
    userService.register(userDTO);
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

### 11.3 CartController.java  
(Quản lý “giỏ hàng” – thêm/xoá người dùng từ giỏ, lưu vào session)

```java
// File: src/main/java/com/example/demo/controller/CartController.java
package com.example.demo.controller;

import com.example.demo.dto.UserDTO;
import com.example.demo.model.Cart;
import com.example.demo.service.UserService;
import com.example.demo.dto.UserMapper;
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
    // Lấy người dùng từ service và chuyển sang DTO
    var userDTO = UserMapper.toDTO(userService.findById(id));
    if (userDTO != null) {
      Cart cart = (Cart) session.getAttribute("cart");
      if (cart == null) {
        cart = new Cart();
      }
      cart.addItem(userDTO);
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

## 12. Thymeleaf Templates

### 12.1 templates/user/list.html  
(Hiển thị danh sách người dùng với nút “Add to Cart”)

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
      <!-- Nút thêm người dùng vào giỏ hàng -->
      <a th:href="@{/cart/add/{id}(id=${user.id})}">Add to Cart</a>
    </div>
  </div>
  <a th:href="@{/users/create}">Create new user</a> |
  <a th:href="@{/cart}">View Cart</a>
</body>
</html>
```

### 12.2 templates/user/create.html  
(Form tạo người dùng mới)

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

### 12.3 templates/cart/view.html  
(Hiển thị giỏ hàng chứa danh sách người dùng được thêm)

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
        <a th:href="@{/cart/remove/{id}(id=${user.id})}">Xoá</a>
      </li>
    </ul>
    <p>Tổng số người trong giỏ hàng: <span th:text="${cart.totalItems}">0</span></p>
  </div>
  <a href="/users">Quay lại danh sách người dùng</a>
</body>
</html>
```

---

## 13. File CSS

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
  width: 200px;
  text-align: center;
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

1. **Build & chạy ứng dụng:**

   - Mở terminal, di chuyển đến thư mục dự án và chạy lệnh:
     ```bash
     mvn clean spring-boot:run
     ```
   - Hoặc bạn có thể chạy class `CartDemoApplication` từ IDE (IntelliJ, Eclipse, v.v).

2. **Kiểm tra các chức năng:**

   - **Danh sách người dùng:**  
     Truy cập [http://localhost:8080/users](http://localhost:8080/users) để xem danh sách người dùng.  
     Tại đây bạn có thể tìm kiếm, “tym”, xoá và bấm nút “Add to Cart” cho từng người dùng.
     
   - **Tạo người dùng mới:**  
     Truy cập [http://localhost:8080/users/create](http://localhost:8080/users/create) để mở form tạo người dùng mới.
     
   - **Giỏ hàng:**  
     Sau khi thêm một số người dùng vào giỏ (bấm “Add to Cart”), truy cập [http://localhost:8080/cart](http://localhost:8080/cart) để xem giỏ hàng.  
     Tại đây bạn có thể xoá người dùng khỏi giỏ.

3. **Đăng nhập & Đăng ký (Security & JWT):**

   - API đăng ký và đăng nhập được cung cấp tại các endpoint `/auth/register` và `/auth/login`.  
   - Khi dùng API, bạn có thể gửi JSON để đăng ký hoặc đăng nhập. (Ví dụ: sử dụng Postman hoặc curl)
   - Các endpoint khác (ngoại trừ các URL công khai như `/users`, `/cart`, `/css/**`) sẽ được bảo vệ bởi Spring Security.

---

## 15. Kết luận

Trong ví dụ này, chúng ta đã xây dựng một ứng dụng Spring Boot “production-ready” với các module:
- **Quản lý người dùng:** sử dụng Entity, DTO, Repository, Service và Controller, với khả năng tìm kiếm, xoá, “tym”.
- **Nguồn dữ liệu:** Sử dụng Factory Method Pattern để lựa chọn nguồn dữ liệu (JSON hoặc DB).
- **Giỏ hàng:** Thêm module giỏ hàng (lưu danh sách người dùng được chọn) sử dụng session thông qua HttpSession.
- **Security:** Tích hợp Spring Security với JWT (các lớp JwtUtils, JwtAuthenticationFilter, SecurityConfig).
- **Giao diện:** Sử dụng Thymeleaf kết hợp với file CSS riêng để hiển thị danh sách người dùng, form tạo và giỏ hàng.

Hy vọng hướng dẫn chi tiết này sẽ giúp bạn hiểu cách code một dự án end-to-end theo chuẩn từ đầu đến cuối. Nếu có bất kỳ thắc mắc hay yêu cầu bổ sung nào, bạn cứ trao đổi thêm nhé!