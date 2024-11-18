介绍：

Xray 前置（监听 443 端口），利用 VLESS+Vision+REALITY 回落 VLESS+XHTTP（套娃），实现 VLESS+Vision+REALITY 与 VLESS+XHTTP+REALITY 应用共用 443 端口，其应用如下：

1、M=VLESS+Vision+REALITY（回落与转发配置，REALITY 所需 TLS 由外部网站提供。）

2、K=VLESS+XHTTP+REALITY（回落配置，REALITY 由 VLESS+Vision+REALITY 启用及处理。）

注意：

1、Xray 版本不小于 v24.10.31，其 XHTTP 才正式支持 REALITY。

2、若目标网站使用外部的，其网站最低要求：国外网站、域名非跳转、支持 TLSv1.3 与 HTTP/2，是否符合要求可用 [SSL Server Test](https://www.ssllabs.com/ssltest/) 检查。

3、若目标网站使用自己的，其配置修改及网站配置可参考 [Xray(M+H+G+B+A)+Nginx](https://github.com/lxhao61/integrated-examples/tree/main/Xray(M%2BH%2BG%2BB%2BA)%2BNginx) 或 [Xray(M+H+D+G+B+A)+Caddy(N+T)](https://github.com/lxhao61/integrated-examples/tree/main/Xray(M%2BH%2BD%2BG%2BB%2BA)%2BCaddy(N%2BT)) 示例。

4、配置1：使用 Local Loopback 连接。配置2：使用 UDS 连接。
