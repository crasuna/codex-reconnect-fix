# Codex 重连 5 次问题解决笔记

## 问题现象

Codex 反复重连，出现类似“重连 5 次”的失败情况。

## 推荐排查顺序

1. 开启代理软件的 TUN 模式。
2. 在 `.codex` 文件夹下添加代理环境变量。
3. 如果前两种方法都不行，优先检查代理/机场质量。
4. 最后再谨慎尝试修改 Codex config 的 `model_provider`。

## 方法一：开启 TUN 模式

打开代理软件的 TUN 模式，让 Codex 的网络请求能走代理。

## 方法二：在 `.codex` 下添加 `.env`

在 `.codex` 文件夹下新建 `.env` 文件，并添加如下内容：

```env
HTTP_PROXY=http://127.0.0.1:7897
HTTPS_PROXY=http://127.0.0.1:7897
ALL_PROXY=http://127.0.0.1:7897
```

端口号 `7897` 需要根据代理软件的实际端口进行设置。

## 代理质量检查

若方法一和方法二都不行，优先检查梯子质量。

实测可用组合：

- 机场：`MESL`
- 代理软件：`clash verge`

## 方法三：修改 Codex config

在 Codex 辅助下更改 config 文件，插入：

```toml
model_provider = "codex-http"

[model_providers.codex-http]
name = "Codex HTTP"
base_url = "https://chatgpt.com/backend-api/codex"
wire_api = "responses"
requires_openai_auth = true
supports_streaming = true
supports_websockets = false
```

注意：此方法会更改 `model_provider`，可能导致使用默认 `openai` provider 的线程在当前视图中不可见。建议只在前面的方法和代理质量检查都无法解决时再尝试。
