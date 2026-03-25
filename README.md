🇮🇳 India General Election 2024 Reporting System

📊 Overview

The India General Election 2024 Reporting System is a Business Intelligence (BI) solution designed to analyze and visualize one of the largest democratic exercises in the world.
This system converts complex electoral data into actionable insights by focusing on:
Alliance-based performance
Multi-level analytics (National → State → Constituency)
Advanced candidate ranking logic
🎯 Target Users: Political analysts, researchers, policymakers


🚀 Project Scope
The system provides a multi-tier architecture:
Level
Description
🌍 National
  Overall alliance performance
🏛 State
  Regional political trends
📍 Constituency
  Candidate-level insights


🧩 Data Categorization & Alliance Logic

🔗 Alliance Mapping
  To ensure accurate analysis, parties are grouped   into:

🟠 NDA Alliance
  BJP, TDP, JD(U), SHS, AJSUP, ADAL, AGP, HAMS, JnP,     JD(S), LJPRV, NCP, RLD, SKM

🟢 I.N.D.I.A Alliance
  INC, AAAP, AITC, CPI(M), CPI, DMK, SP, RJD, JMM,     etc.

⚪ OTHER
  Independent candidates
  Unlisted parties


🛠 DAX: Alliance Mapping

DAX
Party Alliance = 
IF( 
    partywise_results[Party] IN {
        "Bharatiya Janata Party - BJP",
        "Telugu Desam - TDP",
        "Janata Dal  (United) - JD(U)",
        "Shiv Sena - SHS"
    }, 
    "NDA",
    IF(
        partywise_results[Party] IN {
            "Indian National Congress - INC",
            "Aam Aadmi Party - AAAP",
            "All India Trinamool Congress - AITC"
        },
        "I.N.D.I.A.",
        "OTHER"
    )
)


🔄 Data Modeling

Use LOOKUPVALUE to connect tables:

DAX
Party Alliance (result) = 
LOOKUPVALUE(
    partywise_results[Party Alliance], 
    partywise_results[Party ID], 
    constituencywise_results[Party ID]
)
Other mappings:
Party Name
Party Short Name


📈 Core KPI Framework

🏆 Alliance Seat Counts


DAX
NDA Seats count = 
CALCULATE(
    COUNT(constituencywise_results[Constituency Name]), 
    partywise_results[Party Alliance] = "NDA"
)


DAX
I.N.D.I.A Seats count = 
CALCULATE(
    COUNT(constituencywise_results[Constituency Name]), 
    partywise_results[Party Alliance] = "I.N.D.I.A."
)



🥇 Winning Alliance Logic
DAX
Winning Alliance = 
VAR NDASeats = [NDA Seats count]
VAR INDIAseats = [I.N.D.I.A Seats count]
RETURN IF(NDASeats >= INDIAseats, "NDA", "I.N.D.I.A.")



📊 Seat Share %

  Seat Share = Alliance Seats / Total Seats


🧠 Advanced Candidate Metrics

🥈 Runner-Up Logic
DAX
  Runner UP Candidate = 
  VAR Maxvotes = MAX(constituencywise_details[Total              Votes])
  VAR Secondmaxvotes = 
      MAXX(
          FILTER(
              constituencywise_details,
              constituencywise_details[Total Votes]     < Maxvotes
        ),
          constituencywise_details[Total Votes]
      )
  RETURN 
      CALCULATE(
          MAX(constituencywise_details[Candidate]),
          constituencywise_details[Total Votes] =   Secondmaxvotes
    )


🥉 Second Runner-Up

DAX
  Second Runner UP Candidate =
  -- Logic continues similarly


📏 Margin of Victory

  Margin = Winner Votes - Runner-Up Votes



📊 Dashboard Architecture

1️⃣ Overview Dashboard
  KPI Cards (NDA / I.N.D.I.A / Others)
  Seat Share %
  Party-wise Matrix

2️⃣ State Demographic
  🗺 Filled Map (State-wise alliance)
  🔵 Bubble Map (Constituency-level)

3️⃣ Political Landscape
  State Filter (Slicer)
  Party-wise Seat Share (Donut Chart)
  Candidate Grid

4️⃣ Constituency Analysis
  Total Votes
  EVM vs Postal Votes
  Top 3 Candidates

5️⃣ Details Grid
  Full dataset view
  Export to Excel supported

6️⃣ Landing Page
  Navigation hub
  Clean UI with clickable sections

----------------------------------------------------
🔁 Interactivity Features

🔍 Drill-Through
  State → Constituency → Details

🧭 Navigation
  Home Button (All pages)
  Clear Filters (Bookmark)

📤 Export Data
  Download filtered data (Excel)

⚙️ Technical Guidelines

⚡ Performance
  Use VAR in DAX
Avoid repeated calculations
🎨 UI/UX
Minimalist design
Alliance colors:
   🟠 NDA
   🟢 I.N.D.I.A
   ⚪ Others
📱 Responsiveness

Optimized for:
  Desktop
  Large displays
📌 Key Highlights

  ✅ Alliance-based analytics
  ✅ Multi-level drill-down
  ✅ Advanced candidate ranking
  ✅ Interactive dashboards
  ✅ Export-ready data

🏁 Conclusion

This system provides a single source of truth for understanding India’s 2024 election results through:
Data accuracy
Scalable architecture
Insight-driven reporting