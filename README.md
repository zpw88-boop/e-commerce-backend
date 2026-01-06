# 中小型企业电商平台后端系统 (E-Commerce Backend)
基于 Spring Boot 3.x 构建的电商后端服务系统，提供完整的商品管理、订单处理、用户管理、库存管理等核心功能。

## 技术栈
- **框架**: Spring Boot 3.5.9
- **语言**: Java 17
- **数据库**: MySQL 8.0+
- **ORM**: MyBatis 3.0.3
- **安全**: Spring Security + JWT
- **构建工具**: Maven
- **其他**: Lombok, Validation

## 功能模块
- **商品管理** (`goods`): 商品信息管理、分类管理
- **订单管理** (`order`): 订单创建、查询、状态管理
- **购物车** (`cart`): 购物车增删改查
- **用户管理** (`userAdmin` / `userFront`): 后台用户管理、前台用户注册登录
- **库存管理** (`stock`): 库存查询、调整、盘点
- **仓库管理** (`warehouse`): 仓库信息管理
- **物流管理** (`express`): 物流公司管理
- **采购管理** (`procurement`): 采购订单管理
- **登录认证** (`login`): JWT 认证、权限控制

## 环境要求
- JDK 17+
- Maven 3.6+
- MySQL 8.0+
- IDE VS Code(推荐 IntelliJ IDEA)

## 快速开始
### 克隆项目
**git clone <repository-url>**  
**cd e-commerce-backend**

### 数据库配置
创建数据库并导入表结构：  

**CREATE DATABASE e_commerce CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;**  

修改 src/main/resources/application.yml 中的数据库连接信息：  

**spring:**  
  **datasource:**  
    **url: jdbc:mysql://localhost:3306/e_commerce?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai**  
    **username: root**  
    **password: your_password**

### 配置 JWT 密钥
修改 src/main/resources/application.yml 中的 JWT 配置：  

**jwt:**  
  **secret: your-32bit-or-longer-secret-key-for-jwt-signing**  
  **expire-ms: 86400000  # 24小时**

### 运行项目
**# 使用 Maven 运行**  
**mvn spring-boot:run**  

**# 或使用 IDE 直接运行 EcommerceApplication.java**  

项目启动后，访问地址：`http://localhost:8080`

## 项目结构
src/main/java/com/kingso/ecommerce/  
├── common/                    # 公共模块  
│   ├── jwt/                  # JWT 工具类  
│   ├── login/                # 登录通用 DTO/常量  
│   └── result/               # 统一响应结果  
├── config/                    # 配置类  
│   └── SecurityConfig.java   # Spring Security 配置  
├── module/                    # 业务模块  
│   ├── goods/                # 商品模块  
│   ├── order/                # 订单模块  
│   ├── cart/                 # 购物车模块  
│   ├── userAdmin/            # 后台用户模块  
│   ├── userFront/            # 前台用户模块  
│   ├── stock/                # 库存模块  
│   ├── warehouse/            # 仓库模块  
│   ├── express/              # 物流模块  
│   ├── procurement/         # 采购模块  
│   └── login/                # 登录模块  
└── EcommerceApplication.java # 启动类  

src/main/resources/  
├── mapper/                    # MyBatis XML 映射文件  
│   ├── goods/  
│   ├── order/  
│   └── ...  
└── application.yml           # 应用配置文件  

## API 接口
### 认证接口
- `POST /api/auth/login` - 用户登录（无需认证）

### 订单接口
- `POST /api/front/order/add` - 创建订单（需认证）
- `GET /api/front/order/page` - 分页查询订单（需认证）
- `GET /api/front/order/{id}` - 查询订单详情（需认证）

### 商品接口
- `GET /api/front/goods/page` - 分页查询商品（无需认证）
- `GET /api/front/goods/{id}` - 查询商品详情（无需认证）

### 购物车接口
- `POST /api/front/cart/add` - 添加购物车（需认证）
- `GET /api/front/cart/list` - 查询购物车列表（需认证）
- `PUT /api/front/cart/update` - 更新购物车（需认证）
- `DELETE /api/front/cart/delete` - 删除购物车（需认证）

### 用户接口
- `POST /api/userfront/register` - 用户注册（无需认证）
- `GET /api/userfront/{userId}` - 查询用户信息（需认证）

> 更多接口请参考各模块的 Controller 类

## 认证说明
系统使用 JWT (JSON Web Token) 进行身份认证：  

**登录获取 Token**: 调用 `/api/auth/login` 接口，返回 `token`  
**携带 Token**: 在请求头中添加 `Authorization: Bearer {token}`  
**Token 过期**: Token 默认有效期为 24 小时，过期后需重新登录  

### 无需认证的接口
- `/api/auth/login` - 登录接口
- `/api/userfront/register` - 注册接口
- `/static/**` - 静态资源
- `/api/front/goods/**` - 商品查询接口（部分）

## 配置说明
### 数据库配置
**spring:**  
  **datasource:**  
    **url: jdbc:mysql://localhost:3306/e_commerce**  
    **username: root**  
    **password: your_password**  
    **hikari:**  
      **maximum-pool-size: 10**  
      **minimum-idle: 5**

### 文件上传配置
**spring:**  
  **servlet:**  
    **multipart:**  
      **max-file-size: 5MB**  
      **max-request-size: 10MB**  

**goods:**  
  **upload:**  
    **base-path: ./static/images/goods/**  
    **base-url: http://localhost:8080/static/images/goods/**

### MyBatis 配置
**mybatis:**  
  **mapper-locations: classpath:mapper/**/*.xml**  
  **type-aliases-package: com.kingso.ecommerce.module.*.entity**  
  **configuration:**  
    **map-underscore-to-camel-case: true**

## 开发规范
### 代码结构
每个业务模块遵循统一的结构：  

module/  
├── controller/      # 控制器层（REST API）  
├── service/         # 服务层接口  
│   └── impl/        # 服务层实现  
├── mapper/          # 数据访问层接口  
├── entity/          # 实体类  
└── dto/             # 数据传输对象  

### 命名规范
- **Controller**: `XxxController.java`
- **Service**: `XxxService.java` / `XxxServiceImpl.java`
- **Mapper**: `XxxMapper.java` / `XxxMapper.xml`
- **Entity**: `Xxx.java`
- **DTO**: `XxxAddDTO.java`, `XxxUpdateDTO.java`, `XxxQueryDTO.java`

### 统一响应格式
所有接口返回统一的 `Result<T>` 格式：  

**{**  
  **"code": 200,**  
  **"message": "操作成功",**  
  **"data": { ... }**  
**}**

## 常见问题
### 数据库连接失败
- 检查 MySQL 服务是否启动
- 确认数据库名称、用户名、密码是否正确
- 检查时区配置：`serverTimezone=Asia/Shanghai`

### JWT 认证失败
- 确认请求头格式：`Authorization: Bearer {token}`
- 检查 Token 是否过期（默认 24 小时）
- 确认 JWT 密钥配置正确

### 跨域问题
系统已配置 CORS，如遇跨域问题，检查：
- 前端请求地址是否正确
- Security 配置中的 CORS 设置

### 文件上传失败
- 检查文件大小是否超过 5MB
- 确认上传目录权限
- 检查 `goods.upload.base-path` 配置

## 许可证
MIT  
本项目仅供学习使用。

## 联系方式
如有问题，联系项目维护者z_pw@163.com。