  
**MERMAID FLOWCHART**

**INSTRUCTION MANUAL**

Coding Standards, Best Practices & Style Guide

Version 14.0

February 2026

# **1\. Purpose & Scope**

This manual defines the mandatory standards for creating Mermaid flowcharts. It codifies the methodology, coding conventions, shapes, colors, node naming, labeling rules, and structural patterns to be followed on every flowchart produced by this team.

## **1.1 What Is Mermaid?**

Mermaid is a lightweight, text-based diagramming language. You write structured code in a plain text file, then render it into a visual flowchart. There is no drag-and-drop interface; the diagram is generated entirely from code.

## **1.2 Where to Render**

All flowcharts are tested and rendered in **mermaid.live** (https://mermaid.live). Paste your code in the left panel; the rendered diagram appears on the right. This is the canonical rendering environment.

## **1.3 Scope of This Manual**

This manual covers flowchart-type diagrams only (not pie charts, timelines, Gantt charts, or other Mermaid diagram types). All rules below apply to every flowchart produced under this methodology.

## **1.4 How to Use This Manual With a User**

This manual is written for the LLM, not the user. The user will not know or reference section numbers. The LLM must lead the user through the methodology step by step, in section order, starting at Section 2. At each stage, the LLM determines what information is needed, conducts the required searches, produces the required deliverables, and presents them to the user for validation — without requiring the user to know which section applies. When a USER INPUT REQUIRED gate is reached, the LLM must ask the user the specific question that the gate requires, in plain language, without citing section numbers. **WARNING: Never cite section numbers (e.g., “Section 2.1”, “per Section 3.3”) in user-facing output.** The user has not read this manual and section references are meaningless to them. Refer to process steps by their descriptive name (e.g., “the scoping step”, “the source extraction phase”). This applies to all user-facing text — chat messages, file headers, flag tables, and confirmation prompts.

## **1.5 Foundational Design Principles**

Every flowchart produced under this methodology is governed by a set of foundational design principles. These principles are the “why” behind the rules in the sections that follow. All coding conventions, shape choices, color assignments, and structural patterns trace back to one or more of these principles.

1. **End-to-end coverage (“soup to nuts”).** The flowchart must capture the complete process from its initiating trigger to one or more terminal milestones (the normal end milestone plus any kill-path milestones), with an unbroken chain of nodes connecting the start to every terminal milestone. No gaps, no dead ends, no disconnected clusters. A viewer should be able to walk the entire process from start to finish without leaving the chart.

2. **Explicit decision points.** Real-world processes are not linear—they contain gates, approvals, and conditional branches. The flowchart must surface these as diamond-shaped decision nodes with labeled Yes/No outcomes, rather than burying them inside action steps or treating the workflow as a straight sequence.

3. **Role-based accountability.** Different steps are performed by different people. The flowchart must make this visible at a glance by assigning each action node a color tied to the role that currently performs it, and by prefixing every node’s display text with the role name. This single requirement drives the role taxonomy, the color palette, the node naming convention, and the companion RACI matrix.

4. **LLM-driven granularity.** The primary objective of the flowchart is to identify where an LLM (currently Claude, by Anthropic) can be inserted into the production process. Every step is classified into one of four tiers: **LLM** (a frontier LLM can produce the complete, filing-quality work product), **Hybrid** (a frontier LLM can produce a substantial first draft, but the output requires material human judgment to reach filing quality), **Human** (requires 100% human judgment, negotiation, or access to information the LLM cannot obtain), or **External** (performed by an outside party such as the SEC or an exchange). Only LLM and Hybrid steps are expanded into granular sub-steps in the flowchart; Human and External steps remain high-level. Classification is based on the production capability — whether the LLM can produce the work product — not on whether a human will review it afterward (see Section 2.3). This ensures the chart’s detail is concentrated where the automation opportunity exists.

5. **Input-first workflow.** The flowchart assumes source materials are assembled before drafting begins — not generated ad hoc during the drafting process. For an S-1 filing, this means the virtual data room (VDR) is populated before prospectus drafting starts. For other deliverable types, the equivalent input-collection phase (data room, document request list, prior-period filings) is complete before the production workflow begins. This assumption drives the phase sequencing: input assembly always precedes drafting. Without it, the flowchart cannot guarantee that drafting nodes have the inputs they require, and the parallel-track structure (Section 9.1.1) collapses into an unpredictable ad hoc sequence.

*This list is not exhaustive.* Additional principles may be identified as the methodology evolves. When a new principle is established, it should be added here and cross-referenced to the sections it governs.

## **1.6 Execution Sequence Overview**

The manual is organized by topic, not by execution order. This section provides the consolidated execution sequence so the LLM can plan work correctly, including parallelism. Steps marked *(parallel)* may run concurrently.

1. **Domain Acquisition Gate + LLM Capability Acquisition Gate** (Section 2 preamble + Section 2.3.1). Execute both before any other work.
2. **Scope definition** — define the single deliverable, confirm trigger/end condition/setting/variant (Sections 2.1–2.4).
3. **Source extraction and precedent intake** *(parallel)*:
   - (a) Gather sources → extract steps → merge into combined list (Sections 3.1–3.3).
   - (b) Select precedent → run Precedent Filing Intake Protocol → produce Final Section Inventory (Sections 3.5–3.5.1).
4. **Deduplicate** the merged list against the Final Section Inventory (Section 3.4).
5. **Role assignment** — assign single-role ownership to every step (Section 3.6). Run Gate 1.
6. **Classification + consolidation + expansion** — classify by LLM tier, consolidation pass, sub-step expansion, post-expansion consolidation (Section 3.7). Run Gate 2.
7. **Mermaid code production + RACI matrix construction** *(parallel)*:
   - (a) Write Mermaid code: subgraphs → connections → styles (Sections 4–9). Run Gates 3, 4, 5 at each stage boundary.
   - (b) Build RACI matrix concurrently with code (Section 3.8 / 10.5). Run Gate 6 when complete.
8. **Presentation deliverables** — Summary slide (Section 10.6), Cover Memo (Section 10.7), Phase Overview Chart (Section 10.8). Produced after flowchart and RACI are finalized.
9. **Final validation** (Section 11). All 19 checks + cross-deliverable coherence (Section 11.1).

# **2\. Scoping the Flowchart**

Before writing any code, define the workflow boundary. Without a clear scope, flowcharts become infinite and unreadable. Answer four questions before you begin:

| Question | Example |
| :---- | :---- |
| Trigger | What initiates the process? (e.g., sponsor decides to pursue IPO; board authorizes proxy filing) |
| End condition | What signals completion? (e.g., SEC declares registration statement effective; proxy mailed to shareholders) |
| Setting / context | Department, location, system, or channel (e.g., SEC/EDGAR, NYSE listing application portal) |
| Object / variant | What type of case? (e.g., Oil & Gas MLP IPO; REIT 10-K annual report) |

**All four scope parameters must be presented to the user in a single response.** Do not present them piecemeal across multiple round-trips or wait for confirmation of individual parameters. After the Domain Acquisition Gate is complete and the deliverable is defined (Section 2.1), present the complete scope boundary (trigger, end condition, setting/context, and object/variant) with proposed values derived from the domain research. This presentation occurs after completing both the Domain Acquisition Gate (below) and defining the single deliverable (Section 2.1) — not before. The user confirms or adjusts the complete set. Do not proceed to source gathering (Section 3.1) until all four parameters are confirmed.

**DOMAIN ACQUISITION GATE (mandatory before proceeding).** Before answering the four questions above or defining any scope boundary, you must acquire baseline domain knowledge about the entity type, industry sector, and deliverable type. Without this knowledge, you cannot correctly identify the trigger event, end condition, regulatory setting, or scope boundary — and every downstream step (source gathering, precedent selection, role assignment, classification) will inherit the error.

**WARNING:** LIVE SEARCH REQUIRED. Conduct the following searches and document the results before proceeding to Section 2.1:

1. **Entity type and legal structure.** Search for the entity's legal form (C-corp, S-corp, LP, LLC, MLP, REIT, BDC, SPAC, etc.), its governance structure (board of directors vs. general partner, shareholder voting vs. unitholder voting), and how its legal form affects SEC filing requirements. Different entity types have materially different disclosure obligations, financial statement formats, and regulatory frameworks. Example: an MLP files as a partnership (pass-through tax, K-1s, IDR waterfall disclosures) while a C-corp files as a corporation (corporate tax, 1099-DIV, board committee disclosures). If you conflate the two, the entire flowchart will be wrong.

2. **Industry sector and business model.** Search for the entity's industry (e.g., solar EPC, midstream oil & gas, commercial real estate, biotech), its revenue model, and the industry-specific disclosure requirements that apply. Different sectors have different risk factor categories, different regulatory bodies, and different expert requirements. Example: an oil & gas company requires reserve engineer reports (Reg S-X 4-10(a)) and environmental disclosures; a solar EPC contractor requires neither, but may require disclosure of project backlog, ITC/PTC tax credit dependency, and utility-scale contract structures.

3. **Deliverable-specific regulatory framework.** Search for the specific SEC rules, accounting standards, and listing requirements that govern the deliverable for this entity type and sector. Do not assume that rules applicable to one entity type apply to another. Example: PCAOB audit standards apply universally to S-1 filings, but the financial statement presentation requirements differ between a corporate issuer (consolidated statements) and an MLP (combined/carved-out statements of the predecessor).

Document the results of these three searches in a brief summary (3–5 sentences per topic) and present to the user for validation before proceeding. **Execute the first search immediately using whatever entity name or description the user provided.** Do not ask the user to confirm the entity identity before searching — the search results themselves will confirm or disambiguate. If the search reveals ambiguity (e.g., multiple entities with the same name), present the disambiguation to the user as part of the gate output, not as a preliminary question. If the user identifies errors or gaps in your domain understanding, conduct additional searches to resolve them. Do not proceed to Section 2.1 until the user confirms your domain understanding is correct.

**TIP:** USER INPUT REQUIRED. The user may have domain expertise that supplements or corrects search results. The user's corrections are authoritative — update your understanding accordingly. This domain summary is a persistent reference for all subsequent sections — refer back to it when making entity-specific or sector-specific decisions in Sections 3.1, 3.5, 3.6, 3.7, and 8.2.

## **2.1 Define the Single Deliverable**

Before scoping the flowchart, identify the single document or deliverable whose production process you are mapping. Real-world processes (such as an IPO) are vast and contain many workstreams. Attempting to chart the entire process will produce an unreadable diagram. Instead, narrow the scope to one concrete output.

**Example:** An IPO involves roadshows, pricing, allocation, closing, settlement, and more. But the flowchart’s scope is not “the IPO” — it is ***the production of the S-1 registration statement***. Everything in the chart must relate to producing, filing, reviewing, and obtaining effectiveness for that single document. Steps that occur after the S-1 is declared effective (pricing, allocation, closing) are out of scope.

This narrowing decision should be made explicitly and documented before any nodes are drafted. It determines both the start milestone (the event that triggers production of the deliverable) and the end milestone (the event that signals the deliverable is complete).

**WARNING:** LIVE SEARCH REQUIRED. If you are unfamiliar with the deliverable type, search for its regulatory definition, required contents, and how it differs from similar filings (e.g., S-1 vs. F-1 vs. S-11). You must understand what the deliverable contains before you can scope its production process.

**SCOPE ENFORCEMENT GATE (mandatory at every stage).** The scope boundary defined in this section is enforced at every subsequent stage of the methodology — merge (3.3), deduplication (3.4), role assignment (3.6), classification (3.7), and validation (11). Before presenting any deliverable to the user, re-verify that every entry falls within the defined scope boundary. Any step whose trigger event occurs after the end milestone is out of scope and must be removed immediately — do not carry it forward to the next stage. This is not discretionary; it is a hard gate.

**TIP:** USER INPUT REQUIRED. The choice of which deliverable to map is a strategic business decision that depends on the user’s objective. No instruction or source document can make this decision. The user must define the single deliverable before any other work begins.

**After defining the deliverable, present the complete scope boundary** (trigger, end condition, setting/context, and object/variant) to the user for confirmation, with proposed values derived from the domain research conducted during the Domain Acquisition Gate. Do not proceed to Section 2.2 until all four scope parameters are confirmed.

## **2.2 The Soup-to-Nuts Rule**

**Every flowchart must be end-to-end.** It must begin with a start milestone (the trigger event that initiates the process) and end with one or more terminal milestones — the normal end milestone (signaling successful completion) plus any kill-path milestones (signaling abnormal termination). An unbroken chain of nodes must connect the start to every terminal milestone. There must be no gaps — every node must be reachable from the start, and a terminal milestone must be reachable from every branch. If a viewer walks the chart from top to bottom, they should be able to trace the entire process without encountering dead ends or disconnected clusters.

**Kill paths (abnormal termination).** If the process can be abandoned, withdrawn, or terminated before reaching the normal end milestone, the flowchart must include at least one kill-path milestone representing that outcome. Kill-path milestones branch from decision diamonds where the process can be stopped (e.g., IPO abandoned after failed SEC review, S-1 withdrawn before effectiveness). These are terminal nodes — they have inbound edges but no outbound edges, and they use the stadium shape with an ALL-CAPS label (e.g., `Milestone_IPOAbandoned(["IPO ABANDONED — END"])`). See Section 5.1 for the coding pattern and Section 7.4 for the display text convention.

## **2.3 Granularity Rule**

The level of detail in the flowchart is not uniform — it is driven by LLM capability classification (see Principle \#4). Before drafting the chart, classify every step into one of four tiers:

| Tier | Definition | Classification Question | Granularity in Chart |
| :---- | :---- | :---- | :---- |
| LLM | The most advanced available LLM (or its expected successor within 6 months — see Section 2.3.1) can produce the complete, filing-quality work product for this step, given access to the necessary inputs. Determine current and near-term capability via mandatory live search before classifying. | "If I gave the best available LLM the source data and the instructions, could it produce an output that a senior reviewer would accept with only minor, non-substantive edits?" | Expand into granular sub-steps. |
| Hybrid | The most advanced available LLM (or its expected successor within 6 months — see Section 2.3.1) can produce a substantial first draft, but the output requires material human judgment, substantive revision, or domain expertise to reach filing quality. Determine current and near-term capability via mandatory live search before classifying. | "Can the best available LLM get me 60–80% of the way there, but a knowledgeable human must make substantive decisions about content, framing, or completeness?" | Expand into granular sub-steps. |
| Human | The step requires 100% human judgment, negotiation, physical action, or access to information the LLM cannot obtain (e.g., management's undisclosed business plans, board deliberations, negotiated commercial terms). | "Is the core activity something no LLM can meaningfully perform — not because of quality concerns, but because the inputs don't exist in any accessible form or the activity is inherently non-textual?" | Keep high-level (single node). |
| External | Performed by an outside party (SEC, exchange, FINRA, reserve engineer, auditor) whose work product the team receives but does not produce. | "Is this an action taken by a party outside the working group, whose output we accept as delivered?" | Keep high-level (single node). |

**Classification separates production from review.** The tier classifies the **production capability** — whether an LLM can produce the work product — not the **approval workflow** that follows. Every step in a regulated filing goes through human review before filing. That review is a quality gate, not a classification input. If an LLM can produce a filing-quality exhibit index, it is LLM-tier even though a partner will review it. If an LLM can draft 80% of a risk factor section but a lawyer must make substantive judgments about materiality, it is Hybrid even though the same partner reviews it. The review happens regardless; the question is who (or what) does the production work.

**The boundary between LLM and Hybrid is substantive judgment, not quality control.** A step is LLM-tier if the LLM’s output, given proper inputs, would be accepted by a senior reviewer with only formatting, style, or minor factual corrections — the kind of edits that do not change the substance or legal meaning. A step is Hybrid if the LLM’s output requires a human to make material decisions about what to include, how to frame it, or whether the content is legally sufficient. The distinction is not “does a human look at it” (they always do) but “does a human change the substance.”

**Defining “high-level” for Human and External steps.** A Human or External step should represent a complete work product, engagement, or regulatory action — not an individual document section or sub-task. If multiple sections are drafted by the same role in the same work session or phase, they are presumptively one step, not five — subject to the five-factor retention test in Section 3.7.1, which may preserve them as separate nodes if any test returns Yes. Example: “Draft Management, Compensation, and Ownership sections” is one Human step, not three separate nodes.

**Classification is based on the core production activity, not prerequisites.** Classify each step based on what happens when the drafter sits down to produce the output — not on the input-gathering that precedes it. If a step requires human-supplied input data (e.g., management interviews, board decisions) but the drafting or assembly work can be performed or assisted by an LLM once those inputs are in hand, classify it as **Hybrid or LLM**, depending on whether the production activity requires substantive human judgment (Hybrid) or only mechanical assembly, formatting, or drafting from the provided inputs (LLM). The input-gathering is a dependency, not the step itself. The flowchart maps the document production process, and classification should reflect the production activity.

This includes steps that depend on proprietary third-party data the LLM cannot independently access (e.g., reserve engineer reports, auditor comfort letters, management's undisclosed business plans). Once the data is in hand, if the production activity involves substantive drafting or synthesis, the step is **Hybrid**. If the step's core activity is merely receiving and filing the third-party work product without substantive transformation, it is **External**, not Hybrid. The distinguishing question: "Does the team member do substantive production work with this input, or merely receive and transmit it?"

### **2.3.1 LLM Capability Acquisition Gate (mandatory before classification)**

Before classifying any step into the four tiers, the LLM must acquire current knowledge of what frontier LLMs can actually do. Without this knowledge, the LLM will default to conservative assumptions that systematically undercount the LLM tier and defeat the flowchart’s primary purpose.

**Timing:** This gate must be executed before classification begins (Section 3.7). Execute it during or immediately after the Domain Acquisition Gate (Section 2 preamble) to inform granularity decisions throughout the process.

**WARNING:** LIVE SEARCH REQUIRED. Conduct the following searches and document the results before proceeding to classification (Section 3.7):

1. **Current frontier model capabilities.** Search for the most recent frontier LLM capabilities relevant to the deliverable type. For SEC filings: search for LLM performance on legal reasoning benchmarks (e.g., BigLaw Bench), document analysis and drafting capabilities, context window sizes, and agentic multi-step workflow performance. Document: which models, what scores, what tasks they can perform.

2. **Domain-specific LLM deployments.** Search for how frontier LLMs are currently being used in the specific domain of the deliverable. For SEC filings: search for legal AI platforms (e.g., Harvey, Libra, CoCounsel), law firm adoption of AI for drafting and review, and specific use cases in capital markets practice. Document: what tasks are being performed by LLMs in production today, not experimentally.

3. **Near-term capability trajectory (6-month horizon).** Search for credible forecasts of LLM capability improvements over the next 6 months. Document: expected improvements in agentic execution, context handling, and domain-specific performance. The flowchart is a forward-looking tool; classify based on what the best available LLM can do today and what it will likely be able to do within the deliverable’s production timeline.

**The classification question is: “Could Claude Opus 4.6 (or its equivalent successor within 6 months) produce this work product to filing quality, given the necessary inputs?”** This is not a hypothetical — it requires knowing what the model can actually do, which is why the search is mandatory.

**Capability baseline for SEC filing production (as of February 2026).** The following capabilities are documented for Claude Opus 4.6 and comparable frontier models. Use this as a starting baseline; update via live search for each new project:

- **Context window:** 1M tokens (beta), enabling ingestion of an entire S-1 filing (typically 150K–400K tokens) in a single prompt.
- **Legal reasoning:** 90.2% on BigLaw Bench (Harvey evaluation), with 40% perfect scores on litigation and transactional tasks.
- **Document production:** Can produce complete structured documents (reports, spreadsheets, presentations) from raw data. Produces polished, professional output with 120+ inline citations on complex research queries.
- **Agentic execution:** Sustains multi-step workflows across extended sessions. Agent teams — parallel autonomous sub-agents (research preview) — available for complex decomposed tasks.
- **Output length:** Up to 128K tokens per response — sufficient to produce an entire prospectus section in one pass.
- **Deployed in production:** Harvey (legal AI), Libra (Wolters Kluwer), CoCounsel (Thomson Reuters) use frontier Claude models for drafting, review, and research in live law firm workflows. Dentons (global law firm) uses Claude for drafting, review, and research across global teams.

**Illustrative LLM-tier steps for SEC filings.** Based on documented capabilities, the following categories of steps are strong LLM-tier candidates (subject to user validation):

- Compiling exhibit indices from EDGAR filing records
- Drafting boilerplate disclosure sections (Where You Can Find More Information, Enforceability of Civil Liabilities, Legal Matters)
- Assembling subsidiary lists from public records and organizational charts
- Formatting capitalization tables from financial data
- Cross-referencing sections for internal consistency
- Computing filing fee tables per SEC Rule 457
- Drafting sections that synthesize publicly available data (industry overview from public market data, historical financial summary from audited statements)
- Generating redline comparisons between filing versions
- Assembling signature pages and power-of-attorney exhibit lists

**Illustrative Hybrid-tier steps.** Steps where the LLM produces a substantial first draft but a human must make substantive judgments:

- Drafting risk factors (LLM can organize categories and draft boilerplate language, but materiality and company-specific risk assessment require human judgment)
- Drafting MD&A (LLM can structure and draft from financial data, but management’s explanation of trends requires human input)
- Drafting Business section (LLM can compile from VDR data and public sources, but strategic positioning and competitive narrative require human judgment)
- Preparing SEC comment letter responses (LLM can draft proposed responses and redlined prospectus language, but the legal strategy requires human judgment)

**Illustrative Human-tier steps.** Steps that no LLM can meaningfully perform:

- Conducting management interviews
- Negotiating underwriting agreement terms
- Making board resolutions and governance decisions
- Determining pricing and allocation
- Exercising professional judgment on audit opinions or legal opinions

**Expected distribution ranges.** For a typical SEC filing production flowchart, expect approximately 15–30% of parent work steps to be LLM-tier, 35–50% Hybrid, 15–30% Human, and 5–15% External. Significant deviations from these ranges should be investigated — they may be correct (driven by the specific deliverable type or entity structure) or may indicate misapplication of the tier definitions. A zero-LLM or zero-Hybrid result is almost certainly an error.

**This gate is not optional.** If the executing LLM skips this gate, it will lack the factual basis to distinguish LLM from Hybrid, and will systematically downgrade LLM-tier steps to Hybrid — producing a flowchart that understates the automation opportunity and fails to serve its primary purpose.

**TIP:** The four-tier classification is documented in a companion step classification table, not in the flowchart itself. The flowchart reflects the granularity outcome; the companion table records the underlying LLM/Hybrid/Human/External designation.

**TIP:** USER INPUT REQUIRED. The four-tier classification depends on the user’s specific operating environment — their team’s capabilities, risk tolerance, and compliance culture. What qualifies as “LLM” for a sophisticated team may be “Hybrid” or “Human” for a more conservative one. The user must validate every classification before the chart is expanded.

## **2.4 Node and Edge Limits**

**Rendering target:** The flowchart must render without errors in mermaid.live. If the rendered chart is too dense to trace the happy path visually at default zoom, consider consolidation (Section 3.7) or splitting into sub-processes (Section 2.5).

**Total node range:** A single-deliverable flowchart must contain between 60 and 300 total nodes (action steps + decision diamonds + milestones). Below 60, the chart is too abstract to document a meaningful production process. Above 300, the chart exceeds practical readability limits for a single rendered diagram. If the total node count exceeds 300 after expansion (Section 3.7.2), consolidate parent steps before expanding further, or split into sub-processes per Section 2.5.

**Parent work step count** is an observed metric — it is reported in all deliverables but is not a gated range. The parent work step count will vary by document type and process complexity. Consolidation (Section 3.7) remains good practice for eliminating redundancy, but it is not driven by a target range.

**Definition: parent work step.** A parent work step is an action node that existed in the consolidated step inventory (Section 3.7.1) before sub-step expansion (Section 3.7.2). Sub-steps created during expansion are children of the parent and are not counted as parent work steps. The parent work step count is the primary metric used on the summary slide and in LLM opportunity calculations.

**Edge limit (binding technical constraint):** Mermaid's default `maxEdges` parameter is 500. A flowchart that exceeds 500 edges will fail to render. For reference, the Mach Natural Resources S-1 flowchart (123 nodes) produced 169 edges — a ratio of approximately 1.37 edges per node. At that ratio, a 300-node chart would produce approximately 411 edges, within the 500-edge limit. Charts with higher branching density (more decision diamonds, more loop-backs) will produce a higher ratio and may approach the limit at lower node counts. Edge count is validated in Section 11.

## **2.5 When to Split Into Sub-Processes**

Break into sub-processes when:

* A branch or section exceeds approximately 8–10 steps.

* A handoff goes to another department/function with many internal details.

* The detail level jumps dramatically (e.g., from “submit request” to “how IT provisions a laptop”).

**Consolidation checkpoint before splitting.** Before committing to a multi-level split, verify that the step count reflects proper consolidation per Sections 2.3 and 3.7. A two-level split is a structural decision driven by genuine process complexity — not a workaround for overcounting. If the total node count approaches 300 due to insufficient grouping rather than genuine complexity, consolidate first. Only after confirming that consolidation has been fully applied should you proceed to a multi-level architecture.

## **2.6 Step Inclusion Test**

A step merits its own node if it changes:

* Who does it (role shift).

* Where or in what tool it happens.

* What can happen next (a decision point).

* It involves risk, time, or compliance impact.

# **3\. Step Identification Methodology**

The steps in the flowchart are not invented from memory or intuition. They are derived systematically from authoritative source materials and validated against a real precedent transaction. This section documents the process so it can be replicated for any new deliverable.

**Standing rules for Sections 3.1–3.8.** The following two rules apply at every stage in this section where the LLM presents a deliverable, flag table, or proposed list to the user. They are stated here once and cross-referenced from individual sections rather than repeated.

1. **Self-audit before presenting (Standing Rule 1).** Before presenting any proposed list, classification table, or flag table to the user, re-verify every entry against the scope boundary (Section 2.1) and remove any out-of-scope entries. Present validation flags (ambiguous cases requiring user judgment) as a numbered table with: (a) the issue in one sentence, (b) your default recommendation, and (c) an approve/override column. Include the response template: "To approve all defaults, reply APPROVE ALL. To override specific items, list only the overrides (e.g., F-3: override to X; F-7: remove)."

2. **Live search before presenting flags (Standing Rule 2).** Before presenting each flag to the user, conduct a live search to verify your default recommendation. If the search result contradicts your default, revise the default before presenting. Cite the search result that supports your final default in the flag table. Do not rely on training knowledge alone for flag defaults — domain-specific facts (e.g., statutory timing requirements, standard industry practices) may differ from your assumptions.

## **3.1 Gather Authoritative Source Materials**

Identify the leading published guides, checklists, and roadmaps for the type of deliverable being mapped. For the S-1 production flowchart, four primary sources were used:

1. Latham & Watkins — US IPO Guide (full guide \+ checklist annex).

2. Deloitte — IPO readiness guide (IPO Readiness Services & Assessment; DART Roadmap: Initial Public Offerings).

3. PwC — Roadmap for an IPO: A Guide to Going Public.

4. Vinson & Elkins (V\&E) — MLP Primer (for MLP-specific structural and tax steps).

Each source describes the production process from a slightly different angle and level of detail. Using multiple sources ensures comprehensive coverage — no single guide captures every step.

**WARNING:** LIVE SEARCH REQUIRED. Use live web search to identify and locate the leading published guides for your deliverable type. For a different deliverable (e.g., a proxy statement, a 10-K, or a bond offering memorandum), the authoritative sources will be different. Search for “\[deliverable type\] preparation guide,” “\[deliverable type\] checklist,” and equivalent terms to build your source library.

**Source identification procedure:** Search for guides and checklists from at least three categories of publisher: (a) top-10 law firms with capital markets practices (e.g., Latham, Kirkland, Simpson Thacher, V\&E), (b) Big 4 accounting firms (Deloitte, PwC, EY, KPMG), and (c) industry-specific specialists relevant to the deliverable type (e.g., energy-focused firms for MLP filings). Collect a minimum of 3–5 sources before proceeding to extraction. If the deliverable involves a specialized entity structure (MLP, REIT, SPAC, BDC), ensure at least one source addresses that structure specifically.

## **3.2 Extract Steps From Each Source**

Read each source and extract every discrete activity it identifies for producing the deliverable. At this stage, do not filter or consolidate — capture everything. Record each step with its source, the role the source assigns it to, and a brief description.

**Extract from every approved source without exception.** Do not skip sources based on a judgment that “sufficient content” has been gathered from other sources. Each source may contain steps, sub-processes, or regulatory requirements not present in any other source — particularly sources from different professional perspectives (e.g., law firm checklists vs. Big 4 accounting roadmaps). The merge step (Section 3.3) handles deduplication; the extraction step must be exhaustive. If a source yields no unique steps beyond what prior sources already captured, state that explicitly in the extraction output.

**WARNING:** LIVE SEARCH REQUIRED. When a source references a regulatory requirement, accounting standard, or legal concept that is not self-explanatory, search for the underlying detail before recording the step. For example, a source that says “prepare financial statements per PCAOB standards” requires searching what PCAOB standards specifically require in order to determine whether that is one step or five.

**Present each source’s extraction to the user before merging.** After extracting steps from each source, present the extraction to the user with: (a) the source name, (b) the number of discrete steps extracted, and (c) the step list. The user must have the opportunity to validate completeness and flag any steps the LLM may have missed. Only after all source extractions have been presented and the user has confirmed should the LLM proceed to the merge (Section 3.3).

## **3.3 Merge Into a Combined List**

Combine all extracted steps into a single merged list. This raw union will be larger than the final step count because different sources describe the same activity using different terminology (e.g., Latham may say “prepare risk factors” while Deloitte says “draft Item 1A disclosures”). That overlap is expected and is resolved in the next step.

**SCOPE GATE (Section 2.1).** During the merge step, exclude any steps that fall outside the scope boundary defined in Section 2.1. Do not carry out-of-scope entries forward for deduplication — remove them at the merge stage. For example, if the scope ends at SEC effectiveness, steps such as pricing, 424(b) filing, closing documents, and settlement are out of scope and must be removed here, not carried through.

## **3.4 Deduplicate and Reconcile Against a Precedent Transaction**

Select a real, filed precedent transaction as a ground-truth reference (see Section 3.5 for the selection methodology and Section 3.5.1 for the intake protocol — select and process the precedent per those sections before performing deduplication). For the S-1 flowchart, the Mach Natural Resources LP S-1 filing was used.

Walk the merged list against the precedent filing:

* If two entries from different sources describe the same work, merge them into one step.

* If the precedent filing reveals a step that no source captured, add it.

* If a source lists a step that does not apply to the specific deliverable type (e.g., a corporate IPO step that doesn’t apply to an MLP), remove it.

This merge-and-reconcile process causes the step count to fluctuate — the raw union inflates the count, and deduplication brings it back down. That fluctuation is normal and expected.

**The step count at this stage is preliminary.** It will decrease after LLM capability classification (Section 3.7) drives consolidation of Human and External steps into high-level nodes. Do not treat the post-deduplication count as the final number.

**SCOPE GATE (Section 2.1).** Apply the scope gate before presenting the deduplicated list.

**WARNING:** LIVE SEARCH REQUIRED. Use SEC EDGAR (https://efts.sec.gov/LATEST/search-index?q=) or equivalent regulatory databases to locate and access the precedent filing. Search for transaction-specific context that the filing alone may not explain (e.g., entity name changes, concurrent transactions, or unusual structural features).

**TIP:** USER INPUT REQUIRED. When two steps from different sources appear to describe the same work but are not identical, only someone with domain expertise can determine whether they are truly duplicates or two distinct activities. The user must make the merge/split/remove judgment calls during deduplication.

**Propose-and-validate workflow for deduplication flags:** When ambiguous items require user judgment, present them as a numbered flag table with three columns: (a) the issue in one sentence, (b) your default recommendation, and (c) an approve/override column. This enables the user to respond in bulk. Include this response template: “To approve all defaults, reply APPROVE ALL. To override specific items, list only the overrides (e.g., F-3: override to X; F-7: remove).”

**Standing Rule 2 applies.** Live search required before presenting each flag to the user.

## **3.5 Selecting the Right Precedent Filing**

**The precedent filing is the single most critical input to the process.** The firm manuals provide the process skeleton — the generic steps and their sequence. But they are written at a level of abstraction that leaves significant gaps when building a granular, step-by-step flowchart. They will say “draft risk factors” but will not tell you how many distinct categories of risk factors a real filing contains, how the Business section is actually structured, what the exact exhibit list looks like, or how the front-of-prospectus summary relates to the detailed sections.

The actual filed document fills those gaps because it is the finished product. You can reverse-engineer the production steps from the output: if the document contains a section, someone had to draft it; if there is an exhibit, someone had to compile it; if a reserve report or comfort letter is referenced, someone had to obtain it. The document itself is the most complete checklist of what had to be produced.

**Not just any precedent filing will do.** The precedent must match the target deliverable on at least two dimensions:

1. **Deliverable type.** An MLP S-1 for an MLP flowchart, a corporate S-1 for a corporate flowchart. The section structure, required exhibits, and disclosure requirements differ materially across entity types. An Oil & Gas MLP S-1 contains reserve disclosures (Reg S-X 4-10(a)), IDR waterfall mechanics, partnership agreement terms, and pass-through tax treatment — none of which appear in a tech company’s corporate S-1.

2. **Full amendment history.** Ideally, provide the complete filing history from initial confidential submission through final effectiveness. This includes the original DRS (if the confidential submission process was used), all DRS/A amendments filed during SEC confidential review, the public S-1 (the ‘flip’ from confidential to public), and all S-1/A amendments through effectiveness. The DRS/DRS-A sequence documents the SEC’s confidential review cycle — comment letters, response drafting, and iterative revisions — which directly informs the flowchart’s steps for SEC interaction. A single snapshot of the final effective version would miss this entire cycle.

**Example:** For the S-1 production flowchart, four public filings from the Mach Natural Resources transaction were used: the original S-1 (September 22, 2023) plus three S-1/A amendments (September 29, October 5, and October 16, 2023). Note: Mach did not use the confidential DRS process, so no DRS filings existed. For transactions that do use the DRS process (e.g., SOLV Energy, which had one DRS and three DRS/A filings before flipping public), the full DRS → DRS/A → S-1 → S-1/A sequence should be included.

**WARNING:** LIVE SEARCH REQUIRED. Search SEC EDGAR for recent filings of the same type to identify the best precedent match. Filter by SIC code, filing type, and date range. Compare candidates to select the one most analogous to the target deliverable in terms of entity structure, industry, and transaction complexity.

**Precedent selection procedure:** Search EDGAR full-text search (https://efts.sec.gov/LATEST/search-index?q=) for the 3–5 most recent filings of the same type and same SIC code, filed within the last 24 months. Rank candidates by: (a) closeness of entity structure to the target (e.g., MLP vs. corporate), (b) completeness of the amendment history (prefer filings with 2+ S-1/A amendments, which reveal the SEC review cycle), and (c) quality of counsel and advisors (well-known firms tend to produce more thorough filings that serve as better precedents). Select the candidate with the best combination of these factors.

**Once the precedent transaction is selected, follow the Precedent Filing Intake Protocol (Section 3.5.1) to systematically ingest all filings in the transaction’s history.** This protocol handles the practical constraint that filing sets often exceed the capacity of Project Files or a single conversation context. For file format, naming, and storage guidance, see Section 3.5.2.

## **3.5.1 Precedent Filing Intake Protocol**

A precedent transaction's filing history often includes multiple documents — confidential draft registration statements (DRS), amendments (DRS/A), the public registration statement (S-1 or equivalent), and public amendments (S-1/A or equivalent). Together, these filings can total tens of megabytes, exceeding the capacity of Project Files or a single conversation context window.

This protocol solves that constraint by processing filings sequentially and building a cumulative **Section Inventory** — a compact, structured record of every section, exhibit, structural element, expert reference, and regulatory citation found across all filings. The Section Inventory, not the full filing text, becomes the working precedent reference for deduplication (Section 3.4), role assignment (Section 3.6), and classification (Section 3.7).

**Persistent save requirement.** The LLM must save the Section Inventory as a downloadable Markdown file after every processing step — after the baseline (Step 2) and after every delta cycle (Step 3). This serves three functions: (a) context window insurance — if the conversation hits context limits mid-processing, the user can start a new conversation and upload the latest saved inventory to resume; (b) multi-session continuity — processing 6+ filings with user validation will likely span multiple sessions; and (c) rollback — if the user disputes a delta classification, they can revert to the prior version and reprocess from that point.

### **Step 1: Declare the Filing Manifest**

Before any filing is uploaded, the complete filing set must be established. **If the filing history was discovered during the Domain Acquisition Gate (Section 2 preamble), the LLM pre-populates the manifest** with the known filings (type, date, and EDGAR accession number if available) and presents it to the user for confirmation. The user confirms, adjusts, or adds filings — but never re-types information the LLM has already gathered. If the filing history was not discovered during the Domain Gate, the LLM prompts:

> *"How many precedent filings are in this transaction's filing history? Please list each one with: (a) filing type (e.g., DRS, DRS/A, S-1, S-1/A for registration statements; PREC14A, DEF 14A for proxy statements; 10-K, 10-K/A for annual reports — adapt to your deliverable type), (b) filing date, and (c) file name or identifier."*

The user provides the manifest. Example:

| # | Filing Type | Date | File Name |
|---|------------|------|-----------|
| 1 | DRS | 2025-05-09 | SOLV_DRS_2025-05-09.txt |
| 2 | DRS/A | 2025-06-18 | SOLV_DRSA_2025-06-18.txt |
| 3 | DRS/A | 2025-07-11 | SOLV_DRSA_2025-07-11.txt |
| 4 | DRS/A | 2025-12-19 | SOLV_DRSA_2025-12-19.txt |
| 5 | S-1 | 2026-01-16 | SOLV_S1_2026-01-16.txt |
| 6 | S-1/A | 2026-01-23 | SOLV_S1A_2026-01-23.txt |

The LLM records the manifest and confirms the total count: *"Acknowledged: 6 filings. I will process them in chronological order. Please upload Filing 1 of 6: DRS (2025-05-09)."*

**Chronological processing order is mandatory.** Filings must be processed from earliest to latest because this mirrors the actual production sequence. The delta reports (Step 3) track forward evolution — sections added, modified, and removed over time — which maps directly to the production steps the flowchart needs to capture.

**WARNING:** LIVE SEARCH REQUIRED. After the user provides the manifest, search SEC EDGAR to verify completeness. Confirm that no filings are missing from the manifest (e.g., an intermediate DRS/A the user may have overlooked). If the search reveals additional filings, present them to the user for confirmation before proceeding.

**Unavailable filings.** If a filing listed in the manifest cannot be obtained (e.g., a DRS that was withdrawn before the registration statement went public, or a filing that is not yet available on EDGAR), document the gap in the manifest with a note explaining why. Proceed with the available filings. The gap should be flagged during deduplication (Section 3.4) as a potential source of missing steps.

**SEC comment letters as supplementary input.** If SEC comment letters and company response letters are available on EDGAR for the precedent transaction (posted no earlier than 20 business days after effectiveness), they provide direct evidence of what the SEC required to be changed and how the company responded. These letters can be processed through this same protocol as supplementary filings — added to the manifest after the final S-1/A, with filing types "SEC Comment Letter" and "Company Response Letter." Whether to include them is a user decision: they add granularity to the SEC review steps in the flowchart but also add processing time. The LLM should ask:

> *"SEC comment letters and response letters for this transaction may be available on EDGAR. Would you like to include them in the manifest? They would add detail to the SEC review cycle steps but are not required."*

**The LLM must not begin deduplication against the precedent (Section 3.4) until all available filings in the manifest have been processed through this protocol.** Source material extraction (Sections 3.1–3.3) may proceed in parallel, as those sections draw from firm guides and checklists, not from the precedent filing.

### **Step 2: Process the First Filing (Baseline)**

The user uploads the earliest filing (typically the DRS). The LLM reads the full text and produces the **Baseline Section Inventory** — a structured table with the following columns:

| Column | Description |
|--------|-------------|
| # | Sequential row number. |
| Section / Item | The section heading, exhibit number, or structural element as it appears in the filing (e.g., "Prospectus Summary," "Risk Factors — Commodity Price Risk," "Exhibit 3.1 — Certificate of Limited Partnership"). |
| Category | One of: Narrative Section, Financial Statement, Exhibit, Expert Reference, Structural Element (cover page, signature page, underwriter table, etc.), Regulatory Citation. |
| Approximate Length | Short qualitative indicator: Brief (< 1 page), Moderate (1–5 pages), Substantial (5+ pages). |
| Content Detail | A 1–3 sentence description of what the section contains — not just its title, but enough substance to enable meaningful delta comparison in Step 3. For example: "Describes three categories of commodity price risk: crude oil, natural gas, and NGL. References NYMEX strip pricing. Contains two sensitivity analysis tables." |
| Expert References & Consents | Names of any experts, firms, or third parties referenced or whose consent is included (e.g., "DeGolyer and MacNaughton — reserve report," "KPMG LLP — audit opinion," "Latham & Watkins — legal opinion"). |
| Regulatory Citations | Specific SEC rules, PCAOB standards, FINRA rules, exchange listing standards, or other regulatory requirements cited within the section (e.g., "Reg S-X Rule 4-10(a)," "PCAOB AS 3101," "FINRA Rule 5110"). |
| Cross-References | References to other sections within the filing or to other filings/exhibits (e.g., "See Exhibit 10.1 — Partnership Agreement," "Incorporated by reference from Form 8-K filed [date]"). |
| Change History | Filing-level audit trail for this row. Format: "Added F1 (DRS)." Updated at each delta cycle to record when the row was added, modified, or removed — e.g., "Added F1 (DRS); Modified F4 (DRS/A 12-19)." Enables retrospective review of the full evolution of any section without returning to conversation transcripts. |

**Deep extraction requirement.** The Section Inventory must go beyond top-level section headings. The LLM must also extract:

- **Embedded disclosure topics** that do not have their own section heading but represent distinct drafting work (e.g., related-party transactions discussed within MD&A, tax distribution mechanics embedded in the Partnership Agreement summary, environmental liabilities disclosed within Risk Factors).
- **All named expert references and consents**, wherever they appear in the filing (not just in the Experts section).
- **All regulatory citations**, which identify the specific rules governing each section's content and format.
- **All cross-references to exhibits and other filings**, which identify dependencies between sections.

These details are essential for Section 3.4, where the Section Inventory serves as the ground-truth checklist for identifying steps that no source material captured.

**SCOPE GATE (Section 2.1).** Apply the scope gate when building the Baseline Section Inventory. If a section's scope status is ambiguous, include it with a flag for user review.

**TIP:** USER INPUT REQUIRED. Present the Baseline Section Inventory to the user for validation before proceeding. The user may identify sections the LLM missed, correct categorizations, or flag sections that are out of scope.

**Save the Baseline Section Inventory.** The LLM saves the inventory as a Markdown file using the naming convention `[COMPANY]_Section_Inventory_after_F1_[TYPE].md` (e.g., `SOLV_Section_Inventory_after_F1_DRS.md`) and provides it to the user for download. The user should add this file to Project Files to ensure continuity if the conversation is interrupted or a new session is needed.

The LLM confirms: *"Baseline Section Inventory complete for Filing 1 of [N]. [X] sections, [Y] exhibits, [Z] expert references, [W] regulatory citations identified. File saved as [filename]. Please review. When confirmed, upload Filing 2 of [N]: [type] ([date])."*

### **Step 3: Process Each Subsequent Filing (Delta)**

For each subsequent filing (2 through N), the user uploads the file. The LLM reads it and produces a **Delta Report** against the cumulative Section Inventory. The Delta Report classifies each section into one of five change categories:

| Change Type | Description |
|-------------|-------------|
| **Added** | Sections, exhibits, or elements that appear in this filing but not in any prior filing. |
| **Modified** | Sections that exist in prior filings and have materially changed in content, structure, or length (e.g., risk factors expanded, financial statements updated to a new period, pricing information added). |
| **Possibly Modified** | Sections where the LLM cannot determine with certainty whether content changed, based on the prior Section Inventory's Content Detail column. Flagged for user confirmation. |
| **Removed** | Sections or elements present in prior filings but absent from this filing. |
| **Unchanged** | Sections with no material change (listed for completeness but not described in detail). |

**What constitutes a "material change":** A section is flagged as Modified if any of the following are true: (a) substantive content was added or removed (not just formatting or pagination changes), (b) financial data was updated to a different reporting period, (c) new risk factors, exhibits, or expert references were added within the section, or (d) placeholder text (e.g., blank price range) was filled in with actual figures.

**Partial amendments.** Some amendments (particularly the final S-1/A before effectiveness) restate only the sections that changed, not the entire prospectus. These partial amendments are significantly smaller than the full filing. In these cases, sections absent from the amendment should be classified as **Unchanged** (carried forward from the prior filing), not as Removed. The LLM should check whether the amendment's cover page or introductory text states language such as "This Amendment No. [X] amends and restates the following sections..." or "This Amendment No. [X] is being filed to update..." to determine whether it is a full or partial restatement. If the amendment is partial, only the sections it explicitly restates should be evaluated for delta changes; all other sections carry forward unchanged.

After producing the Delta Report, the LLM updates the cumulative Section Inventory to reflect the current state (updating the Content Detail column for all Modified and Added sections, and appending the filing reference to the Change History column for every row classified as Added, Modified, or Removed — e.g., "Modified F3 (DRS/A 07-11)") and presents both to the user.

**WARNING: USER INPUT REQUIRED — HARD GATE.** The LLM must present the complete Delta Report to the user and receive confirmation before proceeding to the next filing. “Present” means the user can see: (a) the classification of every section (Added/Modified/Possibly Modified/Removed/Unchanged), (b) content detail for all Added and Modified sections, (c) the updated cumulative Section Inventory. The user must resolve all “Possibly Modified” flags — confirming whether a change occurred or reclassifying as Unchanged. Do not claim the delta is “complete” without displaying it. Do not proceed to the next filing until the user explicitly confirms.

**Save the updated Section Inventory.** After each delta cycle, the LLM saves the updated cumulative inventory as a new versioned file: `[COMPANY]_Section_Inventory_after_F[X]_[TYPE].md` (e.g., `SOLV_Section_Inventory_after_F3_DRSA.md`). The user should download and add to Project Files, replacing the prior version. The prior version may be retained locally for rollback purposes.

The LLM confirms: *"Delta Report complete for Filing [X] of [N]. [A] added, [M] modified, [P] possibly modified (user review needed), [R] removed, [U] unchanged. Cumulative Section Inventory updated to [total] items. File saved as [filename]. Please review. When confirmed, upload Filing [X+1] of [N]: [type] ([date])."*

**Resuming after a session break.** If processing is interrupted (conversation limit, session timeout, or deliberate multi-session plan), the user starts a new conversation and uploads the most recent saved Section Inventory file plus the Filing Manifest. The LLM reads both, identifies which filing was last processed, and prompts the user to upload the next filing in sequence. No reprocessing of prior filings is needed.

### **Step 4: Produce the Final Cumulative Section Inventory**

After the last filing in the manifest has been processed and the user has validated the final Delta Report, the LLM produces the **Final Cumulative Section Inventory**. This document contains three parts:

**Part 1: Current State Table.** Every section, exhibit, and structural element present in the final effective filing, with all columns from Step 2, fully updated to reflect the final state.

**Part 2: Amendment History Summary.** A compact record of how the filing evolved across the full amendment lifecycle (e.g., DRS → S-1/A for registration statements; PREC14A → DEF 14A for proxy statements):

- Number of confidential or preliminary filings (e.g., DRS + DRS/A for registration statements; PREC14A for proxy statements).
- Number of public or definitive filings (e.g., S-1 + S-1/A; DEF 14A + DEFA14A).
- Key changes at each stage (1–2 sentences per filing, drawn from the Delta Reports).
- Total elapsed time from first filing to final filing or effectiveness.

**Part 3: SEC Review Cycle Indicators.** Evidence of SEC comment-and-response activity inferred from the amendments:

- Number of amendment rounds during confidential review (DRS/A count).
- Time gaps between amendments (which suggest SEC comment letter turnaround).
- Sections that changed most across amendments (likely targets of SEC comments).
- Any large time gap between amendments (which may indicate stale financial statement refresh — e.g., the SOLV transaction had 5 months between the July 2025 DRS/A and the December 2025 DRS/A, consistent with a fiscal year-end FS update).

**WARNING:** LIVE SEARCH REQUIRED. After producing the Final Cumulative Section Inventory, search for the SEC's comment letters and company response letters for the precedent transaction on EDGAR. If available and not already included in the manifest, cross-reference the comment letters against the sections flagged as most-changed in Part 3 to validate the SEC Review Cycle Indicators. If the user opted to include comment letters in the manifest (see Step 1), this validation has already been performed during delta processing.

**TIP:** USER INPUT REQUIRED. The user must review and approve the Final Cumulative Section Inventory before the LLM proceeds to deduplication (Section 3.4). This is the last quality gate before the Section Inventory becomes the working precedent reference.

**Save the Final Cumulative Section Inventory.** The LLM saves the final inventory as `[COMPANY]_Section_Inventory_Final.md` (e.g., `SOLV_Section_Inventory_Final.md`) and provides it to the user for download. This file should be added to Project Files as a persistent reference for all subsequent workflow steps. It replaces all prior versioned inventory files in Project Files (though the user may retain prior versions locally for rollback).

The LLM confirms: *"All [N] filings processed. Final Cumulative Section Inventory contains [X] sections, [Y] exhibits, [Z] expert references, [W] regulatory citations. Amendment history shows [A] confidential rounds and [B] public rounds over [T] months. File saved as [filename]. Ready to proceed to deduplication against the precedent upon your approval."*

### **Step 5: Use the Section Inventory as the Precedent Input**

From this point forward, the Final Cumulative Section Inventory replaces the full filing text as the working precedent reference for:

- **Section 3.4 (Deduplicate Against Precedent):** The Section Inventory serves as the ground-truth checklist. If the inventory lists a section, exhibit, expert reference, or regulatory citation that no source material captured, that item implies a missing production step. Walk every row of the inventory against the merged step list.
- **Section 3.6 (Role Assignment):** Expert References & Consents in the inventory identify which roles were involved (e.g., a reserve engineer report implies the Reserve Engineer role; a legal opinion implies Company Counsel).
- **Section 3.7 (LLM Classification):** The Content Detail and Regulatory Citations columns provide context for whether a section's production is LLM-capable (e.g., a section requiring application of PCAOB AS 3101 is External; a section that compiles publicly available data may be LLM).

**When full text is needed.** If the LLM needs to examine the actual text of a specific section for any downstream step (e.g., to understand the internal structure of risk factors for sub-step expansion in Section 3.7.2, or to resolve an ambiguous role assignment in Section 3.6), it should request that the user upload the relevant filing and specify the section:

> *"To determine the sub-step structure for risk factor drafting, I need to see the Risk Factors section from the final S-1/A. Can you upload SOLV_S1A_2026-01-23.txt?"*

This is a targeted, on-demand retrieval — not a re-processing of the entire filing. The LLM should identify the specific section and the specific reason for the request.

---

## **3.5.2 Practical Notes on Precedent File Management**

This subsection provides operational guidance for managing precedent files. It is separate from the methodological protocol in Section 3.5.1.

**File format for uploads.** Precedent filings should be converted to plain text (.txt) before upload. HTML versions from SEC EDGAR can be stripped of markup using a text extraction tool (e.g., BeautifulSoup in Python). Plain text files are typically 30–60% smaller than the source HTML and contain all content the LLM needs. Do not upload PDF versions — they are the largest format and may not parse cleanly.

**Naming convention for precedent files.** Use the format: `[COMPANY]_[FILING_TYPE]_[YYYY-MM-DD].txt`. Examples: `SOLV_DRS_2025-05-09.txt`, `SOLV_S1A_2026-01-23.txt`, `MNR_S1_2023-09-22.txt`. This convention ensures files sort chronologically and are immediately identifiable.

**Naming convention for Section Inventory files.** Use the format: `[COMPANY]_Section_Inventory_after_F[#]_[TYPE].md` for intermediate versions (e.g., `SOLV_Section_Inventory_after_F1_DRS.md`, `SOLV_Section_Inventory_after_F3_DRSA.md`) and `[COMPANY]_Section_Inventory_Final.md` for the final version. This convention enables clear versioning and rollback identification.

**Project Files vs. per-conversation upload.** If the total filing set fits within Project Files capacity, load all files there for persistent access. If it does not (as is typical for transactions with 4+ filings), keep the Instruction Manual and completed deliverables in Project Files, and upload precedent filings per conversation as directed by the protocol in Section 3.5.1. The Section Inventory files, which are compact, should always be stored in Project Files — replacing the prior version after each delta cycle so that the latest state is always available.

**Multiple precedent transactions.** If the flowchart draws on more than one precedent transaction (e.g., a primary precedent plus a secondary comparator), run the protocol in Section 3.5.1 separately for each transaction. Maintain separate Section Inventories, clearly labeled by transaction (e.g., `SOLV_Section_Inventory_Final.md` and `MNR_Section_Inventory_Final.md`).

**Text extraction from HTML filings.** If the LLM has access to a code execution environment (e.g., Claude's computer tool), it can perform the HTML-to-text conversion directly. The procedure is: (a) user uploads the HTML file, (b) LLM strips all HTML tags and collapses whitespace using BeautifulSoup or equivalent, (c) LLM saves the resulting .txt file for download. This avoids requiring the user to perform the conversion manually.


## **3.6 Assign Single-Role Ownership**

For each deduplicated step, determine which single role currently performs it (see Section 10.1). The role assignments come from the source materials and the precedent transaction — not from abstract reasoning. If a source says “securities counsel drafts risk factors,” that step is assigned to Company Counsel.

Where sources use working-group labels (e.g., “Business Section Working Group”), decompose the working group into its constituent roles and assign the step to the one role that holds the pen or makes the decision. Collaborative involvement by other roles is captured in the RACI matrix, not in the flowchart.

**WARNING:** LIVE SEARCH REQUIRED. When the source materials are ambiguous about which role performs a step, search for practice-specific guidance. For example, whether the FINRA Rule 5110 submission is filed by company counsel or underwriter counsel is a niche practice question that may require searching FINRA guidance or law firm client alerts to resolve.

**Propose-and-validate workflow:** The LLM generates the initial proposed role assignment for every step, then presents the complete list to the user for validation. Do not wait for the user to assign each role. The procedure is:

1. Study the source materials and precedent filing to identify which role each source attributes each step to.

2. Conduct live search for any step where the source materials are ambiguous or silent about which role performs it. Search for practice-specific guidance, law firm client alerts, or regulatory procedures that clarify the responsible party.

3. Propose a single-role assignment for each step, citing the source or search result that supports it.

4. Present the complete proposed role-to-step assignment list to the user for review.

5. The user validates, overrides, or approves each assignment. Genuinely ambiguous cases — where reasonable practitioners would disagree about who holds the pen — require the user's definitive call.

**TIP:** USER INPUT REQUIRED. The user's overrides are final. The LLM's proposals are informed starting points derived from source materials and live search, not determinations.

**Standing Rules 1 and 2 apply.** Self-audit before presenting; live search required before presenting each flag.

## **3.7 Classify by LLM Capability and Expand**

**Before beginning classification, verify you have completed the LLM Capability Acquisition Gate (Section 2.3.1).** The searches required by that gate provide the factual basis for distinguishing LLM from Hybrid — without them, classifications will systematically skew conservative.

With the deduplicated, role-assigned step list in hand, classify each step into one of the four LLM capability tiers (LLM / Hybrid / Human / External) per Section 2.3. Then expand only the LLM and Hybrid steps into granular sub-steps. Human and External steps remain as single nodes.

The result is the final node list that goes into the Mermaid flowchart.

**WARNING:** LIVE SEARCH REQUIRED. Before classifying a step, you must understand what it actually entails. Search for the specific procedure, tool, or regulatory requirement involved. For example, you cannot determine whether “Edgarize the S-1” is LLM-capable without searching what Edgarization involves (HTML/XBRL tagging, EDGAR-specific formatting, filing system constraints).

**Propose-and-validate workflow:** The LLM generates the initial proposed classification for every step, then presents the complete list to the user for validation. Do not wait for the user to classify each step. The procedure is:

1. Study the source materials and precedent filing to understand what each step actually involves.

2. Conduct live search for any step whose procedure, tooling, or regulatory requirements are not clear from the source materials alone.

3. Propose a tier (LLM / Hybrid / Human / External) for each step, with a brief rationale explaining why.

4. Present the complete proposed classification table to the user for review.

5. The user validates, overrides, or approves each classification. Only after user approval should the chart be expanded.

The user’s overrides are final. The LLM’s proposals are informed starting points, not determinations — the user’s operating environment, risk tolerance, and compliance culture may shift classifications in either direction.

**Step classification table format.** The classification table presented to the user must contain the following columns at minimum:

| Column | Content |
|--------|---------|
| # | Sequential step number. |
| Step Name | Descriptive name of the parent work step (e.g., "Draft risk factors"). |
| Role | The single role assigned in Section 3.6. |
| LLM Tier | LLM, Hybrid, Human, or External. |
| Rationale | One sentence explaining why this tier was assigned (cite the classification question from Section 2.3 that was decisive). |
| Sub-steps | Number of proposed sub-steps if LLM or Hybrid (0 for Human and External). |

This table is the **companion step classification table** referenced in Section 2.3. It is saved as a deliverable alongside the Mermaid code and RACI matrix. The tier designations in this table — not in the flowchart — are the authoritative record of LLM capability classification.

**File format:** Save as a standalone Markdown file (`.md`) using the naming convention `[PROJECT]_Step_Classification.md` (e.g., `EP_S1_Step_Classification.md`). Markdown is preferred because the table renders on GitHub and can be pasted into conversation context. Alternatively, it may be included as an additional sheet in the RACI workbook (`.xlsx`) — but a standalone file must always exist as the canonical version, since the RACI workbook may lag during iterative editing.

**Standing Rules 1 and 2 apply.** Self-audit before presenting; live search required before presenting each flag.

### **3.7.1 Consolidation Pass**

After classification is validated by the user, perform a consolidation pass across all tiers — LLM, Hybrid, Human, and External. The purpose is to eliminate nodes that do not represent independently trackable work. Consolidation is complete when every remaining node passes the retention test below — not when a specific count is reached.

**Governing principle.** A node should exist only if it represents a distinct unit of work that someone could independently assign, track, or gate. If two or three sequential same-role nodes produce one deliverable with no intermediate branching, no external dependency on the midpoint, and no role change, they are one task described in multiple lines — not multiple tasks.

**When to perform.** This pass runs after Section 3.7 classification is approved and before sub-step expansion (Section 3.7.2). It is mandatory. A second consolidation pass should be run after sub-step expansion is complete (see Section 3.7.2), because expansion may introduce new sequential same-role chains that are candidates for re-merger.

**The Five-Factor Node Retention Test.** For every pair or triple of sequential same-role nodes (A → B, or A → B → C), apply the following five tests. **If all five tests return “No,” merge the nodes. If any test returns “Yes,” retain them as separate.**

| # | Test | Question | If “Yes” → Retain separately |
|---|------|----------|------------------------------|
| 1 | **Intermediate deliverable** | Does Node A produce a work product that is reviewed, approved, circulated, or filed independently of Node B? | The intermediate output has its own audience. |
| 2 | **Branching or convergence** | Is Node A a fan-out point (multiple downstream targets) or Node B a fan-in point (multiple upstream sources)? | Merging would obscure parallel structure. |
| 3 | **Role boundary** | Do Nodes A and B have different role assignments? | Merging across roles is prohibited (Section 3.6). |
| 4 | **External dependency** | Does any node outside the A → B chain depend on A’s output without also depending on B’s output? | Another workstream needs the intermediate result. |
| 5 | **Distinct regulatory or exhibit identity** | Does Node A correspond to a separately identifiable regulatory filing, exhibit, form, or compliance gate? | Regulatory artifacts have independent review cycles. |

**Merge procedure.** When merging nodes A and B (or A, B, C):

1. **Node ID:** Use the base name without numeric suffixes (e.g., `CompCounsel_RiskFactors1` + `CompCounsel_RiskFactors2` → `CompCounsel_RiskFactors`).

2. **Display text:** Write a single description covering the full scope of the merged action. Preserve the most specific regulatory references from the constituent nodes.

3. **Role assignment:** Inherits from the constituent nodes (which must share the same role per Test 3).

4. **LLM tier:** If constituent nodes had different tiers, assign the tier that requires more human involvement (Human > External > Hybrid > LLM). The merged node’s tier reflects the highest-touch component.

5. **Connections:** The merged node inherits all inbound edges of the first constituent and all outbound edges of the last constituent. Intermediate edges between merged nodes are eliminated.

6. **Styles:** The merged node receives the role-based style of its constituents.

**Common merge patterns.** These patterns recur across document types and should be merged unless a retention test is triggered:

| Pattern | Example | Why merge |
|---------|---------|-----------|
| Prepare-then-file | “Calculate fees” → “Submit fee payments” | Filing is the mechanical completion of the preparation |
| Draft-then-distribute | “Draft compliance plan” → “Distribute to working group” | Distribution is ministerial; the deliverable is the plan |
| Identify-then-evaluate | “Identify non-GAAP measures” → “Evaluate Reg G compliance” | Evaluation is the purpose of the identification |
| S-1 section sub-categories | “Draft industry risks” → “Draft UP-C risks” → “Draft IPO risks” | Sub-topics within one section drafted as a unit by one person |
| Parallel admin actions, same role | “Reserve ticker” + “Select transfer agent” | Same role, same timing, no external dependency on either individually |

**Patterns that must not be merged.**

| Pattern | Example | Why retain |
|---------|---------|------------|
| Distinct exhibits or forms | Charter (Ex 3.2) / Bylaws (Ex 3.4) / LLC Agreement (Ex 10.4) | Each is separately filed with independent review cycles |
| Fan-out source nodes | Node feeding multiple parallel tracks | Merging obscures the parallel structure |
| Cross-role sequences | CC drafts → Acct reviews | Different accountability |
| Regulatory gate nodes | “Submit DRS” / “File FINRA” / “Submit Nasdaq” | Each triggers a separate regulatory review |
| Decision diamonds and milestones | All | Never merge decisions or milestones into action nodes |

**Single-role verification (mandatory before proceeding to 3.7.2).** After consolidation, verify that every parent work step has exactly one role assigned. Consolidation must not merge steps across roles — “same role” is the first consolidation condition. If any parent work step shows a split role assignment (e.g., “CC/Issuer,” “Acct/CC,” “UW/CC/Issuer”), it must be resolved before proceeding:

1. **Determine which single role holds the pen** — the role that produces the work product or makes the decision. Apply the same logic as the initial role assignment in Section 3.6: the role that holds the pen gets R (Responsible); all other participating roles are captured as C (Consulted) or I (Informed) in the RACI matrix.

2. **If the consolidated step cannot be assigned to a single role without distorting the work,** split it back into separate single-role parent steps. A multi-role parent step usually indicates that steps from different roles were incorrectly merged during consolidation.

Do not proceed to sub-step expansion (Section 3.7.2) with any multi-role assignments remaining. Present unresolved ambiguities to the user for a definitive call using the standard flag table format (Section 3.6).

After consolidation, record the parent work step count for reporting in deliverables.

### **3.7.2 Sub-Step Expansion Limits**

When expanding LLM and Hybrid steps into sub-steps, observe these limits:

* **Target 2–3 sub-steps per expanded parent.** Individual exceptions up to 5 are acceptable for genuinely complex steps, but the average across all expanded parents should not exceed 3.

* **After expansion, verify the total node count** (action + decision + milestone) does not exceed 300. If it does, consolidate parent steps before expanding further. The expansion phase should not be used to compensate for insufficient consolidation in earlier stages.

* **Cross-role sub-steps.** During sub-step expansion, a parent step may decompose into sub-steps performed by different roles. Each sub-step node gets its own role color regardless of the parent step’s role assignment. If a sub-step belongs to a different role, it is a standalone node in the flowchart — not visually grouped under the parent’s color.

* **Post-expansion consolidation.** After expansion is complete, re-run the consolidation pass per Section 3.7.1 on the expanded node set. Expansion frequently introduces sequential same-role sub-step chains that are candidates for re-merger.

## **3.8 Build the RACI Matrix During Production**

**The RACI matrix is not just an output deliverable — it is an active quality control instrument during flowchart production.** Build it concurrently with the flowchart, not after. The RACI serves four production functions:

1. Role assignment validation. Declaring R/A/C/I for every node × every role forces you to confront ambiguities that the flowchart alone does not surface — cases where a node is assigned to one role in the chart but another role is actually Responsible, or where an uninvolved role should have been Consulted.

2. Completeness check. The RACI is a flat table where every node gets a row. This makes it easy to spot nodes that exist in the Mermaid code but have no RACI row (or vice versa). It is a cross-reference tool that catches orphans and omissions.

3. Scope discipline. If you cannot fill out a coherent R/A/C/I row for a step, that step may be poorly defined, too vague, or a duplicate. The RACI forces precision in step definitions.

4. Capturing collaborative reality. The flowchart shows only the Responsible party (one color, one role). The RACI is where you document which roles are Consulted (provide input before/during the work) and which are Informed (notified after completion). Without the RACI, this information is lost entirely.

Add each node to the RACI as soon as it is added to the Mermaid code. Do not defer RACI construction to the end of the project — by then, the validation benefits are lost and reconciliation becomes costly. See Section 10.5 for the full RACI construction specification.

## **3.9 Intermediate Verification Gates**

At each stage boundary in the workflow, the LLM must run a verification gate before proceeding. Each gate consists of a small set of mechanical checks that can be performed by counting, pattern-matching, or cross-referencing within the current work product. The LLM records the gate results in a brief verification table (metric, expected, actual, pass/fail) and only proceeds if all checks pass. If any check fails, the LLM corrects the error within the current stage before moving forward.

These gates supplement — they do not replace — the final validation checklist in Section 11. Section 11 checks the completed deliverable as a whole; intermediate gates check bounded work products at the stage where they are produced.

**Gate 1 — After role assignment (Section 3.6 complete).**

| Check | Rule |
|-------|------|
| Role count | Count distinct roles. Must be ≥3 and ≤10 for a single deliverable. |
| Abbreviation uniqueness | No two roles share the same abbreviation. |
| Color assignment | Every role has exactly one Okabe-Ito color. No color is assigned to more than one role. |
| Naming convention | Every abbreviation is a CamelCase string with no spaces, hyphens, or special characters. |

**Gate 2 — After step inventory and node ID assignment (Section 3.7 classification and consolidation complete, before Mermaid code generation).**

| Check | Rule |
|-------|------|
| Node count | Total nodes (action + decision + milestone) ≤ 300. |
| One role per node | Every action node is assigned to exactly one role from the taxonomy. No multi-role assignments remain. |
| Node ID format | Every action node ID matches `[RoleAbbrev]_[TaskName]`. TaskName is CamelCase, begins with an uppercase letter (see Section 6.2), and contains no spaces or special characters. |
| Decision ID format | Every decision node ID begins with `Decision_`. |
| Milestone ID format | Every milestone node ID begins with `Milestone_`. |
| Kill-path milestone IDs | Every kill-path milestone ID begins with `Milestone_` and includes a termination keyword (e.g., Milestone_IPOAbandoned, Milestone_S1Withdrawn). |
| Intermediate milestone IDs | Every intermediate milestone ID begins with `Milestone_` and includes a process-state keyword (e.g., Milestone_DraftFreeze, Milestone_S1Filed). |
| Scope boundary | Every node falls within the defined scope (Section 2.1). |

**Gate 3 — After connection wiring (all `-->` edges written).**

| Check | Rule |
|-------|------|
| No orphans | Every node has at least one inbound edge, except the start milestone. Every node has at least one outbound edge, except the end milestone and any kill-path milestones. |
| Decision diamonds: exactly 2 outgoing edges | Every `Decision_` node has exactly two outgoing edges with labels. |
| No self-loops | No edge connects a node to itself. (See Section 4.3 for the prohibition and alternative pattern.) |
| Edge labels on decisions | Every outgoing edge from a `Decision_` node has a label (pipe syntax). |
| Start/end connectivity | A path exists from the start milestone to every terminal milestone (end milestone and any kill-path milestones) through connected nodes. |
| Edge count | Total edges < 500 (Mermaid rendering limit). |

**Gate 4 — After style application (all `style` statements written).**

| Check | Rule |
|-------|------|
| Style count = action node count + styled external-party diamonds | The number of `style` statements equals the number of action nodes (rectangles) plus any external-party decision diamonds styled per Section 8.4. |
| Every action node styled | Every action node ID appears in exactly one `style` statement. |
| No decision/milestone styled | No `Decision_` or `Milestone_` node has a `style` statement — except external-party decision diamonds per Section 8.4. |
| Colors match role | Every `style` statement's `fill:` color matches the Okabe-Ito color assigned to that node's role in the taxonomy. |
| External-party diamonds | External-party decision diamonds (if any) are styled with the corresponding external role's color per Section 8.4. |

**Gate 5 — After phase assignment (all subgraphs written).**

| Check | Rule |
|-------|------|
| Every node inside a subgraph | No action, decision, or milestone node exists outside all subgraphs. |
| No node in multiple subgraphs | Every node appears inside exactly one `subgraph ... end` block. |
| Phase numbering | Subgraph IDs are sequential (P1, P2, ..., Pn) with no gaps. |
| Phase merge test | Every adjacent phase pair was evaluated against the three-condition merge test (Section 9.1). |
| Track subgraph IDs | If tracks are used (Section 9.1.1): track subgraph IDs follow the pattern P[#][letter]_T[#] (e.g., P1C_T1). Tracks are used when a phase contains 3+ parallel workstreams owned by different roles. |
| Deliverable grouping IDs | If deliverable groupings are used (Section 9.1.2): deliverable grouping subgraph IDs follow the pattern D_[SectionName] (e.g., D_RiskFactors). |
| No top-level tracks | If tracks are used: track subgraphs are nested inside phase or sub-phase subgraphs, not at the top level. |

**Gate 6 — After RACI matrix construction (Section 3.8 / Section 10.5 complete).**

| Check | Rule |
|-------|------|
| Row count = total node count | The RACI matrix has one row per node in the Mermaid code. |
| Node ID match | Every node ID in the Mermaid code appears as a row in the RACI, and vice versa. |
| Single R per row | Every row has exactly one role marked R (Responsible). |
| R matches flowchart role | The R column in every RACI row matches the role-color assignment of that node in the flowchart. |
| Tier column populated | Every ACTION row has a Tier value (LLM/Hybrid/Human/External); every DECISION and MILESTONE row is blank. |
| Gate Type column populated | Every DECISION row has a Gate Type value (QC Checkpoint / External Gate); every ACTION and MILESTONE row is blank. |
| Conditional column | Populated only for variant-dependent nodes; all other rows blank. |
| Deliverable Section column populated | Every ACTION row has a Deliverable Section value; DECISION and MILESTONE rows blank or 'N/A'. |
| Functional Category column populated | Every ACTION row has a Functional Category value from the controlled vocabulary; DECISION and MILESTONE rows blank. |
| Counting basis documented | The deliverable explicitly states the counting basis (total nodes vs. parent work steps). |

**Execution timing.** Gates 1–2 and 6 execute within the Section 3 workflow. Gates 3–5 execute during Mermaid code production (Sections 4–9) — run each gate immediately after completing the corresponding code block, before proceeding to the next.

**Failure protocol.** If any gate check fails:
1. Identify the failing item(s).
2. Correct the error within the current stage's work product.
3. Re-run the gate.
4. Proceed only when all checks pass.

Do not defer a gate failure to Section 11. The purpose of intermediate gates is to prevent error propagation.

# **4\. Basic Mermaid Syntax**

After completing step identification (Sections 3.1–3.7), with role assignments validated, classifications approved, the RACI under concurrent construction (Section 3.8), and the pre-code verification gates passed (Section 3.9), produce the Mermaid flowchart code using the syntax and conventions defined in Sections 4–9. The code must be a complete, renderable file — not a fragment or outline.

## **4.1 The Flowchart Declaration**

Every flowchart file begins with a YAML front-matter title block, followed by the flowchart declaration and direction:

\---  
title: "S-1 Production Flowchart \- Oil & Gas MLP IPO"  
\---  
flowchart TD

Direction options:

* TD or TB — Top to bottom (standard for process flows; mandatory for this team).

* LR — Left to right.

* RL — Right to left.

* BT — Bottom to top.

**WARNING:** All flowcharts produced under this methodology must use TD (top-down) unless explicitly approved otherwise.

## **4.2 Node Anatomy**

A node consists of three parts: a **name (ID)**, a **shape** (determined by brackets), and **display text** (what appears inside the shape).

NodeName\["Display text that appears in the shape"\]

## **4.3 Connections (Arrows)**

Nodes are connected with arrows:

NodeA \--\> NodeB                    %% solid arrow  
NodeA \--- NodeB                    %% solid line, no arrow  
NodeA \-.-\> NodeB                   %% dashed arrow  
NodeA \--\>|"label text"| NodeB      %% arrow with label  
NodeA \-- "label text" \--\> NodeB    %% alternate label syntax

**Self-loops (prohibited).** Do not connect a node to itself (`A --> A`). Mermaid's dagre layout engine handles self-loops inconsistently — they may render as invisible edges, cause node displacement, or distort surrounding layout. The semantic intent of a self-loop (waiting for an external event, then re-checking) should instead be expressed as a two-node wait-and-recheck pattern:

```
%% Illustrative — substitute your project's role abbreviation for SEC_
Decision_FINRAClear{"FINRA clearance received?"}
SEC_AwaitFINRA["SEC/FINRA/Exchange: Await FINRA clearance response"]
Decision_FINRAClear -->|"Pending"| SEC_AwaitFINRA
SEC_AwaitFINRA --> Decision_FINRAClear
Decision_FINRAClear -->|"No Objections"| NextStep
```

This adds one node per wait loop but produces reliable rendering and makes the wait state explicitly visible as a distinct step.

## **4.4 Branching**

To connect one upstream node to multiple downstream nodes, list the upstream node multiple times:

Decision\_Ready \--\>|Yes| NextStep  
Decision\_Ready \--\>|No| PreviousStep

**TIP:** The second time a node is referenced, you do not need to repeat its shape or display text—just use the node ID.

## **4.5 Line Breaks in Display Text**

Use \<br/\> to insert line breaks inside node display text. This keeps nodes compact:

CompCounsel\_DraftRisks\["Company Counsel: Draft commodity,\<br/\>regulatory & environmental risk factors"\]

## **4.6 Comments**

Use %% for inline comments. Mermaid ignores everything after %% on a line:

%% This is a comment  
%% \============================================  
%% PHASE 1: S-1 Input Assembly (Steps 1-16)  
%% \============================================

## **4.7 Hyperlinks**

To make a node clickable:

click NodeName "https://example.com" "Tooltip text" \_blank

# **5\. Node Shapes**

This methodology uses exactly three node shapes. Do not use any other shapes.

| Shape | Syntax | Use For | Example |
| :---- | :---- | :---- | :---- |
| Rectangle | \["text"\] | Action steps | CompCounsel\_DraftRisks\["Company Counsel: Draft risk factors"\] |
| Diamond | {"text"} | Decision points | Decision\_VDRReady{"VDR ready for S-1 drafting?"} |
| Stadium (rounded) | (\["text"\]) | Start, intermediate, end, and kill-path milestones | Milestone\_Effective(\["SEC DECLARES EFFECTIVE"\]) |

## **5.1 Decision Diamonds**

Keep decisions binary (Yes/No) whenever possible. If you have more than two branches, chain sequential binary decisions rather than using a multi-branch diamond.

Decision labels on outgoing arrows use the pipe syntax:

Decision\_VDRReady \--\>|Yes| NextStep  
Decision\_VDRReady \--\>|No| LoopBackStep

**Descriptive labels:** When the outcomes have domain-specific names that readers would recognize, use descriptive labels instead of generic Yes/No. Examples: |No Objections| / |Unreasonable| for FINRA decisions, |No \- Stale| / |Yes \- Current| for a staleness check. The descriptive label must still map to a binary structure (two outgoing arrows), but replaces Yes/No with terms that convey domain meaning.

**Waiting states:** Self-loops are prohibited (see Section 4.3). For decision nodes that represent waiting states, use the two-node wait-and-recheck pattern defined in Section 4.3.

**Kill-path branches.** When a decision diamond can terminate the process (e.g., board decides not to proceed, SEC issues a stop order), one of its two outgoing edges leads to a kill-path milestone. The kill-path milestone uses the stadium shape and ALL-CAPS text:

```
Decision_SECClear -->|"Yes"| NextStep
Decision_SECClear -->|"No - Stop Order"| Milestone_IPOAbandoned(["IPO ABANDONED — END"])
```

A kill-path branch replaces one of the two binary outcomes — typically the "No" or negative branch. If the negative outcome can lead to either remediation (loop-back) *or* termination (kill), chain two sequential decision diamonds: the first asks whether the issue is resolvable, the second routes to remediation or kill path. Do not add a third outgoing edge to a single diamond.

Kill-path milestones are terminal — no outbound edges. They do not receive role-based styling (default Mermaid styling, same as other milestones). Each kill-path milestone counts toward the total node count but not the parent work step count.

**Decision diamond density (diagnostic, not a gate).** As a readability guideline, target no more than 1 decision diamond per 8–10 action nodes. This ratio is a diagnostic indicator — if exceeded, review whether decisions can be consolidated, but do not omit a decision diamond that the process requires solely to stay within this ratio. If multiple sequential steps each have an approval gate (e.g., six individual agreement approvals), consolidate into a single gate after the group is complete rather than inserting individual diamonds.

**WARNING:** Decision diamonds and milestones receive default Mermaid styling (no custom color). Only action steps receive role-based colors. See Section 8.4 for the exception for external-party decision diamonds.

# **6\. Node Naming Convention**

Every node ID must follow this exact pattern:

\[RoleAbbreviation\]\_\[TaskName\]

## **6.1 Role Abbreviations**

The table below shows the role abbreviations for the S-1 / Oil & Gas MLP IPO flowchart. **For a different document type, define your own role taxonomy** by extracting distinct parties from the precedent filing’s cover page, signature pages, and expert consents (see Section 8.2), then assign each role a short abbreviation following the same naming convention. Do not reuse the S-1 role table below for a different deliverable type — derive the taxonomy fresh from the precedent filing. The Decision and Milestone rows are universal and apply to all document types.

For the S-1 production flowchart (S-1 MLP only), use these exact abbreviations (no variations):

| Role | Abbreviation | Example Node ID |
| :---- | :---- | :---- |
| Company Counsel | CompCounsel | CompCounsel\_DraftRisks |
| Issuer (Management) | Issuer | Issuer\_HoldOrgMeeting |
| Underwriters | UW | UW\_JoinAccelRequest |
| Underwriter Counsel | UWCounsel | UWCounsel\_ReviewS1Draft |
| Accountants | Acct | Acct\_CommenceAudit |
| Reserve Engineer | ResEng | ResEng\_PrepareReserveReport |
| Tax Counsel | TaxCounsel | TaxCounsel\_DraftPassThrough |
| SEC / FINRA / Exchange | SEC | SEC\_DeclaresEffective |
| Decision (no role) | Decision | Decision\_VDRReady |
| Milestone (no role) | Milestone | Milestone\_IPOTrigger |

## **6.2 TaskName Rules**

1. Use CamelCase (e.g., DraftRiskFactors, not draft\_risk\_factors).

2. Begin with a verb (Draft, Review, Compile, Verify, Prepare, Flag, File, Submit, etc.).

3. Be specific enough to distinguish from other steps (DraftCommodityRisks, not DraftRisks if multiple risk sections exist).

4. If a role has sub-steps, append a number suffix (e.g., CompCounsel\_DraftRisks1, CompCounsel\_DraftRisks2).

5. If a well-known domain term begins with a numeral (e.g., 409A, 10b-5, 144A), prepend a verb so the TaskName remains verb-initial: `CompCounsel_Obtain409A`, `UWCounsel_Prepare10b5`, `CompCounsel_Analyze144A`. Do not use the bare numeral as the TaskName start.

# **7\. Node Display Text**

Display text is what appears inside the rendered shape. It follows this mandatory format:

"Role: Verb \+ object (with context)"

## **7.1 Format Rules**

1. Always begin with the full role name followed by a colon (e.g., Company Counsel:, Issuer:, Accountants:).

2. Follow with a verb \+ object phrase. Use crisp, active-voice language.

3. Add parenthetical context where it helps (source document, regulatory cite, or clarifying detail).

4. Use \<br/\> for line breaks to keep nodes compact. Target 2–3 lines maximum.

**WARNING:** LIVE SEARCH REQUIRED. When drafting display text that includes regulatory citations, technical terms, or procedural details, verify the exact citation or terminology via live search. Writing “Reg S-X 4-10(a)” rather than “SEC reserve rules” requires confirming the precise rule reference. Inaccurate citations undermine the credibility of the entire flowchart.

## **7.2 Good vs. Bad Examples**

| Good (Verb \+ Object) | Bad (Vague / Noun-y / Passive) |
| :---- | :---- |
| "Company Counsel: Draft commodity,regulatory & environmental risk factors" | "Risk factor drafting" |
| "Issuer: Compile acreage, drilling locations,infrastructure from VDR\>Ops" | "Operations data" |
| "Accountants: Prepare Item 8 FS & Notes(PCAOB standards; 3yr non-EGC)" | "Financial statement preparation" |

## **7.3 Decision Node Text**

Decision nodes should be phrased as Yes/No questions:

Decision\_VDRReady{"VDR ready for S-1 drafting?"}  
Decision\_FSCurrent{"Financial statements current?"}

## **7.4 Milestone Node Text**

Milestone nodes should be brief and declarative. The milestone keyword (TRIGGER, END, or equivalent) must be ALL CAPS. The descriptive text that follows may use sentence case for readability. The end milestone should be fully capitalized to signal termination visually.

Milestone\_IPOTrigger(\["TRIGGER: Sponsor decides to pursue IPO"\])  
Milestone\_Effective(\["SEC DECLARES REGISTRATION\<br/\>STATEMENT EFFECTIVE\<br/\>— END —"\])

**TIP:** Start milestones use keyword caps (TRIGGER:) with sentence-case description. End milestones use full caps to provide a strong visual signal that the process is complete.

**Intermediate milestones** mark significant process boundaries within the flowchart — points where the nature of work changes (e.g., content drafting ends and mechanical filing begins, or initial filing occurs and SEC review begins). They use the stadium shape and a keyword prefix in ALL CAPS:

```
Milestone_DraftFreeze(["FREEZE: S-1 draft content locked<br/>— filing preparation begins"])
```

Intermediate milestones are not decision points — they have exactly one inbound and one outbound edge. They signal a phase transition to the reader without gating the flow. Use sparingly: 1–3 per flowchart. Overuse dilutes their signaling value.

**Kill-path milestones** (Section 2.2) are not intermediate milestones — they are terminal. They have inbound edges only and no outbound edges. The ALL-CAPS convention applies to both, but their edge connectivity differs: intermediate milestones pass through (1-in, 1-out); kill-path milestones terminate (N-in, 0-out).

# **8\. Color System**

## **8.1 Why Colors, and Why This Palette**

Each role gets a unique fill color so the responsible party is identifiable at a glance. This methodology uses the Okabe-Ito colorblind-safe palette exclusively, for two reasons:

1. It provides 8 visually distinct colors that remain distinguishable under the most common forms of color vision deficiency (deuteranopia, protanopia), covering approximately 8% of the male population.

2. It provides up to 8 visually distinct colors, supporting role taxonomies of 3–8 roles with no ambiguity. Unused colors remain in reserve for future document types.

The text color rule (white text on dark fills, black text on light fills) is also an accessibility decision, ensuring sufficient contrast for readability regardless of the background color.

Do not substitute, add, or modify colors without explicit approval.

## **8.2 Color-to-Role Mapping**

**Deriving the role taxonomy:** Extract all distinct parties named in the precedent filing’s cover page, signature pages, legal opinion exhibits, comfort letter references, and expert consents. Each distinct functional role becomes one entry in the taxonomy. For the S-1 production flowchart, this yielded 8 roles: Company Counsel, Issuer (Management), Underwriters, Underwriter Counsel, Accountants, Reserve Engineer, Tax Counsel, and SEC/FINRA/Exchange. A different deliverable type may yield fewer or more roles.

| Role | Fill Hex | Stroke | Text Color | Color Name |
| :---- | ----- | :---- | :---- | :---- |
| Company Counsel | \#0072B2 | \#000000 | \#FFFFFF | Blue |
| Issuer (Management) | \#E69F00 | \#000000 | \#000000 | Orange |
| Underwriters | \#56B4E9 | \#000000 | \#000000 | Sky Blue |
| UW Counsel | \#009E73 | \#000000 | \#FFFFFF | Bluish Green |
| Accountants | \#F0E442 | \#000000 | \#000000 | Yellow |
| Reserve Engineer | \#D55E00 | \#000000 | \#FFFFFF | Vermillion |
| Tax Counsel | \#CC79A7 | \#000000 | \#000000 | Reddish Purple |
| SEC/FINRA/Exchange | \#000000 | \#000000 | \#FFFFFF | Black |

## **8.3 Style Syntax**

Colors are applied via style statements at the bottom of the Mermaid file, after all node and connection definitions:

%% \--- Company Counsel (Blue \#0072B2) \---  
style CompCounsel\_DraftRisks stroke:\#000,fill:\#0072B2,color:\#FFF

%% \--- Issuer / Management (Orange \#E69F00) \---  
style Issuer\_HoldOrgMeeting stroke:\#000,fill:\#E69F00,color:\#000

## **8.4 What NOT to Style**

Decision diamonds and milestone nodes receive default Mermaid styling. Do not assign colors to them. This ensures they are visually distinct from role-based action steps.

**Exception — external-party decision diamonds:** When a decision diamond represents an external party’s action or determination (not an internal process gate), style it with that party’s role color. This applies to regulatory gates where showing the responsible external party adds information — e.g., SEC comment letter decisions styled black (\#000000). These styled diamonds still use the diamond shape and are still counted as decision nodes, but their color communicates that an outside party controls the outcome.

# **9\. Structure & Organization**

## **9.1 Subgraphs (Phases)**

Use subgraphs to group nodes into logical phases or sections. Subgraphs render as labeled boxes around their child nodes:

subgraph P1\["Phase 1: S-1 Input Assembly"\]

    subgraph P1A\["VDR Readiness"\]  
        Milestone\_IPOTrigger(\["TRIGGER: Sponsor decides to pursue IPO"\])  
        Issuer\_EstablishVDR\["Issuer: Establish & populate VDR folders"\]  
    end

    subgraph P1B\["Section Assignment & Kickoff"\]  
        Issuer\_AssignSectionOwners\["Issuer: Assign S-1 section owners"\]  
    end

end

**TIP:** Subgraphs can be nested. Use a parent subgraph for each major phase and child subgraphs for sub-phases within it.

**Phase consolidation.** After all nodes are placed in subgraphs, review adjacent phases for merge candidates. Phases should represent genuinely distinct process stages — not internal methodology distinctions, content sub-categories, or LLM capability tiers. Two adjacent phases are candidates for merger if all three of the following conditions are true:

1. **No regulatory or process gate at the boundary.** There is no decision diamond, milestone, or external-party gate separating the two phases. (If Phase N+1 cannot begin until an external event occurs — e.g., SEC declares no further comments — the phases represent distinct stages.)

2. **No primary actor shift.** The role with the plurality of nodes does not change from Phase N to Phase N+1. (If different teams drive different phases, the phases represent different workstreams.)

3. **No distinct entry condition.** Phase N+1’s entry condition is simply “Phase N is complete” — not a separately identifiable precondition. (If Phase N+1 requires a condition beyond Phase N’s completion, such as “all SEC comments resolved,” it is a distinct stage.)

If all three conditions are met, merge the phases. Use the broader title (or create a combined title ≤ 8 words), adopt the lower phase number, and renumber subsequent phases to close the gap. Update all phase references in the RACI matrix and summary slide.

Phases must not be merged solely to reach a target count, and phases must not be split solely to stay under a cap. The number of phases is determined by the process structure — not by a numeric guideline.

**Subgraph naming convention:** Subgraph IDs follow a hierarchical pattern: P \+ phase number for top-level phases (P1, P2, P3), and P \+ phase number \+ letter suffix for sub-phases (P1A, P1B, P2A). This mirrors the nesting structure and keeps IDs short.

### **9.1.1 Tracks (Named Parallel Workstreams)**

Within a phase, parallel workstreams that run concurrently but serve different functional purposes should be labeled as named tracks. A track is a child subgraph within a phase that groups nodes by workstream identity (e.g., "Track: Reserve Engineering", "Track: Audit", "Track: Legal DD"). Tracks make parallelism visible in the subgraph structure, not just in the connection wiring.

**Track naming convention:** Subgraph ID = P\[phase#\]\[letter\]\_T\[track#\] (e.g., P1C\_T1, P1C\_T2). Display label = "Track: \[Workstream Name\]".

**Tracks vs. sub-phases:** Sub-phases (P1A, P1B) represent sequential stages within a phase. Tracks represent concurrent workstreams within a sub-phase or phase. A sub-phase may contain multiple tracks:

```
subgraph P1C["Phase 1C: Direct Input Production"]
    subgraph P1C_T1["Track: Reserve Engineering"]
        ResEng_PrepareReserveReport[...]
    end
    subgraph P1C_T2["Track: Audit"]
        Acct_CommenceAudit[...]
    end
    subgraph P1C_T3["Track: Legal DD"]
        CompCounsel_ReviewMatlContracts[...]
    end
end
```

**When to use tracks:** Use tracks when a phase contains 3+ parallel workstreams owned by different roles. Do not use tracks for 2 parallel nodes — fan-out/fan-in (Section 9.2) is sufficient. Tracks add a nesting level; verify render stability in mermaid.live after adding.

### **9.1.2 Deliverable Grouping (Optional Third Nesting Level)**

For flowcharts where multiple nodes contribute to producing a single identifiable section or exhibit of the deliverable, an optional third nesting level groups nodes by the output they produce. This level sits between the track/sub-phase level and the individual nodes.

**Deliverable grouping naming convention:** Subgraph ID = D\_\[SectionName\] (e.g., D\_RiskFactors, D\_MDA, D\_ExhibitIndex). Display label = "Deliverable: \[Section Name\]".

```
subgraph D_RiskFactors["Deliverable: Risk Factors"]
    CompCounsel_DraftCommodityRisks[...]
    CompCounsel_DraftRegEnvRisks[...]
    CompCounsel_DraftStructuralRisks[...]
end
```

**When to use:** Use deliverable grouping when 3+ nodes produce components of a single identifiable output (e.g., multiple risk factor categories within Item 1A). Do not use for single-node deliverables. This level is optional — it adds visual structure but also adds nesting complexity. Test in mermaid.live before committing; Mermaid's dagre layout handles 3 nesting levels but may produce unexpected layouts at 4+.

**Maximum nesting depth:** Phase → Sub-phase/Track → Deliverable → Node. Four levels maximum. Do not add further nesting.

## **9.2 Parallel Workstreams (Fan-Out / Fan-In)**

Real-world processes are not purely sequential. After certain trigger points, multiple independent workstreams kick off simultaneously and run in parallel until their outputs converge at a downstream integration point. The flowchart must represent this accurately.

**Fan-out:** One node connects to multiple downstream nodes, each representing an independent parallel track. This is coded by listing the upstream node multiple times with different targets:

Issuer\_HoldOrgMeeting \--\> ResEng\_PrepareReserveReport  
Issuer\_HoldOrgMeeting \--\> Acct\_CommenceAudit  
Issuer\_HoldOrgMeeting \--\> CompCounsel\_ReviewMatlContracts  
Issuer\_HoldOrgMeeting \--\> Issuer\_CompileOpsData

**Fan-in:** Multiple parallel tracks converge back into a single node when their outputs are needed together for the next step:

CompCounsel\_DraftBizStrategy \--\> Decision\_SectionsConsistent  
Acct\_PrepareFinStmts \--\> Decision\_SectionsConsistent  
TaxCounsel\_VerifyTaxAccuracy \--\> Decision\_SectionsConsistent

Without fan-out/fan-in, the chart artificially serializes activities that actually happen concurrently. This misrepresents the real timeline, makes the process look much longer than it is, and obscures the fact that different roles are working independently during the same period.

**WARNING:** Do not serialize steps that are actually parallel just because it produces a cleaner vertical line. If activities are independent of each other, they must branch and converge. Ask: “Does Step B truly require Step A’s output to begin, or can they run at the same time?” If they can run simultaneously, fan out.

**TIP:** USER INPUT REQUIRED. Logical independence and practical independence are not the same thing. Only someone who has lived through the production process knows the real dependencies — for example, whether the reserve engineer actually starts before or after the organizational meeting, or whether auditors wait for certain VDR materials before commencing. The user must validate which steps are truly parallel vs. sequential.

## **9.3 Code Layout Order**

Organize your Mermaid file in this exact order:

1. YAML front-matter (title block).

2. Flowchart declaration (flowchart TD).

3. Architecture comment block (immediately after the flowchart declaration, before subgraphs). A structured comment documenting the flowchart's structural design decisions. This block has zero visual impact — it is metadata for Claude and human maintainers. Include:

```
%% ============================================================
%% ARCHITECTURE NOTES
%% ============================================================
%% Structural pattern: [e.g., "6 sequential phases with fan-out
%%   in P1C (4 parallel tracks) and fan-in at Decision_SectionsConsistent"]
%% Cross-phase loops: [e.g., "Decision_FSCurrent loops back to
%%   Acct_UpdateFS on No; Decision_SECClear loops to CompCounsel_DraftResponse on No"]
%% Sync points: [e.g., "Decision_SectionsConsistent requires convergence
%%   of all P2 drafting tracks before P3 filing"]
%% Kill paths: [e.g., "Milestone_IPOAbandoned reachable from Decision_SECClear"]
%% Error propagation: [e.g., "Failed QC gates in P4 route back to P2 amendment nodes,
%%   not to P1 input assembly"]
%% Total nodes: [N] (action: [X], decision: [Y], milestone: [Z])
%% Total edges: [E]
%% ============================================================
```

This block is mandatory for flowcharts with 60+ nodes. For smaller diagrams, it is optional but recommended.

4. Subgraph definitions with all nodes declared inside them.

5. Connection statements (after all subgraphs, grouped by phase).

6. Style definitions (at the very end, grouped by role).

Use comment banners to separate sections:

%% \============================================================  
%% PHASE 1: S-1 Input Assembly  
%% \============================================================

## **9.4 Connection Placement**

Place all connection statements outside of subgraphs, after the subgraph definitions. Group them by phase with comment headers. This keeps the subgraph blocks clean and makes the flow logic easy to audit.

**Cross-phase connections:** Connections that span across phase boundaries — where an arrow connects a node in one phase to a node in a different phase — are expected, particularly for feedback loops (e.g., a failed staleness check in Phase 5 routing back to an amendment step in Phase 4). Group cross-phase connections with the originating phase’s connection block. Add a comment noting the target phase for clarity.

## **9.5 Style Placement**

Place all style statements at the very end of the file, grouped by role with comment headers. This makes it easy to verify color assignments during review.

**Style section legend:** Before the first style statement, include a comment block listing every role, its color name, hex code, text color, and which node categories receive default styling. This serves as an in-code reference for anyone editing the file:

%% \============================================================  
%% STYLE DEFINITIONS — Okabe-Ito Colorblind-Safe Palette  
%% \============================================================  
%%  
%% Color Legend (8 roles):  
%% Company Counsel          \= Blue           \#0072B2  (white text)  
%% Issuer (Management)      \= Orange         \#E69F00  (black text)  
%% Underwriters             \= Sky Blue       \#56B4E9  (black text)  
%% Underwriters' Counsel    \= Bluish Green   \#009E73  (white text)  
%% Accountants              \= Yellow         \#F0E442  (black text)  
%% Reserve Engineer         \= Vermillion     \#D55E00  (white text)  
%% Tax Counsel              \= Reddish Purple \#CC79A7  (black text)  
%% SEC/FINRA/Exchange       \= Black          \#000000  (white text)  
%%  
%% Decision diamonds: default Mermaid styling (except external-party diamonds)  
%% Start/End milestones: default Mermaid styling

After the legend, group style statements by role with comment headers:

%% \--- Company Counsel (Blue \#0072B2) \---  
style CompCounsel\_ReviewMatlContracts stroke:\#000,fill:\#0072B2,color:\#FFF  
style CompCounsel\_ReviewRelPartyTxn stroke:\#000,fill:\#0072B2,color:\#FFF  
...

%% \--- Issuer / Management (Orange \#E69F00) \---  
style Issuer\_EstablishVDR stroke:\#000,fill:\#E69F00,color:\#000  
...

# **10\. Handoffs & Responsibilities**

Failures in real-world workflows most often occur at transitions between people or teams. The flowchart must make handoffs painfully explicit.

## **10.1 Single-Role Ownership, Not Working Groups**

In practice, complex transactions organize around working groups — cross-functional teams where counsel, management, bankers, and accountants all collaborate on the same sections. However, working groups defeat the purpose of the flowchart. A working group is not a single accountable party; it is a collection of roles.

The rule is: assign each step to the one role that currently performs it. One node, one color, one responsible party. The collaborative reality — multiple roles consulting, reviewing, or being informed — is captured in the companion RACI matrix (C and I columns), not in the flowchart.

This single-role ownership principle is what makes the color system work. Without it, nodes would need multiple colors or ambiguous labels, and the chart would lose its ability to show accountability at a glance.

## **10.2 Scope Determines Role Distribution**

Once you assign single-role ownership, the resulting distribution of steps across roles is a direct consequence of the scope decision (Section 2.1). Expect it to be lopsided — and resist the impulse to “rebalance” it.

**Example:** In the S-1 production flowchart, Company Counsel owns 44 of 76 steps while Underwriters own only 1\. This initially seemed wrong, but it was correct: the S-1 is a legal document, so counsel holds the pen for most of it. The underwriters’ heavy lifting (book building, pricing, allocation) happens outside S-1 production. Had the scope been “the IPO” rather than “the S-1,” the distribution would look entirely different.

A seemingly lopsided role distribution should be interrogated against the scope, not artificially balanced. If it tracks to the deliverable being mapped, it is correct.

**Distribution validation procedure:** If the role distribution appears lopsided, cross-check each high-count role’s steps against the deliverable’s section structure. For every step assigned to the dominant role, confirm it maps to a distinct section, exhibit, or activity in the precedent filing. If each step has a unique counterpart in the filing, the distribution is correct. If steps appear duplicative or artificially split, consolidate them. The distribution is a diagnostic output of the methodology, not a target to be engineered.

## **10.3 In-Chart Handoff Rules**

1. Label every action node with the primary role performing it (via both the display text prefix and the node color).

2. When a handoff occurs (Role A to Role B), ensure the arrow clearly connects the two differently-colored nodes.

3. For decision-based handoffs, use labeled arrows (Yes/No) so the reader knows which outcome triggers the handoff.

## **10.4 Companion Deliverables**

The flowchart alone cannot capture all handoff detail. Pair it with:

* A RACI matrix (8 roles × all steps) — see Section 10.5 for the full construction specification. The RACI workbook includes the Legend sheet (role colors, glossary, methodology notes — see Section 10.5.4) and analytical sheets (tier breakdowns, inputs/outputs — see Sections 10.5.6–10.5.7).

* A step classification table documenting the four-tier LLM capability classification (LLM/Hybrid/Human/External) with rationale for each step — see Section 3.7. This is the authoritative record of tier designations.

* A Summary slide (single 16:9 slide) — the visual entry point for the deliverable package — see Section 10.6.

* A Cover Memo (1–2 pages) framing the deliverable package for its target audience — see Section 10.7.

* A Phase Overview Chart — a simplified Mermaid diagram showing only the top-level phases as boxes with arrows between them — see Section 10.8.

**Deliverable package summary.** The complete package comprises six deliverables: (1) Mermaid flowchart, (2) RACI matrix workbook, (3) Step classification table, (4) Summary slide, (5) Cover Memo, (6) Phase Overview Chart. Items 1–3 are the core analytical deliverables; items 4–6 are presentation/framing deliverables.

**File inventory.** The naming convention uses the pattern `[PROJECT]_[Deliverable]_[Qualifier].[ext]`, where PROJECT is a short identifier (e.g., "EP_S1" for the S-1 flowchart project). All output files:

| # | Deliverable | Format | Naming Convention | Example |
|---|------------|--------|-------------------|---------|
| 1 | Mermaid flowchart | `.md` | `[PROJECT]_Flowchart_Mermaid_vF.md` | `EP_S1_Flowchart_Mermaid_vF.md` |
| 2 | RACI matrix workbook | `.xlsx` | `[PROJECT]_RACI_Matrix.xlsx` | `EP_S1_RACI_Matrix.xlsx` |
| 3 | Step classification table | `.md` | `[PROJECT]_Step_Classification.md` | `EP_S1_Step_Classification.md` |
| 4 | Summary slide | `.pptx` | `[PROJECT]_Flowchart_Slide_vF.pptx` | `EP_S1_Flowchart_Slide_vF.pptx` |
| 5 | Cover Memo | `.docx` or `.md` | `[PROJECT]_Cover_Memo.docx` | `EP_S1_Cover_Memo.docx` |
| 6 | Phase Overview Chart | `.md` | `[PROJECT]_Phase_Overview.md` | `EP_S1_Phase_Overview.md` |

Intermediate files (Section Inventories, Filing Manifest) use the naming conventions in Sections 3.5.1–3.5.2. Use `_vF` suffix for final versions; use `_vT[##]` for test/working versions.

**TIP:** The flowchart shows who currently performs each step. LLM-capable designations and other metadata go in the companion table, never in the flowchart itself.

## **10.5 RACI Matrix Construction Specification**

This section defines the structure, assignment rules, and validation requirements for building a RACI matrix that matches the flowchart.

### **10.5.1 Workbook Structure**

The RACI workbook contains seven core sheets plus optional chart embedding:

1. **RACI Matrix** — the primary grid (one row per node, one column per role, plus analytical columns). See Section 10.5.2.

2. **Legend** — RACI definitions, role color key, interpretive notes, precedent transaction reference, entity/deliverable definitions, key terms glossary, tier classification methodology, source material citations, and team attribution. See Section 10.5.4.

3. **Summary** — role × R/A/C/I count matrix, mechanically derived from the RACI Matrix sheet. See Section 10.5.5.

4. **Chapter-Level Tier Matrix** — deliverable sections × tier counts. See Section 10.5.6.

5. **Per-Phase Tier Breakdown** — phases × tier counts. See Section 10.5.6.

6. **Per-Phase Inputs & Outputs** — phase-level document flow. See Section 10.5.6.

7. **Team Allocation Matrix** — roles × tier counts. See Section 10.5.6.

Charts (Per-Phase Tier Bar Chart, Tier Distribution Donut Chart) may be embedded on the Summary sheet or placed on a dedicated Charts sheet (Sheet 8). See Section 10.5.7.

### **10.5.2 RACI Matrix Sheet Layout**

The RACI Matrix sheet has 18 columns:

| Column | Header | Content |
| :---- | :---- | :---- |
| A | \# | Sequential number (1 to N). Blank for phase separator rows. |
| B | Phase | Phase or sub-phase name (e.g., VDR Readiness, Phase 2B: S-1 Prospectus Drafting). |
| C | Node ID | Exact node ID from the Mermaid code (e.g., CompCounsel\_DraftRisks). Blank for phase separator rows. |
| D | Step Description | Display text from the Mermaid node (without \<br/\> tags). |
| E | Type | ACTION, DECISION, or MILESTONE. |
| F | Tier | LLM, Hybrid, Human, or External. Populated for ACTION rows only; blank for DECISION and MILESTONE rows. Values must match the companion step classification table. This is the highest-value column for LLM evaluation audiences. |
| G | Gate Type | QC Checkpoint or External Gate. Populated for DECISION rows only; blank for ACTION and MILESTONE rows. QC Checkpoint = internal quality verification. External Gate = outcome controlled by an outside party (SEC, FINRA, exchange). |
| H | Conditional | Deal-variant conditions under which this node executes. Blank for nodes that execute in all deals. Examples: "Skip if no selling unitholders", "2yr FS if EGC", "Skip if no IDRs". Approximately 10–15 nodes in a typical S-1 flowchart have conditional logic. |
| I | Deliverable Section | The section of the deliverable this node produces or contributes to. Examples for an S-1: "Item 1A: Risk Factors", "Item 7: MD\&A", "Exhibit 5.1: Legal Opinion". For a proxy statement: "Item 1: Election of Directors", "Item 2: Ratification of Auditors". Cross-cutting nodes (e.g., consistency checks, filing mechanics) marked "Cross-section." Derivable mechanically from node display text. |
| J | Functional Category | The type of work this node represents. Controlled vocabulary: Disclosure Drafting, Compliance Verification, Exhibit Assembly, Regulatory Filing, Due Diligence, Financial Production, Review \& QC, Tax \& Structure, Administrative. Derivable from node verbs and display text. |
| K | Company Counsel | R, A, C, I, or blank. |
| L | Issuer (Mgmt) | R, A, C, I, or blank. |
| M | Underwriters | R, A, C, I, or blank. |
| N | UW Counsel | R, A, C, I, or blank. |
| O | Accountants | R, A, C, I, or blank. |
| P | Reserve Engineer | R, A, C, I, or blank. |
| Q | Tax Counsel | R, A, C, I, or blank. |
| R | SEC/FINRA/Exch | R, A, C, I, or blank. |

**Column population responsibility.** Tier (F) and Functional Category (J) are LLM-derivable — the LLM proposes values based on the step classification table and node display text, and the user validates. Gate Type (G) and Deliverable Section (I) are mechanically derivable from existing data (Type column and display text respectively). Conditional (H) requires domain judgment — the LLM proposes based on source materials and precedent filing, and the user validates.

**Functional Category definitions (controlled vocabulary for column J):**

| Category | Definition | Example Nodes |
|----------|-----------|---------------|
| Disclosure Drafting | Producing narrative text for prospectus/filing sections. | Draft risk factors, draft Business section, draft MD&A |
| Compliance Verification | Checking adherence to specific regulatory rules or standards. | Verify Reg G compliance for non-GAAP measures, check Section 11 exposure |
| Exhibit Assembly | Compiling, formatting, or organizing filing exhibits. | Compile exhibit index, assemble subsidiary list, prepare signature pages |
| Regulatory Filing | Submitting documents to SEC, FINRA, exchange, or EDGAR. | File DRS on EDGAR, submit FINRA 5110, submit exchange listing application |
| Due Diligence | Reviewing source documents to identify facts, risks, or issues. | Review material contracts, review related-party transactions, review title documents |
| Financial Production | Preparing or auditing financial statements, tables, or schedules. | Prepare Item 8 financial statements, compute filing fee table, format capitalization table |
| Review & QC | Cross-checking deliverables for internal consistency, accuracy, or completeness. | Cross-reference S-1 sections, review draft for consistency, verify exhibit completeness |
| Tax & Structure | Analyzing or drafting tax opinions, structure memos, or partnership/entity provisions. | Draft pass-through tax disclosure, prepare tax opinion, analyze IDR waterfall |
| Administrative | Logistics, scheduling, coordination, or ministerial tasks with no substantive content. | Reserve ticker symbol, select transfer agent, establish VDR |

**Phase separator rows:** Insert a row with no number, no Node ID, no Type, and no RACI values at the start of each phase or sub-phase. The Phase column contains the phase name. These rows visually group the matrix by phase.

### **10.5.3 RACI Assignment Rules**

**Responsible (R):** The one role that performs the work. Must match the flowchart’s color assignment for that node. Every ACTION node gets exactly one R.

**Accountable (A):** The role that approves or owns the outcome. The table below applies to the S-1 MLP role taxonomy. **For a different deliverable type with a different role taxonomy (Section 6.1), derive the Accountable mapping fresh** by asking: "Who has the legal or contractual obligation to approve this role's work product?" The underlying principle — the engaging or liable party is Accountable — is universal, but the specific role-to-role mappings will differ. For example, in a proxy statement, the Board of Directors (not Issuer Management) may be Accountable for governance disclosures.

| If R is... | Then A is... | Rationale |
| :---- | :---- | :---- |
| Company Counsel | Issuer (Mgmt) | Registrant has Section 11 liability |
| Accountants | Issuer (Mgmt) | Registrant engages the auditors |
| Tax Counsel | Issuer (Mgmt) | Registrant is the taxpayer |
| Reserve Engineer | Issuer (Mgmt) | Registrant owns the reserves |
| UW Counsel | Underwriters | UWs engage their own counsel |
| Issuer (Mgmt) | (none shown) | R and A are the same party (see Note 5\) |
| SEC/FINRA/Exchange | (none) | External regulatory actions (see Note 4\) |
| Underwriters | (none shown) | R and A are the same party (see Note 5\) |

**Consulted (C):** Roles that provide input before or during the work. Assign C when:

* The step’s output feeds directly into another role’s subsequent work.

* The step requires technical input from a specialist (e.g., Tax Counsel consulted on tax-related disclosures drafted by Company Counsel).

* The step involves terms or provisions that affect another party’s interests.

**Informed (I):** Roles that are notified of the outcome after completion. Assign I when:

* The step has regulatory or filing implications that affect another role’s timeline.

* The step produces an output that another role will need to reference later (but does not need their input now).

* The step represents a milestone or status change that the broader working group should know about.

**Decision and milestone nodes:** Leave all RACI cells blank. These are process control points, not work steps.

**Propose-and-validate workflow for C/I assignments:** The Responsible (R) and Accountable (A) assignments follow deterministic rules (the tables above). The Consulted (C) and Informed (I) assignments require domain judgment. The LLM generates the initial proposed C/I assignments for every action node, then presents the complete matrix to the user for validation. The procedure is:

1. Study the source materials and precedent filing to understand the collaborative relationships between roles — which roles provide input to which steps, and which roles need to know about which outcomes.

2. Conduct live search for any step where the collaborative relationships are not clear. Search for how working groups are typically structured for this deliverable type, which roles attend which review cycles, and which regulatory filings trigger notification obligations.

3. Apply the C and I heuristics above to propose assignments for every action node × every role.

4. Present the complete proposed RACI matrix (including R, A, C, and I) to the user for review.

5. The user validates, overrides, or approves the C/I assignments. The user may identify consulting or notification relationships that the source materials do not document.

### **10.5.4 Legend Sheet**

The Legend sheet contains nine sections:

1. **RACI Definitions** — R (Responsible: performs the work), A (Accountable: approves/owns the outcome), C (Consulted: provides input before/during), I (Informed: notified after completion).

2. **Role Color Key** — the Okabe-Ito hex code and role name for each of the 8 roles (must match Section 8.2).

3. **Interpretive Notes** — document any rules that govern how the RACI should be read. At minimum, include:

   1. Decision and milestone nodes have no RACI assignments (process control points).
   2. Issuer (Management) is Accountable for most steps as the registrant with Section 11 liability.
   3. Underwriters are Accountable for UW Counsel steps as the engaging party.
   4. SEC/FINRA/Exchange steps have no separate Accountable — external regulatory actions.
   5. Where a role is both R and A, only R is shown.

4. **Precedent Transaction Reference** — Identifies the precedent filing used to build the step inventory. Include: entity name, CIK number, filing type, filing dates (first to last), amendment count, and EDGAR URL. Example: "Mach Natural Resources LP — CIK 0001990354 — S-1 filed 2023-09-22, 3 S-1/A amendments through 2023-10-16. EDGAR: \[URL\]." Provides a direct pointer to the training data source for engineering audiences.

5. **Entity & Deliverable Definitions** — Plain-language definitions of the entity type and the deliverable type, written in practitioner English (not statutory language). Two entries, 2–3 sentences each. Example: "An MLP is a publicly traded partnership — not a corporation — that typically owns midstream oil & gas infrastructure. Pass-through tax; K-1s, not 1099s." Makes the RACI self-contained for non-domain readers.

6. **Key Terms Glossary** — 10–15 domain-specific terms that appear in the flowchart node display text and would be unfamiliar to a general business or engineering audience. Each entry: term (color-coded to associated role using Okabe-Ito palette) + one-line definition (≤10 words). Include both regulatory terms (DRS, Reg S-K, PCAOB) and practitioner shorthand (red herring, gun-jumping, comfort letter, VDR). Term selection procedure: scan all node display text for acronyms, legal citations, and technical terms; select terms that appear multiple times and are essential to understanding the process.

7. **Tier Classification Methodology** — Documents the criteria used to classify steps into the four LLM tiers. Include: (a) the production capability basis (classification reflects whether the LLM can produce the work product, not whether a human reviews it afterward), (b) the classification factors (boilerplate density, data dependency, substantive judgment required, coordination complexity), (c) the Hybrid data-dependency clarification (steps where the LLM can produce filing-quality output but requires proprietary third-party data as inputs are Hybrid, not Human; if the step’s core activity is merely receiving and filing the third-party work product without substantive transformation, it is External, not Hybrid — see Section 2.3), and (d) a reference to the Instruction Manual for full tier definitions. This note explains what the Tier column (column F) means and how it was populated.

8. **Source Material Citations** — Lists the authoritative sources used to build the step inventory: (a) law firm guides and checklists (with firm name and publication title), (b) accounting firm roadmaps, (c) industry-specific primers, and (d) regulatory references (SEC rules, PCAOB standards, FINRA rules, exchange listing standards). Tells the reader where the methodology came from and provides additional source documents for independent verification.

9. **Team Roster & Attribution** — Lists the team members who produced the deliverable package, with name, role, and specific responsibilities (e.g., "Ezequiel Padilla — VDR Captain / Methodology Author: defined scope, validated role assignments, approved tier classifications. Claude Opus 4.6 — LLM Co-Producer: drafted Mermaid code, proposed tier classifications, built RACI matrix."). Establishes provenance for all judgments in the deliverable.

### **10.5.5 Summary Sheet**

The Summary sheet is a role × R/A/C/I count matrix, mechanically derived from the RACI Matrix sheet. It has 5 columns (Role, R count, A count, C count, I count), one row per role, and a Total row. All values are computed by counting the corresponding letter in each role’s column across all ACTION rows. This sheet is fully automatable and should be regenerated whenever the RACI Matrix changes.

### **10.5.6 Analytical Sheets**

The RACI workbook includes four analytical sheets that pivot the Matrix data into views optimized for different audiences. All four are mechanically derived from the Matrix sheet — they contain no independent data. When the Matrix changes, these sheets must be regenerated.

**Sheet 4: Chapter-Level Tier Matrix.** Rows = deliverable sections (from column I, Deliverable Section). Columns = LLM count, Hybrid count, Human count, External count, Total, Dominant Tier. One row per distinct deliverable section. Shows which sections of the deliverable have the highest automation opportunity. Derived from columns F (Tier) and I (Deliverable Section) of the Matrix sheet.

**Sheet 5: Per-Phase Tier Breakdown.** Rows = phases (from column B, Phase). Columns = LLM count, Hybrid count, Human count, External count, Total, LLM+Hybrid %. One row per phase/sub-phase. Shows which phases of the production process have the highest automation opportunity. Derived from columns B (Phase) and F (Tier) of the Matrix sheet.

**Sheet 6: Per-Phase Inputs & Outputs.** Rows = phases/sub-phases. Columns = Phase Name, Key Documents Ingested, Key Outputs Produced. Approximately one row per sub-phase. This sheet requires domain judgment — the LLM proposes values based on node display text and source materials; the user validates. It is not fully mechanically derivable because input/output relationships are not explicit in the Matrix data.

**Sheet 7: Team Allocation Matrix.** Rows = roles (from the R designations in role columns K–R). Columns = LLM count, Hybrid count, Human count, External count, Total. One row per role. Shows how each role's work distributes across automation tiers. Derived from the R designations in role columns (K–R) and column F (Tier) of the Matrix sheet. The SEC/FINRA/Exchange role should show all steps as External; any deviation indicates a classification error.

### **10.5.7 Workbook Charts**

The RACI workbook includes two charts that visualize the tier distribution data. Both are mechanically derived from the analytical sheets and must be regenerated when underlying data changes.

**Chart 1: Per-Phase Tier Bar Chart.** A stacked horizontal bar chart with one bar per phase. Each bar is segmented by tier (LLM, Hybrid, Human, External) using a four-color scheme: LLM = #0072B2 (blue), Hybrid = #E69F00 (orange), Human = #56B4E9 (sky blue), External = #000000 (black). Data source: Sheet 5 (Per-Phase Tier Breakdown). This chart shows at a glance which phases have the highest automation opportunity.

**Chart 2: Tier Distribution Donut Chart.** A four-slice donut chart showing the overall distribution of steps across LLM/Hybrid/Human/External tiers. Each slice labeled with tier name, count, and percentage. Data source: the Tier column (F) of the Matrix sheet, counting ACTION rows only. Same four-color scheme as Chart 1. This chart is the single most important visual for audiences evaluating LLM insertion opportunities.

Both charts may be embedded on the Summary sheet (Sheet 3) or placed on a dedicated Charts sheet (Sheet 8). If embedded on the Summary sheet, position them below the role × R/A/C/I count matrix.

**Color disambiguation note.** The tier color scheme reuses Okabe-Ito hex values for accessibility but carries a different semantic meaning than role colors in the flowchart. Blue (#0072B2) means "Company Counsel" in the flowchart but "LLM-tier" in the charts. The two color systems are never displayed in the same visual context — role colors appear in the flowchart and RACI role columns; tier colors appear only in the analytical charts.

## **10.6 Summary Slide Specification**

The summary slide is a single-page visual overview of the entire deliverable package. It serves as the entry point for stakeholders who need to understand the scope, structure, and LLM opportunity at a glance before diving into the flowchart or RACI. Most of its content is derived mechanically from the flowchart, RACI, and step classification table.

**Production timing:** The summary slide is produced after the Mermaid flowchart and RACI matrix are finalized and validated (Section 11). All numbers on the slide are derived from the final, validated deliverables. Do not produce the slide before validation is complete — premature production will require rework when numbers change.

### **10.6.1 Layout Architecture**

The slide uses a 3-column layout on a single widescreen (16:9) slide:

| Column | Width | Content |
| :---- | :---- | :---- |
| Left | \~25% of slide width | Context definitions \+ LLM Opportunity panel |
| Center | \~45% of slide width | Phase bar \+ flowchart thumbnail \+ stats \+ RACI link |
| Right | \~30% of slide width | Roles legend \+ Key Terms glossary |

Above all three columns, a header bar spans the full slide width containing the title, deliverable type, and attribution.

### **10.6.2 Header Bar**

* Left-aligned title: the deliverable name \+ em dash \+ entity/transaction type (e.g., “S-1 Production Flowchart — Oil & Gas MLP IPO”).

* Right-aligned attribution: the role and name of the deliverable owner (e.g., “VDR Captain: Ezequiel Padilla”). This is a user-supplied field.

**TIP:** USER INPUT REQUIRED. The attribution name and title must be provided by the user.

### **10.6.3 Left Column — Context Definitions**

The left column contains three short context boxes that define the key concepts for a non-specialist reader:

1. Entity type definition — 2–3 sentences explaining what the entity type is (e.g., what an MLP is) in plain, accessible language. Assume the reader is an intelligent professional without domain-specific expertise (e.g., a first-year associate or an engineer evaluating LLM deployment).

2. Deliverable type definition — 2 sentences explaining what the deliverable is (e.g., what an S-1 is) and what it contains.

3. Flowchart purpose statement — 1–2 sentences stating what the flowchart maps, with key stats: total work steps, number of roles, and approximate timeline.

**WARNING:** LIVE SEARCH REQUIRED. If you are unfamiliar with the entity type or deliverable type, search for plain-language definitions before writing these boxes. The definitions must be accurate and accessible.


**Formatting rules for context definitions:**
* Write definitions in practitioner plain-English, not statutory or textbook language. Write as if explaining to a first-year associate, not citing a statute. Example: "An MLP is a publicly traded partnership — not a corporation — that typically owns midstream oil & gas infrastructure. Pass-through tax; K-1s, not 1099s." Not: "A Master Limited Partnership (MLP) is an exchange-traded limited partnership that earns ≥90% of gross income from qualifying natural resource activities under IRC §7704."
* Do not use section subheadings (e.g., "ENTITY TYPE", "DELIVERABLE TYPE", "FLOWCHART PURPOSE") within the left column. Instead, use bold lead-in phrases: "**An MLP**…", "**An S-1**…", "**This flowchart**…".

### **10.6.4 Left Column — LLM Opportunity Panel**

Below the context boxes, display the LLM opportunity breakdown. This is derived mechanically from the step classification table:

* Header: “LLM OPPORTUNITY” in accent color (Okabe-Ito blue \#0072B2).

* For each of the 4 tiers (LLM, Hybrid, Human, External), display: percentage \+ step count \+ tier label.

* Percentages are computed on the parent work-step basis (not total nodes). Use the counting denominator defined in Section 11.1.

* Each tier is separated by a colored underline: blue (\#0072B2) for LLM, orange (\#E69F00) for Hybrid, sky blue (\#56B4E9) for Human, black (\#000000) for External.


**Formatting:** Each LLM tier row uses a colored horizontal bar (matching the tier's accent color) with the percentage in large bold type, the step count in parentheses, and the tier label below. Do not use plain text lists or horizontal rule separators.

### **10.6.5 Center Column — Phase Bar**

A horizontal row of colored boxes showing each phase’s abbreviated name (e.g., “P1: VDR & Setup”, “P2: S-1 Drafting”). Derive phase names from the flowchart’s top-level subgraphs. Use the Okabe-Ito blue (\#0072B2) as the box fill with white text.


**Formatting:** Phase bar boxes must accommodate the full phase name without truncation. If the flowchart has more than 8 phases, abbreviate phase names to fit (e.g., "P3: VDR & DD" not "P3: VDR Assembly & Due Diligence") or use a two-row layout.

### **10.6.6 Center Column — Flowchart Thumbnail**

Embed a small-scale rendering of the complete Mermaid flowchart as an image. This provides a visual preview of the chart’s structure, density, and color distribution.

**Procedure:** Render the Mermaid code in mermaid.live. Export as PNG at sufficient resolution (minimum 1200px wide). Embed in the center column at thumbnail scale, sized to show the overall shape and color patterns without requiring readability of individual node text.


**Formatting:** The LLM should leave a sized placeholder with dimensions matching the center column width (~45% of slide width) and approximately 50% of slide height. Include the instruction text: "Render in mermaid.live → Export as PNG → Insert here."

### **10.6.7 Center Column — Stats and Links**

Display three key stats derived from the flowchart:

* Total parent work steps (e.g., “76 work steps”).

* Total decision diamonds (e.g., “12 decisions”).

* Total phases (e.g., “6 phases”).

Include two functional hyperlinks:

* “Open Flowchart” — links to the Mermaid live rendering or the canonical Mermaid code file.

* “RACI Matrix: \[filename\]” — links to the RACI workbook file.


**Formatting:** Stats (parent work steps, decisions, phases) stack vertically to the left of the thumbnail, not horizontally below it. Use 28–36pt bold for numbers with a small label below each. Do not use display-size type (60pt+).

### **10.6.8 Right Column — Roles Legend**

Derived mechanically from the flowchart and RACI:

* Header: “ROLES (N)” in accent color (Okabe-Ito orange \#E69F00), where N is the number of roles.

* One row per role: Okabe-Ito color swatch \+ role name \+ step count on the parent work-step basis.

* Step counts must match the RACI Summary sheet’s R column and the flowchart’s style statement counts (per the cross-deliverable coherence rules in Section 11.1).


**Formatting:** Role swatches must use the exact Okabe-Ito hex values from Section 8.2. Provide the hex codes as a lookup table in the pptxgenjs code. Verify rendered swatch colors match the flowchart node colors.

### **10.6.9 Right Column — Key Terms Glossary**

A glossary of 10–15 domain-specific terms that appear in the flowchart and would be unfamiliar to a general business or engineering audience.

* Header: “KEY TERMS” in accent color (Okabe-Ito orange \#E69F00).

* Each entry: term in colored monospace font \+ one-line plain-language definition in gray.

* Term colors should use the Okabe-Ito palette where the term is associated with a specific role (e.g., “VDR” in orange for Issuer, “Section 11” in blue for Company Counsel). General terms use a neutral color.

**WARNING:** LIVE SEARCH REQUIRED. Verify the accuracy of every term definition via live search. Domain terms have precise legal or regulatory meanings that must not be approximated.

**Term selection procedure:** Scan all node display text in the Mermaid code for acronyms, legal citations, and technical terms. Select terms that: (a) appear multiple times in the flowchart, (b) are essential to understanding the process, and (c) would not be known to a general business reader. Exclude terms that are self-explanatory from context.


**Formatting:** Each term uses monospace bold for the abbreviation, rendered in an Okabe-Ito accent color (orange \#E69F00), followed by a space and a plain-text definition of ≤10 words on the same line. Include practitioner shorthand alongside regulatory terms. The target audience knows the law but not necessarily deal-execution jargon. Examples of practitioner terms to include: red herring, gun-jumping, comfort letter, DRS.

### **10.6.10 Visual Design Standards**

* Accent colors: use Okabe-Ito blue (\#0072B2) for the title, phase bar, and LLM-related headers. Use Okabe-Ito orange (\#E69F00) for section headers (ROLES, KEY TERMS).

* Fonts: use a clean sans-serif (e.g., Arial, Calibri) for body text. Use monospace (e.g., Consolas, Courier New) for key terms.

* Color swatches in the Roles legend must use the exact Okabe-Ito hex codes from Section 8.2.

* The slide must be legible when projected or viewed on screen. Minimum body text size: 10pt. Minimum header size: 14pt.

* No decorative elements. Every visual element must convey information.


**Footer:** The precedent citation belongs in the flowchart purpose statement (left column), not in a separate footer bar. If a footer is used, limit it to file references only.

**Column weight:** The center column (thumbnail + stats + hyperlinks) should carry approximately 45% of the visual weight. Left and right columns are reference panels, not primary content areas.

## **10.7 Cover Memo Specification**

The Cover Memo is a 1–2 page document that frames the deliverable package for its target audience. It answers three questions: What is this? Why does it exist? How should you use it?

**Production timing:** The Cover Memo is produced after all other deliverables are finalized. All references and statistics are derived from the final, validated deliverables.

### **10.7.1 Content Sections**

The Cover Memo contains four sections in this order:

1. **Transaction & Regulatory Context.** One paragraph explaining what the precedent transaction is, why it was selected, and what regulatory framework governs the deliverable. Include: entity name, entity type, deliverable type, governing statutes and rules (e.g., Securities Act §5, Reg S-K, PCAOB standards), and the rationale for selecting this specific precedent (recency, completeness of amendment history, quality of counsel). Include a sentence stating the input-first workflow assumption (Principle #5): "This flowchart assumes the VDR is fully populated before S-1 drafting begins; the phase sequence reflects this assumption." This section gives the reader enough context to understand the deliverable package without domain expertise.

**WARNING:** LIVE SEARCH REQUIRED. Verify all regulatory citations and transaction details before writing this section.

2. **Strategic Rationale.** One paragraph explaining why this document type was chosen as the target for LLM capability mapping. Address: (a) economic value per transaction, (b) boilerplate density and repeatability, (c) structured regulatory framework that provides clear production requirements, and (d) scalability across similar transactions. This section answers: "Why this document type first?"

3. **Evaluation Framing.** One paragraph stating how the deliverable package should be used for LLM evaluation. Include: (a) the tier classifications serve as ground truth (LLM-tier steps are the test targets), (b) the production sequence in the flowchart maps to the testing sequence (each LLM-tier node is a discrete test case), (c) success = the LLM produces filing-quality output that a senior reviewer would accept with only minor, non-substantive edits, and (d) the RACI matrix documents the inputs each step requires, enabling test case construction.

4. **Testing Questions.** A numbered list of 5–10 concrete evaluation questions that map directly to LLM-tier and Hybrid-tier nodes in the flowchart. Each question should be answerable by running the node as a test case. Examples: "Can Claude draft Item 1A risk factors from VDR source documents?", "Can Claude compile an exhibit index from EDGAR filing records?", "Can Claude compute a filing fee table per SEC Rule 457?", "Can Claude produce a redline comparison between S-1 amendment versions?" These questions convert the abstract tier classifications into actionable test specifications.

**TIP:** USER INPUT REQUIRED. The testing questions depend on the user's evaluation priorities. The LLM proposes questions derived from LLM-tier nodes; the user validates, reorders, or adds questions based on their team's testing roadmap.

### **10.7.2 Phase Overview Chart Embed**

The Phase Overview Chart (Section 10.8) is embedded in the Cover Memo as a visual centerpiece, positioned between the Strategic Rationale and the Evaluation Framing sections. Layout: use a half-page center-aligned chart between sections 2 and 3 — sections 1–2 appear above the chart; sections 3–4 appear below. If the memo exceeds 2 pages with this layout, move the chart to a labeled appendix ("Appendix A: Phase Overview") and replace the in-line embed with a cross-reference.

## **10.8 Phase Overview Chart Specification**

The Phase Overview Chart is a simplified Mermaid diagram showing only the top-level phases as nodes with arrows between them. It serves as a visual table of contents — a 30,000-foot view for stakeholders who need to understand the process structure before diving into the full flowchart.

**Structure:** One stadium-shaped node per phase, labeled with the phase number and name. Arrows connect phases in sequence. If the flowchart contains cross-phase loops (e.g., failed QC routing back to an earlier phase), include those as labeled loop-back arrows.

**Derivation procedure:** Extract the top-level subgraph names from the flowchart's Mermaid code. For each cross-phase connection in the flowchart (an edge connecting a node in Phase X to a node in Phase Y where X ≠ Y), add a labeled loop-back arrow between the corresponding phase nodes. Verify loop-back targets against the actual flowchart structure before committing.

**Example (illustrative — verify loop-back targets against actual flowchart):**

```
---
title: "S-1 Production — Phase Overview"
---
flowchart TD
    P1(["Phase 1: S-1 Input Assembly"])
    P2(["Phase 2: S-1 Drafting"])
    P3(["Phase 3: S-1 Filing"])
    P4(["Phase 4: SEC Review & Response"])
    P5(["Phase 5: Final S-1 Preparation"])
    P6(["Phase 6: Effectiveness"])
    P1 --> P2 --> P3 --> P4 --> P5 --> P6
    P4 -->|"Comments received"| P2
    P5 -->|"FS stale"| P2
    %% Staleness triggers FS update + re-drafting, routing back to drafting phase
```

**Styling:** Use default Mermaid styling (no role colors). The overview chart is role-agnostic — it shows process flow, not accountability.

# **11\. Validation & Testing**

Before a flowchart is considered complete, run through this checklist. This checklist is the final acceptance test. It assumes all intermediate verification gates (Section 3.9) have already passed during production. If any gate was skipped, run it now before proceeding.

1. Paste the full code into mermaid.live and confirm it renders without errors.

2. Verify every node is connected (no orphan nodes floating disconnected). Verify no self-loops exist (no edge connects a node to itself).

3. Confirm every action node has a style statement assigning its role color.

4. Confirm decision diamonds and milestones have no style statements, except for external-party decision diamonds which are styled per Section 8.4.

5. Verify all node IDs follow the \[RoleAbbrev\]\_\[TaskName\] naming convention.

6. Verify all display text follows the "Role: Verb \+ object" format.

7. Check that all decision branches are labeled (Yes/No or descriptive labels per Section 5.1).

8. Walk the happy path from start milestone to end milestone to confirm logical completeness.

9. Cross-reference against the RACI matrix: every node in the flowchart must have a corresponding RACI row, and vice versa.

10. Test all subgraph boundaries: every node should appear inside the correct phase subgraph.

11. Scope boundary verification: confirm every node falls within the scope defined in Section 2.1. Any node whose trigger event occurs after the end milestone is out of scope and must be removed.

12. Node count verification: confirm the total node count (action + decision + milestone) is between 60 and 300 (Section 2.4). Record the parent work step count for reporting. If the total node count exceeds 300, investigate whether upstream consolidation was insufficient before proceeding.

13. Edge count verification: count all arrows (`-->` connections) in the Mermaid code. If the edge count exceeds 500, the chart will fail to render under Mermaid’s default `maxEdges` setting. Reduce edges by consolidating nodes, eliminating redundant loop-backs, or splitting the chart per Section 2.5. This is a hard technical gate — not a readability guideline.

14. LLM tier verification: confirm the LLM tier is not zero. A zero-LLM classification across all steps indicates the LLM Capability Acquisition Gate (Section 2.3.1) was skipped or the tier definitions were misapplied. Verify the tier distribution falls within the expected ranges documented in Section 2.3.1. If it does not, investigate before proceeding.

15. Consolidation verification: confirm the consolidation pass (Section 3.7.1) was applied. Spot-check for sequential same-role node pairs that fail all five retention tests — these indicate missed merge candidates. If any are found, merge them and update all deliverables before proceeding.

16. Verify all kill-path milestones (if any) are terminal — inbound edges only, no outbound edges. Verify they use the stadium shape and ALL-CAPS text.

17. Verify the architecture comment block is present (mandatory for 60+ node flowcharts) and that its node/edge counts match the actual counts in the code.

18. Verify intermediate milestones (FREEZE, FILED, etc.) have exactly one inbound and one outbound edge.

19. Verify track subgraphs (if used) are nested inside phase subgraphs, not at the top level. Verify deliverable grouping subgraphs (if used) are nested inside tracks, sub-phases, or phases.

## **11.1 Cross-Deliverable Coherence**

A flowchart is typically one deliverable in a larger package that includes a RACI matrix, a summary slide, a step classification table, and possibly a companion table. All deliverables must reconcile to the same numbers. Inconsistencies between deliverables erode credibility and trigger costly reconciliation rework.

**The root problem: multiple valid counting methodologies.** A single flowchart can be counted several ways:

* Total nodes (action steps \+ decision diamonds \+ milestones).

* Parent work steps only (excluding sub-steps created during LLM/Hybrid expansion).

* Action nodes only (excluding decisions and milestones).

* Steps per role (a subset of any of the above).

Each of these is a valid count, but they produce different numbers from the same chart. If the summary slide uses one basis, the RACI uses another, and the Mermaid code implies a third, every reviewer will flag a discrepancy.

**The rule:** Define your counting denominator once, at the start of the project, and document it explicitly. Every deliverable must reference the same basis. When a deliverable requires a different count (e.g., the RACI lists all 123 nodes while the summary slide reports 76 parent work steps), state the basis and the relationship between the two counts in that deliverable.

**Example:** The S-1 production flowchart has 123 total nodes (109 action \+ 12 decision \+ 2 milestone). It also has 76 parent work steps (the basis used on the summary slide). The RACI matrix lists all 123 nodes. The summary slide states “76 parent work steps” with a footnote explaining the relationship to 123 total nodes. Both numbers are correct; the basis is documented in each deliverable.

Before delivering any output, verify:

* Every node in the Mermaid code has a corresponding row in the RACI matrix, and vice versa.

* Every count cited in a summary slide or companion document matches the defined basis.

* Role-level counts (e.g., Company Counsel: 44 steps) are consistent across all deliverables that report them.

* If any deliverable was updated independently (e.g., a node was added to the Mermaid code), all other deliverables have been updated to match.

* The RACI’s Responsible (R) column must exactly match the flowchart’s role-color assignments. If a node is blue (Company Counsel) in the chart, it must have R under Company Counsel in the RACI. Any mismatch indicates an error in one or both deliverables.

* The Tier column (F) in the RACI Matrix must exactly match the companion step classification table. Every ACTION row’s tier value must be identical in both deliverables.

* The Deliverable Section column (I) must be consistent with the deliverable grouping subgraphs in the flowchart (if used). If a node is inside a “Deliverable: Risk Factors” subgraph, its Deliverable Section value must be “Item 1A: Risk Factors”.

* The Cover Memo’s statistics (step counts, tier percentages, phase counts) must match the RACI Summary sheet and the flowchart’s actual counts.

* The Phase Overview Chart’s phases must match the flowchart’s top-level subgraph names exactly.

**WARNING:** LIVE SEARCH REQUIRED. During validation, verify that the sequence of steps reflects current actual practice. Search for current SEC review timelines, FINRA processing procedures, exchange listing requirements, and any recent rule changes that may affect the accuracy of the flowchart. A chart that follows all coding conventions but misrepresents the actual process is a failure.

**TIP:** USER INPUT REQUIRED. The user is the ultimate quality gate. An LLM can check syntax, orphan nodes, and naming conventions, but only the user can confirm that the flowchart represents the actual process as practiced — not just as described in manuals. The user must perform the final accuracy review before the flowchart is considered complete.

# **12\. Quick Reference Card**

## **12.1 Shape Cheat Sheet**

| Element | Syntax | When to Use |
| :---- | :---- | :---- |
| Rectangle | ID\["text"\] | Action steps (the vast majority of nodes) |
| Diamond | ID{"text"} | Decision points (labeled gates) |
| Stadium | ID(\["text"\]) | Start, intermediate, end, and kill-path milestones |
| Subgraph | subgraph ID\["label"\] ... end | Phase/section grouping |
| Arrow | A \--\> B | Flow connection |
| Labeled arrow | A \--\>|"Yes"| B | Decision branches |
| Style | style ID stroke:...,fill:...,color:... | Role color assignment |

## **12.2 External Resources**

* Mermaid Official Documentation: https://mermaid.js.org/syntax/flowchart.html

* Mermaid Live Editor: https://mermaid.live

* Mermaid Tutorials: https://mermaid.js.org/ecosystem/tutorials.html

* Okabe-Ito Color Palette Reference: https://jfly.uni-koeln.de/color/