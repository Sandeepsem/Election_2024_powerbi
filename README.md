India General Election Results 2024 Dashboard Analysis
Project Report
1. Introduction

The India General Election Results 2024 Dashboard is designed to provide a comprehensive analytical view of election outcomes across India. The dashboard allows users to analyze political performance at multiple levels such as national, state, and constituency.

The project was developed using Microsoft Power BI, enabling interactive visualizations, drill-through analysis, and dynamic filtering. It helps political analysts, researchers, students, and the general public to understand the political landscape of the country through data-driven insights.

The dashboard includes multiple pages that display:

Alliance-wise seat distribution

State-wise election performance

Constituency-level candidate analysis

Party performance comparison

Detailed tabular data for deeper analysis

2. Objectives of the Project

The main objectives of this dashboard are:

To analyze seat distribution among major alliances such as NDA and I.N.D.I.A.

To identify state-wise political dominance

To analyze candidate performance at constituency level

To create interactive dashboards for data exploration

To enable drill-through and export functionality for deeper analysis

3. Tools & Technologies Used
Tool	Purpose
Power BI	Data visualization and dashboard creation
DAX	Data modeling and calculations
Excel / Dataset	Source data
Map Visualization	State and constituency analysis
4. Data Model Overview

The dataset consists of multiple tables used to analyze election results.

Main Tables

constituencywise_results

Constituency Name

Party ID

Candidate Name

Total Votes

Margin

constituencywise_details

Candidate

Party

Total Votes

Vote Share %

partywise_results

Party ID

Party Name

Party Alliance

states

State Name

State ID

Relationships are created between tables using Party ID, Constituency ID, and State ID.

5. Dashboard 1: Overview Analysis

The Overview Analysis Dashboard provides a national level summary of election results focusing on alliance performance.

Key KPIs
1. NDA Performance

Total seats won by the NDA Alliance

Percentage of seats secured by NDA

Bookmark table showing all NDA parties and their seat counts

2. I.N.D.I.A. Performance

Total seats won by the I.N.D.I.A Alliance

Percentage of seats secured by I.N.D.I.A

Bookmark grid displaying alliance parties and seat distribution

3. Independent & Other Parties Performance

Total seats won by independent candidates and smaller parties

Percentage share of total seats

Grid matrix showing seat distribution for other parties

Alliance Analysis

NDA Alliance Analysis

Identifies all parties within the NDA coalition

Displays party logos and seat counts

I.N.D.I.A Alliance Analysis

Identifies all parties within the alliance

Shows party-wise seat share

6. Dashboard 2: State Demographic Analysis

This dashboard focuses on state-wise election analysis.

Features
1. Total Seats & Alliance Majority by State

Displays:

Total Seats

NDA Seats

I.N.D.I.A Seats

Majority Alliance

Visualization

Map Chart by State

Tooltip showing seat distribution

Drill-through option for detailed data

2. Winning Candidate by Constituency

Displays:

Winning Candidate

Party Name

Total Votes

Margin

Visualization

Bubble Map Chart

Each bubble represents a constituency

Color coding:

Color	Alliance
Saffron	NDA
Blue	I.N.D.I.A
Grey	Others
3. State with Maximum Seats

This visualization identifies the state where:

NDA or I.N.D.I.A secured the most seats.

Visualization includes:

State Map Chart

Majority alliance color coding

Drill-through data table

7. Dashboard 3: Political Landscape by State

This dashboard allows state-level analysis.

Dynamic Filter

Users can select a specific state.

Key KPIs

Seats won by NDA

Seats won by I.N.D.I.A

Seats won by Independent / Other parties

Visualizations
1. State Map

Shows constituency boundaries and seat distribution.

2. Party-wise Result Grid

Displays:

Party Name

Alliance

Seats won

3. Party Seat Share

Donut chart showing:

Percentage share of seats by each party.

8. Dashboard 4: Constituency Analysis

This dashboard provides detailed insights into individual constituency election results.

Primary KPIs

Total Votes Cast

Total EVM Votes

Total Postal Votes

Total Candidates Participated

Candidate Performance KPIs
Winning Candidate

State

Candidate Name

Party

Total Votes

Vote Share %

Runner-Up Candidate

State

Candidate Name

Party

Total Votes

Vote Share %

Second Runner-Up Candidate

State

Candidate Name

Party

Total Votes

Vote Share %

9. Dashboard 5: Details Grid

This dashboard provides a complete tabular dataset view.

Fields Included

Constituency Name

Winning Candidate

Runner-Up Candidate

Party Name

Party Alliance

EVM Votes

Postal Votes

Total Votes

Margin

Functionalities
1. Drill-Through

Users can drill from other dashboards to view detailed constituency data.

2. Export Data

Users can export the grid into Excel format.

3. Show All Data Button

A bookmark button is implemented to reset filters and display complete data.

10. Dashboard 6: Landing Page

The landing page acts as the central navigation hub for the dashboard.

Navigation Buttons

Users can navigate to:

Overview Analysis

State Demographics

Political Landscape by State

Constituency Analysis

Features

Clean UI Design

Interactive Icons

Hover Effects

Responsive Layout for different screen sizes

Each dashboard contains a Home Button to return to the landing page.
