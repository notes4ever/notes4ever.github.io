## secure和httponly属性

`Set-Cookie: sessionid=123456789; Secure; HttpOnly`

httpOnly 是 Cookie 中的一个属性，它的主要作用是提高 web 应用程序的安全性。

当**设置了 HttpOnly 属性的 Cookie 会在服务器端创建，但不会被 JavaScript 访问**。

这意味着即使攻击者在网页中注入了恶意脚本，这些脚本也无法读取带有 HttpOnly 属性的 Cookie，从而有效防止了跨站脚本攻击（XSS）中的 Cookie 泄露问题。

在实际开发中，通常建议对所有涉及身份验证或敏感信息的 Cookie 设置 HttpOnly 属性，以增强安全性。

但是，需要注意的是，即使设置了 HttpOnly 属性，也不应将重要信息存储在 Cookie 中，因为**它并不能完全防止所有的攻击，如 CSRF 攻击等**。

设置了Secure，sessionid Cookie值将**只会在 HTTPS 连接中被发送**。

需要注意的是，如果网站没有使用 HTTPS，那么 Secure 属性将不会生效，因为 HTTP 连接下不会发送带有 Secure 属性的 Cookie。

因此，为了充分利用 Secure 属性的优势，建议网站启用 HTTPS。

总结来说，Secure 属性是增强 Cookie 安全性的重要手段，通过确保 Cookie 仅在加密的 HTTPS 连接中传输，有助于保护用户数据免受中间人攻击。

此外，如果浏览器或服务器端支持，还可以将 Secure 属性与 HttpOnly 属性结合使用，以提供额外的安全保护。HttpOnly 属性可以防止客户端脚本访问 Cookie，从而减少跨站脚本攻击（XSS）的风险。

## 内容安全策略CSP
内容安全策略（Content Security Policy，简称 CSP）是一种额外的安全层，用于检测并削弱某些特定类型的攻击，包括跨站脚本（XSS）和数据注入攻击等 。

CSP 的主要目标是减少和报告 XSS 攻击，通过指定有效域——即浏览器认可的可执行脚本的有效来源——使服务器管理者有能力减少或消除 XSS 攻击所依赖的载体 。

CSP 的工作原理是，通过在**服务器响应头中**添加 **Content-Security-Policy** HTTP 标头，或者在 HTML 中使用 <meta> 标签来定义策略。这些策略可以控制和限制网页可以加载的资源类型，比如脚本、样式表、图片等 。CSP 策略由一系列指令组成，每个指令都描述了针对某个特定资源类型的策略 。

例如，如果你想限制网页只能加载同源的脚本和样式，并且所有图片都只能从特定域名加载，可以设置如下 CSP 策略 ：

`Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self'; img-src example.com
`

这个策略的含义是：

- default-src 'self'：默认情况下，只允许加载同源资源。
- script-src 'self'：脚本资源只能加载自同源。
- style-src 'self'：样式表资源也只能加载自同源。
- img-src example.com：图片资源可以加载自 example.com。

CSP 还支持报告机制，即 report-uri 指令，用于定义接收违反 CSP 策略报告的 URL，以便开发者及时了解和处理安全问题。

## 超级cookie
超级cookie是一种强大的跟踪技术，它能够绕过传统的隐私保护措施，对用户的网络活动进行跟踪；这种种特殊的Cookie，能够在浏览器的隐私模式下仍然保留用户信息。

它通过在用户首次访问时生成一个随机的24位整数作为指纹，并在后续访问中通过HSTS头请求多个URL来实现。

超级Cookie的主要功能是确保用户在下次访问时自动使用HTTPS通道，即使在隐私模式下也能记住用户的访问记录。

这种Cookie可以被第三方网络程序利用，例如广告和社交媒体按钮，以进行用户追踪。

Mozilla已经在最新版Firefox中修复了这一问题，而Chrome仍然保留了HTTPS记忆功能。