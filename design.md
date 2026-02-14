# Design Document
## AI-Powered Demand-First Market Intelligence Platform

### System Architecture Overview

The platform follows a linear pipeline that transforms citizen feedback into actionable business intelligence:

**Public Citizen Feedback → Demand Understanding → Market Intelligence → Decision Reasoning → Dashboard**

---

## MVP Architecture

### Design Philosophy
The MVP uses a minimal set of AWS services to validate the core concept:
- **Single Database**: PostgreSQL on RDS (with PostGIS) for all data storage
- **Single AI Service**: Amazon Bedrock for all NLP and AI tasks
- **Serverless Compute**: Lambda for all backend processing
- **No Caching**: Direct database queries (caching added in production)
- **No ETL Tools**: Manual data loading (AWS Glue added later)

### Stage 1: Public Citizen Feedback

**Purpose**: Collect raw demand signals from citizens

**Input Channels:**
- Web scraping (Google Maps reviews, social media, local forums)
- Open data sources (census, municipal data, POI datasets)
- Location dropdown (predefined areas)

**Data Collected:**
- User location (area/neighborhood)
- Service/shop type (dropdown categories)
- Comments and reviews (scraped or submitted)
- Timestamps

**Implementation:**
- Python scraping scripts (BeautifulSoup/Scrapy)
- REST API (API Gateway + Lambda)
- PostgreSQL on RDS (demands table)
- S3 for storing raw scraped data

---

### Stage 2: Demand Understanding

**Purpose**: Extract insights from citizen feedback

**Approach:**
- Use Amazon Bedrock for sentiment analysis on scraped reviews/comments
- Bedrock for keyword extraction and service categorization
- Bedrock for pain-point detection (e.g., "no pharmacy", "need grocery store")
- Latent demand inference from complaints and feedback using Bedrock
- Manual category tagging as fallback

**Implementation:**
- Lambda function calls Bedrock API for all NLP tasks:
  - Sentiment analysis
  - Entity extraction
  - Pain-point detection
  - Demand categorization
- Store results in PostgreSQL
- Batch processing for scraped data
- Text processing pipeline (clean → analyze → categorize)

---

### Stage 3: Market Intelligence

**Purpose**: Identify demand-supply gaps

**Approach:**
- Integrate open data (pre-loaded POI dataset)
- Distance calculations for proximity analysis using PostGIS
- Clustering: group demands by area using SQL queries
- Competition data analysis
- Demand-supply gap detection at neighbourhood level
- Footfall estimation using spatial patterns, demographic data, and POI density

**Implementation:**
- PostgreSQL with PostGIS for location queries and spatial analysis
- SQL aggregations (COUNT, GROUP BY)
- Demographic data integration
- Spatial pattern analysis for footfall estimation

---

### Stage 4: Decision Reasoning

**Purpose**: Generate recommendations

**Approach:**
- Use Amazon Bedrock to generate WHY/WHERE/HOW explanations
- Hybrid scoring: heuristic baseline combined with AI-driven demand inference and LLM reasoning and clustering
- AI-driven location suitability scoring and opportunity ranking
- Template-based reports with AI-generated insights

**Implementation:**
- Lambda function with Bedrock prompt engineering
- Business logic for viability assessment
- Location suitability scoring algorithm
- Opportunity ranking based on multiple factors (demand, competition, footfall, demographics)
- JSON response with recommendations

---

### Stage 5: Dashboard

**Purpose**: Display insights

**Features:**

**Citizen View:**
- View top demands list

**Entrepreneur View:**
- Heatmap (color-coded areas by demand)
- Top opportunities list (ranked by suitability)
- Report card (WHY/WHERE/HOW)
- Demand-supply gap visualization
- Footfall estimates for locations

---

## Tech Stack

### Core Services
- **Frontend**: React + AWS Amplify
- **API**: API Gateway + Lambda (Python)
- **Database**: PostgreSQL on RDS (with PostGIS extension)
- **AI/NLP**: Amazon Bedrock (for all NLP tasks)
- **Maps**: Amazon Location Service
- **Auth**: Amazon Cognito
- **Storage**: Amazon S3 (raw scraped data)

---

## Data Model

### PostgreSQL Tables

**scraped_data**
- id, source (google_maps/social_media), content, location, sentiment, created_at

**demands**
- id, user_id, location, category, description, source (scraped/user_submitted), created_at

**users**
- id, email, role (citizen/entrepreneur), created_at

**opportunities**
- id, location, category, score, why, where, how, supporting_data, created_at

---

## Implementation Workflow

### Phase 1: Foundation & Data Collection
- Set up AWS infrastructure
- Create PostgreSQL database schema
- Build web scraping scripts (Google Maps, social media)
- Scrape and store raw data in S3
- Build REST API (submit demand, list demands)
- Develop React frontend (forms and lists)

### Phase 2: AI Integration & Analysis
- Process scraped data with Bedrock (sentiment analysis)
- Extract pain points and infer demands
- Integrate open data sources
- Implement scoring algorithm
- Generate WHY/WHERE/HOW with Bedrock prompts
- Add Amazon Location Service map integration

### Phase 3: Testing & Refinement
- Populate database with analyzed data
- Add UI styling and polish
- Test end-to-end user flows
- Optimize performance
- Validate AI-generated insights

---

## Demo Flow

### Data Collection & Analysis
1. **Web Scraping**: Scrape reviews and comments from Google Maps, social media, local forums
2. **Open Data Integration**: Load census data, POI datasets, demographic information
3. **Sentiment Analysis**: Use Bedrock to analyze scraped content and extract pain points
4. **Demand Inference**: Identify latent demand patterns (e.g., "no pharmacy nearby" complaints → pharmacy demand)
5. **Database Population**: Populate demands table with inferred needs

### User Experience Flow
1. **Pre-Analyzed Data**: Dashboard displays demands identified from scraped data
2. **Citizen Interaction**: Submit new demand "Need a pharmacy in Koramangala"
3. **Entrepreneur Interaction**: View heatmap → See "Pharmacy in Koramangala" as top opportunity → Read AI-generated report
4. **AI Intelligence Display**: 
   - Show original scraped reviews/comments
   - Display Bedrock sentiment analysis results
   - Present WHY/WHERE/HOW recommendations based on combined data

---
## Key Design Considerations

1. **Data Processing**: Batch process demands periodically for efficiency
2. **Geographic Scope**: Start with focused geographic areas for validation
3. **Data Sources**: Combine scraped data with open datasets
4. **Scoring Approach**: Formula-based scoring with AI-enhanced explanations
5. **User Experience**: Prioritize functionality and clear insights
6. **Scalability**: Single-region deployment with room for expansion

---

## Future Enhancements & Production Roadmap(Concept)

### Phase 1: Enhanced AI/ML Capabilities
- **Custom ML Models**: Train custom models for:
  - More accurate demand prediction
  - Advanced clustering algorithms for neighborhood segmentation
  - Viability prediction models based on historical business success data

### Phase 2: Data Pipeline & Integration
- **AWS Glue ETL Pipelines**: Automated data integration from multiple open data sources
- **Real-time Data Processing**: Replace batch processing with streaming architecture
- **Live Open Data APIs**: Direct integration with:
  - Census APIs for demographic data
  - Municipal data feeds
  - Business registry APIs
  - Traffic and mobility data
- **Data Lake Architecture**: Comprehensive data lake on S3 with Athena for analytics
- **Conversational AI Assistant**: Interactive Q&A system powered by RAG to help users explore location insights, learn about historical data and demand patterns through natural language queries

### Phase 3: Advanced Analytics & Intelligence
- **Amazon QuickSight Dashboards**: Replace Chart.js with embedded QuickSight for:
  - Interactive analytics
  - Custom reports and visualizations
  - Business intelligence for entrepreneurs
- **Predictive Analytics**: Forecast future demand trends using SageMaker
- **Success Tracking**: Monitor businesses launched via platform and track survival rates
- **Competitive Intelligence**: Automated competitor analysis and market dynamics
- **AI-Powered Guidance**: Intelligent assistant to guide entrepreneurs through location selection, viability assessment, and business planning based on platform insights

### Phase 4: Performance & Scalability
- **Caching Layer**: Amazon ElastiCache (Redis) for:
  - Frequently accessed reports
  - API response caching
  - Session management
- **Asynchronous Processing**: Amazon SQS for heavy ML workloads
- **Read Replicas**: PostgreSQL read replicas for query performance
- **Auto-scaling**: Dynamic scaling based on demand patterns

### Phase 5: User Experience & Accessibility
- **Voting & Community Features**:
  - Upvote/downvote system for demand requests
  - Community-driven demand prioritization
  - User reputation system
- **Mobile Application**: Native iOS and Android apps
- **Multi-language Support**: 
  - English, Hindi, and regional Indian languages
  - Localized content and recommendations
- **Progressive Web App (PWA)**: Offline capabilities

### Phase 6: Advanced Features
- **Financing Integration**: Connect entrepreneurs with funding platforms
- **Mentorship Platform**: Business planning and advisory tools
- **Supply Chain Connections**: Link retailers with suppliers
- **Franchise Matching**: Identify franchise opportunities
- **Community Engagement**: Forums and discussion boards
- **Success Stories**: Showcase businesses launched via platform

### Phase 7: Geographic Expansion
- **Multi-region Deployment**: Expand beyond single region
- **Localized Data**: Region-specific open data integration
- **International Markets**: Adapt platform for markets beyond India

### Phase 8: Advanced Business Intelligence
- **Market Trend Analysis**: Long-term trend identification
- **Seasonal Patterns**: Detect and predict seasonal demand variations
- **Economic Indicators**: Integrate macroeconomic data
- **Risk Modeling**: Advanced risk assessment for business ventures
- **ROI Calculator**: Detailed financial projections and break-even analysis
- **Comparative Analytics**: Benchmark against industry standards

---
