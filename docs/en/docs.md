---
hide:
  - navigation
---
# T1K Documentation

The T1K module provided by Chaitin Technology can add WAF / API security features to any proxy service based on NGINX (including NGINX Plus).

Compared to the WAF modules written in Lua based on OpenResty's capabilities, such as our [Lua-Resty-T1K](https://github.com/chaitin/lua-resty-t1k), the T1K module written in C language boasts higher performance and exclusive response detection capabilities.

## Request Detection

All configurations related to request detection are prefixed with `t1k_`.

### t1k_intercept

<pre><code>Syntax:  <b>t1k_intercept</b> uri | off;
Default: t1k_intercept off;
Context: http, server, location</code></pre>

Sends the request to `uri` for detection, where `uri` is a `location` within the same `server`.

### t1k_error_page

<pre><code>Syntax:  <b>t1k_error_page</b> status_code uri;
Default: t1k_error_page 403 default;
Context: http, server, location</code></pre>

Sets the status code and content when a request is intercepted.

`status_code` must satisfy `200 <= status_code <= 599`.

`uri` is a `(named) location` within the same `server`.

### t1k_pass

<pre><code>Syntax:  <b>t1k_pass</b> uri;
Default: -
Context: location</code></pre>

Sends the request to the given detection service. `uri` is a `(named) location` within the same `server`.

### t1k_bind

<pre><code>Syntax:  <b>t1k_bind</b> address [transparent] | off;
Default: -
Context: http, server, location</code></pre>

Allows specifying a local address when sending a request to a detection service.

Refer to `[proxy_bind](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind)`.

### t1k_body_size

<pre><code>Syntax:  <b>t1k_body_size</b> size;
Default: t1k_body_size 0;
Context: http, server, location</code></pre>

Limits the size of the request body sent, where 0 means no limit.

### t1k_buffer_size

<pre><code>Syntax:  <b>t1k_buffer_size</b> size;
Default: -;
Context: http, server, location</code></pre>

Modifies the buffer size for reading detection results from the detection service, defaulting to the system page size.

### t1k_connect_timeout

<pre><code>Syntax:  <b>t1k_connect_timeout</b> time;
Default: t1k_connect_timeout 10s;
Context: http, server, location</code></pre>

The timeout for connecting to the detection service for request detection.

### t1k_send_timeout

<pre><code>Syntax:  <b>t1k_send_timeout</b> time;
Default: t1k_send_timeout 10s;
Context: http, server, location</code></pre>

The timeout for sending data to the detection service per request detection.

### t1k_read_timeout

<pre><code>Syntax:  <b>t1k_read_timeout</b> time;
Default: t1k_read_timeout 10s;
Context: http, server, location</code></pre>

The timeout for reading data from the detection service per request detection.

### t1k_next_upstream

<pre><code>Syntax:  <b>t1k_next_upstream</b> error | timeout | invalid_response | off;
Default: t1k_next_upstream error timeout;
Context: http, server, location</code></pre>

Behavior on request detection failure.

Refer to `[proxy_next_upstream](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream)`.

## Response Detection

All configurations related to response detection are prefixed with `tx_`.

### tx_intercept

<pre><code>Syntax:  <b>tx_intercept</b> uri | off;
Default: tx_intercept off;
Context: http, server, location</code></pre>

Sends the response to `uri` for detection, where `uri` is a `location` within the same `server`.

### tx_error_page

<pre><code>Syntax:  <b>tx_error_page</b> status_code uri;
Default: tx_error_page 403 default;
Context: http, server, location</code></pre>

Sets the status code and content when a response is intercepted.

`status_code` must satisfy `200 <= status_code <= 599`.

`uri` is a `(named) location` within the same `server`.

### tx_pass

<pre><code>Syntax:  <b>tx_pass</b> uri;
Default: -
Context: location</code></pre>

Sends the response to the given detection service. `uri` is a `(named) location` within the same `server`.

### tx_bind

<pre><code>Syntax:  <b>tx_bind</b> address [transparent] | off;
Default: -
Context: http, server, location</code></pre>

Allows specifying a local address when sending a response to a detection service.

Refer to `[proxy_bind](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind)`.

### tx_body_size

<pre><code>Syntax:  <b>tx_body_size</b> size;
Default: tx_body_size 4k;
Context: http, server, location</code></pre>

Limits the size of the response body sent, where 0 means no limit.

### tx_buffer_size

<pre><code>Syntax:  <b>tx_buffer_size</b> size;
Default: -;
Context: http, server, location</code></pre>

Modifies the buffer size for reading detection results from the detection service, defaulting to the system page size.

### tx_connect_timeout

<pre><code>Syntax:  <b>tx_connect_timeout</b> time;
Default: tx_connect_timeout 10s;
Context: http, server, location</code></pre>

The timeout for connecting to the detection service for response detection.

### tx_send_timeout

<pre><code>Syntax:  <b>tx_send_timeout</b> time;
Default: tx_send_timeout 10s;
Context: http, server, location</code></pre>

The timeout for sending data to the detection service per response detection.

### tx_read_timeout

<pre><code>Syntax:  <b>tx_read_timeout</b> time;
Default: tx_read_timeout 10s;
Context: http, server, location</code></pre>

The timeout for reading data from the detection service per response detection.

### tx_next_upstream

<pre><code>Syntax:  <b>tx_next_upstream</b> error | timeout | invalid_response | off;
Default: tx_next_upstream error timeout;
Context: http, server, location</code></pre>

Behavior on response detection failure.

Refer to `[proxy_next_upstream](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream)`.

## Response Modification

### t1k_extra_header

<pre><code>Syntax:  <b>t1k_extra_header</b> [on|off];
Default: t1k_extra_header off;
Context: http, server, location</code></pre>

Determines whether additional request headers are allowed to be inserted.

### t1k_extra_body

<pre><code>Syntax:  <b>t1k_extra_body</b> [on|off];
Default: t1k_extra_body off;
Context: http, server, location</code></pre>

Determines whether additional request bodies are allowed to be inserted.

### t1k_extra_body_types

<pre><code>Syntax:  <b>t1k_extra_body_types</b> { ... };
Default: t1k_extra_body_types text/html;
Context: http, server, location</code></pre>

Specifies the types of request bodies that are allowed for additional content insertion.

## Miscellaneous

### t1k_ulog

<pre><code>Syntax:  <b>t1k_ulog</b> [number|off];
Default: t1k_ulog off;
Context: http, server, location</code></pre>

Determines whether to send Access Log related data.

### t1k_stat

<pre><code>Syntax:  <b>t1k_stat</b> [number|off];
Default: t1k_stat off;
Context: http, server, location</code></pre>

Determines whether to send detection module performance statistics data.

### t1k_src_ip

<pre><code>Syntax:  <b>t1k_src_ip</b> value;
Default: -
Context: http, server, location</code></pre>

Sets the source IP address for requests sent to the detection service.

`value` can be a string or variable.

### t1k_src_port

<pre><code>Syntax:  <b>t1k_src_port</b> value;
Default: -
Context: http, server, location</code></pre>

Sets the source port for requests sent to the detection service.

`value` can be a string or variable.

### t1k_dst_ip

<pre><code>Syntax:  <b>t1k_dst_ip</b> value;
Default: -
Context: http, server, location</code></pre>

Sets the destination IP address for requests sent to the detection service.

`value` can be a string or variable.

### t1k_dst_port

<pre><code>Syntax:  <b>t1k_dst_port</b> value;
Default: -
Context: http, server, location</code></pre>

Sets the destination port for requests sent to the detection service.

`value` can be a string or variable.

### foreach_server

<pre><code>Syntax:  <b>foreach_server</b> { ... };
Default: -
Context: http</code></pre>

Inserts directives into each **already appeared** `server`.

**Already appeared** `servers` refer to those `server` configurations that have appeared before this directive and have been parsed.

Therefore, this directive is typically used at the end of all the server configuration blocks that require protection, ensuring that there are `server` configurations before this directive.

It is recommended to use `foreach_server_include` instead.

### foreach_server_include

<pre><code>Syntax:  <b>foreach_server_include</b> file;
Default: -
Context: http</code></pre>

Inserts directives into each **already appeared** `server`.

**Already appeared** `servers` refer to those `server` configurations that have appeared before this directive and have been parsed.
