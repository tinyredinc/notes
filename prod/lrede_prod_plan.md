# LREDE™ by CoStone Group - Business Plan

The LREDE™ series, an internally incubated project of CoStone Group, features a suite of applications specifically designed to advance the real estate industry through sophisticated data analytics and AI technology.

## LREDE™ Team

- Samuel Zhang, B.Eng, Captain
    - Currently the CIO of CoStone Group, and formerly the VP of Technology at EQ Consulting, CEO at SideAI, and CTO at Keller Williams TeamLeads. Over a decade of leadership experience in the software and technology industry, with extensive knowledge in the full life cycle of software development, data and AI technology and team cultivation.

- Leo Liu, B.Eng, System Architect
    - Former System Architect at Keller Williams TeamLeads and VP of Technology at Kaitong (NEEQ:20000426). Over 25 years of experience in the enterprise software industry, specialized in the design of system architecture for robust, scalable data and application infrastructure.

- Future Xu, UI/UX Engineer
    - Former UI/UX Engineer and Dev Team Manager at SideAI. With 8 years of experience in the software industry, specialized in UI/UX design and general management of dev team.

- Mike Huang, Back-End Engineer
    - Former Back-End Engineer at Keller Williams TeamLeads. With 10 years of experience in the technology industry, specialized in the back-end development of mobile and web applications. 

- Bao Ye, Front-End Engineer
    - Former Front-End Engineer at SideAI. With 5 years of experience in the technology industry, specialized in the front-end development of mobile and web applications. 

- Yanbo Xue, PhD, Senior Consultant(AI)
    - IEEE Fellow, formerly the Chief Scientific Officer at Boss Zhipin (NASDAQ:BZ) and Senior Deep Learning Researcher at D-Wave Systems. One of the very few deep learning experts with hands-on experience in various types of neural network design and modeling.

- Eric Yu, M.Eng, Senior Consultant(Real Estate)
    - The CEO of CoStone Group and former President of Landeal Developments. Market-proven real estate developer and investor with over 20 years of experience in land acquisition, development, and construction.

- Sarah Yang, Senior Consultant(Realtor)
    - Licensed Real Estate Agent and Founding Partner of BOXIN REALTY (Brokerage). Over a decade of experience as a realtor and broker, with extensive knowledge in the full life cycle of property transactions.
 
## LREDE™ Applications

LREDE™ applications, built on the LREDE™ infrastructure, offer a diverse set of features to meet the needs of different groups of people.

### Future Analytics™

- Description

    - Future Analytics provides comprehensive data for analyzing the real estate market, enabling you to study past patterns, understand current dynamics, and predict future trends. 

- Highlight

    - Data Analytics: The data from various vendors, presented in a visually intuitive format, include not only real estate market data (such as transactions and supply and demand metrics), but also indirect factors that influence the market (such as mortgage and exchange rates, construction material and cost indices, and economic indicators). 

    - AI Trend Prediction: Powered by the LREDE™ Neural Network Engine, this feature utilizes RNN models to predict future trends in real estate market metrics. The design benchmarks, established using 10 years of historical data, are detailed as follows*:
      | Forecast Period | Accuracy |
      | --- | --- |
      | 1-Month | 90% + |
      | 2-Month | 75% + |
      | 3-Month | 60% + |
    
    - AI Appraisal: Powered by the LREDE™ Neural Network Engine, this feature utilizes MLP models to estimate property value and generate appraisal reports. The design benchmarks, established using the most recent year's transaction data, are detailed as follows*:
      | Properties | Accuracy |
      | --- | --- |
      | 30% | ± 3% |
      | 50% | ± 5% |
      | 90% | ± 10% |

- Target Customer

    - Professionals and agencies involved in real estate investment, such as developers, land bankers, realtors, market researchers, and high-net-worth individuals.

- Business Model

    - Subscription Fee:  Each user will purchase their account for a fixed monthly fee. 
    - Enterprise Customization: Customized reports and neural network models based on specific requirements.

- User Scenarios
    
    The real estate retail market can experience considerable fluctuations, with the average or median price of a single house varying by up to 15% or $200,000 in just one month. Consequently, optimal timing can lead to substantial wealth gains or savings.

    - Developer: Select the best timeframe for pre-sale, land acquisition, and liquidation activities.
    
    - Agency: Offer professional advice to clients, backed by solid data and AI analysis.

    - Individual: Determine the most favorable period to buy or sell properties.

### GemHunt Realty™

- Description

    - GemHunt Realty is the next-generation property search site, powered by AI, enables consumers to find properties listed below their actual value, a feature we call "Gem Hunt".

- Highlight

    - Gem Rating: This feature leverages MLP models to appraise the value of all active listings on the market. It compares the listing price with the AI-appraised property value, identifying the discrepancy between the two, which is termed the Gem Rating. Consumers can search and filter properties based on the Gem Rating, enabling them to find the best deals in their designated area.

    - Tender Marker: This feature identifies properties listed for sale by tender, meaning they are listed without a set price or at a price significantly below market value, thereby inviting buyers to submit their bids (tenders). Tender Marker tags properties declared for sale by tender and also flags listing brokers who have frequently employed this strategy in the past, as determined by algorithms. These listings can be filtered using the Gem Hunt Search.

    - AI Appraisal: This feature leverages MLP models to estimate property values and generate appraisal reports. Home buyers can download appraisal reports for existing listings or sellers can have their homes appraised by providing key property attributes such as address, property type, bedrooms, bathrooms, lot size, etc.
    
- Target Customer

    - Consumer User: Home Buyer and Sellers
    - Business User: Real Estate Agency
    
- Business Model

    - Consumer User: Free to use for registered user
    - Business User:
        - Lead Generation, advertising space bidding
        - Lead Referral, pre-qualified lead refering


- User Scenarios

    - Consumer User: Users can search for properties within their budget and designated area as usual. The Gem Rating feature simplifies the process of finding the best deals on the market. On the property detail page, realtors are listed in order of their bidding price. Alternatively, consumers can contact GemHunt Realty directly for generic, free consulting. We will respect the consumer's preference regarding whether to refer them to a verified, affiliated real estate professional for further services.

    - Business User: Users have the ability to bid for advertising space targeted at specific municipalities and property types (such as Richmond Hill Townhouse/Detached or Toronto C01 Condo). Their advertisements will appear on the relevant property detail pages. Additionally, users can apply to become affiliated professionals. Once approved, they are eligible to receive pre-qualified referrals from GemHunt Realty.


## System Architecture

![REIF STRUCTURE](img/lrede_arch.png)

## System Infrastructure

The LREDE™ infrastructure comprises six major engines/components, each serving a different functionality:

- Universal ETL Engine
    - Universal ETL acts as a gateway to various data vendors, synchronizing data in real-time. This includes, but is not limited to, demographic and economic data from Statistics Canada, real estate market data from various Regional Real Estate Boards, and financial data from Bloomberg and Reuters.

- Data Warehouse
    - Considering its specific nature and use cases, this read-intensive data warehouse employs a traditional relational database in a master-slave architecture, complemented by a memory database for caching. The master-slave architecture enhances availability and read capabilities, while the memory database increases throughput and speeds up access to frequently accessed ('hot') data.

- Analytics Engine
    - Analytics Engine extracts raw data from the data warehouse and generates various kinds of intuitive analysis reports using traditional statistical approaches. Designed with extensibility in mind, new reports can be implemented relatively quickly and efficiently.

- Neural Network Engine
    - The Neural Network Engine leverages deep learning models trained with an extensive amount of historical data from the data warehouse and facilitates advanced data analysis using cutting-edge AI approaches for functionalities like future prediction and value assessment. The forecasting models, designed for trend prediction, are built on hybrid-tuned Multivariate Recurrent Neural Networks. The appraisal models, designed for value assessment, are built on hybrid-tuned Multilayer Perceptrons.

- Data Maintenance Engine
    - The Maintenance Engine archives deprecated data to improve efficiency, resyncs corrupted data through ETL jobs, constructs memory caches for both frequently accessed ('hot') data and complex report ainquiries, and performs data backups to both local and remote locations for disaster recovery.

- Application API Engine
    - The Application API Engine, a RESTful API framework, is designed to allow applications to access LREDE™'s internal components, including the Data Warehouse, Analytics, and Neural Network Engines. This facilitates the development of a wide range of applications, each with its own unique and complex business logic, leveraging a robust infrastructure of data, analytics, and AI.

## SWOT Analysis

