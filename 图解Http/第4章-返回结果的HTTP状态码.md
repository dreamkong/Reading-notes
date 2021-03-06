# 第4章-返回结果的HTTP状态码

HTTP状态码负责表示客户端 HTTP 请求的返回结果，标记服务端的处理是否正常、通知出现的错误等工作。

<img src="http://ww3.sinaimg.cn/large/98900c07gw1fazqd2hvedj216209e0vr.jpg"/>


状态码以**3为数字和原因短语组成**，如 `200 OK`。

状态码的类别：  

|      | 类别                     | 原因短语          |
| ---- | ---------------------- | ------------- |
| 1XX  | Informational(信息性状态码)  | 接收的请求正在处理     |
| 2XX  | Success(成功状态码)         | 请求正常处理完毕      |
| 3XX  | Redirection(重定向状态码)    | 需要进行附加操作以完成请求 |
| 4XX  | Client Error(客户端错误状态码) | 服务器无法处理请求     |
| 5XX  | Server Error(服务器错误状态码) | 服务器处理请求出错     |


状态码数量达60余种，但是常用的大概就14种。

做客户端的，还是要稍微了解一下的，有个印象。在做一些需求，比如第三方授权时遇到 401等错误也不至于无从下手；遇到500错误也不会傻乎乎的在找自己的代码哪里写错了。

这里简单列举了一下(3XX 的太绕，就不多写了)：  

|            | 状态码                       | 含义                                       |
| :--------: | :------------------------ | ---------------------------------------- |
|   2XX 成功   |                           | 请求被正常处理了                                 |
|            | 200 OK                    | 从客户端发来的请求在服务端被**正常处理**了                  |
|            | 204 No Content            | 服务器接收的请求已处理成功，但在返回的响应报文中**不含实体的主体部分**    |
|            | 206 Partial Content       | 客户端进行了范围请求，服务器成功执行了这部分的 GET 请求(Content-Range) |
|  3XX 重定向   |                           |                                          |
|            | 301 Moved Permanently     | 永久性重定向                                   |
|            | 302 Found                 | 临时性重定向                                   |
|            | 303 See Other             |                                          |
|            | 304 Not Modified          |                                          |
|            | 307 Temporary Redirect    | 与302的差别是，不会从 POST 变成 GET                 |
| 4XX 客户端错误  |                           | 客户端是发生错误的原因所在                            |
|            | 400 Bad Request           | 请求报文中存在语法错误                              |
|            | 401 Unauthorized          | 需要有通过 HTTP 认证的认证信息                       |
|            | 404 Not Found             | 服务器上找不到资源                                |
| 5XX  服务器错误 |                           | 服务器本身发生错误                                |
|            | 500 Internal Server Error | 服务端在执行请求时发生了错误                           |
|            | 503 Service Unavailable   | 服务端暂时处于超负载或正在进行停机维护，现在无法处理请求。            |

