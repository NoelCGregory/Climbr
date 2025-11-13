# Refferral Finder Web Application – Requirements Document

## 1. Introduction

### 1.1 Purpose
The purpose of this document is to define the requirements for the Internship Finder web application.  
The system is designed to help students and young professionals find internships more effectively by:  
- Uploading resumes.  
- Automatically tailoring resumes for different positions.  
- Displaying the latest job postings updated every 4 hours.  
- Automatically applying to internships on behalf of the user.  
- Providing access to networking and referral opportunities.  

The app will streamline the application process, reduce repetitive manual tasks, and increase the chances of success in obtaining internships.

### 1.2 Scope
The system will consist of the following modules:  
- **Resume Manager** – Store user resumes, parse information, and generate tailored resumes per position.  
- **Job Feed** – Continuously fetch internships from job boards/APIs and display the latest postings within the last 4 hours.  
- **Auto-Apply Engine** – Automatically submit tailored resumes to selected internships.  
- **Networking Hub** – Show potential referral opportunities based on user’s school, company alumni, and LinkedIn-like integrations.  
- **Admin Portal** – Manage users, job sources, and system operations.  

The backend will be developed with **Ruby on Rails** using **GraphQL** for API communication.  
The frontend will be built with **React.js** for a responsive and modern user experience.  
**AWS** will be used for hosting, storage, and scaling, while **Sidekiq** will handle background jobs such as resume tailoring and auto-apply tasks.

### 1.3 Intended Users
- **Students** – looking for internship opportunities.  
- **Recent graduates** – seeking entry-level positions.  
- **University career centers** – offering tools to students.  
- **Recruiters** – posting internship opportunities.  
- **Administrators** – managing the platform.  

---

## 2. Functional Requirements

### 2.1 User Management
- Users can register via email or OAuth (Google, LinkedIn).  
- Users can manage personal profiles (education, skills, experience).  
- Passwords must be stored securely using hashing (bcrypt).  
- Admins can activate/deactivate accounts.  

### 2.2 Resume Management
- Users can upload resumes (PDF, DOCX).  
- Resume data will be parsed (name, contact info, skills, experience).  
- The system can generate **tailored resumes** based on job descriptions using pre-defined templates and AI-driven recommendations.  
- Multiple versions of resumes can be stored and managed.  

### 2.3 Job Feed
- Job postings will be pulled from APIs and scraped sources every **4 hours**.  
- Users can filter by location, company, position type, and keywords.  
- Each posting includes: job title, company name, description, requirements, and application link.  
- Users can save or favorite internships.  

### 2.4 Auto-Apply Engine
- Users can enable **Auto-Apply** for selected jobs.  
- The system will match the most relevant tailored resume to the job description.  
- Auto-apply submissions will be logged and displayed in the user’s application history.  
- Users can review past applications (date, company, outcome status).  

### 2.5 Networking & Referrals
- Users can view **referral suggestions** based on:  
  - Alumni at the company.  
  - LinkedIn-like connections.  
  - Mentors or recruiters in the system.  
- Option to request referrals via in-app messaging.  

### 2.6 Notifications
- Email and in-app notifications for:  
  - New job postings within last 4 hours.  
  - Auto-apply confirmations.  
  - Referral matches.  

### 2.7 Admin Features
- Manage job sources (APIs, scraping rules).  
- Monitor system health (failed jobs, API limits).  
- Access dashboards for user engagement and system usage.  

---

## 3. Non-Functional Requirements

### 3.1 Performance
- Resume tailoring must complete within **10 seconds**.  
- Auto-apply background jobs should not exceed **30 seconds** per application.  
- The job feed should support **100,000 postings** without performance degradation.  

### 3.2 Scalability
- Use **AWS EC2** for compute and **S3** for file storage.  
- Use **AWS RDS (PostgreSQL)** as the primary database.  
- Support horizontal scaling with load balancers.  

### 3.3 Security
- Use **HTTPS/TLS** for all communications.  
- Use **JWT tokens** for user authentication with session expiry.  
- Sensitive data (resume files) must be encrypted at rest (AWS KMS).  
- Comply with **GDPR/CCPA** for user data.  

### 3.4 Reliability & Availability
- System uptime target: **99.9%**.  
- Daily backups of databases and files.  
- Automatic failover for database servers.  

### 3.5 Usability
- **Mobile-first design** with responsive layouts.  
- Accessibility compliance: **WCAG 2.1 Level AA**.  
- Simple navigation with intuitive dashboards.  

---

## 4. System Architecture

### 4.1 Frontend
- **React.js** with Apollo Client for GraphQL queries/mutations.  
- State management using **Redux Toolkit** or **React Context**.  
- UI library: Material UI or TailwindCSS.  

### 4.2 Backend
- **Ruby on Rails** with GraphQL API endpoints.  
- Business logic implemented as service objects.  
- Background job scheduling with **Sidekiq** + Redis.  

### 4.3 Database
- **PostgreSQL** for relational data.  
- **Redis** for caching and background jobs.  
- Tables:  
  - `users`  
  - `resumes`  
  - `job_postings`  
  - `applications`  
  - `referrals`  
  - `admin_logs`  

### 4.4 Infrastructure
- **AWS EC2** for app hosting.  
- **AWS S3** for resume file storage.  
- **AWS RDS** for PostgreSQL.  
- **AWS CloudWatch** for monitoring logs/metrics.  
- **Docker** for containerization.  
- **Kubernetes/ECS** for orchestration (future scaling).  

### 4.5 Workflow Example
1. User uploads a resume → Stored in S3 → Parsed and saved in DB.  
2. System fetches new job postings → Stores in `job_postings`.  
3. User enables auto-apply → Sidekiq job matches resume → Applies → Logs to `applications`.  
4. Notifications sent to user via email and frontend.  

---

## 5. User Stories

1. **Resume Tailoring**  
   - As a student, I want my resume to be tailored automatically for each internship so I can save time and increase my chances.  

2. **Job Feed**  
   - As a user, I want to see only the most recent job postings within the last 4 hours so I always have fresh opportunities.  

3. **Auto-Apply**  
   - As a student, I want the app to automatically apply to jobs on my behalf so I don’t miss deadlines.  

4. **Referral Network**  
   - As a user, I want to see possible referral opportunities so I can improve my application success rate.  

5. **Admin Control**  
   - As an admin, I want to monitor system usage and job-fetching services so I can ensure reliability.  

---

## 6. Constraints

- **Technical**:  
  - Must use GraphQL for backend API.  
  - Backend must be Ruby on Rails.  
  - Sidekiq required for job queue processing.  
  - Frontend must be React.  
  - Must run on AWS infrastructure.  

- **Business**:  
  - Must comply with data privacy regulations (GDPR, CCPA).  
  - Must support integrations with external job boards (Indeed, LinkedIn, Handshake).  

- **Time**:  
  - MVP target: 6 months.  

---

## 7. Future Enhancements
- AI-powered resume optimization with NLP.  
- Chatbot for career advice.  
- Mobile app (iOS/Android).  
- Premium subscription model for advanced features.  
- Integration with university career portals.  

---

## 8. Glossary
- **MVP** – Minimum Viable Product.  
- **GraphQL** – Query language for APIs.  
- **Sidekiq** – Background job processing library in Ruby.  
- **S3** – Amazon Simple Storage Service for file storage.  

---

