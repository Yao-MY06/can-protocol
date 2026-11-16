# can-protocol

项目 A（S32K144 车身 ECU）与项目 B（i.MX6ULL 车载网关）共用的车身 CAN 通信约定。`body_can.dbc` 是唯一来源：两个项目都从它生成代码或直接加载，不手抄。

- 以 git submodule 挂进项目 A 与项目 B 的 `can-protocol/`。
- **只有项目 A 的编码会话 1（规划）能改**；项目 B 要改，在进度日志交接块里提，由项目 A 改完再通知。
- 执行 AI（Codex）不改本仓库；有疑问写进所在项目的 `spec/<模块>/questions.md`。

## 内容

| 文件 | 说明 |
| --- | --- |
| `body_can.dbc` | 报文与信号（500 kbit/s，标准帧，全部 Intel 字节序） |
| `CHANGELOG.md` | 变更记录（SemVer；DBC 里的 `VERSION` 同步改） |

## 报文一览

| 报文 | ID | 发送方 | 周期 |
| --- | --- | --- | --- |
| `GW_BodyCmd` | 0x100 | 网关 / 上位机 | 100 ms，变化时立即补发一帧 |
| `BCM_Status` | 0x200 | BCM | 100 ms |
| `BCM_Event` | 0x201 | BCM | 事件触发，最小间隔 20 ms |
| `Diag_Req_Phys` / `Diag_Req_Func` / `Diag_Resp` | 0x7E0 / 0x7DF / 0x7E8 | 诊断仪 / BCM | ISO-TP，不定义信号 |

E2E：字节 0 是 CRC8（SAE J1850：多项式 0x1D，初值 0xFF，结果异或 0xFF）。数据 ID = 完整 11 位报文 ID 的两个字节（0x100 → 0x0100），CRC 输入顺序为数据 ID 低字节、高字节，再字节 1 ~ 7（项目 A 需求说明 v1.2 FR-COM-03）。字节 1 低 4 位是计数器，0 ~ 14 循环。

按键编码（`BCM_Event`）：`KeyId` 0 = 按键 1，1 = 按键 2（2 ~ 3 保留）；`KeyAction` 1 = 短按，2 = 长按（0 / 3 保留；长按阈值 800 ms）。
