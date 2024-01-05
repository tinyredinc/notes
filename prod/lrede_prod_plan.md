# LREDE™ by Nexus AI - Business Plan
LREDE™ comprises a series of engines and products designed to empower the real estate industry with advanced data and AI technology.

## Abstact Archtecture

![REIF STRUCTURE](img/lrede_arch.png)

## LREDE™ - Legend Real Estate Data Engine

The engine comprises six major engines/components, each serving a different functionality:

- Universal ETL Engine
    - Universal ETL acts as a gateway to various data vendors, synchronizing data in real-time. This includes, but is not limited to, demographic and economic data from Statistics Canada, real estate market data from various Regional Real Estate Boards, and financial data from Bloomberg and Reuters.

- Data Warehouse
    - Considering its specific nature and use cases, this read-intensive data warehouse employs a traditional relational database in a master-slave architecture, complemented by a memory database for caching. The master-slave architecture enhances availability and read capabilities, while the memory database increases throughput and speeds up access to frequently accessed ('hot') data.

- Analytics Engine
    - Analytics Engine extracts raw data from the data warehouse and generates various kinds of intuitive analysis reports using traditional statistical approaches. Designed with extensibility in mind, new reports can be implemented relatively quickly and efficiently.

- Neural Network Engine
    - The Neural Network Engine leverages deep learning models trained with an extensive amount of historical data from the data warehouse and facilitates advanced data analysis using cutting-edge AI approaches for functionalities like future prediction and value assessment. The forecasting models, designed for trend prediction, are built on hybrid-tuned Multivariate Recurrent Neural Networks. The appraisal models, designed for value assessment, are built on hybrid-tuned Multilayer Perceptrons.

- Data Maintenance Engine
    - The Maintenance Engine archives deprecated data to improve efficiency, resyncs corrupted data through ETL jobs, constructs memory caches for both frequently accessed ('hot') data and complex report inquiries, and performs data backups to both local and remote locations for disaster recovery.