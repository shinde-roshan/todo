# SDLC, Development & Deployment — Team Process Overview

## 1. Purpose

The purpose of this project is not only to build a working Python/Django application, but to gain practical, end-to-end knowledge of the software development lifecycle (SDLC).

We will use this project as a hands-on learning environment where the team experiences the complete journey:

```
Requirement
↓
Definition
↓
Refinement
↓
Estimation
↓
Development
↓
Code Review
↓
Continuous Integration
↓
Merge
↓
Staging Deployment
↓
QA / Verification
↓
Release
↓
Production Deployment
↓
Monitoring / Troubleshooting
```
The emphasis is on understanding:
* What each step is
* Why the step exists
* Which tool is used
* What inputs and outputs each step has
* Who is responsible
* What happens when something fails
* How one step connects to the next

We will prefer free or low-cost services wherever practical so that the team can gain hands-on experience without significant infrastructure cost.

---

## 2. Learning Objectives

By completing this project, every team member should gain practical familiarity with:

### Software Development
* Python
* Django
* REST APIs where applicable
* PostgreSQL
* Automated testing
* Application configuration
* Database migrations

### Git & GitHub
* Git fundamentals
* Branching
* Commits
* Pull Requests
* Code reviews
* Merge conflicts
* Branch protection
* Issues
* GitHub Projects
* Tags and releases

### CI/CD
* Continuous Integration
* Automated tests
* Linting
* Code-quality checks
* Build pipelines
* Deployment pipelines
* Environment-specific configuration
* Deployment approvals

### DevOps / Infrastructure
* Linux
* Docker
* Docker Compose
* AWS EC2
* Nginx
* Gunicorn
* PostgreSQL
* HTTPS
* DNS
* Server configuration
* Logs
* Deployment troubleshooting
* Rollback

### Software Process
* Requirement definition
* Technical refinement
* Estimation
* Code review
* QA
* Release management
* Production deployment
* Incident/defect handling

---

## 3. Guiding Principles

The following principles will guide the project.

### 3.1 Learn by doing
We will deliberately perform the complete process instead of simply reading about it. For example, rather than only learning what a merge conflict is, we will intentionally create and resolve one.

### 3.2 Keep the initial system simple
We will not introduce every available technology at the beginning. We will start with the minimum required tools and progressively introduce additional technologies.

**Initial stack:**
* Git
* GitHub
* Python
* Django
* PostgreSQL
* GitHub Actions
* Docker
* AWS EC2
* Nginx
* Gunicorn

> Technologies such as Redis, Celery, RDS, Terraform, Kubernetes, advanced monitoring, etc. may be introduced later as separate learning exercises.

### 3.3 Understand why a tool is being used
For every technology or process, the team should be able to answer:
* What is it?
* What problem does it solve?
* Why are we using it?
* What happens without it?
* How does it fit into our architecture?
* What happens when it fails?

### 3.4 Production should never be treated as a development environment
Development, staging, and production will be treated as separate environments.
```
Development
↓
Staging
↓
Production
```
Each environment should have its own configuration and secrets.

---

## 4. Tool Stack

| Area | Tool | Purpose |
| :--- | :--- | :--- |
| Source control | Git | Local version control |
| Remote repository | GitHub | Store and collaborate on source code |
| Work management | GitHub Projects | Feature/bug tracking and workflow |
| Requirements | GitHub Issues | Feature and bug tickets |
| Documentation | Markdown | Requirements, technical documentation, runbooks |
| Application | Python + Django | Web application |
| Database | PostgreSQL | Relational database |
| Testing | Django Test Framework / pytest | Automated testing |
| Code quality | Ruff | Linting and formatting |
| CI | GitHub Actions | Automated validation |
| Containerization | Docker | Consistent application environment |
| Container orchestration | Docker Compose | Run multiple local/staging services |
| Container registry | GitHub Container Registry | Store Docker images |
| Server | AWS EC2 | Remote Linux server |
| Reverse proxy | Nginx | HTTP/HTTPS handling and reverse proxy |
| Application server | Gunicorn | Run Django in production |
| Process management | Docker Compose / system services | Keep services running |
| SSL | Let's Encrypt / Certbot | HTTPS |
| Secrets | GitHub Secrets / environment configuration | Secure credentials |
| Logs | Docker/Linux/GitHub Actions logs | Troubleshooting |

*The exact tools may evolve as the team learns more.*

---

## 5. High-Level Architecture

The final learning architecture will approximately look like this:
```
                    TEAM
                      │
                      ▼
               GitHub Project
                      │
               Feature Ticket
                      │
                      ▼
               GitHub Repository
                      │
                      ▼
               Feature Branch
                      │
                      ▼
                 Pull Request
                      │
             ┌────────┴─────────┐
             │                  │
         Code Review         CI Checks
             │                  │
             └────────┬─────────┘
                      │
                    Merge
                      │
                      ▼
                   develop
                      │
                      ▼
               CI/CD Pipeline
                      │
                      ▼
                   STAGING
                      │
                      ▼
                     QA
                      │
                      ▼
                   TESTED
                      │
                      ▼
                   RELEASE
                      │
                   PRODUCTION
                      │
                      ▼
                     AWS
                      │
             ┌────────┴──────────┐
             │                   │
            EC2              PostgreSQL
             │
           Nginx
             │
          Gunicorn
             │
           Django
```
---

## 6. Environments

We will maintain three logical environments.

### 6.1 Development
Used by developers on their local machines.
```
Developer Machine
│
├── Python / Django
├── PostgreSQL
└── Docker (where applicable)
```
**Typical characteristics:**
* `DEBUG=True`
* Local database
* Development configuration
* Development secrets

### 6.2 Staging
Used for integration and QA testing.
```
AWS EC2
│
├── Nginx
└── Docker
│
├── Django/Gunicorn
└── PostgreSQL
```
*Staging should be as similar to production as reasonably possible.*

### 6.3 Production
Production contains the version available to actual users.
```
Internet
│
▼
Nginx
│
▼
Django / Gunicorn
│
▼
PostgreSQL
```
**Production should have:**
* `DEBUG=False`
* Production secrets
* Production database
* HTTPS
* Production configuration

---

## 7. Git Branching Strategy

We will initially use two long-lived branches:
* `main`
* `develop`

And short-lived feature branches:
* `feature/*`
* `bugfix/*`

### 7.1 main
`main` represents production-ready code.
> `main` = Production

*Direct pushes to `main` should be disabled.*

### 7.2 develop
`develop` represents the latest integrated development code.
> `develop` = Staging

*Feature Pull Requests will normally target `develop`.*

### 7.3 Feature branches
**Examples:**
* `feature/123-user-registration`
* `feature/145-password-reset`
* `feature/160-payment-api`

**Bug fixes:**
* `bugfix/172-invalid-email-validation`
* `bugfix/181-login-error`

Branches should be created from the latest `develop`.

**Example:**
```bash
git checkout develop
git pull origin develop

git checkout -b feature/123-user-registration
```

---

## 8. Feature Lifecycle

Every feature will follow the same general lifecycle:
```
DEFINITION
    ↓
REFINEMENT
    ↓
OPEN
    ↓
IN PROGRESS
    ↓
IN REVIEW
    ↓
PREPARED
    ↓
IN VERIFICATION
    ↓
TESTED
    ↓
RELEASED
```
Deployment is tracked separately:
```
Not Deployed
    ↓
Staging
    ↓
Staging Verified
    ↓
Production
```
Separating feature status from deployment status avoids confusion when multiple features are being prepared for a release.

---

## 9. Step 1 — Definition

The feature begins as a requirement. A feature ticket is created in GitHub Projects.

Initial state: `DEFINITION`

The ticket should clearly explain WHAT needs to be built and WHY. It should not initially dictate implementation details.

### 9.1 Feature Ticket Structure

A feature ticket should contain:

- Title
- Business Context
- Problem Statement
- Objective
- WHAT
- User Story / Requirement
- Acceptance Criteria
- Specification Documents
- Dependencies
- Out of Scope
- HOW
- Estimation
- Implementation Notes
- Testing
- Deployment Considerations

The HOW section will be completed during refinement

---

## 10. Step 2 — Refinement

Once the requirement is sufficiently understood, a developer starts refinement.
Ticket moves: `DEFINITION` $\rightarrow$ `REFINEMENT`
The purpose of refinement is to determine HOW the requirement will be implemented.

The refinement should consider:
- Application architecture
- Django components
- Database changes
- API changes
- UI changes where applicable
- Validation
- Authentication/authorization
- Security
- Error handling
- Testing strategy
- Deployment requirements
- Dependencies
- Potential risks
- Rollback considerations

---

## 11. The WHAT and HOW Concept

The team should clearly distinguish between WHAT and HOW.

- WHAT:
  
  Describes the required behavior.

  > Example:
  >
  > Users should be able to register using an email address and password.

- HOW:

  Describes the proposed implementation.

  > Example:
  >
  > Create registration endpoint:
  >
  > `POST /api/users/register/`
  >
  > Add validation for:
  > - email uniqueness
  > - password complexity
  >
  > Use existing User model.
  >
  > Add automated tests for:
  > - successful registration
  > - duplicate email
  > - invalid password
  
  The HOW section is reviewed by the team.

---

## 12. Refinement Review Meeting

Once the refinement is complete, the team conducts a review meeting.

The team reviews:

* Requirement
* Proposed implementation
* Architecture
* Database changes
* API changes
* Testing
* Risks
* Dependencies
* Deployment impact

The team may:

* Approve the approach
* Request changes
* Ask for more investigation
* Split the feature
* Reject the proposed implementation
* Identify additional tasks

---

## 13. Estimation

After the implementation approach is understood, the team estimates the feature.

A simple story-point system may be used: `1, 2, 3, 5, 8, 13`

The purpose of estimation is not to produce a mathematically perfect number.

The purpose is to understand:

* Complexity
* Unknowns
* Development effort
* Testing effort
* Integration risk

---

## 14. Step 3 — Open

Once the team approves the refinement and estimation:

`REFINEMENT` $\rightarrow$ `OPEN`

The feature is now ready to be picked by a developer.

The ticket should contain enough information for another developer to understand what needs to be done.

---

## 15. Step 4 — In Progress

A developer takes ownership of the ticket.

`OPEN` $\rightarrow$ `IN PROGRESS`

The developer creates a feature branch from develop.

```bash
git checkout develop
git pull origin develop

git checkout -b feature/123-user-registration
```

Development is performed locally.

The developer should:

* Follow the approved HOW
* Write clean code
* Add automated tests
* Update documentation where required
* Create database migrations where required
* Keep commits meaningful
* Avoid committing secrets

---

## 16. Commit Guidelines

Commits should describe meaningful changes.

Good examples:

- Add user registration endpoint
- Add registration validation
- Add registration tests
- Fix duplicate email validation
- Update registration documentation

Avoid commits such as:

- update
- changes
- final
- final2
- test
- stuff

The objective is to maintain useful project history.

---

## 17. Database Migrations

If a feature changes the database schema, Django migrations must be created.

> Example:
>
> `python manage.py makemigrations`

Migration files must be committed to Git.

> Example:
>
> ```bash
> apps/users/migrations/
>    0001_initial.py
>    0002_add_phone_number.py
> ```

Migrations are part of the application version and must travel with the code.

---

## 18. Step 5 — Pull Request

When development is complete:

`IN PROGRESS` $\rightarrow$ `IN REVIEW`

The developer pushes the branch:

```bash
git push origin feature/123-user-registration
```

A Pull Request is created:

```
feature/123-user-registration
             ↓
          develop
```

The PR should reference the feature ticket.

---

### 19. Pull Request Content

A PR should clearly describe:

- What was implemented?
- Why was it implemented?
- What changed?
- How was it tested?
- Are there database changes?
- Are there deployment considerations?
- Are there screenshots/logs where applicable?

A PR checklist should include items such as:

- [ ] Requirement implemented
- [ ] Automated tests added/updated
- [ ] Tests passing
- [ ] Migration reviewed
- [ ] Documentation updated
- [ ] No secrets committed
- [ ] Code follows project standards

---

## 20. Continuous Integration

Every Pull Request should trigger CI.

The initial CI pipeline should perform:

```
Pull Request
     │
     ▼
GitHub Actions
     │
     ├── Checkout
     ├── Setup Python
     ├── Install dependencies
     ├── Run linting
     ├── Run automated tests
     ├── Validate Django configuration
     └── Additional checks
```

The result should be visible directly in the Pull Request.

---

## 21. CI Must Pass Before Merge

The develop branch will be protected.

A developer should not be able to merge a Pull Request when required CI checks fail.

The expected process is:

```
PR
 ↓
CI
 ↓
PASS ──────────────┐
                   │
FAIL               │
 ↓                 │
Developer fixes    │
 ↓                 │
CI runs again ─────┘
```

Only after the required checks pass should the PR proceed toward merge.
