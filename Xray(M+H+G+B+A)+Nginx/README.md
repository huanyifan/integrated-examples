介绍：

Xray 前置（监听 443 端口），利用 VLESS+Vision+REALITY 支持转发给自己的网站及 Nginx 支持 XHTTP、gRPC、HTTPUpgrade 反向代理，实现除 Xray 的 mKCP 应用外各应用共用 443 端口，其应用如下：

1、M=VLESS+Vision+REALITY（转发配置，REALITY 所需 TLS 由 Nginx 提供。）

2、H=VLESS+XHTTP+TLS（反代配置，TLS 由 Nginx 启用及处理。）

3、G=Shadowsocks+gRPC+TLS（反代配置，TLS 由 Nginx 启用及处理。）

4、B=VMess+HTTPUpgrade+TLS（反代配置，TLS 由 Nginx 启用及处理。）

5、A=VLESS+mKCP+seed

注意：

1、Xray 的监听地址不支持 Shadowsocks 协议使用 UDS 监听。

2、Xray 的 Shadowsocks 2022 新协议提升了性能并带有完整的重放保护。

3、Xray 版本不小于 v1.8.16 才支持 XHTTP（原 SplitHTTP） 传输方式。

4、Nginx 支持 HTTP/2 server、HTTP/3 server 需要 Nginx 包含 http_ssl_module、http_v2_module、http_v3_module 模块构建。

5、Nginx 版本不小于 v1.25.0 才支持 HTTP/3 server，且 http_ssl_module 所用 SSL 库必须支持 QUIC。

6、Nginx 支持对请求标头的 PROXY 协议处理需要 Nginx 包含 http_realip_module 模块构建。

7、不要使用 ACME 客户端在采用本示例的服务器上以 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书，因 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书需监听 80 或 443 端口，从而与当前应用端口冲突。

8、配置1：使用 Local Loopback 连接，且启用了 PROXY protocol。配置2：使用 UDS 连接（对应 HTTP/3 server、Shadowsocks+gRPC+TLS 除外），且启用了 PROXY protocol。