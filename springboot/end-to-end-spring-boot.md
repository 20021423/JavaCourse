Dưới đây là tài liệu hướng dẫn end-to-end, step-by-step (bằng tiếng Việt) cho một dự án Spring Boot 3.4.3 với Java 17. Tài liệu này bao gồm các chức năng:

- CRUD sản phẩm (bao gồm ảnh sản phẩm)
- Giỏ hàng lưu tạm (sử dụng session)
- Đặt hàng (chuyển giỏ hàng thành đơn hàng sau khi đăng nhập)
- Xác thực & Phân quyền qua JWT
- Đăng ký người dùng
- Tài liệu hóa API bằng Swagger (sử dụng springdoc-openapi)
- Giao diện được render bởi Thymeleaf (sử dụng layout, meta-data, CSS, JS)
- Các lớp payload (request/response) được định nghĩa riêng nhằm tạo “contract” giữa client và server

---

## 1. Yêu Cầu Nghiệp Vụ & Kiến Trúc Ứng Dụng

### 1.1 Yêu Cầu Nghiệp Vụ

- **Sản phẩm:**  
  - Thực hiện CRUD sản phẩm với các trường: `id`, `name`, `description`, `price`, `imageUrl` (URL ảnh lấy từ dịch vụ như dummyimage.com).

- **Giỏ hàng:**  
  - Cho phép người dùng (chưa đăng nhập) thêm sản phẩm vào giỏ hàng (lưu theo session) và cập nhật số lượng.

- **Đặt hàng:**  
  - Khi bấm “Thanh toán”: nếu chưa đăng nhập, hệ thống yêu cầu xác thực (dùng JWT) hoặc chuyển giao diện đăng nhập.
  - Sau khi đăng nhập, giỏ hàng chuyển thành đơn hàng và lưu vào DB.

- **Xác thực & Phân quyền:**  
  - Sử dụng Spring Security kết hợp với JWT để xây dựng API REST stateless, bảo vệ các endpoint nhạy cảm.

- **Đăng ký người dùng:**  
  - Cho phép người dùng mới đăng ký qua endpoint `/register`.

- **Tài liệu hóa API:**  
  - Tích hợp Swagger (Springdoc-openapi) để tự động tạo tài liệu OpenAPI cho các API.

### 1.2 Kiến Trúc Ứng Dụng

- **Mô hình MVC:**  
  - **Model:** Các entity (Product, User, Order, OrderItem).  
  - **View:** Giao diện HTML được render bằng Thymeleaf (sử dụng layout, meta-data, CSS, JS).  
  - **Controller:** Xử lý các request, chuyển dữ liệu giữa view và service.

- **Các Layer Chính:**  
  - **Domain/Entity:** Sử dụng *jakarta.persistence* (Spring Boot 3.x) để định nghĩa entity.
  - **Repository:** Sử dụng Spring Data JPA để giao tiếp với cơ sở dữ liệu PostgreSQL.
  - **Service:** Xử lý nghiệp vụ (CRUD, giỏ hàng, đặt hàng, đăng ký).
  - **Controller:** Định nghĩa các API endpoint cho Auth, Registration, Product, Cart, Order, Login.
  - **Security:** Cấu hình Spring Security, tích hợp JWT (với JwtUtil, JwtRequestFilter, CustomUserDetailsService).
  - **Payload:** Tạo các lớp DTO (request & response) như LoginRequest, ProductRequest, RegistrationRequest, JwtResponse, RegistrationResponse – nhằm tách biệt dữ liệu API với entity.
  - **Swagger:** Tích hợp springdoc-openapi để tự động tài liệu hóa API.

---

## 2. Thiết Lập Môi Trường & Tạo Project

### 2.1 Tạo Project với Spring Initializr

- Truy cập [Spring Initializr](https://start.spring.io/) và cấu hình:
  - **Project:** Maven
  - **Language:** Java
  - **Spring Boot:** 3.4.3
  - **Java:** 17
  - **Dependencies:**  
    - Spring Web  
    - Spring Data JPA  
    - Thymeleaf  
    - Spring Security  
    - PostgreSQL Driver  
    - Lombok  
    - JJWT (ví dụ: version 0.9.1)  
    - springdoc-openapi-starter-webmvc-ui

Sau đó, tải về project và mở trong IDE (IntelliJ IDEA, Eclipse,…).

### 2.2 File pom.xml

_Dưới đây là file pom.xml đã cập nhật (lưu ý Spring Boot 3 sử dụng Java 17 và package jakarta.persistence):_

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>spring-web-app</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>
  <name>Spring Web App</name>
  <description>Dự án demo với Spring Boot 3, JPA, Thymeleaf, Security, JWT và PostgreSQL</description>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.4.3</version>
    <relativePath/>
  </parent>

  <properties>
    <java.version>17</java.version>
    <lombok.version>1.18.24</lombok.version>
  </properties>

  <dependencies>
    <!-- Spring Boot Starter Web -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot Starter Data JPA -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- Thymeleaf -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <!-- Spring Security -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <!-- JJWT (JWT) -->
    <dependency>
      <groupId>io.jsonwebtoken</groupId>
      <artifactId>jjwt</artifactId>
      <version>0.9.1</version>
    </dependency>

    <!-- Lombok -->
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>${lombok.version}</version>
      <scope>provided</scope>
    </dependency>

    <!-- PostgreSQL Driver -->
    <dependency>
      <groupId>org.postgresql</groupId>
      <artifactId>postgresql</artifactId>
      <scope>runtime</scope>
    </dependency>

    <!-- Springdoc-openapi Starter for Swagger UI -->
    <dependency>
      <groupId>org.springdoc</groupId>
      <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
      <version>2.2.0</version>
    </dependency>

    <!-- Spring Boot Starter Test -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <!-- Maven Compiler Plugin -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
          <source>${java.version}</source>
          <target>${java.version}</target>
          <annotationProcessorPaths>
            <path>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
              <version>${lombok.version}</version>
            </path>
          </annotationProcessorPaths>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

### 2.3 File application.properties

```properties
# Kết nối PostgreSQL
spring.datasource.url=jdbc:postgresql://localhost:5432/yourdb
spring.datasource.username=postgres
spring.datasource.password=yourpassword

spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# Cấu hình Thymeleaf (tắt cache khi phát triển)
spring.thymeleaf.cache=false

# Log Hibernate (tuỳ chọn)
logging.level.org.hibernate.SQL=DEBUG

# Thông số cho JWT
jwt.secret=MyJwtSecretKey12345
jwt.expiration=3600000
```

---

## 3. Cài Đặt Database: SQL Script

### 3.1 Tạo Bảng

```sql
-- Tạo bảng products
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    price NUMERIC(10,2),
    image_url VARCHAR(255)
);

-- Tạo bảng users
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role VARCHAR(50)
);

-- Tạo bảng orders
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    order_date TIMESTAMP,
    user_id INTEGER REFERENCES users(id)
);

-- Tạo bảng order_items
CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    product_id INTEGER REFERENCES products(id),
    quantity INTEGER,
    order_id INTEGER REFERENCES orders(id)
);
```

### 3.2 Chèn Dữ Liệu Mẫu

```sql
-- Chèn dữ liệu mẫu cho sản phẩm (URL ảnh từ dummyimage.com)
INSERT INTO products (name, description, price, image_url) VALUES
('Sản phẩm A', 'Mô tả sản phẩm A', 10.99, 'https://dummyimage.com/600x400/000/fff&text=Sản+phẩm+A'),
('Sản phẩm B', 'Mô tả sản phẩm B', 20.50, 'https://dummyimage.com/600x400/000/fff&text=Sản+phẩm+B'),
('Sản phẩm C', 'Mô tả sản phẩm C', 15.75, 'https://dummyimage.com/600x400/000/fff&text=Sản+phẩm+C');

-- Chèn dữ liệu mẫu cho người dùng  
-- Lưu ý: Mật khẩu phải được mã hóa BCrypt (ví dụ hash)
INSERT INTO users (username, password, role) VALUES
('user1', '$2a$10$exampleHashedPassword1', 'ROLE_USER'),
('admin', '$2a$10$exampleHashedPassword2', 'ROLE_ADMIN');
```

---

## 4. Triển Khai Code Ứng Dụng

### 4.1 Cấu Trúc Thư Mục Dự Án

Ví dụ về cấu trúc thư mục (theo nguyên tắc Separation of Concerns):

```
com.example.demo
├── config
│   └── SecurityConfig.java
├── controller
│   ├── AuthController.java
│   ├── CartController.java
│   ├── OrderController.java
│   ├── ProductController.java
│   └── RegistrationController.java
├── filter
│   └── JwtRequestFilter.java
├── model
│   ├── Order.java
│   ├── OrderItem.java
│   ├── Product.java
│   └── User.java
├── payload
│   ├── request
│   │   ├── LoginRequest.java
│   │   ├── ProductRequest.java
│   │   └── RegistrationRequest.java
│   └── response
│       ├── JwtResponse.java
│       ├── RegistrationResponse.java
│       └── ResponseObject.java  (Tùy chọn)
├── repository
│   ├── OrderRepository.java
│   ├── ProductRepository.java
│   └── UserRepository.java
├── service
│   ├── CartService.java
│   ├── OrderService.java
│   ├── ProductService.java
│   └── UserService.java
├── service/impl
│   ├── CartServiceImpl.java
│   ├── OrderServiceImpl.java
│   ├── ProductServiceImpl.java
│   ├── UserServiceImpl.java
│   └── CustomUserDetailsService.java
├── util
│   └── JwtUtil.java
└── Application.java
```

---

### 4.2 Entity (Sử dụng jakarta.persistence)

#### 4.2.1 Product.java

```java
package com.example.demo.model;

import lombok.*;
import jakarta.persistence.*;

/**
 * Entity lưu thông tin sản phẩm.
 */
@Entity
@Table(name = "products")
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String description;
    private Double price;
    
    // Đường dẫn ảnh sản phẩm (URL)
    @Column(name = "image_url")
    private String imageUrl;
}
```

#### 4.2.2 User.java

```java
package com.example.demo.model;

import lombok.*;
import jakarta.persistence.*;

/**
 * Entity lưu thông tin người dùng.
 */
@Entity
@Table(name = "users")
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String username;
    
    private String password;
    private String role;
}
```

#### 4.2.3 Order.java

```java
package com.example.demo.model;

import lombok.*;
import jakarta.persistence.*;
import java.time.LocalDateTime;
import java.util.List;

/**
 * Entity lưu thông tin đơn hàng.
 */
@Entity
@Table(name = "orders")
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "order_date")
    private LocalDateTime orderDate;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id")
    private User user;
    
    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
    @JoinColumn(name = "order_id")
    private List<OrderItem> items;
}
```

#### 4.2.4 OrderItem.java

```java
package com.example.demo.model;

import lombok.*;
import jakarta.persistence.*;

/**
 * Entity lưu thông tin chi tiết sản phẩm trong đơn hàng.
 */
@Entity
@Table(name = "order_items")
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class OrderItem {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "product_id")
    private Product product;
    
    private Integer quantity;
}
```

---

### 4.3 Repository

#### 4.3.1 ProductRepository.java

```java
package com.example.demo.repository;

import com.example.demo.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;

/**
 * Repository cho Product.
 */
public interface ProductRepository extends JpaRepository<Product, Long> { }
```

#### 4.3.2 UserRepository.java

```java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

/**
 * Repository cho User.
 */
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByUsername(String username);
}
```

#### 4.3.3 OrderRepository.java

```java
package com.example.demo.repository;

import com.example.demo.model.Order;
import org.springframework.data.jpa.repository.JpaRepository;

/**
 * Repository cho Order.
 */
public interface OrderRepository extends JpaRepository<Order, Long> { }
```

---

### 4.4 Service Layer

#### 4.4.1 ProductService.java

```java
package com.example.demo.service;

import com.example.demo.model.Product;
import java.util.List;
import java.util.Optional;

/**
 * Interface định nghĩa các thao tác cho sản phẩm.
 */
public interface ProductService {
    List<Product> findAll();
    Optional<Product> findById(Long id);
    Product save(Product product);
    void deleteById(Long id);
}
```

#### 4.4.2 ProductServiceImpl.java

```java
package com.example.demo.service.impl;

import com.example.demo.model.Product;
import com.example.demo.repository.ProductRepository;
import com.example.demo.service.ProductService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.Optional;

/**
 * Triển khai ProductService.
 */
@Service
@RequiredArgsConstructor
public class ProductServiceImpl implements ProductService {
    private final ProductRepository productRepository;
    
    @Override
    public List<Product> findAll() {
        return productRepository.findAll();
    }
    
    @Override
    public Optional<Product> findById(Long id) {
        return productRepository.findById(id);
    }
    
    @Override
    public Product save(Product product) {
        return productRepository.save(product);
    }
    
    @Override
    public void deleteById(Long id) {
        productRepository.deleteById(id);
    }
}
```

#### 4.4.3 CartService.java

```java
package com.example.demo.service;

import com.example.demo.model.Product;
import java.util.Map;

/**
 * Interface quản lý giỏ hàng.
 */
public interface CartService {
    void addProduct(Product product);
    void updateQuantity(Long productId, int quantity);
    Map<Long, Integer> getCartItems();
    void clearCart();
}
```

#### 4.4.4 CartServiceImpl.java

```java
package com.example.demo.service.impl;

import com.example.demo.model.Product;
import com.example.demo.service.CartService;
import org.springframework.stereotype.Service;
import org.springframework.web.context.annotation.SessionScope;
import java.util.HashMap;
import java.util.Map;

/**
 * Triển khai CartService, lưu giỏ hàng trong session.
 */
@Service
@SessionScope
public class CartServiceImpl implements CartService {
    private final Map<Long, Integer> cartItems = new HashMap<>();
    
    @Override
    public void addProduct(Product product) {
        cartItems.merge(product.getId(), 1, Integer::sum);
    }
    
    @Override
    public void updateQuantity(Long productId, int quantity) {
        if (quantity <= 0) {
            cartItems.remove(productId);
        } else {
            cartItems.put(productId, quantity);
        }
    }
    
    @Override
    public Map<Long, Integer> getCartItems() {
        return cartItems;
    }
    
    @Override
    public void clearCart() {
        cartItems.clear();
    }
}
```

#### 4.4.5 OrderService.java

```java
package com.example.demo.service;

import com.example.demo.model.Order;
import com.example.demo.model.User;
import java.util.Map;

/**
 * Interface xử lý đặt hàng.
 */
public interface OrderService {
    Order createOrder(User user, Map<Long, Integer> cartItems);
}
```

#### 4.4.6 OrderServiceImpl.java

```java
package com.example.demo.service.impl;

import com.example.demo.model.Order;
import com.example.demo.model.OrderItem;
import com.example.demo.model.Product;
import com.example.demo.model.User;
import com.example.demo.repository.OrderRepository;
import com.example.demo.repository.ProductRepository;
import com.example.demo.service.OrderService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import java.time.LocalDateTime;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

/**
 * Triển khai OrderService: tạo đơn hàng từ giỏ hàng.
 */
@Service
@RequiredArgsConstructor
public class OrderServiceImpl implements OrderService {
    private final OrderRepository orderRepository;
    private final ProductRepository productRepository;
    
    @Override
    public Order createOrder(User user, Map<Long, Integer> cartItems) {
        List<OrderItem> orderItems = cartItems.entrySet().stream().map(entry -> {
            Long productId = entry.getKey();
            Integer quantity = entry.getValue();
            Product product = productRepository.findById(productId)
                    .orElseThrow(() -> new RuntimeException("Product not found with id: " + productId));
            return OrderItem.builder()
                    .product(product)
                    .quantity(quantity)
                    .build();
        }).collect(Collectors.toList());
        
        Order order = Order.builder()
                .orderDate(LocalDateTime.now())
                .user(user)
                .items(orderItems)
                .build();
        return orderRepository.save(order);
    }
}
```

#### 4.4.7 UserService.java (Đăng ký)

```java
package com.example.demo.service;

import com.example.demo.model.User;
import com.example.demo.payload.request.RegistrationRequest;

/**
 * Interface xử lý đăng ký người dùng.
 */
public interface UserService {
    User registerUser(RegistrationRequest registrationRequest);
}
```

#### 4.4.8 UserServiceImpl.java

```java
package com.example.demo.service.impl;

import com.example.demo.model.User;
import com.example.demo.payload.request.RegistrationRequest;
import com.example.demo.repository.UserRepository;
import com.example.demo.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;

/**
 * UserServiceImpl: Xử lý đăng ký người dùng.
 */
@Service
@RequiredArgsConstructor
public class UserServiceImpl implements UserService {
    private final UserRepository userRepository;
    private final PasswordEncoder passwordEncoder;
    
    @Override
    public User registerUser(RegistrationRequest registrationRequest) {
        if (userRepository.findByUsername(registrationRequest.getUsername()).isPresent()) {
            throw new RuntimeException("Username already taken");
        }
        User user = User.builder()
                .username(registrationRequest.getUsername())
                .password(passwordEncoder.encode(registrationRequest.getPassword()))
                .role("ROLE_USER")
                .build();
        return userRepository.save(user);
    }
}
```

#### 4.4.9 CustomUserDetailsService.java

```java
package com.example.demo.service.impl;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.*;
import org.springframework.stereotype.Service;
import java.util.Collections;

/**
 * CustomUserDetailsService: Load thông tin người dùng từ DB cho Spring Security.
 */
@Service
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService {
    private final UserRepository userRepository;
    
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
          .orElseThrow(() -> new UsernameNotFoundException("User not found"));
        return new org.springframework.security.core.userdetails.User(
                user.getUsername(), 
                user.getPassword(), 
                Collections.singletonList(new SimpleGrantedAuthority(user.getRole()))
        );
    }
}
```

---

### 4.5 Security & JWT

#### 4.5.1 JwtUtil.java

```java
package com.example.demo.util;

import io.jsonwebtoken.*;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import java.util.Date;

/**
 * JwtUtil: Utility class để tạo và xác thực JWT.
 */
@Component
public class JwtUtil {

    @Value("${jwt.secret}")
    private String jwtSecret;
    
    @Value("${jwt.expiration}")
    private long jwtExpirationMs;
    
    // Tạo JWT dựa trên username
    public String generateJwtToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date((new Date()).getTime() + jwtExpirationMs))
                .signWith(SignatureAlgorithm.HS512, jwtSecret)
                .compact();
    }
    
    // Lấy username từ JWT
    public String getUserNameFromJwtToken(String token) {
        return Jwts.parser()
                .setSigningKey(jwtSecret)
                .parseClaimsJws(token)
                .getBody()
                .getSubject();
    }
    
    // Xác thực JWT
    public boolean validateJwtToken(String authToken) {
        try {
            Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(authToken);
            return true;
        } catch (JwtException e) {
            // Log lỗi nếu cần
        }
        return false;
    }
}
```

#### 4.5.2 JwtRequestFilter.java

```java
package com.example.demo.filter;

import com.example.demo.service.impl.CustomUserDetailsService;
import com.example.demo.util.JwtUtil;
import lombok.RequiredArgsConstructor;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;
import org.springframework.security.core.userdetails.UserDetails;
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.*;
import java.io.IOException;

/**
 * JwtRequestFilter: Lọc các request, kiểm tra JWT từ header và thiết lập SecurityContext.
 */
@Component
@RequiredArgsConstructor
public class JwtRequestFilter extends OncePerRequestFilter {
    private final JwtUtil jwtUtil;
    private final CustomUserDetailsService userDetailsService;
    
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
                                    throws ServletException, IOException {
        String headerAuth = request.getHeader("Authorization");
        if (headerAuth != null && headerAuth.startsWith("Bearer ")) {
            String jwt = headerAuth.substring(7);
            if (jwtUtil.validateJwtToken(jwt)) {
                String username = jwtUtil.getUserNameFromJwtToken(jwt);
                UserDetails userDetails = userDetailsService.loadUserByUsername(username);
                UsernamePasswordAuthenticationToken authentication =
                        new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authentication);
            }
        }
        filterChain.doFilter(request, response);
    }
}
```

#### 4.5.3 AuthController.java

```java
package com.example.demo.controller;

import com.example.demo.payload.request.LoginRequest;
import com.example.demo.payload.response.JwtResponse;
import com.example.demo.util.JwtUtil;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.*;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.bind.annotation.*;
import com.example.demo.service.impl.CustomUserDetailsService;
import java.util.List;

@RestController
@RequiredArgsConstructor
public class AuthController {
    private final AuthenticationManager authenticationManager;
    private final CustomUserDetailsService userDetailsService;
    private final JwtUtil jwtUtil;
    
    @PostMapping("/authenticate")
    public ResponseEntity<JwtResponse> createAuthenticationToken(@RequestBody LoginRequest loginRequest) {
        authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(loginRequest.getUsername(), loginRequest.getPassword())
        );
        final UserDetails userDetails = userDetailsService.loadUserByUsername(loginRequest.getUsername());
        final String jwt = jwtUtil.generateJwtToken(userDetails.getUsername());
        return ResponseEntity.ok(new JwtResponse(jwt, "Bearer", 1L, loginRequest.getUsername(), List.of("ROLE_USER")));
    }
}
```

#### 4.5.4 SecurityConfig.java

```java
package com.example.demo.config;

import com.example.demo.filter.JwtRequestFilter;
import com.example.demo.service.impl.CustomUserDetailsService;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.*;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.*;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {
    private final CustomUserDetailsService userDetailsService;
    private final JwtRequestFilter jwtRequestFilter;
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeRequests()
                .antMatchers("/", "/home", "/authenticate", "/register", "/css/**", "/js/**", "/products/**").permitAll()
                .anyRequest().authenticated()
            .and()
                .sessionManagement().sessionCreationPolicy(org.springframework.security.config.http.SessionCreationPolicy.STATELESS)
            .and()
                .formLogin().disable();
        http.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);
        return http.build();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    @Bean
    public AuthenticationManager authenticationManager(HttpSecurity http, PasswordEncoder passwordEncoder)
            throws Exception {
        return http.getSharedObject(AuthenticationManagerBuilder.class)
                   .userDetailsService(userDetailsService)
                   .passwordEncoder(passwordEncoder)
                   .and().build();
    }
}
```

---

### 4.6 Controller: API & Giao Diện

#### 4.6.1 ProductController.java

```java
package com.example.demo.controller;

import com.example.demo.model.Product;
import com.example.demo.payload.request.ProductRequest;
import com.example.demo.service.ProductService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@Controller
@RequestMapping("/products")
@RequiredArgsConstructor
public class ProductController {
    private final ProductService productService;
    
    @GetMapping
    public String listProducts(Model model) {
        List<Product> products = productService.findAll();
        model.addAttribute("products", products);
        return "products/list";
    }
    
    @GetMapping("/new")
    public String newProductForm(Model model) {
        model.addAttribute("product", new ProductRequest());
        return "products/form";
    }
    
    @PostMapping
    public String saveProduct(@ModelAttribute("product") ProductRequest productRequest) {
        Product product = Product.builder()
                .name(productRequest.getName())
                .description(productRequest.getDescription())
                .price(productRequest.getPrice())
                .imageUrl(productRequest.getImageUrl())
                .build();
        productService.save(product);
        return "redirect:/products";
    }
}
```

#### 4.6.2 CartController.java

```java
package com.example.demo.controller;

import com.example.demo.model.Product;
import com.example.demo.service.CartService;
import com.example.demo.service.ProductService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

@Controller
@RequestMapping("/cart")
@RequiredArgsConstructor
public class CartController {
    private final CartService cartService;
    private final ProductService productService;
    
    @PostMapping("/add/{productId}")
    public String addToCart(@PathVariable Long productId) {
        Product product = productService.findById(productId)
                .orElseThrow(() -> new RuntimeException("Product not found"));
        cartService.addProduct(product);
        return "redirect:/products";
    }
    
    @GetMapping
    public String viewCart(Model model) {
        model.addAttribute("cartItems", cartService.getCartItems());
        return "cart/view";
    }
    
    @PostMapping("/update/{productId}")
    public String updateCart(@PathVariable Long productId, @RequestParam("quantity") int quantity) {
        cartService.updateQuantity(productId, quantity);
        return "redirect:/cart";
    }
    
    @PostMapping("/checkout")
    public String checkout() {
        return "redirect:/order/confirm";
    }
}
```

#### 4.6.3 OrderController.java

```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.CartService;
import com.example.demo.service.OrderService;
import com.example.demo.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
@RequestMapping("/order")
@RequiredArgsConstructor
public class OrderController {
    private final OrderService orderService;
    private final CartService cartService;
    private final UserRepository userRepository;
    
    @PostMapping("/confirm")
    public String confirmOrder(@AuthenticationPrincipal UserDetails userDetails) {
        User user = userRepository.findByUsername(userDetails.getUsername())
                .orElseThrow(() -> new RuntimeException("User not found"));
        orderService.createOrder(user, cartService.getCartItems());
        cartService.clearCart();
        return "redirect:/order/success";
    }
    
    @GetMapping("/success")
    public String orderSuccess() {
        return "order/success";
    }
}
```

#### 4.6.4 RegistrationController.java

```java
package com.example.demo.controller;

import com.example.demo.payload.request.RegistrationRequest;
import com.example.demo.payload.response.RegistrationResponse;
import com.example.demo.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequiredArgsConstructor
public class RegistrationController {
    private final UserService userService;
    
    @PostMapping("/register")
    public ResponseEntity<RegistrationResponse> registerUser(@RequestBody RegistrationRequest registrationRequest) {
        userService.registerUser(registrationRequest);
        return ResponseEntity.ok(new RegistrationResponse("User registered successfully"));
    }
}
```

#### 4.6.5 LoginController.java

*(Nếu bạn muốn giao diện đăng nhập ngoài API JWT)*
```java
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class LoginController {
    @GetMapping("/login")
    public String login() {
        return "login";
    }
}
```

---

### 4.7 Payload (Request & Response)

#### 4.7.1 LoginRequest.java

```java
package com.example.demo.payload.request;

import lombok.Data;

/**
 * LoginRequest: Thông tin đăng nhập nhận từ client.
 */
@Data
public class LoginRequest {
    private String username;
    private String password;
}
```

#### 4.7.2 ProductRequest.java

```java
package com.example.demo.payload.request;

import lombok.Data;

/**
 * ProductRequest: Dữ liệu tạo/sửa sản phẩm.
 */
@Data
public class ProductRequest {
    private String name;
    private String description;
    private Double price;
    private String imageUrl;
}
```

#### 4.7.3 RegistrationRequest.java

```java
package com.example.demo.payload.request;

import lombok.Data;

/**
 * RegistrationRequest: Thông tin đăng ký người dùng.
 */
@Data
public class RegistrationRequest {
    private String username;
    private String password;
}
```

#### 4.7.4 JwtResponse.java

```java
package com.example.demo.payload.response;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.util.List;

/**
 * JwtResponse: Dữ liệu trả về sau khi đăng nhập thành công.
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
public class JwtResponse {
    private String token;
    private String type = "Bearer";
    private Long id;
    private String username;
    private List<String> roles;
}
```

#### 4.7.5 RegistrationResponse.java

```java
package com.example.demo.payload.response;

import lombok.AllArgsConstructor;
import lombok.Data;

/**
 * RegistrationResponse: Phản hồi sau khi đăng ký người dùng.
 */
@Data
@AllArgsConstructor
public class RegistrationResponse {
    private String message;
}
```

#### 4.7.6 ResponseObject.java (Tùy chọn)

```java
package com.example.demo.payload.response;

import lombok.AllArgsConstructor;
import lombok.Data;

/**
 * ResponseObject: Định dạng chung cho phản hồi API.
 */
@Data
@AllArgsConstructor
public class ResponseObject<T> {
    private int status;
    private String message;
    private T data;
}
```

---

### 4.8 Thymeleaf Templates & Tài Nguyên Tĩnh

#### Cấu trúc thư mục:

```
src/main/resources
├── static
│   ├── css
│   │   └── style.css
│   ├── js
│   │   └── script.js
│   └── images
└── templates
    ├── layout.html
    ├── products
    │   ├── list.html
    │   └── form.html
    ├── cart
    │   └── view.html
    ├── order
    │   └── success.html
    └── login.html
```

#### 4.8.1 layout.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8" />
    <!-- Meta-data cho SEO & Responsive -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Ứng dụng demo sử dụng Spring Boot 3, Thymeleaf, JWT, v.v..." />
    <title th:text="${title}">My App</title>
    <!-- Import CSS -->
    <link rel="stylesheet" th:href="@{/css/style.css}" />
</head>
<body>
    <header>
        <h1>Ứng dụng của tôi</h1>
        <!-- Menu điều hướng -->
    </header>
    
    <!-- Nội dung trang -->
    <div th:insert="~{::body}"></div>
    
    <footer>
        <p>Bản quyền © 2025</p>
    </footer>
    <!-- Import JavaScript -->
    <script th:src="@{/js/script.js}"></script>
</body>
</html>
```

#### 4.8.2 products/list.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" th:replace="layout :: body">
<head>
    <title>Danh sách sản phẩm</title>
</head>
<body>
    <h2>Danh sách sản phẩm</h2>
    <div th:if="${products}">
        <ul>
            <li th:each="product : ${products}">
                <strong th:text="${product.name}"></strong>
                - <span th:text="${product.price}"></span>
                <br/>
                <img th:src="${product.imageUrl}" alt="Ảnh sản phẩm" width="150"/>
                <br/>
                <!-- Form thêm sản phẩm vào giỏ hàng -->
                <form th:action="@{'/cart/add/' + ${product.id}}" method="post" style="display:inline;">
                    <button type="submit">Thêm vào giỏ hàng</button>
                </form>
            </li>
        </ul>
    </div>
</body>
</html>
```

#### 4.8.3 products/form.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" th:replace="layout :: body">
<head>
    <title>Form sản phẩm</title>
</head>
<body>
    <h2>Thêm/Sửa sản phẩm</h2>
    <form th:action="@{/products}" th:object="${product}" method="post">
        <div>
            <label>Tên sản phẩm:</label>
            <input type="text" th:field="*{name}" />
        </div>
        <div>
            <label>Mô tả:</label>
            <input type="text" th:field="*{description}" />
        </div>
        <div>
            <label>Giá:</label>
            <input type="number" step="0.01" th:field="*{price}" />
        </div>
        <div>
            <label>Đường dẫn ảnh:</label>
            <input type="text" th:field="*{imageUrl}" />
        </div>
        <button type="submit">Lưu sản phẩm</button>
    </form>
</body>
</html>
```

#### 4.8.4 cart/view.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" th:replace="layout :: body">
<head>
    <title>Giỏ hàng</title>
</head>
<body>
    <h2>Giỏ hàng của bạn</h2>
    <div th:if="${#lists.isEmpty(cartItems)}">
        <p>Giỏ hàng trống.</p>
    </div>
    <div th:if="${!#lists.isEmpty(cartItems)}">
        <table>
            <thead>
                <tr>
                    <th>Product ID</th>
                    <th>Số lượng</th>
                    <th>Thao tác</th>
                </tr>
            </thead>
            <tbody>
                <tr th:each="entry : ${cartItems}">
                    <td th:text="${entry.key}"></td>
                    <td th:text="${entry.value}"></td>
                    <td>
                        <form th:action="@{'/cart/update/' + ${entry.key}}" method="post">
                            <input type="number" name="quantity" min="0" th:value="${entry.value}" />
                            <button type="submit">Cập nhật</button>
                        </form>
                    </td>
                </tr>
            </tbody>
        </table>
        <form action="/cart/checkout" method="post">
            <button type="submit">Thanh toán</button>
        </form>
    </div>
</body>
</html>
```

#### 4.8.5 order/success.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" th:replace="layout :: body">
<head>
    <title>Đơn hàng thành công</title>
</head>
<body>
    <h2>Đơn hàng đã được đặt thành công!</h2>
    <a th:href="@{/products}">Tiếp tục mua sắm</a>
</body>
</html>
```

#### 4.8.6 login.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" th:replace="layout :: body">
<head>
    <title>Đăng nhập</title>
</head>
<body>
    <h2>Đăng nhập</h2>
    <form action="/login" method="post">
        <div>
            <label>Tên đăng nhập:</label>
            <input type="text" name="username" />
        </div>
        <div>
            <label>Mật khẩu:</label>
            <input type="password" name="password" />
        </div>
        <button type="submit">Đăng nhập</button>
    </form>
    <div th:if="${param.error}">
        <p style="color:red;">Tên đăng nhập hoặc mật khẩu không đúng.</p>
    </div>
    <div th:if="${param.logout}">
        <p style="color:green;">Bạn đã đăng xuất thành công.</p>
    </div>
</body>
</html>
```

#### 4.8.7 static/css/style.css

```css
/* style.css: Định dạng chung cho giao diện */
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
}

header, footer {
    background-color: #333;
    color: #fff;
    padding: 1rem;
    text-align: center;
}

h1, h2 {
    color: #333;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin: 1rem 0;
}

table, th, td {
    border: 1px solid #ccc;
}

th, td {
    padding: 0.5rem;
    text-align: left;
}

button {
    padding: 0.5rem 1rem;
    background-color: #007bff;
    border: none;
    color: #fff;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

form {
    margin: 0.5rem 0;
}
```

#### 4.8.8 static/js/script.js

```js
// script.js: Xử lý JavaScript chung cho ứng dụng
console.log("Ứng dụng đã được load.");
```

---

### 4.9 Tích Hợp Swagger với Springdoc-openapi

#### 4.9.1 Dependency

(Đã được thêm trong pom.xml: springdoc-openapi-starter-webmvc-ui)

#### 4.9.2 Sử Dụng Swagger

Sau khi chạy ứng dụng, Springdoc-openapi sẽ tự động tạo tài liệu API. Truy cập:
- [http://localhost:8080/swagger-ui/index.html](http://localhost:8080/swagger-ui/index.html)

Để tùy chỉnh thêm cho một endpoint, bạn có thể sử dụng các annotation OpenAPI:

```java
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@Tag(name = "Demo", description = "API demo cho Swagger")
@RestController
public class DemoController {

    @Operation(summary = "Lấy thông tin demo", description = "Trả về thông tin demo để kiểm tra Swagger")
    @GetMapping("/demo")
    public String getDemo() {
        return "Demo API";
    }
}
```

---

## 5. Kiểm Thử & Triển Khai

### 5.1 Kiểm Thử

- **Chạy ứng dụng:**  
  - Truy cập `/products` để xem danh sách sản phẩm (dữ liệu mẫu từ SQL).  
  - Thêm sản phẩm vào giỏ hàng qua `/cart/add/{productId}`, xem giỏ hàng tại `/cart` và cập nhật số lượng.  
  - Gọi endpoint `/authenticate` với JSON body chứa LoginRequest để nhận JWT.  
  - Test các API bảo vệ (ví dụ, `/order/confirm`) bằng cách gửi header:  
    `Authorization: Bearer <jwt>`  
  - Đăng ký người dùng qua `/register` với RegistrationRequest, nhận RegistrationResponse.  
  - Kiểm tra luồng đặt hàng qua `/order/confirm` và xem trang `/order/success`.  
  - Truy cập Swagger UI tại [http://localhost:8080/swagger-ui/index.html](http://localhost:8080/swagger-ui/index.html) để xem tài liệu API và thử nghiệm các endpoint.

### 5.2 Triển Khai

- **Build ứng dụng:**  
  Sử dụng Maven:
  ```
  mvn clean package
  ```
- **Triển khai:**  
  Chạy file jar được build hoặc deploy lên server (ví dụ: Heroku, AWS, v.v.).

---

## 6. Quy Trình Code & Test Liên Tục

### 6.1 Phát Triển theo Iterative Development

1. **Phân tích & thiết kế:** Xác định yêu cầu, tạo tài liệu thiết kế và sơ đồ kiến trúc.
2. **Xây dựng Domain Model & Repository:** Code các entity và repository, unit test từng phần (có thể dùng H2 cho test).
3. **Xây dựng Service Layer:** Viết business logic (CRUD, giỏ hàng, đặt hàng, đăng ký) và unit test.
4. **Xây dựng Controller & API Endpoint:** Tạo các endpoint cho từng module, test qua Postman và Swagger.
5. **Tích hợp bảo mật:** Cấu hình Spring Security & JWT, test bảo mật (với request có/không token).
6. **Xây dựng giao diện với Thymeleaf:** Tạo các view, layout, CSS, JS, meta-data; test giao diện trên trình duyệt.
7. **Kiểm thử & triển khai:** Chạy integration test, manual test, fix bug và build/deploy ứng dụng.

### 6.2 Lợi Ích

- Phát hiện lỗi sớm khi kiểm thử từng module.
- Giảm rủi ro khi tích hợp nhiều module.
- Dễ bảo trì và mở rộng ứng dụng khi có thay đổi.
- Tích hợp CI/CD tự động build, test và deploy giúp đảm bảo chất lượng phần mềm.

---

## 7. Tổng Kết

Tài liệu này cung cấp quy trình phát triển ứng dụng end-to-end theo Spring Boot 3.4.3 (Java 17) với đầy đủ:

- Phân tích yêu cầu & kiến trúc
- Thiết lập môi trường (pom.xml, application.properties)
- Cài đặt DB (SQL Script tạo bảng và chèn dữ liệu mẫu)
- Code backend (entity, repository, service, controller) với payload (request & response DTO)
- Tích hợp bảo mật qua Spring Security & JWT (JwtUtil, JwtRequestFilter, AuthController, SecurityConfig)
- Xây dựng giao diện người dùng với Thymeleaf (layout, meta-data, CSS, JS)
- Tích hợp Swagger qua springdoc-openapi để tài liệu hóa API
- Quy trình code và test liên tục theo iterative development

Việc phát triển theo từng module và kiểm thử liên tục giúp dễ dàng phát hiện lỗi, giảm thiểu rủi ro và dễ bảo trì mở rộng sau này. Ngoài ra, việc sử dụng Swagger (springdoc-openapi) giúp bạn tài liệu hóa API một cách tự động và tương tác trực tiếp.
