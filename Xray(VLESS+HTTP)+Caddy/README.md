介绍：

利用 Caddy 支持 HTTP/3 转换为 H2C、支持 H2C 反向代理实现 Xray 的 VLESS+HTTP+TLS 应用，TLS 由 Caddy 提供及处理。

原理：

默认流程：WEB client <--------------- HTTP/3或HTTP/2 ---------------> Caddy（WEB server）  
反代流程：Xray client <--------------- HTTP/3或HTTP/2 ---------------> Caddy <-- H2C --> Xray server

注意：

1、Xray 版本不小于 v24.9.30 才支持 HTTP（升级前 HTTP/2） 传输层的 h3 连接，即支持 HTTP/3 传输。

2、Caddy 版本不小于 v2.9.0 才支持 HTTP/3 转换为 H2C 进行反向代理。

3、本示例 Caddy 支持自动 HTTPS，即自动申请与更新 TLS 证书，自动 HTTP 重定向到 HTTPS。

4、本示例支持 HTTP/3 传输。若想要由 Caddy 处理的 HTTP/3 应用高速传输，建议[增加服务端系统的 UDP 缓冲区大小](https://github.com/quic-go/quic-go/wiki/UDP-Buffer-Sizes)。

5、配置1：使用 Local Loopback 连接。配置2：使用 UDS 连接。
