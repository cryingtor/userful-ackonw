# nuclei
# Nuclei 命令行参数详解

## 用法
`nuclei [flags]`

---

## 标志 (Flags)

### 目标 (TARGET)
| 标志 | 描述 |
| :--- | :--- |
| `-u`, `-target` | 要扫描的目标 URL/主机（可指定多个） |
| `-l`, `-list` | 包含要扫描的目标 URL/主机列表的文件路径（每行一个） |
| `-eh`, `-exclude-hosts` | 从输入列表中排除要扫描的主机（IP、CIDR、主机名） |
| `-resume` | 使用 `resume.cfg` 恢复扫描（将禁用集群功能） |
| `-sa`, `-scan-all-ips` | 扫描与 DNS 记录关联的所有 IP |
| `-iv`, `-ip-version` | 要扫描的主机名的 IP 版本（4, 6）-（默认为 4） |

### 目标格式 (TARGET-FORMAT)
| 标志 | 描述 |
| :--- | :--- |
| `-im`, `-input-mode` | 输入文件的模式（list, burp, jsonl, yaml, openapi, swagger）（默认为 "list"） |
| `-ro`, `-required-only` | 生成请求时仅使用输入格式中的必填字段 |
| `-sfv`, `-skip-format-validation` | 解析输入文件时跳过格式验证（如缺少变量） |

### 模板 (TEMPLATES)
| 标志 | 描述 |
| :--- | :--- |
| `-nt`, `-new-templates` | 仅运行最新 `nuclei-templates` 版本中添加的新模板 |
| `-ntv`, `-new-templates-version` | 运行特定版本中添加的新模板 |
| `-as`, `-automatic-scan` | 使用 Wappalyzer 技术检测到标签映射进行自动 Web 扫描 |
| `-t`, `-templates` | 要运行的模板或模板目录列表（逗号分隔，文件） |
| `-turl`, `-template-url` | 要运行的模板 URL 或包含模板 URL 的列表（逗号分隔，文件） |
| `-ai`, `-prompt` | 使用 AI 提示生成并运行模板 |
| `-w`, `-workflows` | 要运行的工作流或工作流目录列表（逗号分隔，文件） |
| `-wurl`, `-workflow-url` | 要运行的工作流 URL 或包含工作流 URL 的列表（逗号分隔，文件） |
| `-validate` | 验证传递给 Nuclei 的模板 |
| `-nss`, `-no-strict-syntax` | 禁用对模板的严格语法检查 |
| `-td`, `-template-display` | 显示模板内容 |
| `-tl` | 列出所有可用的模板 |
| `-tgl` | 列出所有可用的标签 |
| `-sign` | 使用 `NUCLEI_SIGNATURE_PRIVATE_KEY` 环境变量中定义的私钥对模板进行签名 |
| `-code` | 启用加载基于代码协议的模板 |
| `-dut`, `-disable-unsigned-templates` | 禁用运行未签名或签名不匹配的模板 |
| `-esc`, `-enable-self-contained` | 启用加载自包含模板 |
| `-egm`, `-enable-global-matchers` | 启用加载全局匹配器模板 |
| `-file` | 启用加载文件模板 |

### 过滤 (FILTERING)
| 标志 | 描述 |
| :--- | :--- |
| `-a`, `-author` | 基于作者运行模板（逗号分隔，文件） |
| `-tags` | 基于标签运行模板（逗号分隔，文件） |
| `-etags`, `-exclude-tags` | 基于标签排除模板（逗号分隔，文件） |
| `-itags`, `-include-tags` | 要执行的标签，即使它们被默认或配置排除 |
| `-id`, `-template-id` | 基于模板 ID 运行模板（逗号分隔，文件，允许通配符） |
| `-eid`, `-exclude-id` | 基于模板 ID 排除模板（逗号分隔，文件） |
| `-it`, `-include-templates` | 要执行的模板文件或目录的路径，即使它们被默认或配置排除 |
| `-et`, `-exclude-templates` | 要排除的模板文件或目录的路径（逗号分隔，文件） |
| `-em`, `-exclude-matchers` | 要在结果中排除的模板匹配器 |
| `-s`, `-severity` | 基于严重性运行模板。可能的值：info, low, medium, high, critical, unknown |
| `-es`, `-exclude-severity` | 基于严重性排除模板。可能的值：info, low, medium, high, critical, unknown |
| `-pt`, `-type` | 基于协议类型运行模板。可能的值：dns, file, http, headless, tcp, workflow, ssl, websocket, whois, code, javascript |
| `-ept`, `-exclude-type` | 基于协议类型排除模板。可能的值：dns, file, http, headless, tcp, workflow, ssl, websocket, whois, code, javascript |
| `-tc`, `-template-condition` | 基于表达式条件运行模板 |

### 输出 (OUTPUT)
| 标志 | 描述 |
| :--- | :--- |
| `-o`, `-output` | 写入发现的问题/漏洞的输出文件 |
| `-sresp`, `-store-resp` | 将所有通过 Nuclei 的请求/响应存储到输出目录 |
| `-srd`, `-store-resp-dir` | 将所有通过 Nuclei 的请求/响应存储到自定义目录（默认为 "output"） |
| `-silent` | 仅显示发现结果 |
| `-nc`, `-no-color` | 禁用输出内容着色（ANSI 转义码） |
| `-j`, `-jsonl` | 以 JSONL(ines) 格式写入输出 |
| `-irr`, `-include-rr`, `-omit-raw` | 在 JSON、JSONL 和 Markdown 输出中包含请求/响应对（仅用于发现结果）[已弃用，使用 `-omit-raw`]（默认为 true） |
| `-or`, `-omit-raw` | 在 JSON、JSONL 和 Markdown 输出中省略请求/响应对（仅用于发现结果） |
| `-ot`, `-omit-template` | 在 JSON、JSONL 输出中省略编码的模板 |
| `-nm`, `-no-meta` | 在 CLI 输出中禁用打印结果元数据 |
| `-ts`, `-timestamp` | 在 CLI 输出中启用时间戳打印 |
| `-rdb`, `-report-db` | Nuclei 报告数据库（始终使用它来持久化报告数据） |
| `-ms`, `-matcher-status` | 显示匹配失败状态 |
| `-me`, `-markdown-export` | 以 markdown 格式导出结果的目录 |
| `-se`, `-sarif-export` | 以 SARIF 格式导出结果的文件 |
| `-je`, `-json-export` | 以 JSON 格式导出结果的文件 |
| `-jle`, `-jsonl-export` | 以 JSONL(ine) 格式导出结果的文件 |
| `-rd`, `-redact` | 从查询参数、请求头和正文中编辑给定的键列表 |

### 配置 (CONFIGURATIONS)
| 标志 | 描述 |
| :--- | :--- |
| `-config` | Nuclei 配置文件的路径 |
| `-tp`, `-profile` | 要运行的模板配置文件 |
| `-tpl`, `-profile-list` | 列出社区模板配置文件 |
| `-fr`, `-follow-redirects` | 为 HTTP 模板启用跟随重定向 |
| `-fhr`, `-follow-host-redirects` | 跟随同一主机上的重定向 |
| `-mr`, `-max-redirects` | 为 HTTP 模板跟随的最大重定向次数（默认为 10） |
| `-dr`, `-disable-redirects` | 为 HTTP 模板禁用重定向 |
| `-rc`, `-report-config` | Nuclei 报告模块配置文件 |
| `-H`, `-header` | 在所有 HTTP 请求中包含的自定义头/ Cookie，格式为 `header:value`（CLI，文件） |
| `-V`, `-var` | 自定义变量，格式为 `key=value` |
| `-r`, `-resolvers` | 包含 Nuclei 解析器列表的文件 |
| `-sr`, `-system-resolvers` | 使用系统 DNS 解析作为错误回退 |
| `-dc`, `-disable-clustering` | 禁用请求的集群 |
| `-passive` | 启用被动 HTTP 响应处理模式 |
| `-fh2`, `-force-http2` | 在请求上强制使用 HTTP/2 连接 |
| `-ev`, `-env-vars` | 启用可在模板中使用的环境变量 |
| `-cc`, `-client-cert` | 用于对扫描主机进行身份验证的客户端证书文件（PEM 编码） |
| `-ck`, `-client-key` | 用于对扫描主机进行身份验证的客户端密钥文件（PEM 编码） |
| `-ca`, `-client-ca` | 用于对扫描主机进行身份验证的客户端证书颁发机构文件（PEM 编码） |
| `-sml`, `-show-match-line` | 显示文件模板的匹配行，仅适用于提取器 |
| `-ztls` | 对 TLS13 使用 ztls 库并自动回退到标准库[已弃用] 默认启用自动回退到 ztls |
| `-sni` | 要使用的 TLS SNI 主机名（默认值：输入的域名） |
| `-dka`, `-dialer-keep-alive` | 网络请求的保持连接持续时间。 |
| `-lfa`, `-allow-local-file-access` | 允许在系统上的任何位置访问文件（有效负载） |
| `-lna`, `-restrict-local-network-access` | 阻止到本地/专用网络的连接 |
| `-i`, `-interface` | 用于网络扫描的网络接口 |
| `-at`, `-attack-type` | 要执行的负载组合类型（batteringram, pitchfork, clusterbomb） |
| `-sip`, `-source-ip` | 用于网络扫描的源 IP 地址 |
| `-rsr`, `-response-size-read` | 要读取的最大响应大小（字节） |
| `-rss`, `-response-size-save` | 要读取的最大响应大小（字节）（默认为 1048576） |
| `-reset` | 重置会删除所有 Nuclei 配置和数据文件（包括 `nuclei-templates`） |
| `-tlsi`, `-tls-impersonate` | 启用实验性客户端 Hello (JA3) TLS 随机化 |
| `-hae`, `-http-api-endpoint` | 实验性 HTTP API 端点 |

### INTERACTSH
| 标志 | 描述 |
| :--- | :--- |
| `-iserver`, `-interactsh-server` | 自托管实例的 Interactsh 服务器 URL（默认：oast.pro,oast.live,oast.site,oast.online,oast.fun,oast.me） |
| `-itoken`, `-interactsh-token` | 自托管 Interactsh 服务器的身份验证令牌 |
| `-interactions-cache-size` | 要在交互缓存中保留的请求数（默认为 5000） |
| `-interactions-eviction` | 从缓存中驱逐请求之前等待的秒数（默认为 60） |
| `-interactions-poll-duration` | 每次交互轮询请求之前等待的秒数（默认为 5） |
| `-interactions-cooldown-period` | 退出前的交互轮询额外时间（默认为 5） |
| `-ni`, `-no-interactsh` | 禁用用于 OAST 测试的 Interactsh 服务器，排除基于 OAST 的模板 |

### 模糊测试 (FUZZING)
| 标志 | 描述 |
| :--- | :--- |
| `-ft`, `-fuzzing-type` | 覆盖模板中设置的模糊测试类型（replace, prefix, postfix, infix） |
| `-fm`, `-fuzzing-mode` | 覆盖模板中设置的模糊测试模式（multiple, single） |
| `-fuzz` | 启用加载模糊测试模板（已弃用：使用 `-dast` 代替） |
| `-dast` | 启用/运行 DAST（Fuzz）Nuclei 模板 |
| `-dts`, `-dast-server` | 启用 DAST 服务器模式（实时模糊测试） |
| `-dtr`, `-dast-report` | 将 DAST 扫描报告写入文件 |
| `-dtst`, `-dast-server-token` | DAST 服务器令牌（可选） |
| `-dtsa`, `-dast-server-address` | DAST 服务器地址（默认为 "localhost:9055"） |
| `-dfp`, `-display-fuzz-points` | 在输出中显示模糊点以进行调试 |
| `-fuzz-param-frequency` | 模糊测试前跳过无趣参数的频率（默认为 10） |
| `-fa`, `-fuzz-aggression` | 模糊攻击级别控制模糊的负载数量（low, medium, high）（默认为 "low"） |
| `-cs`, `-fuzz-scope` | 模糊器要遵循的范围内 URL 正则表达式 |
| `-cos`, `-fuzz-out-scope` | 模糊器要排除的范围外 URL 正则表达式 |

### UNCOVER
| 标志 | 描述 |
| :--- | :--- |
| `-uc`, `-uncover` | 启用 Uncover 引擎 |
| `-uq`, `-uncover-query` | Uncover 搜索查询 |
| `-ue`, `-uncover-engine` | Uncover 搜索引擎（shodan,censys,fofa,shodan-idb,quake,hunter,zoomeye,netlas,criminalip,publicwww,hunterhow,google）（默认为 shodan） |
| `-uf`, `-uncover-field` | Uncover 要返回的字段（ip,port,host）（默认为 "ip:port"） |
| `-ul`, `-uncover-limit` | Uncover 要返回的结果数（默认为 100） |
| `-ur`, `-uncover-ratelimit` | 覆盖具有未知速率限制的引擎的速率限制（默认为 60 请求/分钟）（默认为 60） |

### 速率限制 (RATE-LIMIT)
| 标志 | 描述 |
| :--- | :--- |
| `-rl`, `-rate-limit` | 每秒发送的最大请求数（默认为 150） |
| `-rld`, `-rate-limit-duration` | 每秒发送的最大请求数（默认为 1s） |
| `-rlm`, `-rate-limit-minute` | 每分钟发送的最大请求数（已弃用） |
| `-bs`, `-bulk-size` | 每个模板并行分析的最大主机数（默认为 25） |
| `-c`, `-concurrency` | 并行执行的最大模板数（默认为 25） |
| `-hbs`, `-headless-bulk-size` | 每个模板并行分析的无头主机最大数量（默认为 10） |
| `-headc`, `-headless-concurrency` | 并行执行的无头模板最大数量（默认为 10） |
| `-jsc`, `-js-concurrency` | 并行执行的 JavaScript 运行时最大数量（默认为 120） |
| `-pc`, `-payload-concurrency` | 每个模板的最大负载并发数（默认为 25） |
| `-prc`, `-probe-concurrency` | 使用 httpx 的 HTTP 探测并发数（默认为 50） |

### 优化 (OPTIMIZATIONS)
| 标志 | 描述 |
| :--- | :--- |
| `-timeout` | 超时前等待的时间（秒）（默认为 10） |
| `-retries` | 重试失败请求的次数（默认为 1） |
| `-ldp`, `-leave-default-ports` | 保留默认 HTTP/HTTPS 端口（例如 host:80,host:443） |
| `-mhe`, `-max-host-error` | 跳过主机扫描前的最大错误数（默认为 30） |
| `-te`, `-track-error` | 将给定错误添加到 max-host-error 监视列表（standard, file） |
| `-nmhe`, `-no-mhe` | 禁用基于错误跳过主机扫描 |
| `-project` | 使用项目文件夹以避免多次发送相同的请求 |
| `-project-path` | 设置特定的项目路径（默认为 `C:\Users\29752\AppData\Local\Temp`） |
| `-spm`, `-stop-at-first-match` | 在第一次匹配后停止处理 HTTP 请求（可能会破坏模板/工作流逻辑） |
| `-stream` | 流模式 - 无需排序输入即可开始处理 |
| `-ss`, `-scan-strategy` | 扫描时使用的策略（auto/host-spray/template-spray）（默认为 auto） |
| `-irt`, `-input-read-timeout` | 输入读取超时（默认为 3m0s） |
| `-nh`, `-no-httpx` | 对非 URL 输入禁用 httpx 探测 |
| `-no-stdin` | 禁用 stdin 处理 |

### 无头浏览器 (HEADLESS)
| 标志 | 描述 |
| :--- | :--- |
| `-headless` | 启用需要无头浏览器支持的模板（Linux 上的 root 用户将禁用沙箱） |
| `-page-timeout` | 在无头模式下等待每个页面的秒数（默认为 20） |
| `-sb`, `-show-browser` | 在无头模式下运行模板时在屏幕上显示浏览器 |
| `-ho`, `-headless-options` | 使用附加选项启动无头 Chrome |
| `-sc`, `-system-chrome` | 使用本地安装的 Chrome 浏览器而不是 Nuclei 安装的浏览器 |
| `-lha`, `-list-headless-action` | 列出可用的无头操作 |

### 调试 (DEBUG)
| 标志 | 描述 |
| :--- | :--- |
| `-debug` | 显示所有请求和响应 |
| `-dreq`, `-debug-req` | 显示所有发送的请求 |
| `-dresp`, `-debug-resp` | 显示所有接收到的响应 |
| `-p`, `-proxy` | 要使用的 HTTP/SOCKS5 代理列表（逗号分隔或文件输入） |
| `-pi`, `-proxy-internal` | 代理所有内部请求 |
| `-ldf`, `-list-dsl-function` | 列出所有支持的 DSL 函数签名 |
| `-tlog`, `-trace-log` | 写入发送的请求跟踪日志的文件 |
| `-elog`, `-error-log` | 写入发送的请求错误日志的文件 |
| `-version` | 显示 Nuclei 版本 |
| `-hm`, `-hang-monitor` | 启用 Nuclei 挂起监控 |
| `-v`, `-verbose` | 显示详细输出 |
| `-profile-mem` | 生成内存（堆）配置文件和跟踪文件 |
| `-vv` | 显示为扫描加载的模板 |
| `-svd`, `-show-var-dump` | 显示变量转储以进行调试 |
| `-vdl`, `-var-dump-limit` | 限制变量转储中显示的字符数（默认为 255） |
| `-ep`, `-enable-pprof` | 启用 pprof 调试服务器 |
| `-tv`, `-templates-version` | 显示已安装 `nuclei-templates` 的版本 |
| `-hc`, `-health-check` | 运行诊断检查 |

### 更新 (UPDATE)
| 标志 | 描述 |
| :--- | :--- |
| `-up`, `-update` | 将 Nuclei 引擎更新到最新发布的版本 |
| `-ut`, `-update-templates` | 将 `nuclei-templates` 更新到最新发布的版本 |
| `-ud`, `-update-template-dir` | 安装/更新 `nuclei-templates` 的自定义目录 |
| `-duc`, `-disable-update-check` | 禁用自动的 Nuclei/模板更新检查 |

### 统计 (STATISTICS)
| 标志 | 描述 |
| :--- | :--- |
| `-stats` | 显示关于正在运行的扫描的统计信息 |
| `-sj`, `-stats-json` | 以 JSONL(ines) 格式显示统计信息 |
| `-si`, `-stats-interval` | 显示统计信息更新之间等待的秒数（默认为 5） |
| `-mp`, `-metrics-port` | 暴露 Nuclei 指标的端口（默认为 9092） |
| `-hps`, `-http-stats` | 启用 HTTP 状态捕获（实验性） |

### 云 (CLOUD)
| 标志 | 描述 |
| :--- | :--- |
| `-auth` | 配置 ProjectDiscovery Cloud (PDCP) API 密钥（默认为 true） |
| `-tid`, `-team-id` | 将扫描结果上传到给定的团队 ID（可选）（默认为 "none"） |
| `-cup`, `-cloud-upload` | 将扫描结果上传到 PDCP 仪表板[已弃用，使用 `-dashboard`] |
| `-sid`, `-scan-id` | 将扫描结果上传到现有的扫描 ID（可选） |
| `-sname`, `-scan-name` | 要设置的扫描名称（可选） |
| `-pd`, `-dashboard` | 在 ProjectDiscovery Cloud (PDCP) UI 仪表板中上传/查看 Nuclei 结果 |
| `-pdu`, `-dashboard-upload` | 在 ProjectDiscovery Cloud (PDCP) UI 仪表板中上传/查看 Nuclei 结果文件（jsonl） |

### 认证 (AUTHENTICATION)
| 标志 | 描述 |
| :--- | :--- |
| `-sf`, `-secret-file` | 包含 Nuclei 认证扫描密钥的配置文件路径 |
| `-ps`, `-prefetch-secrets` | 从密钥文件中预取密钥 |

---

## 示例 (EXAMPLES)

```bash
对coldfusion:
nuclei -u https://target.com -tags coldfusion -silent -rate-limit 20 -bs 10 -c 10 -o coldfusion_scan_results.json