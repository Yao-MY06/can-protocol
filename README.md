# can-protocol

The body-CAN communication contract shared by project A (S32K144 body ECU) and project B (i.MX6ULL gateway). `body_can.dbc` is the only source of truth: both projects generate code or load it directly, and nobody copies it by hand.

- Mounted as a git submodule at `can-protocol/` in both project A and project B.
- **Only project A's coding session 1 (planning) may change it.** If project B needs a change, it raises it in the progress-log handoff; project A changes it and then tells project B.
- Execution AIs (Codex) never change this repo. If you have a question, write it in the current project's `spec/<module>/questions.md`.

## Contents

| File | Description |
| --- | --- |
| `body_can.dbc` | Messages and signals (500 kbit/s, standard frames, all Intel byte order) |
| `CHANGELOG.md` | Change log (SemVer; `VERSION` in the DBC is bumped with it) |

## Messages

| Message | ID | Sender | Cycle |
| --- | --- | --- | --- |
| `GW_BodyCmd` | 0x100 | Gateway / host PC | 100 ms, plus one extra frame immediately on change |
| `BCM_Status` | 0x200 | BCM | 100 ms |
| `BCM_Event` | 0x201 | BCM | Event-triggered, minimum interval 20 ms |
| `Diag_Req_Phys` / `Diag_Req_Func` / `Diag_Resp` | 0x7E0 / 0x7DF / 0x7E8 | Tester / BCM | ISO-TP, no signals |

E2E: byte 0 is the CRC8 (SAE J1850: polynomial 0x1D, initial value 0xFF, final XOR 0xFF). It is computed over DataID (the low byte of the CAN ID) followed by bytes 1..7, in that order. The counter is the low 4 bits of byte 1 and runs 0..14 in a loop.

## Not yet defined

- Value tables for `BCM_Event.KeyId` and `KeyAction` (to be set by the A1 spec).
