# MNR S-1 Phase Overview Chart — vT3

## Purpose
Simplified phase-level view of the S-1 production process. One node per phase, sequential arrows, labeled cross-phase connections derived from the vT3 flowchart (129 nodes, 181 edges).

## Cross-Phase Connections (derived from vT3 Mermaid code)
- **Sequential (P→P+1):** P1→P2→P3→P4→P5→P6→P7→P8→P9→P10→P11→P12
- **Forward skip:** P4→P6 (reserve data and comfort letter readiness feed financial statement preparation before Phase 5 completes)
- **Loop-back:** P11→P9 (repricing requires return to SEC review cycle)
- **Kill paths:** P1, P3, P4, P5, P11 → IPO Abandoned

```mermaid
---
title: "S-1 Registration Statement Production — Phase Overview"
---
flowchart TD
    P1(["Phase 1: Strategic Decision & IPO Readiness"])
    P2(["Phase 2: Assembling the IPO Team"])
    P3(["Phase 3: MLP Structure & Formation"])
    P4(["Phase 4: Organizational Meeting & Due Diligence"])
    P5(["Phase 5: Corporate & Governance Matters"])
    P6(["Phase 6: Financial Statement Preparation"])
    P7(["Phase 7: S-1 Drafting & Content Preparation"])
    P8(["Phase 8: Initial Filing"])
    P9(["Phase 9: SEC Review Cycle"])
    P10(["Phase 10: FINRA Review"])
    P11(["Phase 11: Pre-Effectiveness & Marketing"])
    P12(["Phase 12: Effectiveness"])
    KILL(["IPO ABANDONED"])

    P1 --> P2 --> P3 --> P4 --> P5 --> P6 --> P7 --> P8 --> P9 --> P10 --> P11 --> P12
    P4 -->|"Reserve data & comfort readiness"| P6
    P11 -->|"Reprice"| P9
    P1 -.->|"No-Go"| KILL
    P3 -.->|"No qualifying income"| KILL
    P4 -.->|"DD unacceptable"| KILL
    P5 -.->|"Board declines"| KILL
    P11 -.->|"Abandon"| KILL
```
