# 敏感信息泄露 - 日志与输出

**适用**：被校验的主张是敏感信息经日志、控制台或错误响应泄露。

先按 `false-positives.md` 的敏感信息认定确认字段是否敏感（仅凭据/电话/邮箱为敏感；用户名/ID/地址等不认定；内容不可追溯时按敏感处理）。再核实是否真正落入输出：

- **确认（是漏洞）**：敏感字段原文落入下列 sink 且无脱敏：
  - 日志框架（`logger.*`、log4j、slf4j）字符串拼接敏感参数，确实写入日志文件
  - 控制台输出（`System.out.println`、`print`、`console.log`）含敏感原文
  - 异常消息 / `toString()` / 序列化输出含敏感值
  - HTTP 错误响应体直接返回敏感原文
  - 请求/响应体整体落日志（如 `log.info("Request: "+request.toString())`）未排除敏感字段
- **证伪（非漏洞）**：字段非敏感；或被 `@JsonIgnore`、`@ToString.Exclude`、脱敏注解、自定义 `toString()` 排除；或未实际落入输出。
