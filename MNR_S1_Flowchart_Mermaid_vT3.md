---
title: "S-1 Production Flowchart — Oil & Gas MLP IPO"
---
flowchart TD

%% ============================================================
%% ARCHITECTURE NOTES
%% ============================================================
%% Precedent: Mach Natural Resources LP (CIK 0001980088)
%% Version: vT3 (all audit fixes applied to vT2)
%% Changes from vT2:
%%   Fix 1: Swapped P2/P3 — team assembly now precedes structure work
%%   Fix 2: Added CoordinatePostDRS after Milestone_DRSFiled (1-in, 1-out)
%%   Fix 3: P6 fan-out moved upstream to DetermineNonGAAPMeasures
%%   Fix 4: Architecture notes edge count corrected
%%   Fix 5: P4 CC Admin fan-out (3 tasks parallel from CirculateChecklist)
%%   Fix 6: QC loop routes to InitiateS1Drafting (not arbitrary section)
%%   Fix 7: Decision_VDRReady nested inside P6 (no standalone subgraph)
%%   Fix 8: DraftSummary 22 inbound — flagged for mermaid.live testing
%% Structural pattern: 12 phases. Fan-out/fan-in in:
%%   P2 (5 parallel team selections from SelectCompCounsel),
%%   P3 (5 parallel agreement tracks from DraftLPA),
%%   P4 (4 parallel DD tracks from HoldOrgMeeting;
%%       CC Admin sub-fan-out: 3 tasks from CirculateChecklist),
%%   P5 (4 parallel governance docs from DraftReorgPlan),
%%   P6 (4 parallel tracks from DetermineNonGAAPMeasures),
%%   P7 (3 role-based tracks: CC Prospectus, Tax Counsel, UW Counsel;
%%       within CC track ~20 parallel sections fan-out/fan-in).
%% Cross-phase loops:
%%   Decision_SECFurtherComments loops back to P9 comment response
%%   Decision_RepriceOrAbandon loops back to P9 pricing amendment
%%   Decision_VDRReady loops back to P6 staleness monitoring
%%   Decision_SectionsConsistent loops back to P7 coordinator
%%   Decision_AccelerationGranted loops back to P11 accel request
%% Sync points:
%%   Decision_VDRReady requires convergence of P4-P6 before P7
%%   Decision_SectionsConsistent requires convergence of all P7 tracks
%% Kill paths:
%%   Milestone_IPOAbandoned reachable from Decision_ProceedIPO,
%%   Decision_QualifyingIncome, Decision_DDAcceptable,
%%   Decision_BoardAuthorizes, Decision_RepriceOrAbandon
%% Milestone edge compliance:
%%   All intermediate milestones are 1-in, 1-out.
%%   Milestone_DRSFiled: CoordinatePostDRS absorbs fan-out.
%%   Milestone_SECClear: ConfirmClearance absorbs fan-in.
%% Total nodes: 129 (action: 111, decision: 12, milestone: 6)
%% Total edges: 181
%% ============================================================

%% ============================================================
%% PHASE 1: STRATEGIC DECISION & IPO READINESS
%% ============================================================
subgraph P1["Phase 1: Strategic Decision & IPO Readiness"]
    Milestone_IPOTrigger(["TRIGGER: Sponsor decides<br/>to pursue IPO"])
    Issuer_EvalStrategicOptions["Issuer: Evaluate strategic options<br/>(IPO vs. alternatives; market timing)"]
    Decision_ProceedIPO{"Proceed with IPO?"}
    CompCounsel_DetermineFilerStatus["Company Counsel: Compile revenue, float<br/>& debt data; apply Securities Act<br/>thresholds; produce filer status memo"]
    CompCounsel_SelectAccommodations["Company Counsel: Map & select available<br/>EGC/SRC/JOBS Act/FAST Act accommodations"]
    Issuer_IPOReadiness["Issuer: Perform IPO<br/>readiness assessment"]
    CompCounsel_ProduceStalenessCalendar["Company Counsel: Calculate staleness<br/>deadlines (Reg S-X Rule 3-12);<br/>produce calendar with update triggers"]
    UW_SelectComparables["Underwriters: Search EDGAR for comparable<br/>MLP IPO filings; select based on<br/>market positioning"]
    Issuer_EstablishPMO["Issuer: Establish IPO PMO;<br/>align stakeholders"]
end

%% ============================================================
%% PHASE 2: ASSEMBLING THE IPO TEAM
%% ============================================================
subgraph P2["Phase 2: Assembling the IPO Team"]
    Issuer_SelectCompCounsel["Issuer: Select company counsel"]
    Issuer_SelectAuditor["Issuer: Select independent<br/>auditor (PCAOB-registered)"]
    Issuer_SelectLeadUW["Issuer: Select lead<br/>underwriter(s) & co-managers"]
    UW_SelectUWCounsel["Underwriters: Select<br/>underwriters' counsel"]
    Issuer_RetainResEng["Issuer: Retain<br/>reserve engineers"]
    Issuer_RetainTaxCounsel["Issuer: Retain tax counsel"]
    Issuer_SelectPrinterTA["Issuer: Select financial printer<br/>& transfer agent"]
end

%% ============================================================
%% PHASE 3: MLP STRUCTURE & FORMATION
%% ============================================================
subgraph P3["Phase 3: MLP Structure & Formation"]
    TaxCounsel_DetermineQI["Tax Counsel: Analyze qualifying income<br/>framework (§7704(d)(1)(E)); apply to<br/>revenue streams; issue determination"]
    Decision_QualifyingIncome{"Qualifying income<br/>test met?"}
    Issuer_SelectDistModel["Issuer: Assess cash flow profile;<br/>select distribution model"]
    CompCounsel_DetermineConveyanceFeasibility["Company Counsel: Identify consent<br/>requirements & indenture restrictions;<br/>make feasibility determination"]
    CompCounsel_DraftLPCert["Company Counsel: Draft certificate<br/>of limited partnership (DE)"]
    CompCounsel_FileFormation["Company Counsel: Draft GP entity formation<br/>docs (LLC cert + OA); file with<br/>DE Secretary of State"]
    CompCounsel_DraftCapTermSheet["Company Counsel: Draft capitalization<br/>term sheet (IDR, subordination, MQD)"]
    CompCounsel_NegotiateCapTerms["Company Counsel: Negotiate & finalize<br/>economic terms; produce revised<br/>term sheet reflecting agreed terms"]
    Issuer_EstablishGPGov["Issuer: Establish GP governance;<br/>appoint independent directors"]
    CompCounsel_DraftLPA["Company Counsel: Draft LPA from precedent<br/>& term sheet; make governance &<br/>fiduciary duty decisions; produce<br/>circulation draft (Ex 3.2)"]
    CompCounsel_DraftContribAgreement["Company Counsel: Draft Contribution<br/>Agreement; adapt reps, indemnification<br/>& closing conditions"]
    CompCounsel_DraftRegRights["Company Counsel: Draft Registration Rights<br/>Agreement; adapt demand/piggyback<br/>rights & shelf terms"]
    CompCounsel_DraftMSA["Company Counsel: Draft Services Agreement<br/>(MSA); negotiate scope, fees &<br/>termination provisions"]
    CompCounsel_DraftLTIP["Company Counsel: Draft LTIP plan document;<br/>determine award types, vesting<br/>& share reserve"]
    CompCounsel_DraftAncillary["Company Counsel: Draft ancillary agreements<br/>(Omnibus, Tax Receivable, etc.);<br/>adapt to deal terms; incorporate revisions"]
end

%% ============================================================
%% PHASE 4: ORGANIZATIONAL MEETING & DUE DILIGENCE
%% ============================================================
subgraph P4["Phase 4: Organizational Meeting & Due Diligence"]
    Issuer_HoldOrgMeeting["Issuer: Hold organizational /<br/>all-hands meeting"]

    subgraph P4A_T1["Track: CC Admin"]
        CompCounsel_CirculateChecklist["Company Counsel: Compile & circulate<br/>responsibility checklist & DD request<br/>list to working group"]
        CompCounsel_DistributeQuestionnaire["Company Counsel: Customize & distribute<br/>D&O questionnaires to officers,<br/>directors & nominees"]
        CompCounsel_DraftLockUp["Company Counsel: Draft lock-up agreement;<br/>determine period, exceptions<br/>& early release provisions"]
        CompCounsel_DraftCommsPlan["Company Counsel: Draft quiet period /<br/>gun-jumping compliance policy;<br/>tailor to company circumstances"]
    end

    subgraph P4A_T2["Track: UW Counsel Legal DD"]
        UWCounsel_ConductBizLegalDD["UW Counsel: Conduct business<br/>& legal due diligence"]
    end

    subgraph P4A_T3["Track: Financial DD"]
        Acct_ConductFinDD["Accountants: Conduct<br/>financial due diligence"]
        Acct_ConfirmComfortReadiness["Accountants: Confirm auditor<br/>readiness for comfort letters"]
    end

    subgraph P4A_T4["Track: Reserve Engineering"]
        ResEng_CommissionReserves["Reserve Engineer: Commission<br/>reserve reports (S-K 1200/1202)"]
    end

    Decision_DDAcceptable{"DD findings<br/>acceptable?"}
end

%% ============================================================
%% PHASE 5: CORPORATE & GOVERNANCE MATTERS
%% ============================================================
subgraph P5["Phase 5: Corporate & Governance Matters"]
    CompCounsel_DraftReorgPlan["Company Counsel: Draft reorganization<br/>step plan; determine entity structure,<br/>tax elections & jurisdictions"]
    CompCounsel_DraftGovGuidelines["Company Counsel: Draft governance<br/>guidelines & code of ethics"]
    CompCounsel_DraftInsiderPolicy["Company Counsel: Draft & tailor insider<br/>trading policy per Rule 10b5-1<br/>to entity structure & board prefs"]
    CompCounsel_DraftLTIPAdoption["Company Counsel: Draft plan adoption<br/>resolutions; finalize plan terms<br/>& board approval documentation"]
    CompCounsel_DraftIndemnification["Company Counsel: Draft indemnification<br/>agreement; adapt scope & limitations<br/>to entity & state law"]
    Issuer_ObtainDOInsurance["Issuer: Obtain D&O insurance"]
    Issuer_BoardResolutions["Issuer: Board/GP board resolutions<br/>authorizing IPO"]
    Decision_BoardAuthorizes{"Board authorizes<br/>IPO?"}
    CompCounsel_DraftPOA["Company Counsel: Draft POA forms;<br/>compile executed POAs<br/>for Exhibit 24.1"]
    CompCounsel_DraftConsents["Company Counsel: Draft consent forms;<br/>compile executed consents<br/>for Exhibits 99.5–99.7"]
end

%% ============================================================
%% PHASE 6: FINANCIAL STATEMENT PREPARATION
%% ============================================================
subgraph P6["Phase 6: Financial Statement Preparation"]
    Acct_DetermineRegistrantFS["Accountants: Determine appropriate<br/>registrant FS (predecessor/carve-out)"]
    Acct_PrepareAuditedFS["Accountants: Prepare audited annual FS<br/>per PCAOB / Reg S-X"]
    Acct_PrepareInterimFS["Accountants: Prepare unaudited<br/>interim FS"]
    Acct_DetermineProFormaAdj["Accountants: Structure pro forma template<br/>& draft assumptions (Art. 11);<br/>determine adjustments & validate"]
    Acct_DetermineNonGAAPMeasures["Accountants: Draft reconciliation tables<br/>& Reg G format; determine non-GAAP<br/>measures to present & validate"]
    Issuer_PrepareDCFForecast["Issuer: Prepare four-quarter<br/>DCF forecast (MLP-specific)"]
    Acct_DetermineDCFAdjustments["Accountants: Structure DCF backcast<br/>template; make substantive judgments<br/>on pro forma adjustments"]
    ResEng_PrepareReserveData["Reserve Engineer: Prepare reserve<br/>report data & reconciliation"]
    CompCounsel_DraftMarketRisk["Company Counsel: Draft S-K Item 305<br/>disclosure; integrate hedging data;<br/>make materiality determinations"]
    Acct_XBRLTagging["Accountants: XBRL tagging of FS<br/>(Reg S-T; specialized software)"]
    Acct_MonitorStaleness["Accountants: Monitor & manage<br/>FS staleness; update as needed"]
    Acct_ResolvePredecessor["Accountants: Resolve predecessor<br/>accounting determination (ASC 805)"]
    Decision_VDRReady{"All inputs ready<br/>for S-1 drafting?"}
end

%% ============================================================
%% PHASE 7: S-1 DRAFTING & CONTENT PREPARATION
%% ============================================================
subgraph P7["Phase 7: S-1 Drafting & Content Preparation"]

    CompCounsel_InitiateS1Drafting["Company Counsel: Kick off parallel<br/>S-1 section drafting across all tracks"]

    subgraph P7_T1["Track: CC Prospectus Sections"]
        CompCounsel_DraftFrontMatter["Company Counsel: Assemble cover page<br/>& draft disclaimers (Industry/Market<br/>Data, Trademarks, BoP)"]
        CompCounsel_DraftRiskFactors["Company Counsel: Draft risk factors;<br/>organize categories; make materiality<br/>judgments; cross-reference (Item 1A)"]
        CompCounsel_DraftUseOfProceeds["Company Counsel: Draft Use of Proceeds;<br/>determine allocation disclosure<br/>& specific use language"]
        CompCounsel_DraftDistPolicy["Company Counsel: Draft Cash Distribution<br/>Policy from LPA terms; determine<br/>subordination mechanics; produce<br/>waterfall table & sensitivity analysis"]
        CompCounsel_DraftCapTable["Company Counsel: Assemble capitalization<br/>table from agreed financial data"]
        CompCounsel_DraftDilution["Company Counsel: Compute dilution analysis<br/>from pricing assumptions"]
        CompCounsel_DraftHistFinData["Company Counsel: Compile financial data<br/>into standardized table format;<br/>draft narrative wrapper"]
        CompCounsel_DraftMDA["Company Counsel: Structure MD&A;<br/>draft period-over-period analysis;<br/>incorporate management input &<br/>forward-looking statements"]
        CompCounsel_DraftBusiness["Company Counsel: Draft Business section<br/>from VDR data, reserve reports &<br/>public sources; ensure S-K 1200/1202<br/>compliance"]
        CompCounsel_DraftMgmt["Company Counsel: Draft Management section<br/>from D&O questionnaires; make<br/>disclosure adequacy & related-party<br/>judgments"]
        CompCounsel_DraftComp["Company Counsel: Draft CD&A narrative<br/>& compensation tables; determine<br/>peer group, metrics & EGC scaled<br/>disclosure (S-K Item 402)"]
        CompCounsel_DraftOwnershipTable["Company Counsel: Compile ownership data<br/>& produce table per S-K Item 403"]
        CompCounsel_DraftRelatedTxn["Company Counsel: Draft Related<br/>Transactions; make materiality judgments<br/>on transactions to disclose"]
        CompCounsel_DraftUnitDesc["Company Counsel: Draft Description of<br/>Common Units from LPA terms; verify<br/>accuracy of material rights"]
        CompCounsel_DraftConflicts["Company Counsel: Draft Conflicts of<br/>Interest from LPA fiduciary provisions;<br/>cross-reference against UnitDesc<br/>& LPA terms"]
        CompCounsel_DraftFLS["Company Counsel: Draft PSLRA safe harbor<br/>disclaimer; tailor cautionary language<br/>to company-specific statements"]
        CompCounsel_DraftPartII["Company Counsel: Draft Part II<br/>(Items 13–15: expenses, indemnification,<br/>exhibits & FS schedules)"]
        CompCounsel_CompileExhibitIndex["Company Counsel: Compile exhibit index<br/>cross-referenced to S-K Item 601"]
        CompCounsel_DraftUndertakings["Company Counsel: Draft undertakings<br/>(S-K Item 512); cross-reference<br/>exhibit index against actual exhibits"]
        CompCounsel_AssembleExhibits["Company Counsel: Assemble all exhibits<br/>with correct numbering & EDGAR refs;<br/>verify completeness; manage consent<br/>logistics; produce final package"]
        CompCounsel_DraftBackMatter["Company Counsel: Compile LPA appendix<br/>& generate glossary of defined terms"]
        CompCounsel_AssembleSigPages["Company Counsel: Assemble signature pages<br/>with officer/director names & titles;<br/>attach POA references"]
        CompCounsel_DraftSummary["Company Counsel: Draft Prospectus Summary<br/>from business description & deal terms;<br/>determine emphasis, positioning &<br/>materiality (drafted last / most revised)"]
    end

    subgraph P7_T2["Track: Tax Counsel"]
        TaxCounsel_DraftTax["Tax Counsel: Draft MLP tax section;<br/>determine specific tax positions &<br/>qualifying income conclusions;<br/>incorporate into disclosure"]
    end

    subgraph P7_T3["Track: UW Counsel"]
        UWCounsel_DraftUW["UW Counsel: Draft Underwriting section<br/>from precedent & deal terms; adapt<br/>stabilization, over-allotment &<br/>syndicate provisions"]
    end

    Decision_SectionsConsistent{"Internal cross-reference<br/>QC passed?"}
end

%% ============================================================
%% PHASE 8: INITIAL FILING
%% ============================================================
subgraph P8["Phase 8: Initial Filing"]
    CompCounsel_FileFormID["Company Counsel: File Form ID with SEC<br/>for EDGAR access codes"]
    CompCounsel_AssembleDRS["Company Counsel: Assemble complete DRS<br/>with sections, exhibits & EDGAR headers;<br/>verify completeness; authorize submission"]
    Milestone_DRSFiled(["FILED: DRS confidentially submitted"])
    CompCounsel_CoordinatePostDRS["Company Counsel: Coordinate parallel<br/>post-DRS filings (FINRA + exchange)"]
    UWCounsel_CompileFINRA["UW Counsel: Compile FINRA Rule 5110<br/>information; draft application;<br/>verify compensation; authorize filing"]
    Issuer_FileListingApp["Issuer: File listing application<br/>with stock exchange"]
    CompCounsel_AssembleDRSA["Company Counsel: Assemble DRS/A<br/>incorporating all revisions; verify;<br/>authorize submission"]
    CompCounsel_AssembleS1["Company Counsel: Assemble complete S-1<br/>with all sections & exhibits; authorize<br/>public 'flip'; coordinate EDGAR"]
    Milestone_S1Public(["FILED: S-1 publicly filed"])
    CompCounsel_DraftForm8A["Company Counsel: Draft & format<br/>Form 8-A for EDGAR submission"]
end

%% ============================================================
%% PHASE 9: SEC REVIEW CYCLE
%% ============================================================
subgraph P9["Phase 9: SEC Review Cycle"]
    SEC_ReviewRS["SEC/FINRA/Exchange: SEC reviews<br/>registration statement (~30 days)"]
    Decision_SECComments{"SEC issues<br/>comments?"}
    CompCounsel_DistributeComments["Company Counsel: Parse comment letter;<br/>categorize, assess significance &<br/>develop response strategy; distribute<br/>analyzed comments with assignments"]
    CompCounsel_DraftCORRESP["Company Counsel: Draft proposed responses<br/>& redline prospectus language;<br/>exercise judgment on strategy;<br/>produce formatted CORRESP letter"]
    CompCounsel_AssembleS1A["Company Counsel: Integrate approved<br/>changes into amended S-1; verify<br/>completeness; authorize S-1/A filing"]
    CompCounsel_CompileConsents["Company Counsel: Generate updated consent<br/>forms; compile executed consents<br/>as exhibits for filing"]
    CompCounsel_RefileOpinions["Company Counsel: Re-file opinions<br/>(legality, tax) with each amendment"]
    Acct_UpdateFSDuringReview["Accountants: Update FS if required<br/>during review period"]
    Decision_SECFurtherComments{"SEC has further<br/>comments?"}
    CompCounsel_DraftSubsequent["Company Counsel: Draft responses to<br/>subsequent round comments; finalize<br/>& coordinate amendment"]
    CompCounsel_ConfirmClearance["Company Counsel: Confirm SEC clearance<br/>& coordinate next steps"]
    Milestone_SECClear(["CLEAR: SEC clears —<br/>no further comments"])
    CompCounsel_PopulatePricing["Company Counsel: Populate all pricing<br/>blanks throughout S-1; verify accuracy<br/>of populated fields"]
    CompCounsel_VerifyTiming["Company Counsel: Verify timing compliance<br/>(15-calendar-day rule); assemble<br/>filing; authorize public filing"]
end

%% ============================================================
%% PHASE 10: FINRA REVIEW
%% ============================================================
subgraph P10["Phase 10: FINRA Review"]
    SEC_FINRAReview["SEC/FINRA/Exchange: FINRA reviews<br/>underwriting compensation"]
    Decision_FINRAClears{"FINRA clears?"}
    UWCounsel_ResolveFINRA["UW Counsel: Draft responses to FINRA<br/>compensation comments; negotiate<br/>resolution"]
end

%% ============================================================
%% PHASE 11: PRE-EFFECTIVENESS & MARKETING
%% ============================================================
subgraph P11["Phase 11: Pre-Effectiveness & Marketing"]
    CompCounsel_PrintProspectus["Company Counsel: Print & distribute<br/>preliminary prospectus (red herring)"]
    Issuer_ConductRoadshow["Issuer: Conduct roadshow<br/>(management presentations; 1–2 weeks)"]
    UW_BuildOrderBook["Underwriters: Form syndicate;<br/>build order book"]
    Decision_SufficientDemand{"Sufficient investor<br/>demand at price range?"}
    Decision_RepriceOrAbandon{"Reprice or<br/>abandon?"}
    CompCounsel_DraftAccelRequest["Company Counsel: Draft & format<br/>Rule 461 acceleration request<br/>for EDGAR submission"]
    CompCounsel_ProduceFeeTable["Company Counsel: Compute filing fees<br/>(SEC Rule 457); produce Exhibit 107<br/>filing fee table"]
    Decision_AccelerationGranted{"SEC grants<br/>acceleration?"}
end

%% ============================================================
%% PHASE 12: EFFECTIVENESS
%% ============================================================
subgraph P12["Phase 12: Effectiveness"]
    SEC_DeclaresEffective["SEC/FINRA/Exchange: SEC declares<br/>registration statement effective"]
    Milestone_Effective(["SEC DECLARES REGISTRATION<br/>STATEMENT EFFECTIVE<br/>— END —"])
end

%% ============================================================
%% KILL-PATH MILESTONE
%% ============================================================
subgraph P_Kill["Kill Path"]
    Milestone_IPOAbandoned(["IPO ABANDONED — END"])
end

%% ============================================================
%% CONNECTIONS — PHASE 1
%% ============================================================
Milestone_IPOTrigger --> Issuer_EvalStrategicOptions
Issuer_EvalStrategicOptions --> Decision_ProceedIPO
Decision_ProceedIPO -->|"Yes"| CompCounsel_DetermineFilerStatus
Decision_ProceedIPO -->|"No"| Milestone_IPOAbandoned
CompCounsel_DetermineFilerStatus --> CompCounsel_SelectAccommodations
CompCounsel_SelectAccommodations --> Issuer_IPOReadiness
Issuer_IPOReadiness --> CompCounsel_ProduceStalenessCalendar
CompCounsel_ProduceStalenessCalendar --> UW_SelectComparables
UW_SelectComparables --> Issuer_EstablishPMO

%% ============================================================
%% CONNECTIONS — PHASE 1 → PHASE 2 (team assembly)
%% ============================================================
Issuer_EstablishPMO --> Issuer_SelectCompCounsel

%% ============================================================
%% CONNECTIONS — PHASE 2 (fan-out from SelectCompCounsel)
%% ============================================================
Issuer_SelectCompCounsel --> Issuer_SelectAuditor
Issuer_SelectCompCounsel --> Issuer_SelectLeadUW
Issuer_SelectCompCounsel --> Issuer_RetainResEng
Issuer_SelectCompCounsel --> Issuer_RetainTaxCounsel
Issuer_SelectCompCounsel --> Issuer_SelectPrinterTA
Issuer_SelectLeadUW --> UW_SelectUWCounsel

%% ============================================================
%% CONNECTIONS — PHASE 2 → PHASE 3 (fan-in to structure work)
%% ============================================================
Issuer_SelectAuditor --> TaxCounsel_DetermineQI
UW_SelectUWCounsel --> TaxCounsel_DetermineQI
Issuer_RetainResEng --> TaxCounsel_DetermineQI
Issuer_RetainTaxCounsel --> TaxCounsel_DetermineQI
Issuer_SelectPrinterTA --> TaxCounsel_DetermineQI

%% ============================================================
%% CONNECTIONS — PHASE 3 (MLP structure & formation)
%% ============================================================
TaxCounsel_DetermineQI --> Decision_QualifyingIncome
Decision_QualifyingIncome -->|"Yes"| Issuer_SelectDistModel
Decision_QualifyingIncome -->|"No"| Milestone_IPOAbandoned
Issuer_SelectDistModel --> CompCounsel_DetermineConveyanceFeasibility
CompCounsel_DetermineConveyanceFeasibility --> CompCounsel_DraftLPCert
CompCounsel_DraftLPCert --> CompCounsel_FileFormation
CompCounsel_FileFormation --> CompCounsel_DraftCapTermSheet
CompCounsel_DraftCapTermSheet --> CompCounsel_NegotiateCapTerms
CompCounsel_NegotiateCapTerms --> Issuer_EstablishGPGov
Issuer_EstablishGPGov --> CompCounsel_DraftLPA
%% Fan-out: 5 agreements in parallel from LPA
CompCounsel_DraftLPA --> CompCounsel_DraftContribAgreement
CompCounsel_DraftLPA --> CompCounsel_DraftRegRights
CompCounsel_DraftLPA --> CompCounsel_DraftMSA
CompCounsel_DraftLPA --> CompCounsel_DraftLTIP
CompCounsel_DraftLPA --> CompCounsel_DraftAncillary

%% ============================================================
%% CONNECTIONS — PHASE 3 → PHASE 4 (fan-in to org meeting)
%% ============================================================
CompCounsel_DraftContribAgreement --> Issuer_HoldOrgMeeting
CompCounsel_DraftRegRights --> Issuer_HoldOrgMeeting
CompCounsel_DraftMSA --> Issuer_HoldOrgMeeting
CompCounsel_DraftLTIP --> Issuer_HoldOrgMeeting
CompCounsel_DraftAncillary --> Issuer_HoldOrgMeeting

%% ============================================================
%% CONNECTIONS — PHASE 4 (fan-out from org meeting to 4 tracks)
%% ============================================================
%% Track 1: CC Admin (fan-out from CirculateChecklist)
Issuer_HoldOrgMeeting --> CompCounsel_CirculateChecklist
CompCounsel_CirculateChecklist --> CompCounsel_DistributeQuestionnaire
CompCounsel_CirculateChecklist --> CompCounsel_DraftLockUp
CompCounsel_CirculateChecklist --> CompCounsel_DraftCommsPlan
%% Track 2: UW Counsel DD
Issuer_HoldOrgMeeting --> UWCounsel_ConductBizLegalDD
%% Track 3: Financial DD
Issuer_HoldOrgMeeting --> Acct_ConductFinDD
Issuer_HoldOrgMeeting --> Acct_ConfirmComfortReadiness
%% Track 4: Reserve Engineering
Issuer_HoldOrgMeeting --> ResEng_CommissionReserves

%% Fan-in to DD decision
CompCounsel_DistributeQuestionnaire --> Decision_DDAcceptable
CompCounsel_DraftLockUp --> Decision_DDAcceptable
CompCounsel_DraftCommsPlan --> Decision_DDAcceptable
UWCounsel_ConductBizLegalDD --> Decision_DDAcceptable
Acct_ConductFinDD --> Decision_DDAcceptable
Decision_DDAcceptable -->|"Yes"| CompCounsel_DraftReorgPlan
Decision_DDAcceptable -->|"No"| Milestone_IPOAbandoned

%% ============================================================
%% CONNECTIONS — PHASE 5 (fan-out governance docs)
%% ============================================================
CompCounsel_DraftReorgPlan --> CompCounsel_DraftGovGuidelines
CompCounsel_DraftReorgPlan --> CompCounsel_DraftInsiderPolicy
CompCounsel_DraftReorgPlan --> CompCounsel_DraftLTIPAdoption
CompCounsel_DraftReorgPlan --> CompCounsel_DraftIndemnification
CompCounsel_DraftGovGuidelines --> Issuer_ObtainDOInsurance
CompCounsel_DraftInsiderPolicy --> Issuer_ObtainDOInsurance
CompCounsel_DraftLTIPAdoption --> Issuer_ObtainDOInsurance
CompCounsel_DraftIndemnification --> Issuer_ObtainDOInsurance
Issuer_ObtainDOInsurance --> Issuer_BoardResolutions
Issuer_BoardResolutions --> Decision_BoardAuthorizes
Decision_BoardAuthorizes -->|"Yes"| CompCounsel_DraftPOA
Decision_BoardAuthorizes -->|"No"| Milestone_IPOAbandoned
CompCounsel_DraftPOA --> CompCounsel_DraftConsents

%% ============================================================
%% CONNECTIONS — PHASE 5 → PHASE 6
%% ============================================================
CompCounsel_DraftConsents --> Acct_DetermineRegistrantFS

%% ============================================================
%% CONNECTIONS — PHASE 6 (core FS chain + 4-track fan-out)
%% ============================================================
%% Core sequential chain
Acct_DetermineRegistrantFS --> Acct_PrepareAuditedFS
Acct_PrepareAuditedFS --> Acct_PrepareInterimFS
Acct_PrepareInterimFS --> Acct_DetermineProFormaAdj
Acct_DetermineProFormaAdj --> Acct_DetermineNonGAAPMeasures
%% Fan-out from core FS completion into 4 parallel tracks
%% Track A: DCF / Market Risk chain
Acct_DetermineNonGAAPMeasures --> Issuer_PrepareDCFForecast
Issuer_PrepareDCFForecast --> Acct_DetermineDCFAdjustments
Acct_DetermineDCFAdjustments --> CompCounsel_DraftMarketRisk
ResEng_CommissionReserves --> ResEng_PrepareReserveData
ResEng_PrepareReserveData --> CompCounsel_DraftMarketRisk
%% Track B: XBRL Tagging
Acct_DetermineNonGAAPMeasures --> Acct_XBRLTagging
%% Track C: Staleness Monitoring
Acct_DetermineNonGAAPMeasures --> Acct_MonitorStaleness
%% Track D: Predecessor Accounting
Acct_DetermineNonGAAPMeasures --> Acct_ResolvePredecessor

%% Fan-in to VDR readiness decision
CompCounsel_DraftMarketRisk --> Decision_VDRReady
Acct_XBRLTagging --> Decision_VDRReady
Acct_MonitorStaleness --> Decision_VDRReady
Acct_ResolvePredecessor --> Decision_VDRReady
Acct_ConfirmComfortReadiness --> Decision_VDRReady
Decision_VDRReady -->|"No - Incomplete"| Acct_MonitorStaleness

%% ============================================================
%% CONNECTIONS — PHASE 7 (fan-out from VDR to 3 tracks)
%% ============================================================
Decision_VDRReady -->|"Yes"| CompCounsel_InitiateS1Drafting
%% Fan-out from coordinator to all tracks
%% Track 1: CC Prospectus — independent sections
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftFrontMatter
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftRiskFactors
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftUseOfProceeds
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftDistPolicy
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftCapTable
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftHistFinData
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftMDA
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftBusiness
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftMgmt
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftComp
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftOwnershipTable
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftRelatedTxn
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftUnitDesc
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftFLS
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftPartII
CompCounsel_InitiateS1Drafting --> CompCounsel_CompileExhibitIndex
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftUndertakings
CompCounsel_InitiateS1Drafting --> CompCounsel_AssembleExhibits
CompCounsel_InitiateS1Drafting --> CompCounsel_DraftBackMatter
CompCounsel_InitiateS1Drafting --> CompCounsel_AssembleSigPages
%% Serial dependencies within CC track
CompCounsel_DraftCapTable --> CompCounsel_DraftDilution
CompCounsel_DraftUnitDesc --> CompCounsel_DraftConflicts
%% Track 2: Tax Counsel
CompCounsel_InitiateS1Drafting --> TaxCounsel_DraftTax
%% Track 3: UW Counsel
CompCounsel_InitiateS1Drafting --> UWCounsel_DraftUW

%% Fan-in: all sections converge on Summary (drafted last)
CompCounsel_DraftFrontMatter --> CompCounsel_DraftSummary
CompCounsel_DraftRiskFactors --> CompCounsel_DraftSummary
CompCounsel_DraftUseOfProceeds --> CompCounsel_DraftSummary
CompCounsel_DraftDistPolicy --> CompCounsel_DraftSummary
CompCounsel_DraftDilution --> CompCounsel_DraftSummary
CompCounsel_DraftHistFinData --> CompCounsel_DraftSummary
CompCounsel_DraftMDA --> CompCounsel_DraftSummary
CompCounsel_DraftBusiness --> CompCounsel_DraftSummary
CompCounsel_DraftMgmt --> CompCounsel_DraftSummary
CompCounsel_DraftComp --> CompCounsel_DraftSummary
CompCounsel_DraftOwnershipTable --> CompCounsel_DraftSummary
CompCounsel_DraftRelatedTxn --> CompCounsel_DraftSummary
CompCounsel_DraftConflicts --> CompCounsel_DraftSummary
CompCounsel_DraftFLS --> CompCounsel_DraftSummary
CompCounsel_DraftPartII --> CompCounsel_DraftSummary
CompCounsel_CompileExhibitIndex --> CompCounsel_DraftSummary
CompCounsel_DraftUndertakings --> CompCounsel_DraftSummary
CompCounsel_AssembleExhibits --> CompCounsel_DraftSummary
CompCounsel_DraftBackMatter --> CompCounsel_DraftSummary
CompCounsel_AssembleSigPages --> CompCounsel_DraftSummary
TaxCounsel_DraftTax --> CompCounsel_DraftSummary
UWCounsel_DraftUW --> CompCounsel_DraftSummary

%% Summary → QC gate
CompCounsel_DraftSummary --> Decision_SectionsConsistent
Decision_SectionsConsistent -->|"Yes"| CompCounsel_FileFormID
Decision_SectionsConsistent -->|"No - Fix"| CompCounsel_InitiateS1Drafting

%% ============================================================
%% CONNECTIONS — PHASE 8
%% ============================================================
CompCounsel_FileFormID --> CompCounsel_AssembleDRS
CompCounsel_AssembleDRS --> Milestone_DRSFiled
Milestone_DRSFiled --> CompCounsel_CoordinatePostDRS
CompCounsel_CoordinatePostDRS --> UWCounsel_CompileFINRA
CompCounsel_CoordinatePostDRS --> Issuer_FileListingApp
UWCounsel_CompileFINRA --> CompCounsel_AssembleDRSA
Issuer_FileListingApp --> CompCounsel_AssembleDRSA
CompCounsel_AssembleDRSA --> CompCounsel_AssembleS1
CompCounsel_AssembleS1 --> Milestone_S1Public
Milestone_S1Public --> CompCounsel_DraftForm8A

%% ============================================================
%% CONNECTIONS — PHASE 8 → PHASE 9
%% ============================================================
CompCounsel_DraftForm8A --> SEC_ReviewRS

%% ============================================================
%% CONNECTIONS — PHASE 9
%% ============================================================
SEC_ReviewRS --> Decision_SECComments
Decision_SECComments -->|"Yes"| CompCounsel_DistributeComments
Decision_SECComments -->|"No"| CompCounsel_ConfirmClearance
CompCounsel_DistributeComments --> CompCounsel_DraftCORRESP
CompCounsel_DraftCORRESP --> CompCounsel_AssembleS1A
CompCounsel_AssembleS1A --> CompCounsel_CompileConsents
CompCounsel_CompileConsents --> CompCounsel_RefileOpinions
CompCounsel_RefileOpinions --> Acct_UpdateFSDuringReview
Acct_UpdateFSDuringReview --> Decision_SECFurtherComments
Decision_SECFurtherComments -->|"Yes"| CompCounsel_DraftSubsequent
Decision_SECFurtherComments -->|"No"| CompCounsel_ConfirmClearance
CompCounsel_DraftSubsequent --> CompCounsel_AssembleS1A
CompCounsel_ConfirmClearance --> Milestone_SECClear
Milestone_SECClear --> CompCounsel_PopulatePricing
CompCounsel_PopulatePricing --> CompCounsel_VerifyTiming

%% ============================================================
%% CONNECTIONS — PHASE 9 → PHASE 10
%% ============================================================
CompCounsel_VerifyTiming --> SEC_FINRAReview

%% ============================================================
%% CONNECTIONS — PHASE 10
%% ============================================================
SEC_FINRAReview --> Decision_FINRAClears
Decision_FINRAClears -->|"No Objections"| CompCounsel_PrintProspectus
Decision_FINRAClears -->|"Objections"| UWCounsel_ResolveFINRA
UWCounsel_ResolveFINRA --> Decision_FINRAClears

%% ============================================================
%% CONNECTIONS — PHASE 11
%% ============================================================
CompCounsel_PrintProspectus --> Issuer_ConductRoadshow
Issuer_ConductRoadshow --> UW_BuildOrderBook
UW_BuildOrderBook --> Decision_SufficientDemand
Decision_SufficientDemand -->|"Yes"| CompCounsel_DraftAccelRequest
Decision_SufficientDemand -->|"No"| Decision_RepriceOrAbandon
Decision_RepriceOrAbandon -->|"Reprice"| CompCounsel_PopulatePricing
Decision_RepriceOrAbandon -->|"Abandon"| Milestone_IPOAbandoned
CompCounsel_DraftAccelRequest --> CompCounsel_ProduceFeeTable
CompCounsel_ProduceFeeTable --> Decision_AccelerationGranted
Decision_AccelerationGranted -->|"Yes"| SEC_DeclaresEffective
Decision_AccelerationGranted -->|"No"| CompCounsel_DraftAccelRequest

%% ============================================================
%% CONNECTIONS — PHASE 12
%% ============================================================
SEC_DeclaresEffective --> Milestone_Effective

%% ============================================================
%% STYLE DEFINITIONS — Okabe-Ito Colorblind-Safe Palette
%% ============================================================
%%
%% Color Legend (8 roles):
%% Company Counsel          = Blue           #0072B2  (white text)
%% Issuer (Management)      = Orange         #E69F00  (black text)
%% Underwriters             = Sky Blue       #56B4E9  (black text)
%% Underwriters' Counsel    = Bluish Green   #009E73  (white text)
%% Accountants              = Yellow         #F0E442  (black text)
%% Reserve Engineer         = Vermillion     #D55E00  (white text)
%% Tax Counsel              = Reddish Purple #CC79A7  (black text)
%% SEC/FINRA/Exchange       = Black          #000000  (white text)
%%
%% Decision diamonds: default Mermaid styling (except external-party diamonds)
%% Milestones: default Mermaid styling
%%

%% --- Company Counsel (Blue #0072B2) ---
style CompCounsel_DetermineFilerStatus stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_SelectAccommodations stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_ProduceStalenessCalendar stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DetermineConveyanceFeasibility stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftLPCert stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_FileFormation stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftCapTermSheet stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_NegotiateCapTerms stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftLPA stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftContribAgreement stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftRegRights stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftMSA stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftLTIP stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftAncillary stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_CirculateChecklist stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DistributeQuestionnaire stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftLockUp stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftCommsPlan stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftReorgPlan stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftGovGuidelines stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftInsiderPolicy stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftLTIPAdoption stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftIndemnification stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftPOA stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftConsents stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftMarketRisk stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_InitiateS1Drafting stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftFrontMatter stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftRiskFactors stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftUseOfProceeds stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftDistPolicy stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftCapTable stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftDilution stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftHistFinData stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftMDA stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftBusiness stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftMgmt stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftComp stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftOwnershipTable stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftRelatedTxn stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftUnitDesc stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftConflicts stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftFLS stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftPartII stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_CompileExhibitIndex stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftUndertakings stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_AssembleExhibits stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftBackMatter stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_AssembleSigPages stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftSummary stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_FileFormID stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_AssembleDRS stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_CoordinatePostDRS stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_AssembleDRSA stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_AssembleS1 stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftForm8A stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DistributeComments stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftCORRESP stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_AssembleS1A stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_CompileConsents stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_RefileOpinions stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftSubsequent stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_ConfirmClearance stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_PopulatePricing stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_VerifyTiming stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_PrintProspectus stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_DraftAccelRequest stroke:#000,fill:#0072B2,color:#FFF
style CompCounsel_ProduceFeeTable stroke:#000,fill:#0072B2,color:#FFF

%% --- Issuer / Management (Orange #E69F00) ---
style Issuer_EvalStrategicOptions stroke:#000,fill:#E69F00,color:#000
style Issuer_IPOReadiness stroke:#000,fill:#E69F00,color:#000
style Issuer_EstablishPMO stroke:#000,fill:#E69F00,color:#000
style Issuer_SelectCompCounsel stroke:#000,fill:#E69F00,color:#000
style Issuer_SelectAuditor stroke:#000,fill:#E69F00,color:#000
style Issuer_SelectLeadUW stroke:#000,fill:#E69F00,color:#000
style Issuer_RetainResEng stroke:#000,fill:#E69F00,color:#000
style Issuer_RetainTaxCounsel stroke:#000,fill:#E69F00,color:#000
style Issuer_SelectPrinterTA stroke:#000,fill:#E69F00,color:#000
style Issuer_SelectDistModel stroke:#000,fill:#E69F00,color:#000
style Issuer_EstablishGPGov stroke:#000,fill:#E69F00,color:#000
style Issuer_HoldOrgMeeting stroke:#000,fill:#E69F00,color:#000
style Issuer_ObtainDOInsurance stroke:#000,fill:#E69F00,color:#000
style Issuer_BoardResolutions stroke:#000,fill:#E69F00,color:#000
style Issuer_PrepareDCFForecast stroke:#000,fill:#E69F00,color:#000
style Issuer_FileListingApp stroke:#000,fill:#E69F00,color:#000
style Issuer_ConductRoadshow stroke:#000,fill:#E69F00,color:#000

%% --- Underwriters (Sky Blue #56B4E9) ---
style UW_SelectComparables stroke:#000,fill:#56B4E9,color:#000
style UW_SelectUWCounsel stroke:#000,fill:#56B4E9,color:#000
style UW_BuildOrderBook stroke:#000,fill:#56B4E9,color:#000

%% --- UW Counsel (Bluish Green #009E73) ---
style UWCounsel_ConductBizLegalDD stroke:#000,fill:#009E73,color:#FFF
style UWCounsel_CompileFINRA stroke:#000,fill:#009E73,color:#FFF
style UWCounsel_DraftUW stroke:#000,fill:#009E73,color:#FFF
style UWCounsel_ResolveFINRA stroke:#000,fill:#009E73,color:#FFF

%% --- Accountants (Yellow #F0E442) ---
style Acct_ConductFinDD stroke:#000,fill:#F0E442,color:#000
style Acct_ConfirmComfortReadiness stroke:#000,fill:#F0E442,color:#000
style Acct_DetermineRegistrantFS stroke:#000,fill:#F0E442,color:#000
style Acct_PrepareAuditedFS stroke:#000,fill:#F0E442,color:#000
style Acct_PrepareInterimFS stroke:#000,fill:#F0E442,color:#000
style Acct_DetermineProFormaAdj stroke:#000,fill:#F0E442,color:#000
style Acct_DetermineNonGAAPMeasures stroke:#000,fill:#F0E442,color:#000
style Acct_DetermineDCFAdjustments stroke:#000,fill:#F0E442,color:#000
style Acct_XBRLTagging stroke:#000,fill:#F0E442,color:#000
style Acct_MonitorStaleness stroke:#000,fill:#F0E442,color:#000
style Acct_ResolvePredecessor stroke:#000,fill:#F0E442,color:#000
style Acct_UpdateFSDuringReview stroke:#000,fill:#F0E442,color:#000

%% --- Reserve Engineer (Vermillion #D55E00) ---
style ResEng_CommissionReserves stroke:#000,fill:#D55E00,color:#FFF
style ResEng_PrepareReserveData stroke:#000,fill:#D55E00,color:#FFF

%% --- Tax Counsel (Reddish Purple #CC79A7) ---
style TaxCounsel_DetermineQI stroke:#000,fill:#CC79A7,color:#000
style TaxCounsel_DraftTax stroke:#000,fill:#CC79A7,color:#000

%% --- SEC/FINRA/Exchange (Black #000000) ---
style SEC_ReviewRS stroke:#000,fill:#000000,color:#FFF
style SEC_FINRAReview stroke:#000,fill:#000000,color:#FFF
style SEC_DeclaresEffective stroke:#000,fill:#000000,color:#FFF

%% --- External-party decision diamonds (styled per §8.4) ---
style Decision_SECComments stroke:#000,fill:#000000,color:#FFF
style Decision_SECFurtherComments stroke:#000,fill:#000000,color:#FFF
style Decision_FINRAClears stroke:#000,fill:#000000,color:#FFF
style Decision_AccelerationGranted stroke:#000,fill:#000000,color:#FFF
