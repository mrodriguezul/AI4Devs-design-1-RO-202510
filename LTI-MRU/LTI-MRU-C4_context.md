# LTI Software: C4 Diagram - Context (Level 1 - PlantUML)

```plantuml
@startuml C4_Context
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

LAYOUT_WITH_LEGEND()

Person(Recruiter, "Recruiter/HR", "Manages jobs, candidates, scorecards, and makes hiring decisions.")
Person(Candidate, "External/Internal Candidate", "Applies for jobs, receives automated communications, provides interview feedback.")

System_Boundary(ATS, "LTI Software ATS System") {
    System(LTI, "LTI Software", "The private Applicant Tracking System for structured hiring.")
}

System(JobBoards, "External Job Boards", "e.g., LinkedIn, Indeed, Glassdoor. Source of external applicants.")
System(CalendarAPI, "Calendar API", "e.g., Google/Outlook. Used for interview scheduling and availability sync.")
System(HRIS, "HRIS/Payroll System", "Future integration to onboard the Hired Candidate.")
System(AssessmentTools, "Assessment/BGC Vendors", "Future integration for skills tests and background checks.")

Rel(Recruiter, LTI, "Uses (Manages Workflow)", "Web UI (HTTPS)")
Rel(Candidate, LTI, "Submits Application via", "Web Portal (HTTPS)")

Rel(LTI, JobBoards, "Pushes Job Postings & Monitors Applications", "API/Webhooks")
Rel(LTI, CalendarAPI, "Syncs Availability & Sends Invites", "REST API")
Rel(LTI, HRIS, "Transfers Finalized Hire Record", "Future API Integration (P4)")
Rel(LTI, AssessmentTools, "Triggers Tests & Receives Results", "Future API Integration (P4)")

@enduml
```
