# LTI Software: C4 Diagram - Components (Level 3 - PlantUML)

```plantuml
@startuml C4_Containers
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

Person(Recruiter, "Recruiter/HR")
Person(Candidate, "External/Internal Candidate")

System_Boundary(ATS, "LTI Software ATS System") {
    Container(RecruiterUI, "Recruiter/Admin UI", "React Web App", "The Single Page Application (SPA) for internal users.")
    Container(CandidatePortal, "Candidate Portal", "Next.js / Static Hosting via CDN", "Public-facing portal for applying and viewing jobs.")

    Container(LoadBalancer, "Load Balancer/WAF", "AWS ELB/Nginx", "Distributes traffic and provides initial security layer.")

    Container(APIGateway, "API Gateway", "Kong / AWS API Gateway", "Single entry point. Handles rate limiting and routing.")

    Container(AuthService, "Authentication Service", "Python Microservice", "Manages user authentication (JWT issuance) and authorization.")
    
    Container(Microservices, "Microservices Cluster", "Python/Java", "All core ATS business logic.")
    
    ContainerDb(PostgresDB, "Transactional Database", "PostgreSQL", "Stores core data (Jobs, Applications, Feedback).")
    ContainerDb(BlobStorage, "File Storage", "AWS S3/Azure Blob", "Stores resumes and signed documents.")
    ContainerDb(Cache, "Cache Store", "Redis", "Caches job lists and session data.")
}

System(JobBoards, "External Job Boards")

Rel(Recruiter, RecruiterUI, "Uses")
Rel(Candidate, CandidatePortal, "Accesses")

Rel(RecruiterUI, LoadBalancer, "Requests Application Data", "HTTPS/JSON")
Rel(CandidatePortal, LoadBalancer, "Requests Job Listings/Submits App", "HTTPS/JSON")

Rel(LoadBalancer, APIGateway, "Routes Traffic")
Rel(APIGateway, AuthService, "1. Authenticates User/Token")
Rel(APIGateway, Microservices, "2. Routes Requests (Internal/External)")

Rel(Microservices, PostgresDB, "Reads/Writes Transactional Data")
Rel(Microservices, BlobStorage, "Saves/Retrieves Documents")
Rel(Microservices, Cache, "Reads/Writes Cached Data")
Rel(Microservices, JobBoards, "Pushes/Receives Webhooks", "Asynchronous")

@enduml
```
