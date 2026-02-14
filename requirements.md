# Requirements Document
## AI-Powered Demand-First Market Intelligence Platform

### Overview
An AI-powered platform that connects citizen demand with entrepreneurial opportunity. The platform analyzes scraped reviews, social media feedback, and open data to identify unmet local demand, then provides entrepreneurs with data-driven insights for business planning.

### Target Users
- **Citizens**: Express local service needs through platform
- **Entrepreneurs/MSMEs**: Access market intelligence for business decisions

### Core Features

#### Demand Collection & Analysis
- Web scraping from Google Maps, social media, local forums
- Open data integration (census, POI datasets, demographics)
- AI-powered sentiment analysis and pain-point extraction
- Latent demand inference from citizen feedback
- Location-based demand aggregation

#### Intelligence Dashboard
- Demand heatmaps by location and category
- Trend analysis from scraped data
- Competitive landscape view
- AI-generated viability scoring for business ideas
- WHY/WHERE/HOW recommendations
- Demand-supply gap detection at neighbourhood level
- AI-driven location suitability and opportunity ranking

#### Location Intelligence
- Geographic demand clustering using PostGIS
- Proximity to existing businesses
- Demographic data integration
- Catchment area analysis
- Footfall estimation using spatial patterns and demographic data

### Technical Stack

#### AWS Services (MVP)
- Amazon Bedrock (all AI/NLP tasks)
- Amazon Location Service (geospatial)
- Amazon RDS PostgreSQL with PostGIS (storage)
- AWS Lambda (serverless compute)
- Amazon Cognito (auth)
- Amazon S3 (raw data storage)
- AWS Amplify (frontend hosting)
- API Gateway (REST APIs)

#### AI/ML Components
- Sentiment analysis (Bedrock)
- Entity extraction and pain-point detection (Bedrock)
- Demand categorization (Bedrock)
- Location recommendation engine
- Hybrid viability scoring (heuristic + AI reasoning)

### Key Requirements

#### MVP Scope
- Web application (mobile-responsive)
- Batch data processing
- PostgreSQL database with PostGIS
- Single AI service (Bedrock for all NLP)
- Focus on scraped data analysis
- Single-region deployment

#### Future Enhancements
- Natural-language AI copilot for MSMEs (conversational assistant)
- Voting and community features
- Real-time data processing
- Multi-language support (English, Hindi, regional)
- Mobile native apps
- Advanced caching and performance optimization

### Non-Functional Requirements
- Data encryption and privacy compliance
- User authentication and authorization
- Scalable serverless architecture
- Clear and actionable insights

### Success Metrics

#### MVP Phase
- Platform functionality and uptime
- Quality of scraped data analysis
- Accuracy of AI-generated insights
- User engagement (citizens and entrepreneurs)

#### Production Phase
- Active users expressing demand
- Entrepreneurs using platform insights
- Businesses launched via platform
- Business survival rate vs industry average
- User satisfaction scores
