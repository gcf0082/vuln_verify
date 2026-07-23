# 漏洞规则索引（按漏洞类型加载）

校验时按**被校验漏洞的类型 / 描述的场景**查下表，命中即加载 `vuln-rules/` 下对应的规则文件，把其确认/证伪要点纳入举证。同一类型有多个规则文件的，一并加载--具体适用哪条在读代码后由规则自身的"适用"判断。

**只加载匹配的规则文件**--禁止一次性全量加载，也禁止加载与当前漏洞不匹配的规则文件。索引不是完整清单：未命中的漏洞类型按通用判定方法论校验，不强行套规则。

| 漏洞类型 / 描述场景 | 规则文件 | 校验要点 |
|---|---|---|
| 命令注入 | java-command-array.md、cmd-param-injection.md | 数组执行 ≠ 无注入；参数能否改变命令语义 |
| 敏感信息泄露（日志/控制台/错误响应） | log-sensitive-info.md | 字段是否敏感、是否真正落入输出、是否脱敏 |
| 签名校验篡改/绕过 | digital-signature.md、package-signature-bypass.md | 有签名校验时举证重心转向校验逻辑本身能否绕过 |
| HTTP DoS | http-forward-response-dos.md、http-loop-dos.md | 转发响应未限大小 / 循环次数外部可控且无上限 |
| 提权 | local-privilege-escalation.md、tmp-privesc.md | 低权限用户能否构造输入被高权限进程执行；/tmp 路径可预测可符号链接劫持 |
| 第三方对接凭据泄露 | thirdparty-credential-leak.md | 修改对接信息未指定新凭据致凭据发往伪造地址 |
| 端口监听风险 | port-binding-check.md | 端口随机 / 绑定 0.0.0.0 |
| ReDoS | redos-check.md | 正则灾难性回溯 + 输入可控 |
| 解密后内存驻留 | decrypt-memory-cleanup.md | 解密变量使用后是否及时清除 |
| 压缩炸弹 | zip-bomb.md | 是否限制解压后实际写入量（非解压前大小） |

规则文件给出该类型下的确认（是漏洞）与证伪（非漏洞）要点，作为举证的强化项，不替代判定方法论--最终裁定仍按判定要素与证据强度给出。
