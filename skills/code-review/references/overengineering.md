# 过工程审查（Over-Engineering Review）

审查对象是「这段变更里不必要的复杂度」——不是正确性、安全或性能（那些走主审查流程）。目标：找出该删的东西，让 diff 越短越好。

## 五个标签

| 标签 | 含义 | 替换物 |
| --- | --- | --- |
| `delete:` | 死代码、未用的灵活性、拍脑袋的功能 | 无，直接删 |
| `stdlib:` | 手写的东西标准库已有 | 点名标准库函数 |
| `native:` | 依赖 / 代码在做平台原生已覆盖的事 | 点名原生能力 |
| `yagni:` | 只有一个实现的抽象、没人设置的配置、只有一个调用方的层 | 内联，直到出现第二个 |
| `shrink:` | 同样的逻辑，更少的行 | 给出更短写法 |

## 输出格式

每条一行，定位 + 标签 + 砍什么 + 用什么替代：

`L<line>: <tag> <砍什么>. <替代>.`（多文件用 `<file>:L<line>: ...`）

结尾给唯一指标：`net: -<N> lines possible.` 没得砍就 `Lean already. Ship.`

## 示例

- `L12-38: stdlib: 27 行校验类. "@" in email 一行，真正校验是确认邮件.`
- `L4: native: 为一次 format 引入 moment. Intl.DateTimeFormat，0 依赖.`
- `repo.py:L88: yagni: 只有一个实现的 AbstractRepository. 内联，直到出现第二个.`
- `L52-71: delete: 给幂等本地调用套重试. 无替代.`
- `L30-44: shrink: 手写循环建 map. dict(zip(keys, values)) 一行.`

## 边界

只砍「不必要的复杂度」。正确性 bug、安全漏洞、性能问题**不在此审查**，走主审查。一个冒烟测试 / assert 自检是底线，不算过度工程，绝不建议删。只列问题，不代改。

---

> 灵感来源：[ponytail](https://github.com/dietrichgebert/ponytail) — "The best code is the code never written."
