# OIDC 协议详解

OIDC 是 OpenID Connect 的简称，它是一个建立在 OAuth 2.0 基础上的身份认证标准协议。可以将 OIDC 理解为：身份认证 + 授权 + OAuth 2.0。

**OIDC 的核心优势在于：它在 OAuth2 的基础上增加了一层身份认证功能。**

我们知道 OAuth2 主要是一个授权协议，它本身并不提供完善的身份认证功能。OIDC 巧妙地利用 OAuth2 的授权服务器，为第三方客户端提供用户的身份认证，并将相应的身份信息传递给客户端。

OIDC 的 versatility（多功能性）体现在：

- 适用于各种类型的客户端（如服务端应用、移动 APP、JS 应用等）
- 完全兼容 OAuth2（这意味着你搭建的 OIDC 服务也可以当作 OAuth2 服务使用）

下图展示了 OIDC 的典型应用场景：

![OIDC应用场景](https://thinking-oss.oss-cn-beijing.aliyuncs.com/img/2021/10/202110101111139.png)

## OIDC 协议族：一个完整的生态系统

OIDC 不是单一的协议，而是由多个规范组成的协议族。这些规范包括一个核心规范和多个可选的扩展规范：

- **Core**：这是 OIDC 的核心，也是唯一的必选规范。它定义了 OIDC 的基本功能，包括如何在 OAuth 2.0 之上构建身份认证，以及如何使用 Claims 传递用户信息。

- **Discovery**：这个可选规范允许客户端动态获取 OIDC 服务的元数据描述信息，比如支持哪些规范、接口地址等。

- **Dynamic Registration**：另一个可选规范，使客户端能够动态注册到 OIDC 的 OpenID Provider (OP)。

- **OAuth 2.0 Multiple Response Types**：这是对 OAuth2 的扩展，提供了几个新的 response_type。

- **OAuth 2.0 Form Post Response Mode**：同样是对 OAuth2 的扩展，提供了一种基于表单的方式将数据 POST 给客户端。

- **Session Management**：规定了 OIDC 服务如何管理会话信息。

- **Front-Channel Logout**：定义了一种基于前端的注销机制。

- **Back-Channel Logout**：定义了 Relying Party (RP) 和 OP 之间如何通过后端通信完成注销。

下图展示了 OIDC 的组成结构：

![OIDC组成结构](https://thinking-oss.oss-cn-beijing.aliyuncs.com/img/2021/10/202110101111003.png)

值得注意的是，OIDC 并非全新的技术，而是巧妙地融合了多种现有技术：

- 借鉴了 OpenID 的身份标识概念
- 采用了 OAuth2 的授权流程
- 使用 JWT 格式封装数据

## OIDC 的核心概念

OIDC 在 OAuth2 的基础上做了两个关键的扩展：

- OAuth2 提供了 **Access Token** 来解决第三方客户端访问受保护资源的授权问题
- OIDC 增加了 **ID Token** 来解决第三方客户端识别用户身份的认证问题

OIDC 的核心创新在于：在 OAuth2 的授权流程中，同时提供用户的身份认证信息（ID Token）给第三方客户端。ID Token 采用 JWT 格式封装，具有以下优势：

- 自包含性：token 本身包含了所有必要信息
- 紧凑性：数据结构紧凑，便于传输
- 防篡改机制：确保数据的完整性和真实性

这些特性使得 ID Token 可以安全地传递给第三方客户端并且易于验证。

此外，OIDC 还提供了 **UserInfo** 接口，用于获取用户的更详细信息。

### OIDC 中的关键术语

为了更好地理解 OIDC，我们需要熟悉以下术语：

- **EU (End User)**：最终用户，即人类用户。
- **RP (Relying Party)**：相当于 OAuth2 中的受信任客户端，是身份认证和授权信息的消费方。
- **OP (OpenID Provider)**：能够为 EU 提供认证的服务，为 RP 提供 EU 的身份认证信息。
- **ID Token**：采用 JWT 格式的数据，包含 EU 的身份认证信息。
- **UserInfo Endpoint**：一个受 OAuth2 保护的用户信息接口。当 RP 使用 Access Token 访问时，返回授权用户的信息。该接口必须使用 HTTPS 协议。

### OIDC 的工作流程

OIDC 的工作流程可以概括为以下五个步骤：

1. RP 向 OP 发送一个认证请求。
2. OP 对 EU 进行身份认证，并获取授权。
3. OP 将 ID Token 和 Access Token（如果需要）返回给 RP。
4. RP 使用 Access Token 向 UserInfo Endpoint 发送请求。
5. UserInfo Endpoint 返回 EU 的 Claims（用户信息）。

下图直观地展示了这个流程：

![OIDC工作流程](https://thinking-oss.oss-cn-beijing.aliyuncs.com/img/2021/10/202110101111497.jpg)

注：图中的 AuthN 代表 Authentication（认证），AuthZ 代表 Authorization（授权）。

### 深入理解 ID Token

ID Token 是 OIDC 对 OAuth2 最重要的扩展。它是一个安全令牌，采用 JWT 格式，包含用户信息，由授权服务器提供。

一个典型的 ID Token 包含以下字段：

- **iss (Issuer Identifier)**：必填。标识提供认证信息的服务提供商，通常是一个 HTTPS URL。
- **sub (Subject Identifier)**：必填。标识用户的唯一标识，在 iss 范围内唯一。
- **aud (Audience)**：必填。标识 ID Token 的接收者，必须包含 OAuth2 的 client_id。
- **exp (Expiration time)**：必填。过期时间。
- **iat (Issued At Time)**：必填。JWT 的构建时间。
- **auth_time (Authentication Time)**：用户完成认证的时间。
- **nonce**：RP 提供的随机字符串，用于防止重放攻击。
- **acr (Authentication Context Class Reference)**：可选。用于标识认证上下文类。
- **amr (Authentication Methods References)**：可选。表示一组认证方法。
- **azp (Authorized party)**：可选。通常与 aud 一起使用，只在被认证方与受众不一致时使用。

### Access Token 与 ID Token 的区别

虽然两者都是 token，但它们的用途不同：

- **Access Token** 用于授权第三方客户端访问受保护的资源。
- **ID Token** 用于第三方客户端识别用户的身份。

### OIDC 的认证流程

OIDC 支持三种主要的认证流程：

- **Authorization Code Flow**：使用 OAuth2 的授权码来交换 ID Token 和 Access Token。
- **Implicit Flow**：使用 OAuth2 的隐式流程直接获取 ID Token 和 Access Token。
- **Hybrid Flow**：混合使用 Authorization Code Flow 和 Implicit Flow。

每种流程都有其适用场景，开发者可以根据具体需求选择合适的流程。

通过以上详细介绍，我们可以看到 OIDC 是如何在 OAuth2 的基础上，通过增加身份层来提供更完善的身份认证和授权解决方案的。它的灵活性和安全性使其成为现代身份认证的重要标准。
