# Phase 1: (Micro-Services based project)
## Technical Goals
- Languages
    - Go
    - .Net Core
- Storage and Queues
    - Redis
    - Neo4J
- Tooling
    - Helm (Package Manager)
- Deployment
    - Docker (services should be docker based)
    - Orchestration
        - K8s 
            - AWS
        - Service Fabric / [Mesh](https://docs.microsoft.com/en-us/azure/service-fabric-mesh/)
            - Azure
## Services
### Stackoverflow scraper 
The **scraper** service it a batch service which listen to REDIS Queue (of user names) and grab information on a user (question, answers, ranks, etc).
Send the Data to the **Persistance** Service
### Feeder Service (Web API Front-End) 
Get user names and send it to the REDIS Queue.
### Persistance Service (Back-End http service)
Will be responsible to save the scaped data into Neo4J.
### Query Service (Web API Front-End)
Return aggregate user information.
## Services Design
### Scraper (Golang)
The scraper will be responsible for the following functionality:
#### Listen and download
- Get starting point from queue (start with REDIS, will be replace with Kafka / NATS / RabbitMQ on second phase).
    - The queued data will be in known json format and will be mapped into go a struct.
        - scraping deep (recursion deep)
        - One of the following
            - User
            - Question
            - Answer
- Search Stack overflow for matching (User, Question, Answer) 
- check if the data already processed in the last day (REDIS key with expiration)
- Download the page data (update the REDIS key)
- Format it to well formatted xml (TBD)
- Parse the data and form a info struct [User, Questions, Answers, Follower, Following, Answered To]
- Save it Neo4J (Data + relationships within the data and to existing data) 
- resend recursive call into the queue (deep - 1, as long as it > 0)
##### Technical note
The scraper will use go routine + cyclic channel in order to form the following steps

# Phase 2:
## Technical Goals
- Languages
    - Node.js
- Storage and Queues
    - Kafka / NATS / RabbitMQ
- Deployment
    - Serverless: nne or more of the services should be deploy on Lambda / Azure Function   
### Services
- Each stage will be operate as separate micro-service
- TBD: the communication within the internal services (REDIS, Other Queue, Direct Http or gRpc)
# Phase 3
## Technical Goals
- Deployment
    - Micro-Services platform ([Istio](https://istio.io/)) 
- Monitoring
    - [Prometheus](https://prometheus.io/)
- Tracing
    - [Jaeger](https://github.com/jaegertracing/jaeger)
- Logging
    - ELK
- Storage and Queues
    - ELK
