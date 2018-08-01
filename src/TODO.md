# Phase 1:
## Micro-Services based project
### Technical Goals
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
### Services
#### Stackoverflow scraper 
The **scraper** service it a batch service which listen to REDIS Queue (of user names) and grab information on a user (question, answers, ranks, etc).
Send the Data to the **Persistance** Service
#### Feeder Service (Web API Front-End) 
Get user names and send it to the REDIS Queue.
#### Persistance Service (Back-End http service)
Will be responsible to save the scaped data into Neo4J.
#### Query Service (Web API Front-End)
Return aggregate user information.

# Phase 2:
## Technical Goals
- Languages
    - Node.js
- Storage and Queues
    - Kafka / NATS / RabbitMQ
- Deployment
    - Serverless: nne or more of the services should be deploy on Lambda / Azure Function   
### Services
Add Node.js implementation for some of the existing services

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