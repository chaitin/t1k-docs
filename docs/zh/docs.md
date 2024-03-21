---
hide:
  - navigation
---
# T1K 文档

长亭科技提供的 T1K 模块可以为任何基于 Nginx（包括 Nginx Plus）的代理服务增加 WAF / API 安全功能。

相比于基于 OpenResty 能力用 Lua 写的 WAF 模块，例如我们的 [Lua-Resty-T1K](https://github.com/chaitin/lua-resty-t1k)，C 语言编写的 T1K 模块拥有更高的性能，以及独家的响应检测能力。

## 请求检测

请求检测相关配置均以 `t1k_` 开头。

### t1k_intercept

<pre><code>Syntax:  <b>t1k_intercept</b> uri | off;
Default: t1k_intercept off;
Context: http, server, location</code></pre>

将请求发送到 `uri` 处进行检测，`uri` 是位于同 `server` 内的一个 `location`。

### t1k_error_page

<pre><code>Syntax:  <b>t1k_error_page</b> status_code uri;
Default: t1k_error_page 403 default;
Context: http, server, location</code></pre>

设置请求被拦截时的状态码与返回内容。

`status_code` 需满足 `200 <= status_code <= 599`。

`uri` 为同 `server` 内的一个 `(named) location`。

### t1k_pass

<pre><code>Syntax:  <b>t1k_pass</b> uri;
Default: -
Context: location</code></pre>

将请求发送到给定检测服务，`uri` 为同 `server` 内的一个 `(named) location`。

### t1k_bind

<pre><code>Syntax:  <b>t1k_bind</b> address [transparent] | off;
Default: -
Context: http, server, location</code></pre>

将请求发送到给定检测服务时，可以指定本地地址。

参考 `[proxy_bind](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind)`。

### t1k_body_size

<pre><code>Syntax:  <b>t1k_body_size</b> size;
Default: t1k_body_size 0;
Context: http, server, location</code></pre>

限制发送请求体的大小，0 为不限制大小。

### t1k_buffer_size

<pre><code>Syntax:  <b>t1k_buffer_size</b> size;
Default: -;
Context: http, server, location</code></pre>

修改从检测服务读取请求检测结果的缓存区大小，默认为系统页大小。

### t1k_connect_timeout

<pre><code>Syntax:  <b>t1k_connect_timeout</b> time;
Default: t1k_connect_timeout 10s;
Context: http, server, location</code></pre>

请求检测连接检测服务超时时间。

### t1k_send_timeout

<pre><code>Syntax:  <b>t1k_send_timeout</b> time;
Default: t1k_send_timeout 10s;
Context: http, server, location</code></pre>

请求检测单次发送数据到检测服务的超时时间。

### t1k_read_timeout

<pre><code>Syntax:  <b>t1k_read_timeout</b> time;
Default: t1k_read_timeout 10s;
Context: http, server, location</code></pre>

请求检测单次读取检测服务返回数据的超时时间。

### t1k_next_upstream

<pre><code>Syntax:  <b>t1k_next_upstream</b> error | timeout | invalid_response | off;
Default: t1k_next_upstream error timeout;
Context: http, server, location</code></pre>

请求检测失败行为，配置参考 `[proxy_next_upstream](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream)`。

## 响应检测

响应检测相关配置均以 `tx_` 开头。

### tx_intercept

<pre><code>Syntax:  <b>tx_intercept</b> uri | off;
Default: tx_intercept off;
Context: http, server, location</code></pre>

将响应发送到 `uri` 处进行检测，`uri` 是位于同 `server` 内的一个 `location`。

### tx_error_page

<pre><code>Syntax:  <b>tx_error_page</b> status_code uri;
Default: tx_error_page 403 default;
Context: http, server, location</code></pre>

设置响应被拦截时的状态码与返回内容。

`status_code` 需满足 `200 <= status_code <= 599`。

`uri` 为同 `server` 内的一个 `(named) location`。

### tx_pass

<pre><code>Syntax:  <b>tx_pass</b> uri;
Default: -
Context: location</code></pre>

将响应发送到给定检测服务，`uri` 为同 `server` 内的一个 `(named) location`。

### tx_bind

<pre><code>Syntax:  <b>tx_bind</b> address [transparent] | off;
Default: -
Context: http, server, location</code></pre>

将响应发送到给定检测服务时，可以指定本地地址。

参考 `[proxy_bind](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind)`。

### tx_body_size

<pre><code>Syntax:  <b>tx_body_size</b> size;
Default: tx_body_size 4k;
Context: http, server, location</code></pre>

限制发送响应体的大小，0 为不限制大小。

### tx_buffer_size

<pre><code>Syntax:  <b>tx_buffer_size</b> size;
Default: -;
Context: http, server, location</code></pre>

修改从检测服务读取响应检测结果的缓存区大小，默认为系统页大小。

### tx_connect_timeout

<pre><code>Syntax:  <b>tx_connect_timeout</b> time;
Default: tx_connect_timeout 10s;
Context: http, server, location</code></pre>

响应检测连接检测服务超时时间。

### tx_send_timeout

<pre><code>Syntax:  <b>tx_send_timeout</b> time;
Default: tx_send_timeout 10s;
Context: http, server, location</code></pre>

响应检测单次发送数据到检测服务的超时时间。

### tx_read_timeout

<pre><code>Syntax:  <b>tx_read_timeout</b> time;
Default: tx_read_timeout 10s;
Context: http, server, location</code></pre>

响应检测单次读取检测服务返回数据的超时时间。

### tx_next_upstream

<pre><code>Syntax:  <b>tx_next_upstream</b> error | timeout | invalid_response | off;
Default: tx_next_upstream error timeout;
Context: http, server, location</code></pre>

响应检测失败行为，配置参考 `[proxy_next_upstream](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream)`。

## 响应修改

### t1k_extra_header

<pre><code>Syntax:  <b>t1k_extra_header</b> [on|off];
Default: t1k_extra_header off;
Context: http, server, location</code></pre>

是否允许插入额外的请求头。

### t1k_extra_body

<pre><code>Syntax:  <b>t1k_extra_body</b> [on|off];
Default: t1k_extra_body off;
Context: http, server, location</code></pre>

是否允许插入额外的请求体。

### t1k_extra_body_types

<pre><code>Syntax:  <b>t1k_extra_body_types</b> { ... };
Default: t1k_extra_body_types text/html;
Context: http, server, location</code></pre>

允许插入额外的请求体的类型。

## 杂项

### t1k_ulog

<pre><code>Syntax:  <b>t1k_ulog</b> [number|off];
Default: t1k_ulog off;
Context: http, server, location</code></pre>

是否发送 Access Log 相关数据。

### t1k_stat

<pre><code>Syntax:  <b>t1k_stat</b> [number|off];
Default: t1k_stat off;
Context: http, server, location</code></pre>

是否发送检测模块性能统计数据。

### t1k_src_ip

<pre><code>Syntax:  <b>t1k_src_ip</b> value;
Default: -
Context: http, server, location</code></pre>

设置发送给检测服务的请求源 IP，`value` 可以为字符串或变量。

### t1k_src_port

<pre><code>Syntax:  <b>t1k_src_port</b> value;
Default: -
Context: http, server, location</code></pre>

设置发送给检测服务的请求源端口，`value` 可以为字符串或变量。

### t1k_dst_ip

<pre><code>Syntax:  <b>t1k_dst_ip</b> value;
Default: -
Context: http, server, location</code></pre>

设置发送给检测服务的请求目标 IP，`value` 可以为字符串或变量。

### t1k_dst_port

<pre><code>Syntax:  <b>t1k_dst_port</b> value;
Default: -
Context: http, server, location</code></pre>

设置发送给检测服务的请求目标端口，`value` 可以为字符串或变量。

### foreach_server

<pre><code>Syntax:  <b>foreach_server</b> { ... };
Default: -
Context: http</code></pre>

向每个**已出现**的 `server` 中插入指令。

**已出现**的 `server` 是指出现在该指令之前的、已经被解析的 `server` 配置。

因此一般该指令的使用是在所有要防护的server配置块的最后，并且保证该指令前面一定要有 `server` 配置。

推荐使用 `foreach_server_include`。

### foreach_server_include

<pre><code>Syntax:  <b>foreach_server_include</b> file;
Default: -
Context: http</code></pre>

向每个**已出现**的 `server` 中插入指令。

**已出现**的 `server` 是指出现在该指令之前的、已经被解析的 `server` 配置。
