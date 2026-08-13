---
title: "Worklog Tuáº§n 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Má»¥c tiÃªu Tuáº§n 6:

* Há»c AWS Step Functions vÃ  máº«u Wait for Task Token.
* XÃ¢y dá»±ng quy trÃ¬nh phÃª duyá»‡t tÃ i liá»‡u.
* ThÃªm thÃ´ng bÃ¡o email SNS.

### Nhiá»‡m vá»¥ trong tuáº§n:

| STT | Nhiá»‡m vá»¥ | NgÃ y báº¯t Ä‘áº§u | NgÃ y hoÃ n thÃ nh | TÃ i liá»‡u tham kháº£o |
| --- | --- | --- | --- | --- |
| 1 | - Há»c AWS Step Functions vÃ  khÃ¡i niá»‡m state machine | 27/07/2026 | 28/07/2026 | |
| 2 | - Táº¡o SNS topic vÃ  email subscription | 28/07/2026 | 28/07/2026 | |
| 3 | - Thiáº¿t káº¿ state machine phÃª duyá»‡t (CaptureToken â†’ Decision â†’ Mark â†’ Notify) | 29/07/2026 | 29/07/2026 | |
| 4 | - TÃ­ch há»£p Step Functions vÃ o backend (startExecution, SendTaskSuccess) | 30/07/2026 | 31/07/2026 | |
| 5 | - Xá»­ lÃ½ task token callback vÃ  cáº­p nháº­t tráº¡ng thÃ¡i tÃ i liá»‡u | 31/07/2026 | 01/08/2026 | |
| 6 | - Test toÃ n bá»™ luá»“ng phÃª duyá»‡t end-to-end | 01/08/2026 | 02/08/2026 | |

### Káº¿t quáº£ Tuáº§n 6:

* Hiá»ƒu AWS Step Functions vÃ  máº«u **Wait for Task Token** cho phÃª duyá»‡t con ngÆ°á»i.
* Táº¡o SNS topic vÃ  xÃ¡c nháº­n email subscription.
* Thiáº¿t káº¿ vÃ  Ä‘á»‹nh nghÄ©a state machine phÃª duyá»‡t (ASL).
* TÃ­ch há»£p Step Functions vÃ o backend: `startExecution` khi submit, `SendTaskSuccess` khi approve/reject.
* LÆ°u task token vÃ  xá»­ lÃ½ callback Ä‘á»ƒ cáº­p nháº­t tráº¡ng thÃ¡i tÃ i liá»‡u.
* Test toÃ n bá»™ luá»“ng phÃª duyá»‡t end-to-end vÃ  xÃ¡c minh email SNS.
