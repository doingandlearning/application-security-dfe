# Introduction to Application Security

Course materials for **Introduction to Application Security**: threats, secure SDLC, OWASP Top 10, secure coding, session/data protection, security testing, DevSecOps, and LLM/AI-assisted coding security.

## Course structure

Instructor-only documents (outline, schedule, checklists, reviews, notes) are in **for_kevin/** and are not committed (see `.gitignore`).

| Doc | Purpose |
|-----|--------|
| **for_kevin/COURSE_OUTLINE.md** | Audience, learning outcomes, module list. |
| **for_kevin/DAY_SCHEDULE.md** | Three-day schedule. |
| **for_kevin/BEFORE_DELIVERY.md** | Pre-course environment and materials checks. |
| **for_kevin/PRE_DELIVERY_CHECKLIST.md** | Last-minute checklist. |
| **for_kevin/DEMO_SCRIPTS_SUMMARY.md** | Index of demos. |
| **for_kevin/WELCOME_SLIDES.md** | Welcome and housekeeping. |

## Modules

| Module | Folder | Title |
|--------|--------|--------|
| 1 | [module-01-intro-application-security](module-01-intro-application-security/) | Introduction to Application Security |
| 2 | [module-02-secure-sdlc](module-02-secure-sdlc/) | Secure SDLC |
| 3 | [module-03-common-vulnerabilities](module-03-common-vulnerabilities/) | Common Security Vulnerabilities |
| 4 | [module-04-input-validation-secure-coding](module-04-input-validation-secure-coding/) | Input Validation and Secure Coding |
| 5 | [module-05-session-management-data-protection](module-05-session-management-data-protection/) | Session Management and Data Protection |
| 6 | [module-06-security-testing](module-06-security-testing/) | Security Testing and Vulnerability Assessment |
| 7 | [module-07-secure-devops](module-07-secure-devops/) | Secure DevOps (DevSecOps) |
| 8 | [module-08-llm-security](module-08-llm-security/) | LLM and AI-Assisted Coding Security |
| 9 | [module-09-summary-next-steps](module-09-summary-next-steps/) | Summary and Next Steps |

Each module contains: **README.md**, **slides.md**, **TEACHING_NOTES.md**, **index.html**, **demos/**, **exercises/**.

## Deliberately vulnerable app

**[TrustyTickets](TrustyTickets/)** — ASP.NET Core (C#) + vanilla JS ticket app used for hands-on exploitation and fixing (SQLi, IDOR, XSS, CSRF, auth, headers). See `TrustyTickets/README.md` to run it.

