response.message

获取响应的状态消息. 默认情况下, response.message 与 response.status 关联.
response.message=

将响应的状态消息设置为给定值。
response.length=

将响应的 Content-Length 设置为给定值。
response.length

以数字返回响应的 Content-Length，或者从ctx.body推导出来，或者undefined。
response.body

获取响应主体。
response.body=

将响应体设置为以下之一：

    string 写入
    Buffer 写入
    Stream 管道
    Object || Array JSON-字符串化
    null 无内容响应
	Content-Type 默认为
String  text/html 或 text/plain, 同时默认字符集是 utf-8。Content-Length 字段也是如此。
Buffer  application/octet-stream, 并且 Content-Length 字段也是如此。
Stream  application/octet-stream。
Object  application/json. 这包括普通的对象 { foo: 'bar' } 和数组 ['foo', 'bar']。
response.get(field)         	不区分大小写获取响应标头字段值 field。	const etag = ctx.response.get('ETag');
response.set(field, value)	设置响应标头 field 到 value: 		ctx.set('Cache-Control', 'no-cache');
response.append(field, value)	用值 val 附加额外的标头 field。	     ctx.append('Link', '<http://127.0.0.1/>');
response.set(fields) 		用一个对象设置多个响应标头fields: 	   ctx.set({'Etag': '1234','Last-Modified': date});
response.remove(field)		删除标头 field。			
response.type			获取响应 Content-Type 不含参数 "charset"。	const ct = ctx.type; 	// => "image/png"
response.type=			设置响应 Content-Type 通过 mime 字符串或文件扩展名。	ctx.type = 'text/plain; charset=utf-8';
											ctx.type = 'image/png';
											ctx.type = '.png';
											ctx.type = 'png';

注意: 在适当的情况下为你选择 charset, 比如 response.type = 'html' 将默认是 "utf-8". 如果你想覆盖 charset, 使用 ctx.set('Content-Type', 'text/html') 将响应头字段设置为直接值。
response.is(types...)

非常类似 ctx.request.is(). 检查响应类型是否是所提供的类型之一。这对于创建操纵响应的中间件特别有用。

例如, 这是一个中间件，可以削减除流之外的所有HTML响应。

const minify = require('html-minifier');

app.use(async (ctx, next) => {
  await next();

  if (!ctx.response.is('html')) return;

  let body = ctx.body;
  if (!body || body.pipe) return;

  if (Buffer.isBuffer(body)) body = body.toString();
  ctx.body = minify(body);
});

response.redirect(url, [alt])

执行 [302] 重定向到 url.

字符串 “back” 是特别提供Referrer支持的，当Referrer不存在时，使用 alt 或“/”。

ctx.redirect('back');
ctx.redirect('back', '/index.html');
ctx.redirect('/login');
ctx.redirect('http://google.com');

要更改 “302” 的默认状态，只需在该调用之前或之后分配状态。要变更主体请在此调用之后:

ctx.status = 301;
ctx.redirect('/cart');
ctx.body = 'Redirecting to shopping cart';

response.attachment([filename])

将 Content-Disposition 设置为 “附件” 以指示客户端提示下载。(可选)指定下载的 filename。
response.headerSent

检查是否已经发送了一个响应头。 用于查看客户端是否可能会收到错误通知。
response.lastModified

将 Last-Modified 标头返回为 Date, 如果存在。
response.lastModified=

将 Last-Modified 标头设置为适当的 UTC 字符串。您可以将其设置为 Date 或日期字符串。

ctx.response.lastModified = new Date();

response.etag=

设置包含 " 包裹的 ETag 响应， 请注意，没有相应的 response.etag getter。

ctx.response.etag = crypto.createHash('md5').update(ctx.body).digest('hex');

response.vary(field)

在 field 上变化。
response.flushHeaders()

刷新任何设置的标头，并开始主体。
