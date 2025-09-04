The chart below outlines the key phases in the scientific development of **nuRemics Apps**, along with the roles involved in each phase. Each role is associated with specific responsibilities and expertise. This structure ensures a reproducible and high-quality development process.

| Phase | Role | Scientific Expertise | Python Expertise| Software Expertise | Main Responsabilities |
|-------|------|----------------------------|-----------------|--------------------|-----------------------|
| Use Case | Requirements Scientist | ✅ | ❌ | ❌ | Defines the scientific/business need into a clear use case, constituting the Functional Requirement Specifications (FRS). |
| Architecture | Workflow Architect | ❌ | ✅ | ❌ | Translate the use case into a structured scientific workflow, formalizing the Software Requirement Specifications (SRS). |
| Implementation | Scientific Developer | ✅ | ✅ | ❌ | Implement the software items constituting the workflow in compliance with the defined SRS. |
| Integration | Software Developer | ❌ | ✅ | ✅ | Support the implementation, optimize the code, and develop tests for integration and Software Quality Assurance (SQA). |
| Deployment | DevOps | ❌ | ❌ | ✅ | Ensure reliable deployment and installation processes for end-users. |
| Production | End-User Scientist | ✅ | ❌ | ❌ | Execute workflow to produce, interpret, and report scientific results. |

✅ Required <br>
❌ Not required


<!-- ---

## 1. Scientific Requirement Expert / Use Case Author

**Phase:** Use case definition / specification

**Role Overview:**  
Responsible for defining the scientific or business problem to solve and documenting it in the form of a use case. This serves as a functional specification and guides the subsequent development process.

**Responsibilities:**

- Define the scientific or business problem to solve.
- Write the use case as a structured document or scientific article.
- Identify expected outcomes or performance indicators.

**Skills:**

- Domain expertise (science or business)
- Ability to formalize user needs
- Scientific writing

**Python:** Not required  
**Science / Domain Knowledge:** Required

---

## 2. Scientific Workflow Architect / Lead Framework Developer

**Phase:** Workflow design and architecture

**Role Overview:**  
Responsible for translating the use case into a structured scientific workflow in **nuRemics** and defining the overall software architecture.

**Responsibilities:**
- Decompose workflows into modular software units with defined inputs/outputs.
- Define software architecture and interfaces between modules.
- Ensure consistency with **nuRemics** best practices.
- Train and supervise developers on the framework and architecture.

**Skills:**
- Software engineering
- Advanced Python programming
- Expertise in the nuRemics framework
- Scientific workflow design

**Python:** Advanced  
**Science / Domain Knowledge:** Not required but helpful

---

## 3. Scientific Programmer / Unit Lead

**Phase:** Unit implementation

**Role Overview:**  
Responsible for implementing software units that accurately reflect the scientific or business requirements of the use case.

**Responsibilities:**
- Develop software units according to specifications.
- Validate scientific correctness of calculations or data processing.
- Supervise support developers (Python developers without domain expertise).

**Skills:**
- Scientific or domain knowledge
- Scientific programming in Python

**Python:** Required  
**Science / Domain Knowledge:** Required

---

## 4. Python Developer / Software Engineer

**Phase:** Unit implementation and integration

**Role Overview:**  
Supports the development of software units, ensuring code quality and efficiency.

**Responsibilities:**
- Implement technical parts of the software units.
- Optimize code performance.
- Develop unit tests and support CI/CD integration.
- Assist Scientific Programmers as needed.

**Skills:**
- Python programming
- Software development best practices
- Testing and CI/CD

**Python:** Required  
**Science / Domain Knowledge:** Not required (optional)

---

## 5. Test / Validation Lead

**Phase:** Unit testing and CI/CD integration

**Role Overview:**  
Responsible for ensuring robustness, reproducibility, and quality of software units.

**Responsibilities:**
- Define and execute unit and integration tests.
- Verify compliance with specifications and expected outcomes.
- Supervise CI/CD pipelines for integration.

**Skills:**
- Python programming
- Software testing and CI/CD
- Basic understanding of scientific requirements for validation

**Python:** Required  
**Science / Domain Knowledge:** Supported by Scientific Programmer

---

## 6. End-User / Scientific Producer

**Phase:** Application use and scientific production

**Role Overview:**  
Uses the developed application to generate scientific results and interpret them.

**Responsibilities:**
- Execute scientific workflows using the application.
- Interpret results and write discussions or conclusions.
- Report issues or inconsistencies in the software outputs.

**Skills:**
- Domain expertise (science or business)
- Ability to analyze and interpret results

**Python:** Not required  
**Science / Domain Knowledge:** Required -->