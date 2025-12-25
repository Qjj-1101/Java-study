# Realworld 后端开发学习笔记 - 2025/12/23

## 1. 项目架构重构 (Architecture)

项目从平铺结构进化为标准三层架构：

- **Controller**: 处理 HTTP 请求，负责 JSON 数据的输入输出。
- **Service**: 处理核心逻辑，如 `register` 和 `login`。
- **Repository**: 使用 Spring Data JPA 操作数据库。
- **Entity**: 使用 `@Entity` 注解定义数据库表映射。

## 2. 安全管理 (Security)

- **密码处理**: 使用 `BCrypt` 算法。
- **核心逻辑**: 注册时加密存储，登录时使用 `passwordEncoder.matches()` 验证。
- **数据一致性**: 确保 `UserRepository` 导入的是自定义的 `User` 类，而非 Tomcat 自带类。

## 3. JWT 身份认证 (Authentication)

- **JwtService**: 使用 `jjwt` 库生成 Token。
- **Token 内容**: 包含用户 Email 和过期时间。
- **返回格式**: 严格遵守 Realworld 规范，将用户信息和 Token 封装在 `user` 对象中返回。

## 4. 调试经验总结

- **404 错误**: 通常是因为类缺少 `@RestController` 注解或包扫描路径错误。
- **500 错误**: 需要查看日志。常见原因包括用户不存在（H2 重启导致数据清空）或 JWT 密钥配置错误。
- **自动初始化**: 使用 `@PostConstruct` 在启动时预存测试数据，极大提高开发调试效率。

## 5. 明日目标 (Next Steps)

- 构建 `Article` (文章) 模块。
- 实现 `ArgumentResolver`，自动从 Header 的 Token 中解析出当前用户。
- 挑战“发布文章”接口。
