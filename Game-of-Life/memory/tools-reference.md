> 摘要：measure.py/strwidth.py终端宽度测量工具用法与标记语法、已知问题（heredoc吃反斜杠、标记丢失）、当前不可靠暂停使用

## 工具位置

`tool/measure.py` — 标记测量工具
`tool/strwidth.py` — 终端显示宽度计算库

依赖于 `E:\MyProject\claude_code_object\SuperAI\python-3.12.8-embed-amd64\python.exe`（另一个项目，不可乱动）。

## measure.py 用法

标记语法：`\x[...\]`，x = 1,2,3...N（字符串序号）
- `\x[` 开头，`\]` 结尾（backslash + 右括号）
- 计算 `[...\]` 内部字符在终端中的显示宽度
- 标记语法本身不计入长度

```bash
echo '\1[Hello\] \2[你好\]' | python measure.py
# → 1=5,2=4
```

宽度规则：
- 制表符 U+2500-U+257F：1
- ASCII 0x00-0x7F：1
- 拉丁扩展 0x0080-0x024F（含°）：1
- 其他（CJK、箭头、圈号数字、符号）：2

## 已知问题

1. bash heredoc 会吃掉反斜杠，即使 `<< 'EOF'` 也不稳定。应先用 Python 写入文件再 pipe。
2. `\]` 标记紧邻时（`\1[...\]\2[...\]`）第一个 `\` 被重复消耗，导致第二个标记丢失。标记之间必须加空格。
3. 目前工具仍有未解决的解析问题，**暂时不要使用**。

## 使用限制

- 不可动 SuperAI 项目中的任何其他文件，仅允许使用该项目的 python.exe
- 工具目前不可靠，对齐工作暂停
