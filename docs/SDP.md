# Software Development Plan (SDP)

## 1. Introduction
This Software Development Plan defines the activities, methods, tools, and standards to be used for the development of the **Pitch Hold Flight Control System** software.  

The software is developed in compliance with **RTCA DO-178C**, at **Design Assurance Level C (DAL C)**.  

---

## 2. Project Overview
- **System Function**: Maintain aircraft pitch at a commanded target angle.  
- **Inputs**: Simulated IMU pitch data.  
- **Outputs**: Elevator actuator command.  
- **Failure Effect**: Passenger discomfort (non-catastrophic).  
- **DAL**: C  

---

## 3. Software Life Cycle
The software life cycle follows the DO-178C objectives:  

1. **Requirements Capture**
   - High-Level Requirements (HLR) documented in `docs/Requirements.md`
   - Low-Level Requirements (LLR) defined in module-level design

2. **Design**
   - Architecture diagrams and state machines in `design/`
   - Interfaces defined between modules

3. **Implementation**
   - Code written in C, following MISRA-C guidelines
   - Version control with Git

4. **Verification**
   - Requirements-Based Testing (RBT) using unit tests
   - Code reviews and static analysis
   - Coverage analysis (statement + decision)

5. **Integration**
   - Combining modules and verifying correct interaction
   - Potential Hardware-in-Loop (HIL) simulation (future)

6. **Certification Outputs**
   - Traceability Matrix
   - Test reports
   - Review logs

---

## 4. Methods, Languages, and Tools
- **Programming Language**: C (C99 standard), MISRA-C subset
- **Compiler/Toolchain**: GCC / ARM Embedded Toolchain
- **Version Control**: Git + GitHub
- **Test Framework**: Unity + CMock (for unit tests)
- **Coverage Tool**: gcov
- **Documentation**: Markdown, diagrams (draw.io or PlantUML)

---

## 5. Standards and Guidelines
- **DO-178C** objectives applied at DAL C
- **Coding Standards**: MISRA-C rules enforced manually
- **Reviews**: Structured checklist-based reviews for requirements, design, code, and test

---

## 6. Development Environment
- **Host OS**: Ubuntu Linux / macOS
- **IDE/Editor**: STM32CubeIDE, VSCode, or equivalent
- **Build System**: Makefiles / CMake

---

## 7. Development Activities
- Requirements development and traceability
- Architectural design
- Detailed design (LLR)
- Implementation
- Verification (unit test, coverage, reviews)
- Baseline creation and release tagging

---

## 8. Configuration Management
Configuration management is described in detail in the **SCMP (Software Configuration Management Plan)**.  
Key points:  
- Git used for version control  
- Branching strategy (main + feature branches)  
- Tagging for baselines  

---

## 9. Quality Assurance
Quality assurance is covered in the **SQAP (Software Quality Assurance Plan)**.  
Key points:  
- Independent review of plans, requirements, design, and code  
- Checklists used for compliance verification  
- Deviations documented and tracked  

---

## 10. Deliverables
- PSAC (Plan for Software Aspects of Certification)  
- SDP (this document)  
- SVP (Software Verification Plan)  
- SCMP (Software Configuration Management Plan)  
- SQAP (Software Quality Assurance Plan)  
- Requirements documents (HLR/LLR)  
- Design documents (architecture, state machines)  
- Source code  
- Test cases, test procedures, test results  
- Traceability matrix  
- Review logs  

---
