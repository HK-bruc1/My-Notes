## DNS

域名可以与IP地址形成映射关系。域名与 IP 地址的映射关系通过 DNS（域名系统）来实现。用户在浏览器中输入域名（例如，example.com）时，浏览器需要将这个域名转换成对应的 IP 地址（例如，93.184.216.34），才能与服务器进行通信。这一转换过程就叫做 DNS 解析。

## DNS托管的必要性

域名注册商的 DNS 服务相较于 Cloudflare 可能会稍慢，这种差异在首次访问网站时会更明显地体现在浏览器输入域名后的等待时间上。为提升用户体验，尤其是对于流量较大或要求高性能的网站，将 DNS 托管给像 Cloudflare 这样的专业 DNS 提供商是一个值得考虑的优化措施。（主要是cloud flare还提供其他的配套服务）。

## 所谓托管

所谓的 DNS 托管，就是将域名的名称服务器（Nameserver）从域名注册商的默认服务器更改为 Cloudflare 提供的名称服务器。这样，Cloudflare 就会接管该域名的 DNS 解析，并提供相关的服务。

## Nginx作为反向代理角色

Nginx 在**分布式应用**中和**单服务器应用**中都能发挥重要作用。对于分布式应用，Nginx 的负载均衡、反向代理、服务分发、缓存、SSL/TLS 终止和高可用性功能尤为关键，能够显著提升系统的性能和可靠性。而在**单服务器应用**中，Nginx 仍然通过静态文件处理、SSL/TLS 终止、反向代理、缓存、URL 路由和请求限制等功能，提高应用的安全性、性能和管理便捷性。无论是分布式还是单服务器环境，Nginx 都是一个强大的工具，能够满足各种 Web 应用的需求。

### 反向代理使用前的情况

1. **直接访问后端服务**：在没有NGINX反向代理之前，您需要直接访问后端服务，这意味着需要知道并使用具体的端口号。例如：
   - 使用Postman测试接口时，需要输入类似`http://example.com:8080/api/endpoint`的URL。
   - 访问一个网页时，需要输入`http://example.com:8080/path/to/resource`。
2. **域名解析**：即使有域名，**域名只替代了IP部分**，但端口号依然需要显式指定，例如`http://example.com:8080/path/to/resource`。

### 使用后的情况

1. **引入NGINX反向代理**：通过在前端配置NGINX反向代理，**客户端请求可以不再显式指定后端服务的端口号**。NGINX会监听标准端口（80用于HTTP，443用于HTTPS），然后将这些请求代理到后端服务的具体端口（如8080）。
2. **隐藏端口号**：NGINX代理将客户端请求转发到后端服务时，客户端不再需要知道后端服务的端口号。客户端访问的URL会更加简洁。例如：
   - 使用Postman测试接口时，只需要输入`http://example.com/api/endpoint`或`https://example.com/api/endpoint`。
   - 访问一个网页时，只需要输入`http://example.com/path/to/resource`或`https://example.com/path/to/resource`。

## Nginx的服务器证书

NGINX证书在网络通信中起到了关键的作用，特别是在**加密通信和身份验证方面**。

**加密通信**：NGINX证书用于建立安全的加密连接，保护数据在传输过程中不被窃取或篡改。通过使用证书，NGINX可以实现HTTPS协议，确保客户端和服务器之间的通信是加密的，从而提高数据的安全性。

**身份验证**：证书可以用来验证服务器和客户端的身份。服务器证书（SSL/TLS证书）由受信任的证书颁发机构（CA）签发，证明服务器确实是它声称的那个。客户端可以通过验证服务器的证书来确保它们正在与合法的服务器通信，而不是恶意的第三方攻击者。

**信任链**：NGINX证书中包含了一个信任链，这是一个证明证书有效性的链条。它从服务器证书开始，逐级向上直到根证书颁发机构的证书。客户端通过验证信任链中的每一个证书来确认服务器证书的合法性。

**证书绑定**：证书可以绑定到特定的域名或IP地址，确保客户端连接的是预期的服务器。这有助于防止中间人攻击和其他类型的欺骗行为。

使用NGINX作为反向代理服务器时，通过SSL/TLS证书来确保与外界（例如，其他客户端或用户端）之间通信的安全性和合法性是一个常见的做法。具体来说，SSL/TLS证书可以用于以下两个方面：

1. 客户端与NGINX之间的通信安全性
   - 当客户端（如浏览器或其他服务器）向NGINX发送请求时，NGINX可以通过SSL/TLS证书加密通信，确保传输数据的机密性和完整性。客户端可以通过验证证书来确认NGINX服务器的合法性。
2. NGINX与后端真实服务器之间的通信安全性
   - NGINX作为反向代理服务器，可以将请求转发给后端的真实服务器。为了确保NGINX与后端服务器之间的通信也是安全的，通常也会在这段通信中使用SSL/TLS证书。

**NGINX与真实服务器的关系**：NGINX本身不生成数据内容，而是处理、优化和转发客户端的请求。因此，当我们说NGINX不是一个“真实的服务器”时，指的是它并不是内容的原始来源，而是一个中介或代理，为真实服务器提供服务。

NGINX本身并不是一个内容源服务器，而是一个代理服务器和Web服务器。它可以执行以下几种角色：

1. **反向代理服务器**：在这种角色下，NGINX接受客户端请求并将其转发到后端的真实服务器（如应用服务器或数据库服务器）。客户端认为自己直接在与NGINX通信，但实际上NGINX在中间充当代理，将请求转发并返回响应。这种配置有助于负载均衡、缓存和安全控制。
2. **Web服务器**：NGINX也可以直接作为Web服务器，处理静态内容的请求（如HTML、CSS、JavaScript、图像文件等），而无需将请求转发到后端服务器。
3. **负载均衡器**：NGINX可以将客户端请求分发到多个后端服务器，分担负载，提高性能和可靠性。
4. **邮件代理服务器**：NGINX还可以用作邮件代理服务器处理IMAP、POP3和SMTP协议的邮件请求。

## 关于http和https协议

nginx可以同时处理HTTP和HTTPS的请求，并且与客户端之间的通信协议由客户端请求的协议决定，而nginx与真实后端的通信通常为HTTP协议（我的springboot项目在配置文件中也是使用http协议。 在nginx配置文件中将请求代理到后端真实服务器也是用http协议`proxy_pass http://47.236.26.182:8080;`。

## 关于反向代理的后端服务器地址

如果nginx服务和后端程序不太同一台机器上时。反向代理时需要通过公网ip进行通信。（分布式应用）

如果nginx服务和后端程序在同一台机器上。反向代理时就不需要通过公网ip了。直接使用本地回环地址进行通信。（使用公网ip测试没有成功。。。同一台机器还用ip通信？出去又进来。）

# 前后端分离的前台web和后天web的项目

## 疑问

那么前台web的地址https://www.example.com/和后台web的地址：https://www.example.com/admin 都前后端分离的话，前台web有两个端口，一个运行前端，一个运行后端。 后台web有两个端口，一个运行前端，一个运行后端。 是怎么整合到一个域名下的？是不是可以用nginx代理。 当访问https://www.example.com/时转发到前台的前端端口和后端端口？ 当访问https://www.example.com/admin时转发到后台的前端端口和后端端口？ 一个域名只能解析到一个ip地址下吗？ 是不是说明前台和后台运行的四个端口都在一台机器上运行？

## 解答

你理解得完全正确。前台和后台的前后端服务可以运行在不同的端口上，然后通过 Nginx 代理统一到一个域名下，从而提供给用户一个统一的访问入口。

### 整合到一个域名下的方式

1. **前台 Web**：
   - 前端（例如 React/Vue/Angular 应用）运行在一个端口上，如 `http://localhost:3000`。
   - 后端（例如 Node.js/Java/Spring 应用）运行在另一个端口上，如 `http://localhost:8080`。
2. **后台管理 Web**：
   - 前端（例如 React/Vue/Angular 应用）运行在一个端口上，如 `http://localhost:3001`。
   - 后端（例如 Node.js/Java/Spring 应用）运行在另一个端口上，如 `http://localhost:8081`。
3. 通过 Nginx 代理将不同路径的请求转发到不同的服务端口上

```yaml
server {
    listen 80;
    server_name www.example.com;

    # 前台静态文件
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # 前台后端 API
    location /api/ {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # 后台管理静态文件
    location /admin/ {
        proxy_pass http://localhost:3001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # 后台管理后端 API
    location /admin/api/ {
        proxy_pass http://localhost:8081;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### 配置解释

- **前台**：
  - 当用户访问 `https://www.example.com/` 时，Nginx 将请求代理到前台前端 `http://localhost:3000`。
  - 当用户访问 `https://www.example.com/api/` 时，Nginx 将请求代理到前台后端 `http://localhost:8080`。
- **后台管理**：
  - 当用户访问 `https://www.example.com/admin` 时，Nginx 将请求代理到后台前端 `http://localhost:3001`。
  - 当用户访问 `https://www.example.com/admin/api/` 时，Nginx 将请求代理到后台后端 `http://localhost:8081`。

### 域名解析

- 一个域名通常解析到一个 IP 地址，但是通过 Nginx 或其他反向代理服务器，可以将来自该域名的不同路径的请求转发到不同的内部端口。
- 因此，前台和后台的前后端服务都可以在同一台机器上运行，并通过不同的端口提供服务。

### 总结

通过这种方式，尽管前台和后台的前后端分别运行在不同的端口上，用户依然可以通过统一的域名进行访问，Nginx 会根据请求的路径将其转发到相应的服务端口上。这不仅方便了用户访问，也简化了服务的部署和管理。

## 反问

但实际上一台机器不可能同时运行前后和后台的前后端项目，因为人流量大的时候机器负载不够。又是怎么解决的？

## 解答

在实际生产环境中，为了解决单台机器负载不够的问题，通常会采用分布式部署和负载均衡等技术。如果将四个服务分别运行再不同的机器上。还能实现同一域名下访问吗？ 域名只能解析到一台ip地址的服务器上啊

- 可以将多个服务部署在不同的机器上，并通过负载均衡和反向代理来实现同一域名下的访问。虽然一个域名通常解析到一个 IP 地址，但通过使用负载均衡器（如 Nginx、HAProxy 或云提供商的负载均衡服务），可以将请求分发到不同的服务器上。

- 解析到一个IP地址的问题：虽然域名解析只能解析到一个 IP 地址，但这个 IP 地址可以指向**负载均衡器**。负载均衡器会根据配置将请求分发到不同的服务器上。
- 用户通过同一个域名访问时，实际访问的首先是负载均衡器（或反向代理服务器）。负载均衡器根据预先配置的转发规则，将请求分发到不同的服务器和端口上。这种方式在用户看来，依然是通过同一个域名在访问服务，保持了用户体验的一致性。
