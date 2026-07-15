# Aevi 家庭斗地主

一张带实时裁判、AI 命令适配器、聊天、表情、互动道具、四套桌布、背景音乐和完整扑克牌素材的浏览器斗地主牌桌。

这个仓库只包含斗地主模块，不包含 Aevi 主站、聊天系统、Bio、记忆、人格提示词、VPS 配置或任何真实运行存档。

## 直接运行

需要 Node.js 18 或更高版本。

```bash
npm install
npm start
```

打开 <http://127.0.0.1:8788/doudizhu/>。

公开版默认给三个 AI 座位接入了真实的本地策略玩家，不需要 API Key，也不会使用 mock 数据。裁判服务会在首次运行时创建 `data/doudizhu/`，保存积分、资料和牌局状态。

## 包含什么

- 54 张完整牌组、牌型识别、比较、发牌、叫分、过牌、炸弹/王炸、春天/反春天与跨局积分
- 15 秒回合时限，适配器异常时由裁判兜底，牌局不会卡死
- WebSocket 实时状态同步和 HTTP 操作接口
- 8 / 16 / 24 局家庭场，固定一个真人座位，从三位 AI 中任选两位上桌
- 牌桌聊天、13 个表情、番茄/鸡蛋/干杯互动道具、玩家资料与解散投票
- 四套桌布、完整图片素材、浏览器音效，以及 `normal.mp3` / `intense.mp3` 两首背景音乐
- 可替换的 stdin/stdout JSON 命令玩家协议

## 项目结构

```text
public/doudizhu/          前端牌桌与全部图片/音频素材
src/doudizhu-rules.js    牌组和牌型规则
src/doudizhu-service.js  权威裁判、状态机、计分和持久化
src/doudizhu-adapters.js 命令玩家进程管理与输出校验
src/server.js             独立 HTTP/WebSocket 服务
scripts/                  本地策略玩家和测试
```

## 接入自己的 AI

每个命令玩家从 stdin 接收一行 JSON，并向 stdout 输出一个 JSON 对象。最小输出示例：

```json
{"action":{"type":"play","cards":["S3"]},"say":"","emote":null,"prop":null}
```

首次运行后可修改 `data/doudizhu/players.json` 中对应玩家的 `command`、`cwd` 和 `env`。裁判会验证所有动作；超时、崩溃、非法 JSON 或非法出牌会重试一次，仍失败则自动代打。

## 测试

```bash
npm test
```

测试覆盖牌型、压制关系、AI 输出归一化、发牌与叫分、炸弹倍数、春天/反春天、持久化、互动、解散投票，以及完整扑克牌和两首 MP3 的资源校验。

## 许可

程序代码和文档使用 MIT License。图片与音频的许可边界见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)，Kenney 音效许可保留在素材目录中。
