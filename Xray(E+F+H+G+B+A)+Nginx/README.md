介绍：

利用 Nginx 支持 SNI 分流特性，对 VLESS+Vision+TLS、Trojan+RAW+TLS、HTTP/2 server、HTTP/3 server 进行 SNI 分流（四层转发），实现除 Xray 的 mKCP 应用外各应用共用 443 端口。其中 Nginx 同时为 VLESS+Vision+TLS 与 Trojan+RAW+TLS 提供 WEB 服务，为 Xray 的 SplitHTTP、gRPC、HTTPUpgrade 提供反向代理，其应用如下：

1、E=VLESS+Vision+TLS（回落配置，TLS 由自己启用及处理。）

2、F=Trojan+RAW+TLS（回落配置，TLS 由自己启用及处理。）

3、H=VLESS+SplitHTTP+TLS（反代配置，TLS 由 Trojan+RAW+TLS 或 Nginx 启用及处理。）

4、G=Shadowsocks+gRPC+TLS（反代配置，TLS 由 Trojan+RAW+TLS 或 Nginx 启用及处理。）

5、B=VMess+HTTPUpgrade+TLS（反代配置，TLS 由 Trojan+RAW+TLS 或 Nginx 启用及处理。）

6、A=VLESS+mKCP+seed

注意：

1、Nginx 支持 SNI 分流需要 Nginx 包含 stream_core_module 和 stream_ssl_preread_module 模块构建。

2、Xray 的监听地址不支持 Shadowsocks 协议使用 UDS 监听。

3、Xray 版本不小于 v1.8.16 才支持 SplitHTTP 传输方式。

4、Nginx 支持 H2C server、HTTP/2 server、HTTP/3 server 需要 Nginx 包含 http_ssl_module、http_v2_module、http_v3_module 模块构建。

5、Nginx 版本不小于 v1.25.1 才支持 H2C server 与 HTTP/1.1 server 共用一个端口或一个进程。若 Nginx 版本小于 v1.25.1，Xray 的 fallbacks 配置必须分成回落给 H2C server 与回落给 HTTP/1.1 server 分别对应 Nginx 的 H2C server 与 HTTP/1.1 server。

6、Nginx 版本不小于 v1.25.0 才支持 HTTP/3 server，且 http_ssl_module 所用 SSL 库必须支持 QUIC。

7、Nginx 支持对请求标头的 PROXY 协议处理需要 Nginx 包含 http_realip_module 模块构建。

8、不要使用 ACME 客户端在采用本示例的服务器上以 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书，因 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书需监听 80 或 443 端口，从而与当前应用端口冲突。

9、本示例 F 兼容原版 Trojan 应用，即可使用原版 Trojan 客户端连接；但原版 Trojan 客户端不支持指纹伪造，故不推荐使用。

10、配置1：使用 Local Loopback 连接，且启用了 PROXY protocol。配置2：使用 UDS 连接（对应 HTTP/3 server、Shadowsocks+gRPC+TLS 除外），且启用了 PROXY protocol。