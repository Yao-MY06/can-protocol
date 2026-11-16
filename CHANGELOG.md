# 变更记录

`body_can.dbc` 的变更都记在这里。版本遵循 SemVer，DBC 的 `VERSION` 与最新一条一致。

## [0.2.0] - 2026-11-16

### 变更

- E2E 注释：数据 ID 由「CAN ID 低字节」改为「完整 11 位 ID 两字节，低字节先」（项目 A 需求说明 v1.2 FR-COM-03）。报文布局不变；按旧规则算 CRC 的实现需要跟着改。

### 新增

- `BCM_Event` 的 `KeyId`、`KeyAction` 取值表与长按阈值注释；0x200、0x201 的 E2E 注释。

## [0.1.0] - 2026-11-11

### 新增

- `body_can.dbc`：`GW_BodyCmd`（0x100）、`BCM_Status`（0x200）、`BCM_Event`（0x201）与三个 UDS 诊断 ID（0x7E0 / 0x7DF / 0x7E8），按项目 A 需求说明 v1.1 第 6.1 节转写。
