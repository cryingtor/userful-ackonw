# Katana 工具帮助文档

*一款专注于自动化流水线执行的快速爬虫，支持无头和非无头爬取模式。*

## 使用方法
katana.exe [flags]

## 参数说明

### 输入 (INPUT)
| 参数 | 描述 |
|------|------|
| `-u`, `-list` | 目标URL/待爬取列表 |
| `-resume` | 使用 resume.cfg 文件恢复扫描 |
| `-e`, `-exclude` | 排除符合指定过滤条件的主机（'cdn'、'private-ips'、cidr、ip、正则表达式） |

### 配置 (CONFIGURATION)
| 参数 | 描述 |
|------|------|
| `-r`, `-resolvers` | 自定义解析器列表（文件或逗号分隔） |
| `-d`, `-depth` | 最大爬取深度（默认：3） |
| `-jc`, `-js-crawl` | 启用JavaScript文件中的端点解析/爬取 |
| `-jsl`, `-jsluice` | 启用JavaScript文件中的jsluice解析（内存密集型） |
| `-ct`, `-crawl-duration` | 爬取目标的最大持续时间（单位：s, m, h, d）（默认：s） |
| `-kf`, `-known-files` | 启用已知文件的爬取（all,robotstxt,sitemapxml），需要最小深度为3以确保所有已知文件被正确爬取 |
| `-mrs`, `-max-response-size` | 要读取的最大响应大小（默认：4194304） |
| `-timeout` | 请求等待时间（秒）（默认：10） |
| `-time-stable` | 等待页面稳定的时间（秒）（默认：1） |
| `-aff`, `-automatic-form-fill` | 启用自动表单填充（实验性功能） |
| `-fx`, `-form-extraction` | 在jsonl输出中提取表单、输入框、文本区域及选择元素 |
| `-retry` | 请求重试次数（默认：1） |
| `-proxy` | 要使用的HTTP/SOCKS5代理 |
| `-td`, `-tech-detect` | 启用技术检测 |
| `-H`, `-headers` | 要包含在所有HTTP请求中的自定义头/Cookie（格式：header:value）（文件） |
| `-config` | katana配置文件路径 |
| `-fc`, `-form-config` | 自定义表单配置文件路径 |
| `-flc`, `-field-config` | 自定义字段配置文件路径 |
| `-s`, `-strategy` | 访问策略（深度优先-depth-first，广度优先-breadth-first）（默认："depth-first"） |
| `-iqp`, `-ignore-query-params` | 忽略爬取具有不同查询参数值的相同路径 |
| `-tlsi`, `-tls-impersonate` | 启用实验性客户端hello（ja3）TLS随机化 |
| `-dr`, `-disable-redirects` | 禁用跟随重定向（默认：false） |
| `-pc`, `-path-climb` | 启用路径爬升（自动爬取父路径） |

### 调试 (DEBUG)
| 参数 | 描述 |
|------|------|
| `-health-check`, `-hc` | 运行诊断检查 |
| `-elog`, `-error-log` | 写入发送请求错误日志的文件 |
| `-pprof-server` | 启用pprof服务器 |

### 无头模式 (HEADLESS)
| 参数 | 描述 |
|------|------|
| `-hl`, `-headless` | 启用无头混合爬取（实验性功能） |
| `-sc`, `-system-chrome` | 使用本地安装的Chrome浏览器而非katana自带的 |
| `-sb`, `-show-browser` | 在无头模式下显示浏览器窗口 |
| `-ho`, `-headless-options` | 使用附加选项启动无头Chrome |
| `-nos`, `-no-sandbox` | 以--no-sandbox模式启动无头Chrome |
| `-cdd`, `-chrome-data-dir` | 存储Chrome浏览器数据的路径 |
| `-scp`, `-system-chrome-path` | 使用指定Chrome浏览器进行无头爬取 |
| `-noi`, `-no-incognito` | 以非无痕模式启动无头Chrome |
| `-cwu`, `-chrome-ws-url` | 使用在其他地方启动的Chrome浏览器实例，侦听此URL的调试器 |
| `-xhr`, `-xhr-extraction` | 在jsonl输出中提取xhr请求的URL和方法 |

### 范围 (SCOPE)
| 参数 | 描述 |
|------|------|
| `-cs`, `-crawl-scope` | 爬虫要跟随的范围内URL正则表达式 |
| `-cos`, `-crawl-out-scope` | 爬虫要排除的范围外URL正则表达式 |
| `-fs`, `-field-scope` | 预定义的范围字段（dn,rdn,fqdn）或自定义正则表达式（例如：'(company-staging.io\|company.com)'）（默认："rdn"） |
| `-ns`, `-no-scope` | 禁用基于主机的默认范围 |
| `-do`, `-display-out-scope` | 显示来自范围爬取的外部端点 |

### 过滤 (FILTER)
| 参数 | 描述 |
|------|------|
| `-mr`, `-match-regex` | 匹配输出URL的正则表达式或列表（cli, file） |
| `-fr`, `-filter-regex` | 过滤输出URL的正则表达式或列表（cli, file） |
| `-f`, `-field` | 要在输出中显示的字段（url,path,fqdn,rdn,rurl,qurl,qpath,file,ufile,key,value,kv,dir,udir）（已弃用：使用-output-template代替） |
| `-sf`, `-store-field` | 要按主机存储的字段（url,path,fqdn,rdn,rurl,qurl,qpath,file,ufile,key,value,kv,dir,udir） |
| `-em`, `-extension-match` | 匹配给定扩展名的输出（例如：-em php,html,js） |
| `-ef`, `-extension-filter` | 过滤给定扩展名的输出（例如：-ef png,css） |
| `-mdc`, `-match-condition` | 使用基于dsl的条件匹配响应 |
| `-fdc`, `-filter-condition` | 使用基于dsl的条件过滤响应 |
| `-duf`, `-disable-unique-filter` | 禁用重复内容过滤 |

### 速率限制 (RATE-LIMIT)
| 参数 | 描述 |
|------|------|
| `-c`, `-concurrency` | 要使用的并发抓取器数量（默认：10） |
| `-p`, `-parallelism` | 要处理的并发输入数（默认：10） |
| `-rd`, `-delay` | 每个请求之间的延迟（秒） |
| `-rl`, `-rate-limit` | 每秒最大发送请求数（默认：150） |
| `-rlm`, `-rate-limit-minute` | 每分钟最大发送请求数 |

### 更新 (UPDATE)
| 参数 | 描述 |
|------|------|
| `-up`, `-update` | 将katana更新至最新版本 |
| `-duc`, `-disable-update-check` | 禁用自动katana更新检查 |

### 输出 (OUTPUT)
| 参数 | 描述 |
|------|------|
| `-o`, `-output` | 写入输出的文件 |
| `-ot`, `-output-template` | 自定义输出模板 |
| `-sr`, `-store-response` | 存储HTTP请求/响应 |
| `-srd`, `-store-response-dir` | 将HTTP请求/响应存储到自定义目录 |
| `-ncb`, `-no-clobber` | 不覆盖输出文件 |
| `-sfd`, `-store-field-dir` | 将每主机字段存储到自定义目录 |
| `-or`, `-omit-raw` | 在jsonl输出中省略原始请求/响应 |
| `-ob`, `-omit-body` | 在jsonl输出中省略响应体 |
| `-j`, `-jsonl` | 以jsonl格式写入输出 |
| `-nc`, `-no-color` | 禁用输出内容着色（ANSI转义码） |
| `-silent` | 仅显示输出 |
| `-v`, `-verbose` | 显示详细输出 |
| `-debug` | 显示调试输出 |
| `-version` | 显示项目版本 |

*注：此文档由katana.exe -h命令输出翻译整理而成。*