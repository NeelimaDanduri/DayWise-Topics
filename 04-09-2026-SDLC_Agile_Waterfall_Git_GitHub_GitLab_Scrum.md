# Software Development Fundamentals

## 1. Sprint
A **Sprint** is a fixed period of time in which a Scrum Team works on selected work and tries to create a usable part of the product. A Sprint usually lasts from 1 to 4 weeks. A 2-week Sprint is very common.

* **Example Sprint Goal:** Build user login and password reset.
* The team selects work for the Sprint.
* Developers build the functionality.
* Testing is done during the Sprint.
* At the end, the team should have a usable Increment.

---

## 2. Sprint Principles and Values
Scrum encourages people to work together, stay focused, and continuously improve.

### The Five Scrum Values
* **Commitment** – The team works toward the Sprint Goal.
* **Focus** – The team concentrates on the important Sprint work.
* **Openness** – Team members are honest about progress and problems.
* **Respect** – Team members respect each other's skills and responsibilities.
* **Courage** – Team members have the courage to raise problems and make decisions.

### The Three Pillars of Scrum
* **Transparency** – The work and its status are visible.
* **Inspection** – The team regularly checks progress and quality.
* **Adaptation** – The team changes its approach when necessary.

---

## 3. Scrum Methodology
Scrum is an Agile framework used to develop products in small, useful pieces. Instead of waiting until the whole product is completed, the team develops and delivers smaller increments regularly.

```text
Plan → Develop → Test → Review → Improve → Repeat
```

---

## 4. Scrum Working Flow
```text
Product Backlog
      ↓
Sprint Planning
      ↓
Sprint Backlog + Sprint Goal
      ↓
Sprint
      ↓
Development + Testing
      ↓
Sprint Review
      ↓
Increment
      ↓
Sprint Retrospective
      ↓
Next Sprint
```

---

## 5. Product Backlog
The **Product Backlog** is an ordered list of work that may be needed for a product. The Product Owner is accountable for effective Product Backlog management and ordering.

It can contain:
* User stories
* Features
* Bug fixes
* Improvements
* Technical work

### Priority Example
1. User registration
2. User login
3. Product search
4. Shopping cart
5. Payment
6. Order tracking

---

## 6. Sprint Backlog
The **Sprint Backlog** contains the work selected for the current Sprint, together with the plan for delivering that work. 

> **Example:** If the Product Backlog contains *login, payment, wishlist, and order tracking*, and the team selects only login-related work for the current Sprint, those selected items become part of the Sprint Backlog.

### Key Difference
* **Product Backlog:** Work that may be needed for the *whole* product.
* **Sprint Backlog:** Work selected and planned *only* for the current Sprint.

---

## 7. Scrum Team Roles

### Product Owner
* Accountable for maximizing product value and managing the Product Backlog.
* Understands customer and business needs.
* Orders and clarifies Product Backlog items.
* Works with stakeholders.

### Scrum Master
* Helps everyone understand and use Scrum effectively.
* Facilitates Scrum events when needed.
* Helps the team deal with impediments (blockers).
* Coaches the team in Scrum and supports continuous improvement.
* *Note: A Scrum Master is not simply a project manager.*

### Developers
* The people who create the product Increment. 
* Depending on the organization, this can include software developers, testers, automation engineers, DevOps engineers, and other specialists.

---

## 8. Scrum Events

* **Sprint Planning:** The team discusses why the Sprint is valuable, what can be done, and how the selected work will be approached. The Sprint Goal and Sprint Backlog are created.
* **Daily Scrum:** A short daily event for Developers to inspect progress toward the Sprint Goal and adapt the plan. Teams may discuss what they completed, what they are doing next, and blockers (Scrum does not require a strict three-question format).
* **Sprint Review:** Held at the end of the Sprint. The team and stakeholders inspect the Increment, discuss feedback, and consider what should be done next. Focuses mainly on the **product and feedback**.
* **Sprint Retrospective:** The team discusses how the work was done, what went well, and what can be improved. Focuses mainly on the **team and process**.

---

## 9. User Story
A **User Story** is a short description of a requirement written from the user's point of view.

> **Example:** As a registered user, I want to reset my password so that I can access my account if I forget it.

### INVEST Checklist for Good User Stories
* **I**ndependent
* **N**egotiable
* **V**aluable
* **E**stimable
* **S**mall
* **T**estable

---

## 10. Story Pointing
Story points are relative estimates used to compare the effort, complexity, risk, and uncertainty of user stories. They are **not** directly equal to hours.

* **Common Scale (Fibonacci):** 1, 2, 3, 5, 8, 13, 21
* **Estimation Examples:**
  * Small text change: `1 point`
  * Login feature: `3 points`
  * Payment integration: `8 points`
  * Complex reporting feature: `13 points`

* **Planning Poker:** Teams often use this technique where each member selects an estimate, differences are discussed, and the team agrees on a reasonable value.

---

## 11. Git
Git is a distributed version control system. It helps developers track code changes, work with branches, collaborate, and return to earlier versions when needed.

### Common Git Commands
* `git init`
* `git clone`
* `git status`
* `git add`
* `git commit`
* `git push`
* `git pull`
* `git branch`
* `git merge`

### Basic Git Workflow
```text
Working Directory
      ↓
   git add
      ↓
Staging Area
      ↓
 git commit
      ↓
Local Repository
      ↓
  git push
      ↓
Remote Repository
```

---

## 12. GitHub
GitHub is a platform used to host Git repositories and collaborate on software projects.
* Repositories
* Pull Requests
* Code Reviews
* Issues
* GitHub Actions and CI/CD
* Project collaboration

---

## 13. GitLab
GitLab is another platform for hosting Git repositories and supporting software development and DevOps workflows.
* Git repositories
* Merge Requests
* Code Reviews
* Issues
* CI/CD
* Security and DevOps features

> **Key Difference:** **Git** is the local version control software tool. **GitHub/GitLab** are web-based cloud platforms that host those Git repositories and add collaboration tools.

---

## 14. SDLC – Software Development Life Cycle
SDLC describes the main stages involved in planning, building, testing, releasing, and maintaining software.

```text
Requirements → Analysis → Design → Development → Testing → Deployment → Maintenance
```

* **Requirements:** Understand what the customer or business needs.
* **Analysis:** Study scope, feasibility, risks, and technical requirements.
* **Design:** Plan the architecture, database, APIs, UI, and system components.
* **Development:** Developers write and integrate the code.
* **Testing:** The software is checked for defects and validated against requirements.
* **Deployment:** The software is released to users or target environments.
* **Maintenance:** Bugs are fixed and improvements are made after release.

---

## 15. Agile
Agile is an approach to software development that emphasizes frequent delivery, customer collaboration, feedback, adaptability, and continuous improvement.
* Work is delivered in smaller increments.
* Feedback is collected regularly.
* Requirements can change as the team learns more.
* Teams continuously improve their way of working.

---

## 16. Waterfall
Waterfall is a sequential development approach where work moves linearly through defined phases in order.

```text
Requirements → Design → Development → Testing → Deployment → Maintenance
```

---

## 17. Agile vs. Waterfall

| Feature | Agile | Waterfall |
| :--- | :--- | :--- |
| **Approach** | Iterative and incremental | Sequential and phase-based |
| **Flexibility** | Highly flexible when requirements change | Changes are difficult after a phase finishes |
| **Feedback** | Frequent customer and stakeholder feedback | Feedback is received at defined milestones |
| **Delivery** | Software is delivered in small, ongoing increments | A single major release comes at the end |
| **Testing** | Happens continuously throughout development | Occurs in a dedicated phase after development |
| **Best Fit** | Dynamic projects with evolving needs | Stable, well-understood, fixed requirements |

---

## 18. How the Concepts Connect
Here is how these topics tie together in a real-world software environment:

```text
Customer Requirement
      ↓
User Story
      ↓
Product Backlog
      ↓
Sprint Planning
      ↓
Sprint Backlog
      ↓
Development + Testing
      ↓
Git → GitHub/GitLab → Code Review → CI/CD
      ↓
Working Increment
      ↓
Sprint Review
      ↓
Retrospective
      ↓
Next Sprint
```
