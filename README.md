# Cyclistic Bike-Share Case Study
## Data Analysis Report: ASK & PREPARE Phases

**Analyst:** [Your Name]  
**Date:** February 3, 2026  
**Project:** Converting Casual Riders to Annual Members

---

## PHASE 1: ASK

### Business Problem

Cyclistic, a Chicago-based bike-share company, has identified that annual members are significantly more profitable than casual riders. The Director of Marketing, Lily Moreno, believes that maximizing annual memberships is critical for future growth. Rather than targeting entirely new customers, the company sees an opportunity to convert existing casual riders who are already familiar with the Cyclistic program.

### Guiding Questions

**What is the problem we are trying to solve?**

The company lacks understanding of how casual riders and annual members use Cyclistic bikes differently. Without this insight, we cannot design effective, targeted marketing strategies to convert casual riders into annual members.

**How can insights drive business decisions?**

This analysis will reveal behavioral patterns and usage differences between customer segments, enabling:
- Targeted marketing message development based on actual usage patterns
- Strategic campaign design that addresses specific casual rider needs
- Data-backed recommendations for executive decision-making on resource allocation

### Business Task Statement

**"Analyze Cyclistic historical bike trip data to identify how annual members and casual riders use bikes differently, in order to inform marketing design strategies aimed at converting casual riders into annual members."**

### Key Stakeholders

**Primary Stakeholders:**
- **Lily Moreno** (Director of Marketing) - Requires insights to design conversion marketing strategies
- **Cyclistic Executive Team** - Must approve marketing program; requires compelling data insights and professional visualizations
- **Cyclistic Marketing Analytics Team** - Responsible for data collection, analysis, and reporting

**Stakeholder Requirements:**
- Executive team is "notoriously detail-oriented" - analysis must be thorough and well-documented
- Recommendations must be backed by compelling data insights
- Visualizations must be sophisticated and polished
- Insights must be actionable and specific for campaign design

### Initial Hypothesis

Based on industry patterns, we anticipate:
- **Annual Members:** Use bikes as a commuting tool
  - Short trips (approximately 15 minutes)
  - Weekday patterns during rush hours (7-9am, 5-7pm)
  - Consistent, utilitarian usage ("last mile" transportation between public transit and offices)

- **Casual Riders:** Use bikes for leisure and recreation
  - Longer trips (45+ minutes)
  - Weekend patterns during midday hours (11am-3pm)
  - Recreational routes (lakefront, sightseeing, exercise)

### Key Metrics for Analysis

To understand behavioral differences, we will compare:
1. **Trip Duration** - How long each customer type rides
2. **Day of Week Patterns** - When each customer type rides (weekly patterns)
3. **Time of Day Patterns** - When each customer type rides (daily patterns)

---

## PHASE 2: PREPARE

### Data Source Description

**Data Source:** Cyclistic historical bike trip data  
**Time Period:** Previous 12 months (February 2025 - January 2026)  
**Organization:** 12 separate monthly CSV files  
**Provider:** Motivate International Inc. (under public license)  

**Note:** Cyclistic is a fictional company; actual data comes from Divvy bike-share system in Chicago, used for case study purposes.

### Data Privacy Considerations

Due to data privacy regulations, the dataset does not include:
- Personally identifiable information
- Individual rider tracking capabilities
- Credit card information
- Residential location data

**Impact on Analysis:** We will work with aggregated trip-level data grouped by customer type. This does not prevent us from answering the business question, as we are analyzing group patterns rather than individual behavior.

### Data Credibility Assessment (ROCCC)

**Reliable:** ✅
- Data captured by automated bike tracking system
- Exact timestamps for lock/unlock events
- No human memory or estimation errors
- Every trip systematically recorded

**Original:** ✅
- First-party data directly from bike-share operating system
- Not filtered or interpreted by third parties
- Provides analytical flexibility and verification capabilities
- Allows for custom metric calculations

**Comprehensive:** ✅
- Contains all necessary fields for behavioral comparison
- Includes temporal data (when)
- Includes duration data (how long)
- Includes customer type classification (who)

**Current:** ✅
- Last 12 months captures latest customer behaviors
- Reflects post-pandemic commuting patterns
- Accounts for recent infrastructure changes
- Relevant for 2026 marketing strategy design

**Cited:** ✅
- Source clearly identified (Divvy/Motivate International Inc.)
- Proper licensing documented
- Transparent data provenance

### Expected Data Structure

**Raw Data Fields (per trip record):**
- `ride_id` - Unique identifier for each trip
- `rideable_type` - Type of bike used
- `started_at` - Trip start timestamp
- `ended_at` - Trip end timestamp
- `start_station_name` - Origin station
- `start_station_id` - Origin station ID
- `end_station_name` - Destination station
- `end_station_id` - Destination station ID
- `member_casual` - Customer type (member or casual)

**Derived Fields (to be calculated):**
- `ride_length` - Trip duration (ended_at - started_at), formatted as HH:MM:SS
- `day_of_week` - Day extracted from started_at (1=Sunday, 7=Saturday)
- `time_of_day` - Hour extracted from started_at (for rush hour analysis)

### Data Organization Strategy

**File Structure:**
- 12 monthly CSV files (one per month)
- Compressed in ZIP format for storage efficiency
- Each file contains all trips for that month

**Rationale for Monthly Organization:**
- Manageable file sizes for spreadsheet software
- Enables seasonal pattern analysis (summer vs. winter)
- Facilitates month-specific executive inquiries
- Allows incremental processing and verification

**Analysis Workflow:**
1. Process and clean each monthly file individually
2. Create calculated fields in each monthly file
3. Verify data quality on smaller datasets
4. Merge files for full-year analysis

### Data Integrity & Cleaning Strategy

**Pre-Processing Verification:**
1. ✅ Confirm column consistency across all 12 monthly files
2. ✅ Verify column names and data types match
3. ✅ Check for structural issues before merging

**Data Quality Checks & Cleaning Steps:**

**1. Invalid Time Data**
- **Issue:** Rows where ended_at < started_at (impossible trips)
- **Impact:** Creates negative ride_length values
- **Action:** Remove rows (data integrity error)
- **Justification:** Cannot determine correct values; large dataset means minimal impact

**2. Text Standardization**
- **Issue:** Inconsistent capitalization in member_casual field (member, Member, MEMBER)
- **Impact:** Creates artificial categories in analysis
- **Action:** Standardize to lowercase using Excel text functions
- **Justification:** Ensures accurate grouping and counting

**3. Missing Critical Data**
- **Issue:** Blank/null values in member_casual field
- **Impact:** Cannot classify trip for comparative analysis
- **Action:** Remove rows
- **Justification:** Customer type is essential for analysis; guessing introduces contamination

**4. Missing Non-Critical Data**
- **Issue:** Blank/null values in station name fields
- **Impact:** None for core analysis (duration, timing patterns)
- **Action:** Retain rows
- **Justification:** Does not affect metrics needed to answer business question

### Calculated Field Creation

**Process:** Create derived fields in each monthly file before merging

**Field 1: ride_length**
- **Formula:** =ended_at - started_at
- **Format:** HH:MM:SS (Excel Time format)
- **Purpose:** Enables duration comparison between customer types

**Field 2: day_of_week**
- **Formula:** =WEEKDAY(started_at, 1)
- **Format:** Number (1-7, where 1=Sunday, 7=Saturday)
- **Purpose:** Enables weekly pattern analysis (weekday vs. weekend usage)

**Field 3: time_of_day** (optional enhancement)
- **Formula:** =HOUR(started_at)
- **Format:** Number (0-23)
- **Purpose:** Enables rush hour vs. off-peak analysis

**Rationale for Monthly Processing:**
- Easier error detection on smaller datasets
- Maintains monthly granularity for seasonal insights
- Prepares data for executive questions about specific time periods
- Simplifies troubleshooting of calculation errors

### Documentation Standards

All cleaning and manipulation steps will be documented including:
- Number of rows affected by each cleaning action
- Rationale for decisions (remove vs. keep)
- Formulas used for calculated fields
- Percentage of data removed (to demonstrate minimal impact)

This documentation ensures:
- Transparency in methodology
- Reproducibility of analysis
- Credibility with stakeholders
- Ability to defend analytical choices
