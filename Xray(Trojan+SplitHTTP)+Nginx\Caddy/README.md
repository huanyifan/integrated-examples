介绍：

利用 Nginx 或 Caddy 支持 SplitHTTP 反向代理实现 Xray 的 Trojan+SplitHTTP+TLS 应用，TLS 由 Nginx 启用及处理或由 Caddy 提供及处理。SplitHTTP 使用 HTTP 分块传输编码流式响应处理下载、使用多个 HTTP POST 请求进行上传，可解决 CDN 不支持 WebSocket/HTTPUpgrade、gRPC 问题。

原理：

默认流程：WEB client <-------- HTTP/3或HTTP/2 --------> Nginx/Caddy（WEB server）  
反代流程：Xray client <---------- SplitHTTP+TLS ---------> Nginx/Caddy <-- SplitHTTP --> Xray server

注意：

1、Xray 版本不小于 v1.8.16 才支持 SplitHTTP 传输方式。

2、Nginx 支持 HTTP/2 server、HTTP/3 server 需要 Nginx 包含 http_ssl_module、http_v2_module、http_v3_module 模块构建。

3、Nginx 版本不小于 v1.25.0 才支持 HTTP/3 server，且 http_ssl_module 所用 SSL 库必须支持 QUIC。

4、若选用 Nginx 实现应用，不要使用 ACME 客户端在采用本示例的服务器上以 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书，因 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书需监听 80 或 443 端口，从而与当前应用端口冲突。

5、若选用 Caddy 实现应用，本示例 Caddy 支持自动 HTTPS，即自动申请与更新 TLS 证书，自动 HTTP 重定向到 HTTPS。

6、配置1：使用 Local Loopback 连接。配置2：使用 UDS 连接。
