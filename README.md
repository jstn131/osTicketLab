# osTicket Helpdesk Lab

## Overview
This project is a helpdesk ticketing system built using osTicket,
a free open source ticketing platform used by real small and medium 
businesses. I built this project to gain hands on experience with 
how a helpdesk ticketing system actually works in a professional 
environment and to understand the full ticket lifecycle from 
submission to resolution.

osTicket serves as the management layer of helpdesk operations —
it organizes, tracks, and documents support requests so that 
technicians can manage their workload efficiently without users 
having to call or physically visit the helpdesk.

---

## Technologies Used
- osTicket v1.18.3
- XAMPP (Apache, MySQL, PHP)
- phpMyAdmin
- Windows 11 (Host Machine)
- Browser based administration and user portal

---

## What I Configured

### Departments
Created three departments to simulate a real company IT structure:
- `IT Support` — handles account issues, password resets, and general IT requests
- `Network Support` — handles connectivity and network related issues
- `Hardware Support` — handles physical device and hardware related issues

### Help Topics
Help topics are the categories users select when submitting a ticket.
They automatically route tickets to the correct department and set 
the initial priority level:

| Help Topic | Department | Priority |
|---|---|---|
| Password Reset | IT Support | Normal |
| Account Locked | IT Support | High |
| New Employee Setup | IT Support | Normal |
| Network Connectivity | Network Support | High |

### SLA Plans
SLA stands for Service Level Agreement. An SLA defines how long a 
helpdesk technician has to respond to and resolve a ticket before 
it is flagged as overdue. Higher priority issues have shorter SLA 
windows to ensure critical problems are addressed immediately.

| SLA Plan | Grace Period | Schedule | Use Case |
|---|---|---|---|
| SEV-A Critical | 1 hour | 24/7 | Company wide outages, critical failures |
| SEV-B High | 4 hours | 24/7 | Multiple users affected, urgent issues |
| SEV-C Normal | 8 hours | Business Hours | Single user issues, everyday requests |

The schedule defines when the SLA clock runs. A 24/7 schedule means 
the clock runs continuously including nights and weekends. A Business 
Hours schedule only counts time during working hours — useful for 
lower priority tickets that don't require after hours response.

### Agents and Roles
Agents are the helpdesk staff who receive and resolve tickets.
Access levels follow Role Based Access Control (RBAC) — the same 
principle seen in Active Directory Security Groups where users only 
get access to what their role requires.

| Agent | Department | Role |
|---|---|---|
| Mike Johnson | IT Support | All Access |
| Justin Fox | IT Support | Limited Access |
| Karen Davis | Network Support | All Access |

**All Access** — agent can view, edit, assign, and resolve any 
ticket within their department. Suitable for senior technicians 
and team leads.

**Limited Access** — agent can only work tickets specifically 
assigned to them and has restricted ability to make changes. 
Suitable for junior technicians and new hires.

This mirrors how real helpdesk tiers work:
Level 1 — Limited Access — handles basic everyday tickets
Level 2 — All Access — handles escalated and complex tickets
Level 3/Admin — Full Control — manages agents and system settings

### Users
Users are the employees who submit support tickets through the 
customer portal. In a real environment users submit tickets instead 
of calling or walking to the helpdesk directly.

| User | Email |
|---|---|
| John Smith | john.smith@helpdesk.com |
| Sarah Connor | sarah.connor@helpdesk.com |
| Harvey Specter | harvey.specter@helpdesk.com |
| Michael Ross | michael.ross@helpdesk.com |

> **Note:** In a real environment new employees would not submit 
> their own onboarding tickets. HR or their manager would submit 
> on their behalf before their first day since the new employee 
> has no company account yet.

---

## Ticket Lifecycle

Every ticket in osTicket follows this lifecycle from creation to closure:

| Step | Action |
|---|---|
| **1** | User submits ticket via customer portal |
| **2** | Ticket appears in agent queue unassigned |
| **3** | Agent triages ticket — sets priority, SLA, and department |
| **4** | Ticket assigned to agent or department |
| **5** | Agent investigates and works the issue |
| **6** | Agent uses actual IT tools to resolve — AD, RDP, command line |
| **7** | Agent posts response to user with resolution |
| **8** | Ticket marked Resolved and closed |
| **9** | Ticket archived permanently — never deleted |

Tickets are never deleted because they serve as an audit trail —
providing proof of work, reference for recurring issues, and 
documentation for compliance purposes.

---

## Tickets Worked

| Ticket | User | Help Topic | Priority | SLA | Resolution |
|---|---|---|---|---|---|
| Unable to login — password not working | John Smith | Password Reset | Normal | SEV-C | Password reset performed, temporary credentials provided |
| Account locked after too many attempts | Harvey Specter | Account Locked | High | SEV-B | Account unlocked immediately, user notified |
| Cannot connect to company network | Sarah Connor | Network Connectivity | High | SEV-B | Assigned to Network Support, awaiting diagnostic information |
| New employee needs account setup | Michael Ross | New Employee Setup | Normal | SEV-C | Account created, credentials provided |
| Entire 3rd floor cannot access network | Michael Ross | Network Connectivity | Emergency | SEV-A | Escalated to Network Support department, all hands response initiated |

---

## What I Learned

- How to install and configure osTicket using XAMPP as a local 
  web server environment
- How to set up departments, help topics, SLA plans, and agents 
  to simulate a real helpdesk environment
- How ticketing systems serve as the management layer of helpdesk 
  operations while actual troubleshooting happens in separate tools
- How SLA plans enforce response and resolution time requirements 
  based on ticket priority
- How Role Based Access Control applies to ticketing systems the 
  same way it applies to Active Directory
- How to work tickets through the full lifecycle from triage to 
  resolution including proper escalation procedures
- The difference between public replies visible to users and 
  internal notes visible only to agents
- Why tickets are never deleted and serve as a permanent audit trail
- How new employee onboarding tickets work in practice and why 
  new hires cannot submit their own onboarding requests

---

## Key Concepts

**SLA (Service Level Agreement)**
A formal agreement defining how quickly tickets must be responded 
to and resolved based on priority level. Missing an SLA is flagged 
automatically by the system and can result in escalation.

**Ticket Triage**
The process of reviewing a new ticket, assigning the correct 
priority, SLA, department, and agent before work begins. Similar 
to how emergency rooms triage patients by severity.

**Escalation**
When a ticket exceeds the ability of the current agent or tier 
it is escalated to a higher level team or specialist. Critical 
tickets are escalated immediately to the entire department rather 
than a single agent.

**Internal Notes**
Notes added to a ticket that are only visible to agents — not 
the user who submitted the ticket. Used for documenting 
troubleshooting steps, agent to agent communication, and 
sensitive information.

**RBAC in osTicket**
Role Based Access Control controls what each agent can do within 
the system. All Access agents can manage any ticket in their 
department while Limited Access agents can only work tickets 
assigned directly to them. This mirrors how Security Groups work 
in Active Directory.

**Ticketing vs Actual Troubleshooting**
The ticketing system is the management layer — it organizes, 
tracks, and documents work. Actual troubleshooting happens in 
separate tools like Active Directory, Remote Desktop, and command 
line utilities. A technician reads the ticket in osTicket then 
opens AD or RDP to actually fix the problem, then documents the 
resolution back in the ticket.

---

## Author
**Justin Hernandez**
CIS Student — Cal Poly Pomona
IT/Cybersecurity Intern — LA-Tech.org
[GitHub](https://github.com/jstn131)
[LinkedIn](www.linkedin.com/in/jstn131)
