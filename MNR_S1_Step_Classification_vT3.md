# Step Classification Table — vT3

**Flowchart:** S-1 Production Flowchart — Oil & Gas MLP IPO (Mach Natural Resources LP precedent)
**Version:** vT3 (129 total nodes; 111 action nodes classified below)
**Capability baseline:** Claude Opus 4.6 or equivalent successor within 6 months (per LLM Capability Acquisition Gate)
**Classification basis:** Production capability — whether the LLM can produce the work product — not the approval workflow that follows

---

| # | Step Name | Node ID | Role | Tier | Rationale |
|--:|-----------|---------|------|------|-----------|
| 1 | Evaluate strategic options (IPO vs. alternatives; market timing) | Issuer_EvalStrategicOptions | Issuer | Human | Requires management's undisclosed business judgment on strategic direction and market timing |
| 2 | Compile revenue, float & debt data; apply Securities Act thresholds; produce filer status memo | CompCounsel_DetermineFilerStatus | CompCounsel | LLM | Mechanical application of quantitative thresholds (revenue, float, debt) to public data; deterministic output |
| 3 | Map & select available EGC/SRC/JOBS Act/FAST Act accommodations | CompCounsel_SelectAccommodations | CompCounsel | LLM | Rule-based mapping of filer status to statutory accommodations; no substantive judgment required once filer status is determined |
| 4 | Perform IPO readiness assessment | Issuer_IPOReadiness | Issuer | Human | Requires management assessment of internal controls, organizational readiness, and resource availability — information not accessible to LLM |
| 5 | Calculate staleness deadlines (Reg S-X Rule 3-12); produce calendar with update triggers | CompCounsel_ProduceStalenessCalendar | CompCounsel | LLM | Deterministic date arithmetic from filing date and fiscal year-end applying Reg S-X Rule 3-12 |
| 6 | Search EDGAR for comparable MLP IPO filings; select based on market positioning | UW_SelectComparables | UW | Hybrid | LLM can search EDGAR and compile comparable filings; selection based on market positioning requires human judgment on deal strategy |
| 7 | Establish IPO PMO; align stakeholders | Issuer_EstablishPMO | Issuer | Human | Organizational decision requiring management authority to assign personnel and allocate resources |
| 8 | Select company counsel | Issuer_SelectCompCounsel | Issuer | Human | Relationship-based selection decision requiring management judgment on firm qualifications, conflicts, and pricing |
| 9 | Select independent auditor (PCAOB-registered) | Issuer_SelectAuditor | Issuer | Human | Relationship-based selection decision requiring management judgment on firm qualifications and independence |
| 10 | Select lead underwriter(s) & co-managers | Issuer_SelectLeadUW | Issuer | Human | Relationship-based selection requiring management judgment on bank capabilities, sector coverage, and economics |
| 11 | Select underwriters' counsel | UW_SelectUWCounsel | UW | Human | Relationship-based selection decision made by underwriters |
| 12 | Retain reserve engineers | Issuer_RetainResEng | Issuer | Human | Selection of technical expert requiring management judgment on qualifications and independence |
| 13 | Retain tax counsel | Issuer_RetainTaxCounsel | Issuer | Human | Relationship-based selection decision requiring management judgment on MLP tax expertise |
| 14 | Select financial printer & transfer agent | Issuer_SelectPrinterTA | Issuer | Human | Vendor selection requiring management judgment on capabilities, pricing, and EDGAR experience |
| 15 | Analyze qualifying income framework (§7704(d)(1)(E)); apply to revenue streams; issue determination | TaxCounsel_DetermineQI | TaxCounsel | Hybrid | LLM can analyze the statutory framework and map revenue streams; the legal determination on qualifying income requires tax counsel's professional judgment |
| 16 | Assess cash flow profile; select distribution model | Issuer_SelectDistModel | Issuer | Human | Requires management's undisclosed cash flow projections and strategic decisions on distribution policy |
| 17 | Identify consent requirements & indenture restrictions; make feasibility determination | CompCounsel_DetermineConveyanceFeasibility | CompCounsel | Hybrid | LLM can identify consent requirements from indenture documents; the feasibility determination requires legal judgment on contractual restrictions |
| 18 | Draft certificate of limited partnership (DE) | CompCounsel_DraftLPCert | CompCounsel | LLM | Short-form statutory document with standardized content required by Delaware RULPA; minimal substantive judgment |
| 19 | Draft GP entity formation docs (LLC cert + OA); file with DE Secretary of State | CompCounsel_FileFormation | CompCounsel | Hybrid | LLM can draft standardized formation documents from precedent; operating agreement governance provisions require legal judgment; filing is a physical/electronic act |
| 20 | Draft capitalization term sheet (IDR, subordination, MQD) | CompCounsel_DraftCapTermSheet | CompCounsel | Hybrid | LLM can draft term sheet template from precedent; substantive economic terms (IDR splits, MQD level, subordination periods) require human negotiation input |
| 21 | Negotiate & finalize economic terms; produce revised term sheet reflecting agreed terms | CompCounsel_NegotiateCapTerms | CompCounsel | Human | Core activity is negotiation between parties — inherently non-textual and requires real-time human interaction |
| 22 | Establish GP governance; appoint independent directors | Issuer_EstablishGPGov | Issuer | Human | Requires board-level governance decisions and director recruitment — human relationship and authority actions |
| 23 | Draft LPA from precedent & term sheet; make governance & fiduciary duty decisions; produce circulation draft (Ex 3.2) | CompCounsel_DraftLPA | CompCounsel | Hybrid | LLM can produce substantial draft from precedent LPA and term sheet; fiduciary duty framework and governance provisions require material legal judgment |
| 24 | Draft Contribution Agreement; adapt reps, indemnification & closing conditions | CompCounsel_DraftContribAgreement | CompCounsel | Hybrid | LLM can draft from precedent; adaptation of representations, indemnification scope, and closing conditions requires legal judgment on deal-specific risks |
| 25 | Draft Registration Rights Agreement; adapt demand/piggyback rights & shelf terms | CompCounsel_DraftRegRights | CompCounsel | Hybrid | LLM can draft from precedent; adaptation of demand rights triggers, piggyback priority, and shelf timing requires legal judgment |
| 26 | Draft Services Agreement (MSA); negotiate scope, fees & termination provisions | CompCounsel_DraftMSA | CompCounsel | Hybrid | LLM can draft from precedent; scope of services, fee structure, and termination triggers require negotiation-informed judgment |
| 27 | Draft LTIP plan document; determine award types, vesting & share reserve | CompCounsel_DraftLTIP | CompCounsel | Hybrid | LLM can draft plan from precedent; award types, vesting schedules, and share reserve size require compensation committee and management input |
| 28 | Draft ancillary agreements (Omnibus, Tax Receivable, etc.); adapt to deal terms; incorporate revisions | CompCounsel_DraftAncillary | CompCounsel | Hybrid | LLM can draft from precedent templates; deal-specific adaptations require legal judgment on tax and operational provisions |
| 29 | Hold organizational / all-hands meeting | Issuer_HoldOrgMeeting | Issuer | Human | Physical/virtual meeting requiring in-person coordination and presentation — inherently non-textual |
| 30 | Compile & circulate responsibility checklist & DD request list to working group | CompCounsel_CirculateChecklist | CompCounsel | LLM | Compilation from precedent checklists and standard DD request templates; output is a standardized list requiring only deal-specific customization |
| 31 | Customize & distribute D&O questionnaires to officers, directors & nominees | CompCounsel_DistributeQuestionnaire | CompCounsel | LLM | Customization of standardized questionnaire forms from precedent; distribution is administrative |
| 32 | Draft lock-up agreement; determine period, exceptions & early release provisions | CompCounsel_DraftLockUp | CompCounsel | Hybrid | LLM can draft from precedent; lock-up period length, exception carve-outs, and early release triggers require negotiation-informed judgment |
| 33 | Draft quiet period / gun-jumping compliance policy; tailor to company circumstances | CompCounsel_DraftCommsPlan | CompCounsel | Hybrid | LLM can draft policy from precedent and SEC guidance; tailoring to company's specific communications practices and business model requires legal judgment |
| 34 | Conduct business & legal due diligence | UWCounsel_ConductBizLegalDD | UWCounsel | Human | Requires interviews, document review with privileged access, and professional judgment on legal risks — information not accessible to LLM |
| 35 | Conduct financial due diligence | Acct_ConductFinDD | Acct | External | Performed by independent auditors exercising professional judgment under PCAOB standards; team receives their work product |
| 36 | Confirm auditor readiness for comfort letters | Acct_ConfirmComfortReadiness | Acct | External | Auditor's professional determination of readiness to issue comfort letters under SAS standards |
| 37 | Commission reserve reports (S-K 1200/1202) | ResEng_CommissionReserves | ResEng | External | Reserve engineer's independent technical evaluation; team commissions and receives the report |
| 38 | Draft reorganization step plan; determine entity structure, tax elections & jurisdictions | CompCounsel_DraftReorgPlan | CompCounsel | Hybrid | LLM can draft reorganization steps from structure chart; entity structure decisions and tax election strategy require legal and tax judgment |
| 39 | Draft governance guidelines & code of ethics | CompCounsel_DraftGovGuidelines | CompCounsel | LLM | Largely standardized corporate governance documents adaptable from exchange listing standards and precedent with minimal substantive judgment |
| 40 | Draft & tailor insider trading policy per Rule 10b5-1 to entity structure & board prefs | CompCounsel_DraftInsiderPolicy | CompCounsel | Hybrid | LLM can draft from precedent and Rule 10b5-1 requirements; tailoring to entity structure (MLP vs. corporate) and board preferences requires legal judgment |
| 41 | Draft plan adoption resolutions; finalize plan terms & board approval documentation | CompCounsel_DraftLTIPAdoption | CompCounsel | Hybrid | LLM can draft resolutions from precedent; finalization of plan terms requires alignment with board compensation decisions |
| 42 | Draft indemnification agreement; adapt scope & limitations to entity & state law | CompCounsel_DraftIndemnification | CompCounsel | Hybrid | LLM can draft from precedent; scope of indemnification and state law limitations require legal judgment on DE LLC Act provisions |
| 43 | Obtain D&O insurance | Issuer_ObtainDOInsurance | Issuer | Human | Commercial procurement requiring broker engagement, underwriting, and pricing negotiation |
| 44 | Board/GP board resolutions authorizing IPO | Issuer_BoardResolutions | Issuer | Human | Board-level authorization requiring director deliberation and vote — inherently human governance act |
| 45 | Draft POA forms; compile executed POAs for Exhibit 24.1 | CompCounsel_DraftPOA | CompCounsel | LLM | Standardized power-of-attorney forms compiled from precedent; assembly of executed POAs is administrative |
| 46 | Draft consent forms; compile executed consents for Exhibits 99.5–99.7 | CompCounsel_DraftConsents | CompCounsel | LLM | Standardized consent forms compiled from precedent; assembly of executed consents is administrative |
| 47 | Determine appropriate registrant FS (predecessor/carve-out) | Acct_DetermineRegistrantFS | Acct | External | Auditor's professional determination under SEC accounting guidance on registrant entity and predecessor presentation |
| 48 | Prepare audited annual FS per PCAOB / Reg S-X | Acct_PrepareAuditedFS | Acct | External | Audited financial statements prepared and opined on by independent auditors under PCAOB standards |
| 49 | Prepare unaudited interim FS | Acct_PrepareInterimFS | Acct | External | Interim financial statements prepared by auditors under SAS review standards |
| 50 | Structure pro forma template & draft assumptions (Art. 11); determine adjustments & validate | Acct_DetermineProFormaAdj | Acct | External | Pro forma adjustments require auditor's professional judgment on Article 11 compliance and adjustment validity |
| 51 | Draft reconciliation tables & Reg G format; determine non-GAAP measures to present & validate | Acct_DetermineNonGAAPMeasures | Acct | External | Selection of non-GAAP measures and reconciliation methodology requires auditor's professional judgment on Reg G compliance |
| 52 | Prepare four-quarter DCF forecast (MLP-specific) | Issuer_PrepareDCFForecast | Issuer | Human | Requires management's undisclosed cash flow projections and operating assumptions |
| 53 | Structure DCF backcast template; make substantive judgments on pro forma adjustments | Acct_DetermineDCFAdjustments | Acct | External | DCF adjustment methodology requires auditor's professional judgment |
| 54 | Prepare reserve report data & reconciliation | ResEng_PrepareReserveData | ResEng | External | Reserve engineer's independent technical work product |
| 55 | Draft S-K Item 305 disclosure; integrate hedging data; make materiality determinations | CompCounsel_DraftMarketRisk | CompCounsel | Hybrid | LLM can draft Item 305 template and integrate quantitative hedging data; materiality determinations on risk exposures require legal judgment |
| 56 | XBRL tagging of FS (Reg S-T; specialized software) | Acct_XBRLTagging | Acct | External | Specialized XBRL tagging performed by auditors or XBRL service providers using dedicated software |
| 57 | Monitor & manage FS staleness; update as needed | Acct_MonitorStaleness | Acct | External | Staleness determination and FS updates require auditor's professional judgment and access to current financial data |
| 58 | Resolve predecessor accounting determination (ASC 805) | Acct_ResolvePredecessor | Acct | External | ASC 805 predecessor determination requires auditor's professional judgment on accounting standards application |
| 59 | Kick off parallel S-1 section drafting across all tracks | CompCounsel_InitiateS1Drafting | CompCounsel | Human | Coordination action requiring human communication and project management to launch parallel workstreams |
| 60 | Assemble cover page & draft disclaimers (Industry/Market Data, Trademarks, BoP) | CompCounsel_DraftFrontMatter | CompCounsel | LLM | Standardized boilerplate disclaimers assembled from precedent with deal-specific data insertion |
| 61 | Draft risk factors; organize categories; make materiality judgments; cross-reference (Item 1A) | CompCounsel_DraftRiskFactors | CompCounsel | Hybrid | LLM can organize categories and draft boilerplate language; materiality and company-specific risk assessment require human judgment |
| 62 | Draft Use of Proceeds; determine allocation disclosure & specific use language | CompCounsel_DraftUseOfProceeds | CompCounsel | Hybrid | LLM can draft from precedent; allocation specificity and use-of-proceeds commitments require management input and legal judgment |
| 63 | Draft Cash Distribution Policy from LPA terms; determine subordination mechanics; produce waterfall table & sensitivity analysis | CompCounsel_DraftDistPolicy | CompCounsel | Hybrid | LLM can draft policy narrative and compute waterfall from LPA terms; subordination mechanics and sensitivity assumptions require legal and financial judgment |
| 64 | Assemble capitalization table from agreed financial data | CompCounsel_DraftCapTable | CompCounsel | LLM | Mechanical assembly of financial data into standardized table format from agreed inputs |
| 65 | Compute dilution analysis from pricing assumptions | CompCounsel_DraftDilution | CompCounsel | LLM | Deterministic arithmetic computation from pricing assumptions and unit counts |
| 66 | Compile financial data into standardized table format; draft narrative wrapper | CompCounsel_DraftHistFinData | CompCounsel | LLM | Compilation of audited financial data into standardized selected financial data table; narrative wrapper is formulaic |
| 67 | Structure MD&A; draft period-over-period analysis; incorporate management input & forward-looking statements | CompCounsel_DraftMDA | CompCounsel | Hybrid | LLM can structure and draft quantitative analysis from financial data; management's explanation of trends and forward-looking statements require human input |
| 68 | Draft Business section from VDR data, reserve reports & public sources; ensure S-K 1200/1202 compliance | CompCounsel_DraftBusiness | CompCounsel | Hybrid | LLM can compile from VDR data and public sources; strategic positioning, competitive narrative, and S-K 1200/1202 reserve disclosure require human judgment |
| 69 | Draft Management section from D&O questionnaires; make disclosure adequacy & related-party judgments | CompCounsel_DraftMgmt | CompCounsel | Hybrid | LLM can draft biographical information from questionnaires; disclosure adequacy and related-party judgments require legal judgment |
| 70 | Draft CD&A narrative & compensation tables; determine peer group, metrics & EGC scaled disclosure (S-K Item 402) | CompCounsel_DraftComp | CompCounsel | Hybrid | LLM can draft tables and narrative from compensation data; peer group selection, metric choices, and scaled disclosure decisions require human judgment |
| 71 | Compile ownership data & produce table per S-K Item 403 | CompCounsel_DraftOwnershipTable | CompCounsel | LLM | Mechanical compilation of ownership data into standardized beneficial ownership table format |
| 72 | Draft Related Transactions; make materiality judgments on transactions to disclose | CompCounsel_DraftRelatedTxn | CompCounsel | Hybrid | LLM can identify and draft related-party transaction descriptions; materiality judgments on which transactions to disclose require legal judgment |
| 73 | Draft Description of Common Units from LPA terms; verify accuracy of material rights | CompCounsel_DraftUnitDesc | CompCounsel | Hybrid | LLM can draft description from LPA provisions; verification of material rights accuracy and completeness requires legal judgment |
| 74 | Draft Conflicts of Interest from LPA fiduciary provisions; cross-reference against UnitDesc & LPA terms | CompCounsel_DraftConflicts | CompCounsel | Hybrid | LLM can draft from LPA fiduciary provisions and cross-reference; substantive characterization of conflict resolution mechanisms requires legal judgment |
| 75 | Draft PSLRA safe harbor disclaimer; tailor cautionary language to company-specific statements | CompCounsel_DraftFLS | CompCounsel | Hybrid | LLM can draft boilerplate safe harbor language; tailoring cautionary statements to company-specific forward-looking statements requires legal judgment |
| 76 | Draft Part II (Items 13–15: expenses, indemnification, exhibits & FS schedules) | CompCounsel_DraftPartII | CompCounsel | LLM | Standardized Part II items with deterministic content (expenses from fee table, indemnification from charter, exhibit/FS references) |
| 77 | Compile exhibit index cross-referenced to S-K Item 601 | CompCounsel_CompileExhibitIndex | CompCounsel | LLM | Mechanical compilation of exhibit list cross-referenced to S-K Item 601 categories; deterministic output from exhibit inventory |
| 78 | Draft undertakings (S-K Item 512); cross-reference exhibit index against actual exhibits | CompCounsel_DraftUndertakings | CompCounsel | LLM | Standardized undertaking language from S-K Item 512; cross-reference verification is mechanical |
| 79 | Assemble all exhibits with correct numbering & EDGAR refs; verify completeness; manage consent logistics; produce final package | CompCounsel_AssembleExhibits | CompCounsel | Hybrid | LLM can assemble and number exhibits from inventory; consent logistics and completeness verification across 35+ exhibits require human coordination |
| 80 | Compile LPA appendix & generate glossary of defined terms | CompCounsel_DraftBackMatter | CompCounsel | LLM | Mechanical extraction of defined terms from LPA and compilation into alphabetical glossary |
| 81 | Assemble signature pages with officer/director names & titles; attach POA references | CompCounsel_AssembleSigPages | CompCounsel | LLM | Mechanical assembly from officer/director list and POA exhibit references |
| 82 | Draft Prospectus Summary from business description & deal terms; determine emphasis, positioning & materiality (drafted last / most revised) | CompCounsel_DraftSummary | CompCounsel | Hybrid | LLM can draft summary from completed sections; emphasis, positioning, and materiality determinations for the most investor-facing section require human judgment |
| 83 | Draft MLP tax section; determine specific tax positions & qualifying income conclusions; incorporate into disclosure | TaxCounsel_DraftTax | TaxCounsel | Hybrid | LLM can draft disclosure from tax framework; specific tax positions and qualifying income conclusions require tax counsel's professional judgment |
| 84 | Draft Underwriting section from precedent & deal terms; adapt stabilization, over-allotment & syndicate provisions | UWCounsel_DraftUW | UWCounsel | Hybrid | LLM can draft from precedent; adaptation of stabilization, over-allotment, and syndicate terms requires legal judgment on negotiated deal terms |
| 85 | File Form ID with SEC for EDGAR access codes | CompCounsel_FileFormID | CompCounsel | LLM | Standardized SEC form with deterministic fields; filing is electronic submission |
| 86 | Assemble complete DRS with sections, exhibits & EDGAR headers; verify completeness; authorize submission | CompCounsel_AssembleDRS | CompCounsel | Hybrid | LLM can assemble sections and generate EDGAR headers; completeness verification across entire filing and authorization require human judgment |
| 87 | Coordinate parallel post-DRS filings (FINRA + exchange) | CompCounsel_CoordinatePostDRS | CompCounsel | Human | Coordination action requiring human communication to launch parallel regulatory filings |
| 88 | Compile FINRA Rule 5110 information; draft application; verify compensation; authorize filing | UWCounsel_CompileFINRA | UWCounsel | Hybrid | LLM can compile Rule 5110 data and draft application; compensation verification and filing authorization require legal judgment |
| 89 | File listing application with stock exchange | Issuer_FileListingApp | Issuer | Human | Administrative filing requiring authorized personnel to submit application to exchange |
| 90 | Assemble DRS/A incorporating all revisions; verify; authorize submission | CompCounsel_AssembleDRSA | CompCounsel | Hybrid | LLM can integrate revisions into amended filing; completeness verification and submission authorization require human judgment |
| 91 | Assemble complete S-1 with all sections & exhibits; authorize public 'flip'; coordinate EDGAR | CompCounsel_AssembleS1 | CompCounsel | Hybrid | LLM can assemble filing; the public 'flip' authorization is a material legal decision with liability implications |
| 92 | Draft & format Form 8-A for EDGAR submission | CompCounsel_DraftForm8A | CompCounsel | LLM | Short standardized SEC form with deterministic content derived from registration statement |
| 93 | SEC reviews registration statement (~30 days) | SEC_ReviewRS | SEC | External | SEC staff review — entirely outside party action |
| 94 | Parse comment letter; categorize, assess significance & develop response strategy; distribute analyzed comments with assignments | CompCounsel_DistributeComments | CompCounsel | Hybrid | LLM can parse and categorize comments; significance assessment and response strategy require legal judgment |
| 95 | Draft proposed responses & redline prospectus language; exercise judgment on strategy; produce formatted CORRESP letter | CompCounsel_DraftCORRESP | CompCounsel | Hybrid | LLM can draft proposed responses and redline language; legal strategy on how to respond to each comment requires human judgment |
| 96 | Integrate approved changes into amended S-1; verify completeness; authorize S-1/A filing | CompCounsel_AssembleS1A | CompCounsel | Hybrid | LLM can integrate changes; completeness verification across amended filing and filing authorization require human judgment |
| 97 | Generate updated consent forms; compile executed consents as exhibits for filing | CompCounsel_CompileConsents | CompCounsel | LLM | Standardized consent forms updated with current filing date; compilation is administrative |
| 98 | Re-file opinions (legality, tax) with each amendment | CompCounsel_RefileOpinions | CompCounsel | Hybrid | LLM can update opinion letter dates and references; the substantive legal and tax opinions require professional judgment to confirm continued validity |
| 99 | Update FS if required during review period | Acct_UpdateFSDuringReview | Acct | External | Financial statement updates require auditor's professional judgment and access to current financial data |
| 100 | Draft responses to subsequent round comments; finalize & coordinate amendment | CompCounsel_DraftSubsequent | CompCounsel | Hybrid | Same classification as initial CORRESP — LLM can draft responses but legal strategy requires human judgment |
| 101 | Confirm SEC clearance & coordinate next steps | CompCounsel_ConfirmClearance | CompCounsel | Human | Communication with SEC staff to confirm no-further-comments status; coordination action |
| 102 | Populate all pricing blanks throughout S-1; verify accuracy of populated fields | CompCounsel_PopulatePricing | CompCounsel | LLM | Mechanical insertion of agreed pricing data into identified blanks throughout the filing; verification is pattern-matching |
| 103 | Verify timing compliance (15-calendar-day rule); assemble filing; authorize public filing | CompCounsel_VerifyTiming | CompCounsel | Hybrid | LLM can compute timing compliance (date arithmetic); assembling the pricing amendment and authorizing public filing require human judgment |
| 104 | FINRA reviews underwriting compensation | SEC_FINRAReview | SEC | External | FINRA staff review — entirely outside party action |
| 105 | Draft responses to FINRA compensation comments; negotiate resolution | UWCounsel_ResolveFINRA | UWCounsel | Hybrid | LLM can draft responses to FINRA comments; negotiation of compensation resolution requires human judgment |
| 106 | Print & distribute preliminary prospectus (red herring) | CompCounsel_PrintProspectus | CompCounsel | Human | Physical production and distribution requiring printer coordination and logistics |
| 107 | Conduct roadshow (management presentations; 1–2 weeks) | Issuer_ConductRoadshow | Issuer | Human | In-person management presentations to investors — inherently non-textual human activity |
| 108 | Form syndicate; build order book | UW_BuildOrderBook | UW | Human | Investor solicitation, syndicate formation, and order book management require human judgment and real-time market interaction |
| 109 | Draft & format Rule 461 acceleration request for EDGAR submission | CompCounsel_DraftAccelRequest | CompCounsel | LLM | Standardized SEC form with deterministic content; formatting for EDGAR is mechanical |
| 110 | Compute filing fees (SEC Rule 457); produce Exhibit 107 filing fee table | CompCounsel_ProduceFeeTable | CompCounsel | LLM | Deterministic fee computation from Rule 457 formula applied to offering amount; standardized table format |
| 111 | SEC declares registration statement effective | SEC_DeclaresEffective | SEC | External | SEC declaration — entirely outside party action |

---

## Tier Distribution Summary

| Tier | Count | % of 111 | Expected Range | In Range? |
|------|:-----:|:--------:|:--------------:|:---------:|
| LLM | 25 | 22.5% | 15–30% | ✓ |
| Hybrid | 44 | 39.6% | 35–50% | ✓ |
| Human | 25 | 22.5% | 15–30% | ✓ |
| External | 17 | 15.3% | 5–15% | See note |
| **Total** | **111** | **100%** | — | — |

LLM, Hybrid, and Human tiers fall within expected ranges. External is at 15.3%, marginally above the 15% upper bound. This is explained by the MLP IPO's heavy reliance on independent auditors (12 Acct nodes, all External) and reserve engineers (2 nodes, both External) — both driven by the oil & gas sector's reserve reporting requirements (S-K 1200/1202) and the MLP structure's predecessor/carve-out accounting complexity. No misclassification indicated.

---

## Tier Distribution by Role

| Role | LLM | Hybrid | Human | External | Total |
|------|:---:|:------:|:-----:|:--------:|:-----:|
| CompCounsel | 25 | 38 | 5 | 0 | 68 |
| Issuer | 0 | 0 | 17 | 0 | 17 |
| UW | 0 | 1 | 2 | 0 | 3 |
| UWCounsel | 0 | 3 | 1 | 0 | 4 |
| Acct | 0 | 0 | 0 | 12 | 12 |
| ResEng | 0 | 0 | 0 | 2 | 2 |
| TaxCounsel | 0 | 2 | 0 | 0 | 2 |
| SEC | 0 | 0 | 0 | 3 | 3 |
| **Total** | **25** | **44** | **25** | **17** | **111** |
