Technical Specification: India General Election 2024 Reporting System

1. Executive Summary and Project Scope

The India General Election 2024 Reporting System is a strategic Business Intelligence solution designed to transform the massive scale of Indian electoral data into a high-fidelity analytical narrative. In the world's largest democracy, interpreting results requires more than simple tallying; it demands the ability to synthesize multi-party performance into coherent alliance-based insights. This system serves political analysts, researchers, and policymakers by providing a single source of truth for understanding regional mandates and voter sentiment.

The project scope encompasses a multi-tiered reporting architecture providing visibility at National, State, and Constituency levels. Utilizing the "India General Election Results – 2024" dataset, the system is engineered to provide a robust framework for tracking performance across complex political coalitions. The technical objective is to deliver a high-performance architecture capable of handling granular voter behavior analysis and sophisticated ranking logic. This technical foundation begins with the rigorous categorization of political entities and their respective alliance affiliations.

2. Data Categorization and Alliance Logic

In India’s multi-party parliamentary system, individual party performance is often secondary to the collective strength of political coalitions. Precise alliance mapping is a strategic necessity for this BI system, as it directly impacts the determination of the "Majority Alliance" and dictates the overall narrative of the election results.

Alliance Classification Table

To maintain analytical integrity, parties must be mapped to specific "buckets." This mapping ensures that the aggregate strength of the primary competing blocs—the National Democratic Alliance (NDA) and the Indian National Developmental Inclusive Alliance (I.N.D.I.A.)—is accurately reflected.

Alliance Group	Included Political Parties
NDA Alliance	BJP, TDP, JD(U), SHS, AJSUP, ADAL, AGP, HAMS, JnP, JD(S), LJPRV, NCP, RLD, SKM
I.N.D.I.A. Alliance	INC, AAAP, AITC, BHRTADVSIP, CPI(M), CPI(ML)(L), CPI, DMK, IUML, JKN, JMM, KEC, MDMK, NCPSP, RJD, RLTP, RSP, SP, SHSUBT, VCK
OTHER	All independent candidates and parties not explicitly listed above.

Technical Implementation: Alliance Mapping

The following DAX calculated column must be implemented in the partywise_results table to establish the alliance logic.

Party Alliance = 
IF( 
    partywise_results[Party] = "Bharatiya Janata Party - BJP" || 
    partywise_results[Party] = "Telugu Desam - TDP" || 
    partywise_results[Party] = "Janata Dal  (United) - JD(U)" || 
    partywise_results[Party] = "Shiv Sena - SHS" || 
    partywise_results[Party] = "AJSU Party - AJSUP" || 
    partywise_results[Party] = "Apna Dal (Soneylal) - ADAL" || 
    partywise_results[Party] = "Asom Gana Parishad - AGP" || 
    partywise_results[Party] = "Hindustani Awam Morcha (Secular) - HAMS" || 
    partywise_results[Party] = "Janasena Party - JnP" || 
    partywise_results[Party] = "Janata Dal  (Secular) - JD(S)" || 
    partywise_results[Party] = "Lok Janshakti Party(Ram Vilas) - LJPRV" || 
    partywise_results[Party] = "Nationalist Congress Party - NCP" || 
    partywise_results[Party]= "Rashtriya Lok Dal - RLD" || 
    partywise_results[Party] = "Sikkim Krantikari Morcha - SKM", 
    "NDA", 
    IF( 
        partywise_results[Party] = "Indian National Congress - INC" || 
        partywise_results[Party] = "Aam Aadmi Party - AAAP" || 
        partywise_results[Party] = "All India Trinamool Congress - AITC" || 
        partywise_results[Party] = "Bharat Adivasi Party - BHRTADVSIP" || 
        partywise_results[Party]= "Communist Party of India  (Marxist) - CPI(M)" || 
        partywise_results[Party] = "Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)" || 
        partywise_results[Party] = "Communist Party of India - CPI" || 
        partywise_results[Party] = "Dravida Munnetra Kazhagam - DMK" || 
        partywise_results[Party] = "Indian Union Muslim League - IUML" || 
        partywise_results[Party] = "Jammu & Kashmir National Conference - JKN" || 
        partywise_results[Party] = "Jharkhand Mukti Morcha - JMM" || 
        partywise_results[Party] = "Kerala Congress - KEC" || 
        partywise_results[Party] = "Marumalarchi Dravida Munnetra Kazhagam - MDMK" || 
        partywise_results[Party] = "Nationalist Congress Party Sharadchandra Pawar - NCPSP" || 
        partywise_results[Party] = "Rashtriya Janata Dal - RJD" || 
        partywise_results[Party] = "Rashtriya Loktantrik Party - RLTP" || 
        partywise_results[Party] = "Revolutionary Socialist Party - RSP" || 
        partywise_results[Party] = "Samajwadi Party - SP" || 
        partywise_results[Party] = "Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT" || 
        partywise_results[Party] = "Viduthalai Chiruthaigal Katchi - VCK", 
        "I.N.D.I.A.", 
        "OTHER" 
    ) 
)


Data Modeling and Relationships

To ensure high-performance filtering across the model, use the LOOKUPVALUE function to propagate party attributes into the constituencywise_results table:

* Party Alliance (result): LOOKUPVALUE(partywise_results[Party Alliance], partywise_results[Party ID], constituencywise_results[Party ID])
* Party Name (result): LOOKUPVALUE(partywise_results[Party], partywise_results[Party ID], constituencywise_results[Party ID])
* Party Short Name: LOOKUPVALUE(partywise_results[short_party], partywise_results[Party ID], constituencywise_results[Party ID])

With this logic established, the system can power the high-level KPI framework.

3. Core KPI Framework and Calculation Logic

Standardized Key Performance Indicators (KPIs) ensure consistency across different analytical lenses. Whether viewing data at a national scale or drilling into a specific state, the underlying logic for calculating seats and alliance dominance remains uniform.

Primary DAX Measures

The following measures are mandatory for the Overview dashboard:

Alliance Seat Counts:

NDA Seats count = CALCULATE(COUNT(constituencywise_results[Constituency Name]), partywise_results[Party Alliance]="NDA")

I.N.D.I.A Seats count = CALCULATE(COUNT(constituencywise_results[Constituency Name]), partywise_results[Party Alliance]="I.N.D.I.A.")


Winning Alliance Logic (with Tie-Break): This measure determines the overall winner. Per technical requirements, the NDA is identified as the winner in the event of a tie.

Winning Alliance = 
VAR NDASeats = [NDA Seats count]
VAR INDIAseats = [I.N.D.I.A Seats count]
RETURN IF(NDASeats >= INDIAseats, "NDA", "I.N.D.I.A.")


Percentage of Total Seats

To provide a relative measure of dominance, the system must calculate seat shares. This is critical for comparative analysis between election years or across states with varying constituency counts.

* Formula: [Alliance Seat Count] / COUNTROWS(constituencywise_results)

This macro-level framework provides the necessary context for analyzing the competitiveness of individual candidates.

4. Advanced Candidate Performance Metrics (Runner-Up Logic)

Tracking runner-up and second runner-up data is essential for identifying "swing" constituencies and understanding the fragmentation of voter choice. This logic identifies candidates who, while not winning, commanded significant voter share.

Ranking and Competitive Metrics

The following DAX logic must be applied to identify the top three performers within a constituency. Note the use of variables (VAR) to manage the filter context of total votes.

Runner-Up Candidate:

Runner UP Condidate = 
VAR Maxvotes = MAX(constituencywise_details[Total Votes]) 
VAR Secondmaxvotes = MAXX( 
    FILTER(constituencywise_details, constituencywise_details[Total Votes] < Maxvotes), 
    constituencywise_details[Total Votes]
) 
RETURN CALCULATE( 
    MAX(constituencywise_details[Candidate]), 
    constituencywise_details[Total Votes] = Secondmaxvotes
)


Runner-Up Statistics (Formatted):

Runner UP vote = "Total Votes :" & [Secondmaxvotes_Logic]

Runner UP vote share = "Total Votes share : " & [Secondmaxvotes_Percent_Logic] & " %"


Second Runner-Up Candidate:

Second Runner UP Condidate = 
VAR Maxvotes = MAX(constituencywise_details[Total Votes]) 
VAR Secondmaxvotes = MAXX( 
    FILTER(constituencywise_details, constituencywise_details[Total Votes] < Maxvotes), 
    constituencywise_details[Total Votes]
) 
VAR thirdmaxvotes = MAXX( 
    FILTER(constituencywise_details, constituencywise_details[Total Votes] < Secondmaxvotes), 
    constituencywise_details[Total Votes]
)
RETURN CALCULATE( 
    MAX(constituencywise_details[Candidate]), 
    constituencywise_details[Total Votes] = thirdmaxvotes
)


Margin of Victory: Calculated as: Winner_Total_Votes - RunnerUp_Total_Votes.

These measures are designed to be context-aware, ensuring they recalculate accurately when the user filters by State or specific Constituency. This complexity is essential for the multi-tiered visual layer.

5. Multi-Tiered Dashboard Functional Specifications

The dashboard suite is designed on an "Exploration Path" philosophy, guiding the user from national trends to micro-level details across six specialized views.

Dashboard 1: Overview Analysis

* Objective: National seat distribution.
* Visuals: KPI cards for NDA, I.N.D.I.A., and Others; Percentage share cards; a Grid Matrix showing seat counts for all alliance parties with logos.

Dashboard 2: State Demographic

* Visual 1 (State Map): A filled map showing states color-coded by the majority alliance.
  * Tooltip: Total Seats, Majority Alliance, NDA Seats count, I.N.D.I.A. Seats count.
* Visual 2 (Bubble Map): A granular map where each bubble represents a constituency.
  * Logic: Color-code bubbles by winning alliance (NDA, I.N.D.I.A., OTHER).

Dashboard 3: Political Landscape by State

* Objective: Comparative party analysis within a specific state.
* Visuals: Dynamic State filter (Slicer); Party-wise Result Grid (Candidate name, Party, Alliance); Donut Chart showing "Party-wise Seat Share."

Dashboard 4: Constituency Analysis

* Objective: Micro-detail on local performance.
* KPIs: Total Votes, EVM Votes, Postal Votes, Total Candidates.
* Candidate Table: List State, Name, Party, Total Votes, and Vote Share (%) for the Winner, Runner-Up, and Second Runner-Up.

Dashboard 5: Details Grid

* Objective: Master data repository and drill-through destination.
* Grid Fields: Constituency Name, Winning Candidate, Runner-Up Candidate, Party Name (Winning), Party Alliance, EVM Votes, Postal Votes, Total Votes, Margin.

Dashboard 6: Landing Page

* Objective: Navigation hub.
* Requirements: Clickable cards for all dashboards, minimalist icons, and clear labeling.

6. Interactivity, Navigation, and Drill-Through Architecture

The user experience is defined by the "Exploration Path," allowing users to navigate between national trends and local outcomes without loss of context.

Navigation and Drill-Through Features

* Drill-Through Logic: Users can right-click a state or constituency bubble in Dashboard 2 and drill through to Dashboard 5 (Details Grid). The destination must be pre-filtered based on the selected geography.
* Bookmark Management:
  * Home Button: A persistent button on all sub-dashboards to return to the Landing Page.
  * Show All Data: A "Clear Filters" button on the Details Grid using bookmarks to reset all drill-through and slicer selections.
* Data Portability: The Details Grid must support "Export Data" functionality, allowing researchers to download filtered views into Excel format.

Technical Constraints and UI Standards

* Performance: DAX measures should utilize variables to minimize repetitive calculations.
* UI/UX: A minimalist design using standard alliance colors (e.g., Saffron/Green logic) for quick recognition.
* Responsiveness: The layout must be optimized for a variety of viewports, including desktops and large-scale analytical displays.
