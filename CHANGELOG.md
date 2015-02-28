# io.js 更新记录

## 2015-02-26, Version 1.4.1, @rvagg

_Note: version **1.4.0** was tagged and built but not released. A libuv bug was discovered in the process so the release was aborted. The tag was straight after [`a558cd0a61`](https://github.com/iojs/io.js/commit/a558cd0a61) but has since been removed. We have jumped to 1.4.1 to avoid confusion._

### 主要更新

* **process** / **promises**: An`'unhandledRejection'` event is now emitted on `process` whenever a `Promise` is rejected and no error handler is attached to the `Promise` within a turn of the event loop. A `'rejectionHandled'` event is now emitted whenever a `Promise` was rejected and an error handler was attached to it later than after an event loop turn. See the [process](https://iojs.org/api/process.html) documentation for more detail. [#758](https://github.com/iojs/io.js/pull/758) (Petka Antonov)
* **streams**: you can now use regular streams as an underlying socket for `tls.connect()` [#926](https://github.com/iojs/io.js/pull/926) (Fedor Indutny)
* **http**: A new `'abort'` event emitted when a `http.ClientRequest` is aborted by the client. [#945](https://github.com/iojs/io.js/pull/945) (Evan Lucas)
* **V8**: Upgrade V8 to 4.1.0.21. Includes an embargoed fix, details should be available at https://code.google.com/p/chromium/issues/detail?id=430201 when embargo is lifted. A breaking ABI change has been held back from this upgrade, possibly to be included when io.js merges V8 4.2. See [#952](https://github.com/iojs/io.js/pull/952) for discussion.
* **npm**: Upgrade npm to 2.6.0. Includes features to support the new registry and to prepare for `npm@3`. See [npm CHANGELOG.md](https://github.com/npm/npm/blob/master/CHANGELOG.md#v260-2015-02-12) for details. Summary:
  * [`38c4825`](https://github.com/npm/npm/commit/38c48254d3d217b4babf5027cb39492be4052fc2) [#5068](https://github.com/npm/npm/issues/5068) Add new logout command, and make it do something useful on both bearer-based and basic-based authed clients. ([@othiym23](https://github.com/othiym23))
  * [`c8e08e6`](https://github.com/npm/npm/commit/c8e08e6d91f4016c80f572aac5a2080df0f78098) [#6565](https://github.com/npm/npm/issues/6565) Warn that `peerDependency` behavior is changing and add a note to the docs. ([@othiym23](https://github.com/othiym23))
  * [`7c81a5f`](https://github.com/npm/npm/commit/7c81a5f5f058941f635a92f22641ea68e79b60db) [#7171](https://github.com/npm/npm/issues/7171) Warn that `engineStrict` in `package.json` will be going away in the next major version of npm (coming soon!) ([@othiym23](https://github.com/othiym23))
* **libuv**: Upgrade to 1.4.2. See [libuv ChangeLog](https://github.com/libuv/libuv/blob/v1.x/ChangeLog) for details of fixes.

### 已知问题

* Surrogate pair in REPL can freeze terminal [#690](https://github.com/iojs/io.js/issues/690)
* Not possible to build io.js as a static library [#686](https://github.com/iojs/io.js/issues/686)
* `process.send()` is not synchronous as the docs suggest, a regression introduced in 1.0.2, see [#760](https://github.com/iojs/io.js/issues/760) and fix in [#774](https://github.com/iojs/io.js/issues/774)
* Calling `dns.setServers()` while a DNS query is in progress can cause the process to crash on a failed assertion [#894](https://github.com/iojs/io.js/issues/894)

## [2015-02-20，版本 1.3.0，@rvagg](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md#2015-02-20-version-130-rvagg)

### 主要更新

* **url**: `url.resolve('/path/to/file', '.')` 现在的返回值会以斜杠结尾（如 `/path/to/`），`url.resolve('/', '.')` 会返回 `/`。[#278](https://github.com/iojs/io.js/issues/278) (Amir Saboury)
* **tls**: `tls` 和 `https` 所使用的默认密码套件被修改成所有现代浏览器已经实现的 `Perfect Forward Secrecy`。除此以外，去除了不安全的 RC4 密码，如果你直接使用了 RC4，请指定你自己的密码套件。 [#826](https://github.com/iojs/io.js/issues/826) (Roman Reiss)

### 已知问题

* REPL 中的代理对会冻结终端 [#690](https://github.com/iojs/io.js/issues/690)
* 无法将 io.js 编译成静态库 [#686](https://github.com/iojs/io.js/issues/686)
* `process.send()` 并非如文档所述是同步的，1.0.2 引入的问题，查看 [#760](https://github.com/iojs/io.js/issues/760)，解决 [#774](https://github.com/iojs/io.js/issues/774)
* 当 DNS 查询正在进行中时调用 `dns.setServers()` 会造成 process 崩溃，原因是断言错误 [#894](https://github.com/iojs/io.js/issues/894)

## [2015-02-10，版本 1.2.0，@rvagg](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md#2015-02-10-version-120-rvagg)

### 主要更新

* **stream**：
  - 简化 stream 构造方法，详情参看 [readable-stream/issues#102](https://github.com/iojs/readable-stream/issues/102)。并且扩展了streams 的基础对象，来让他们的构造方法接受默认的实现方法，减少创建自定义stream 所需的模板(通用)代码。一个更新版的 readable-stream 最终也会发布来匹配 core 的该变化。(@sonewman)
* **dns**：
  - `lookup()` 增加支持 `'all'` 布尔选项，默认为 `false`，如果设为 true，方法会返回一个 *all* 地址解析过的域名数组，参看[#744](https://github.com/iojs/io.js/pull/744) (@silverwind)
* **assert**：
  - `deepEqual()` 方法不再对比 `prototype` 属性，这被认为是 bug，参看[#636](https://github.com/iojs/io.js/pull/636) (@vkurchatkin)
  - 添加方法 `deepStrictEqual()` 功能同 `deepEqual()` 类似，只是在[原型](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)比较时进行严格相等比较，参看[#639](https://github.com/iojs/io.js/pull/639) (@vkurchatkin)
* **tracing**：
  - 如果编译时开启 `--with-lttng` 选项，则会同时编译 [LTTng](http://lttng.org/) (Linux 下一代 Trace 工具)。Trace 分数同 DTrace 和 ETW 相匹配。[#702](https://github.com/iojs/io.js/pull/702) (@thekemkid)
* **docs**：
  - 大量文档更新，具体查看相应提交
  - 新 **Errors** 文档描述了 JavaScript 错误，V8 特定的 和 io.js 特定的错误详情。(@chrisdickinson)
* **npm** 更新到 2.5.1, 简要更新日志：
  - [npm/0e8d473](https://github.com/npm/npm/commit/0e8d4736a1cbdda41ae8eba8a02c7ff7ce80c2ff) [#7281](https://github.com/npm/npm/issues/7281) `npm-registry-mock@1.0.0`: 清理 API，设置 `connection: close`，从而使测试在 io.js 1.1.x 通过。
  ([@robertkowalski](https://github.com/robertkowalski))
  - [npm/f9313a0](https://github.com/npm/npm/commit/f9313a066c9889a0ee898d8a35676e40b8101e7f)
  [#7226](https://github.com/npm/npm/issues/7226) 确保所有的请求设置被复制到 agent 中。
  ([@othiym23](https://github.com/othiym23))
   - [npm/fec4c96](https://github.com/npm/npm/commit/fec4c967ee235030bf31393e8605e9e2811f4a39)
  在环境中允许 `--no-proxy` 覆盖 `HTTP_PROXY` 设置。
  ([@othiym23](https://github.com/othiym23))
  - [npm/9d61e96](https://github.com/npm/npm/commit/9d61e96fb1f48687a85c211e4e0cd44c7f95a38e)
  `npm outdated --long` 添加一列显示依赖的类型。
  ([@watilde](https://github.com/watilde))
* **libuv** 升级到 1.4.0，详情参看 [libuv 更新日志](https://github.com/libuv/libuv/blob/v1.x/ChangeLog)
* 添加贡献者：
  - Aleksey Smolenchuk (@lxe)
  - Shigeki Ohtsu (@shigeki)

### 已知问题

* REPL 中的 Surrogate pair 会导致终端僵死 [#690](https://github.com/iojs/io.js/issues/690)
* io.js 无法作为静态库编译 [#686](https://github.com/iojs/io.js/issues/686)
* `process.send()` 行为并非如文档所说的同步，该问题是在 1.0.2 引入的，参看 [#760](https://github.com/iojs/io.js/issues/760) 和 修复 [#774](https://github.com/iojs/io.js/issues/774) 将会在下个版本发布。

## [2015-02-03, 版本 1.1.0, @chrisdickinson](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md#2015-02-03-version-110-chrisdickinson)

### 主要更新

* debug: 修复 v8 post-mortem 调试 bug.
* crypto: publicEncrypt 开始支持受密码保护的私钥.
* crypto: 哈希方法提速 ~30%.
* crypto: 新增 privateEncrypt/publicDecrypt 方法.
* errors
  - 优化 util.inspect 格式化结果
  - fs 抛出的错误, 新增更详细的描述. 这需要一个 `NODE_MODULE_VERSION` bump.
  - http.setHeader 抛出的错误, 新增更详细的描述
* 依赖更新:
  - npm: 更新到 2.4.1
  - http-parser: 回滚到 2.3.0
  - libuv: 更新到 1.3.0
  - v8: 更新到 4.1.0.14
* http.request: 从选项中继承属性
* buffers 新增了可迭代接口 (`for (let byte of buffer.values()) { }`)
* fs: 修复 `fs.createReadStream` 的 fd 泄露. 详情参看 497fd72.
* installer: Windows 平台完成安装后, 触发 WM_SETTINGCHANGE 事件,
    用以让其他运行中进程知晓 PATH 的变化
* 新增贡献者:
  - Vladimir Kurchatkin (@vkurchatkin)
  - Micleușanu Nicu (@micnic)

### 已知问题

* REPL 中的 Surrogate pair 会导致终端僵死 (https://github.com/iojs/io.js/issues/690)
* io.js 无法作为静态库编译 (https://github.com/iojs/io.js/issues/686)

## [2015-01-24, 版本 1.0.4, @rvagg](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md#2015-01-24-version-104-rvagg)

### 主要更新

* npm 更新到 2.3.0 修复 Windows "uid is undefined" 错误
* crypto.pseudoRandomBytes() 现在是 crypto.randomBytes() 的别名方法,
  如果没有足够的 entropy 用以产生安全的值则会阻塞. 详情参看 https://github.com/iojs/io.js/commit/e5e5980.
* 给 V8 打补丁, 用于检测 ARMv6; 使程序又能在 ARMv6 上运行 (Raspberry Pi 等.)
* V8 小幅更新 4.1.0.7 to 4.1.0.12
* 'punycode' 核心模块状态, 从不稳定变为稳定
* 新增贡献者:
  - Thorsten Lorenz (@thlorenz)
  - Stephen Belanger (@qard)
  - Jeremiah Senkpiel (@fishrock123)
  - Evan Lucas (@evanlucas)
  - Brendan Ashworth (@brendanashworth)

## [2015-01-20, 版本 1.0.3, @rvagg](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md#2015-01-20-version-103-rvagg)

### 主要更新

* V8 从 3.31 升级到 4.1, 这并不是一个大版本升级, 版本号 "4.1" 表示
 与 Chrome 41 保持同步. 3.31 分支 没有与稳定版本保持同步.
* 重新支持 Windows XP / 2003
* npm 更新 2.2.0
* 提高 FreeBSD 支持

### 已知问题

* ARMv6 平台仍然无法编译, V8 的 this 有一个停顿(hold-up), 问题 #283
* Template strings 在 V8 4.1 会导致segfaults, https://codereview.chromium.org/857433004, 问题 #333

## [2015-01-16, 版本 1.0.2, @rvagg](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md#2015-01-16-version-102-rvagg)

### 主要更新

* Windows 安装包问题修复
* windows 安装包的 node-gyp 问题修复
* [http_parser v2.4.1 升级](https://github.com/joyent/http-parser/compare/v2.3...v2.4.1)
* [libuv v1.2.1 升级](https://github.com/libuv/libuv/compare/v1.2.0...v1.2.1)

## [2015-01-14, 版本 1.0.1, @rvagg](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md#2015-01-14-version-101-rvagg)

因为 1.0.0 版的旧的编译 slave git reflogs 重新编译

* doc: 提高书写一致性 (Rui Marinho)
* win,msi: 修复文档网址链接 (Bert Belder)

--------------------------------------

以下是当前_稳定版_的 Node.js(v0.10.35) 到 io.js(v1.0.0) 的变更概要。v1.0.0 发布时 Node.js 最新的_非稳定版_为 v0.11.14，并正在准备发布 v0.11.15。io.js 的代码是源于 [joyent/node](https://github.com/joyent/node) v0.11 分支，包括其中的大部分变更，因此可视为 v0.11 的扩充。

## [Node.js v0.10.35 到 io.js v1.0.0 的变更概要](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md#summary-of-changes-from-nodejs-v01035-to-iojs-v100)

### General

- io.js 内置的 V8 引擎升级幅度很大，从 Node.js v0.10.35 的 3.14.5.9 和 Node.js v0.11.14 升级到 io.js v1.0.0 的 3.31.74.1。他修复了很多 bug 且提升了性能，而且额外增加了新的 ES6 特性。获取更多信息请查看 [io.js ES6 页面](https://iojs.org/es6.html)
- 其他内置的模块也相应升级了：
  - c-ares: 1.9.0-DEV 升级到 1.10.0-DEV
  - http_parser: 1.0 升级到 2.3
  - libuv: 0.10.30 升级到 1.2.0
  - npm: 1.4.28 升级到 2.1.18
  - openssl: 1.0.1j 升级到 1.0.1k
  - punycode: 1.2.0 升级到 1.3.2.
- 所有平台优化了性能和稳定性

### buffer

https://iojs.org/api/buffer.html

- 新增 `buf.writeUIntLE`, `buf.writeUIntBE`, `buf.writeIntLE`, `buf.writeIntBE`, `buf.readUIntLE`, `buf.readUIntBE`, `buf.readIntLE` and `buf.readIntBE` 方法，支持6个字节读写。
- 新增 `Buffer.compare()` 方法，使用 `memcmp()` 对比两个 Buffer 实例的内存区域，实例也有一个 `compare()` 方法。
- 新增 `buffer.equals()` 方法，对比 Buffer 的内容是否一致。
- 新增 `new Buffer(otherBuffer)` 构造函数。
- 调整 `SlowBuffer` 的语义
- 更新 `buffer.toJSON()` 的输出，不再输出数组而是输出一个对象。此对象可被标记为一个 buffer 并且可以传入到 Buffer 构建函数中重新实例化。

### child_process

https://iojs.org/api/child_process.html

- `child_process.exec` 新增了一个 `shell` 的参数。
- 新增一系列同步的方法：`child_process.spawnSync`, `child_process.execSync`, and `child_process.execFileSync`。
- 给 `ENOENT` 错误新增了路径，方便调试。

### console

https://iojs.org/api/console.html

- `console.dir` 新增了一个 `options` 参数。

### cluster

https://iojs.org/api/cluster.html

- 更新了 cluster 模块，在非 Windows 平台默认使用 round-robin 负载均衡算法，该项可以配置。
- 支持了 `--debug` 调试
- 修复了很多 bug

### crypto

https://iojs.org/api/crypto.html

- Added support for custom generator values to `DiffieHellman` (defaulting to 2 for backwards compatibility).
- Added support for custom pbkdf2 digest methods.
- Added support for elliptic curve-based Diffie-Hellman.
- Added support for loading and setting the engine for some/all OpenSSL functions.
- Added support for passing in a passphrase for decrypting the signing key to `Sign.sign()`.
- Added support for private key passphrase in every method that accepts it.
- Added support for RSA public/private encryption/decryption functionality.
- Added support for setting and getting of authentication tags and setting additional authentication data when using ciphers such as AES-GCM.

### dgram

https://iojs.org/api/dgram.html

- 新增支持接受 UDP 空包。

### dns

https://iojs.org/api/dns.html

- 新增 `dns.resolveSoa`, `dns.getServers`, and `dns.setServers` 方法。
- 在错误信息上添加 `hostname`
- 优化错误处理的一致性

### events

https://iojs.org/api/events.html

- 新增 `EventEmitter.setMaxListeners` 的链式调用
- `require('events')` 直接返回 `EventEmitter` 构造函数，可直接使用这个模块，如 `var EventEmitter = require('events')` 而不是 `var EventEmitter = require('events').EventEmitter`。

### fs

https://iojs.org/api/fs.html

- 新增 `fs.access`，弃用 `fs.exists`，请仔细阅读文档。
- NODE_DEBUG 的值非空时显示更多的错误信息以及方法调用的具体位置来优化调试。
- 新增 `fs.watch` 的参数支持子目录递归（只支持 OS X）。
- 修复 callback 不存在会异常的情况，只打印错误。

### http

https://iojs.org/api/http.html

- 新增支持 `response.write` 和 `response.end` 接受一个 callback，当操作完成后被执行。
- 新增 308 状态码的支持（详见 RFC 7238）。
- 新增 `http.METHODS` 数组，列出所有解析器支持的 HTTP 方法。
- 新增 `request.flush` 方法。
- 新增 `response.getHeader('header')` 方法，可以在发送响应头之前调用。
- 新增 `response.statusMessage` 属性。
- 新增客户端 Keep-Alive 行为，在 request 的 options 设置 `keepAlive:true` 可无限重用连接。
- 在 incoming message 新增 `rawHeaders` 和 `rawTrailers` 属性。
- 删除 `DELETE` 和 `OPTIONS` 默认的分块编码。

### net

https://iojs.org/api/net.html

- `net.Server.listen` 变更，当绑定的地址被忽略后，先尝试 IPv6，如果失败再尝试 IPv4。

### os

https://iojs.org/api/os.html

- `os.networkInterfaces` 方法新增 IPv6 的 MAC 地址，网络掩码和范围 ID。
- 更新 Windows 平台的 `os.tmpdir`，当定义临时目录的时候，使用 `%SystemRoot%` 或 `%WINDIR%` 环境变量，而不是硬编码的 `c:\windows`。

### path

https://iojs.org/api/path.html

- 增加了 `path.isAbsolute` 和 `path.parse` 方法。
- 增加了 `path.win32` 和 `path.posix` 对象，他们分别包含了对应平台的 `path` 的实现。
- 提升了 `path.join` 的性能。

### process

https://iojs.org/api/process.html

- 增加了 `beforeExit` 事件。
- 增加了 `process.mainModule` 和 `process.exitCode`。

### querystring

https://iojs.org/api/querystring.html

- stringifying 或者 parsing 一个 query string 的时候可传入自定义的 `encodeURIComponent` 和 `decodeURIComponent` 方法。
- 修复了几个格式化 query string 边缘场景下的问题。

### smalloc

https://iojs.org/api/smalloc.html

`smalloc` 是一个新的核心模块， 用于在 javascript 中 分配、释放和复制(外部的)内存空间。

### streams

https://iojs.org/api/stream.html

streams 的变化没有从 stream1 到 stream2 那样剧烈：它们是对现有想法的改进，让 API 更加友好，同时运行速度更快。所有的这些变更也称为 "stream3"，但是这些变更对大多数使用者来说是透明的，大都不需要太关心。

#### Readable streams

The distinction between "flowing" and "non-flowing" modes has been refined. Entering "flowing" mode is
no longer an irreversible operation&mdash;it is possible to return to "non-flowing" mode from "flowing" mode.
Additionally, the two modes now flow through the same machinery instead of replacing methods. Any time
data is returned as a result of a `.read` call that data will *also* be emitted on the `"data"` event.

As before, adding a listener for the `"readable"` or `"data"` event will start flowing the stream; as
will piping to another stream.

#### Writable streams

The ability to "bulk write" to underlying resources has been added to `Writable` streams. For stream
implementers, one can signal that a stream is bulk-writable by specifying a [_writev](https://iojs.org/api/stream.html#stream_writable_writev_chunks_callback) method.
Bulk writes will occur in two situations:

1. When a bulk-writable stream is clearing its backlog of buffered write requests,
2. or if an end user has made use of the new `.cork()` and `.uncork()` API methods.

`.cork` and `.uncork` allow the end user to control the buffering behavior of writable streams separate
from exerting backpressure. `.cork` indicates that the stream should accept new writes (up to `highWaterMark`),
while `.uncork` resets that behavior and attempts to bulk-write all buffered writes to the underlying resource.

The only core stream API that **currently** implements `_writev` is `net.Socket`.

In addition to the bulk-write changes, the performance of repeated small writes to non-bulk-writable streams
(such as `fs.WriteStream`) has been drastically improved. Users piping high volume log streams to disk should
see an improvement.

For a detailed overview of how streams3 interact, [see this diagram](https://cloud.githubusercontent.com/assets/37303/5728694/f9a3e300-9b20-11e4-9e14-a6938b3327f0.png).

### timers

https://iojs.org/api/timers.html

- 移除 `process.maxTickDepth`, 允许 `process.nextTick` 被无限迭代调用
- 更新 `setImmediate` 允许在一次事件循环处理完整队列, 而不是每次一个.

### tls

https://iojs.org/api/tls.html

- 新增 `detailed` 布尔标志到 `getPeerCertificate` 来返回详细的认证信息 (带有原始 DER 字节).
- 新增 `renegotiate(options, callback)` 方法用于回话 renegotiation.
- 新增 `setMaxSendFragment` 方法用以调整 TLS 碎片大小.
- 新增 `dhparam` 选项 到 DH 密码.
- 新增 `ticketKeys` 选项到 TLS ticket AES 加密钥匙设置.
- 新增异步 OCSP-stapling 回调.
- 新增异步会话存储事件
- 新增异步 SNI 回调.
- 新增多 key 服务器支持 (例如, ECDSA+RSA 服务器).
- 新增可选回调到 `checkServerIdentity` 来让用户手动认证验证
- 新增对 ECDSA/ECDHE 密码支持.
- 用 C++ 实现 TLS 流, 从而提升性能.
- 把 `createCredentials` 移到 `tls` 并重命名为 `createSecureContext`.
- 移除对 SSLv2 和 SSLv3 的支持.

### url

https://iojs.org/api/url.html

- 优化特殊字符的转义.
- 提升解析速度

### util

https://iojs.org/api/util.html

- 新增 `util.debuglog`.
- 新增多个类型检测方法. 查看[文档](https://iojs.org/api/util.html).
- 对 `util.format` 进行了几处更新:
  - `-0` 原样打印, 而不是 `0`.
  - 所有 error 实例 `instanceof Error` 都会被格式化成 error.
  - JavaScript 中的循环引用, 现在被处理为 `%j` 说明符. (TO REVIEW)
  - 允许自定义 `inspect` 方法返回一个对象.
  - 自定义 `inspect` 方法现在会接受所有传给 `util.inspect` 的参数.

## v8

https://iojs.org/api/v8.html

`v8` 是一个全新的核心模块, 用于直接同 V8 引擎交互.

### vm

https://iojs.org/api/vm.html

`vm` 被基于卓越的[Contextify](https://github.com/brianmcd/contextify) native 模块完全重写, 从而工作的更好,
Contextify 所有的功能被加入到了核心模块中, 并进行了优化.

- 新增 `vm.isContext(object)` 方法来确定一个`object` 是否被 contextified.
- 新增 `vm.runInDebugContext(code)` 方法用以在 V8 调试环境编译和执行 `code`.
- 更新 `vm.createContext(sandbox)` 来 "contextify" 沙盒, 使它能够作为 `vm` 脚本的全局环境, 然后返回它. 它不会创建单独的 context 对象
- 更新 `vm` 和 `vm.Script` 的大部分方法, 能接受一个 `options` 对象, 允许用户为脚本自定义 timeout, 错误的展示方式, 以及文件名 (用户栈追踪).
- 修改提供的沙盒对象, 从而可直接用做全局环境, 去除提供的沙盒对象和通过运行 `vm` 模块出现在脚本内部的全局对象之间的错误试探性属性拷贝
关于详细信息, 参看上面的文档链接

### zlib

https://iojs.org/api/zlib.html

- 新增支持 `zlib.flush` 指定特殊的 flush 方法 (默认为 `Z_FULL_FLUSH`).
- 新增支持 `zlib.params` 来在压缩时动态更新压缩级别和策略.
- 新增同步版本的 zlib 方法.

### C++ API Changes

https://iojs.org/api/addons.html

通常情况下, 建议使用 [NAN](https://github.com/rvagg/nan) 作为 addons 的兼容层, 未来他也会帮助处理 V8, 以及 Node/io.js C++ API 的变化.
下面的大部分修改 NAN 都有对应的 wrapper 进行处理.

#### V8 highlights

- Exposed method signature has changed from `Handle<Value> Method(const Arguments& args)` to `void Method(const v8::FunctionCallbackInfo<Value>& args)` with the newly introduced `FunctionCallbackInfo` also taking the return value via `args.GetReturnValue().Set(value)` instead of `scope.Close(value)`, `Arguments` has been removed.
- Exposed setter signature has changed from `void Setter(Local<String> property, Local<Value> value, const v8::AccessorInfo& args)` `void Setter(Local<String> property, Local<Value> value, const v8::PropertyCallbackInfo<void>& args)`.
- Exposed getter signature has changed from `void Getter(Local<String> property, Local<Value> value, const v8::AccessorInfo& args)` `void Setter(Local<String> property, Local<Value> value, const v8::PropertyCallbackInfo<Value>& args)`.
- Exposed property setter signature has changed from `Handle<Value> Setter(Local<String> property, Local<Value> value, const v8::AccessorInfo& args)` `void Setter(Local<String> property, Local<Value> value, const v8::PropertyCallbackInfo<Value>& args)`.
- Exposed property getter signature has changed from `Handle<Value> Getter(Local<String> property, Local<Value> value, const v8::AccessorInfo& args)` `void Setter(Local<String> property, Local<Value> value, const v8::PropertyCallbackInfo<Value>& args)`.
- Similar changes have been made to property enumerators, property deleters, property query, index getter, index setter, index enumerator, index deleter, index query.
- V8 objects instantiated in C++ now require an `Isolate*` argument as the first argument. In most cases it is OK to simply pass `v8::Isolate::GetCurrent()`, e.g. `Date::New(Isolate::GetCurrent(), time)`, or `String::NewFromUtf8(Isolate::GetCurrent(), "foobar")`.
- `HandleScope scope` now requires an `Isolate*` argument, i.e. `HandleScope scope(isolate)`, in most cases `v8::Isolate::GetCurrent()` is OK.
- Similar changes have been made to `Locker` and `Unlocker`.
- V8 objects that need to "escape" a scope should be enclosed in a `EscapableHandleScope` rather than a `HandleScope` and should be returned with `scope.Escape(value)`.
- Exceptions are now thrown from isolates with `isolate->ThrowException(ExceptionObject)`.
- `Context::GetCurrent()` must now be done on an isolate, e.g. `Isolate::GetCurrent()->GetCurrentContext()`.
- `String::NewSymbol()` has been removed, use plain strings instead.
- `String::New()` has been removed, use `String::NewFromUtf8()` instead.
- `Persistent` objects no longer inherit from `Handle` and cannot be instantiated with another object. Instead, the `Persistent` should simply be declared, e.g. `Persistent<Type> handle` and then have a `Local` assigned to it with `handle.Reset(isolate, value)`. To get a `Local` from a `Persistent` you must instantiate it as the argument, i.e. `Local::New(Isolate*, Persistent)`.

#### Node / io.js

- 更新 `node::Buffer::New()` 来直接返回一个 `Handle`, 这样就没有必要再去获取 `handle_` 属性.
- 更新 `node::MakeCallback()` 它需要 `Isolate*` 作为第一个参数. 通常情况 `Isolate::GetCurrent()` 就已经够用.

--------------------------------------

**[之前的更新记录从 joyent/node 项目继承到 io.js 项目中。](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md#20140924-version-01114-unstable)**
