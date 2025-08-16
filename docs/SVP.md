# Software Verification Plan (SVP)

## 1. Introduction
This Software Verification Plan defines the verification activities for the **Pitch Hold Flight Control System** project.  

The objective is to demonstrate compliance with **RTCA (Radio Technical Commission for Aeronautics) DO-178C** by showing that the software implementation satisfies its requirements and is free of unintended functionality.  

---

## 2. Verification Objectives
- Verify all **High-Level Requirements (HLR)** and **Low-Level Requirements (LLR)**.  
- Ensure **traceability** from requirements → design → code → test.  
- Demonstrate compliance with **DO-178C DAL C** objectives.  
- Provide **independent review evidence** for requirements, design, code, and test.  
- Collect **coverage evidence** (statement and decision coverage).  

---

## 3. Verification Approach

### 3.1 Requirements-Based Verification
- Each HLR and LLR shall be verified by at least one test case.  
- Tests shall be documented with expected results.  
- Pass/fail criteria shall be objective and reproducible.  

### 3.2 Reviews & Analysis
- Requirements Reviews: Verify clarity, consistency, testability.  
- Design Reviews: Check modularity, data/control flow, adherence to architecture.  
- Code Reviews: Verify compliance with MISRA-C, absence of dead/unused code.  
- Test Reviews: Ensure test procedures align with requirements.  

### 3.3 Testing Strategy
- **Unit Tests**: Verify individual modules (Unity + CMock framework).  
- **Integration Tests**: Verify correct interactions between modules.  
- **Fault Injection**: Simulate sensor failures or invalid inputs.  
- **Regression Tests**: Re-run test suite after changes.  

### 3.4 Coverage
- Statement Coverage: Every statement executed at least once.  
- Decision Coverage: Each branch (true/false) evaluated.  
- Note: MC/DC (Modified Condition/Decision Coverage) not required at DAL C.  

---

## 4. Test Environment
- **Host OS**: Ubuntu / macOS  
- **Toolchain**: GCC / ARM Embedded  
- **Unit Test Framework**: Unity + CMock  
- **Coverage Tool**: gcov  
- **Simulator**: Software stubs for IMU and actuator modules  

---

## 5. Verification Independence
- Requirements and design reviews performed by someone not directly implementing.  
- Code reviews performed by a peer developer.  
- Test results independently reviewed.  

---

## 6. Configuration & Traceability
- All test cases stored under `tests/`.  
- Each test case uniquely identified and linked in the **Traceability Matrix**.  
- Results archived as verification evidence.  

---

## 7. Deliverables
- Review checklists and logs (`reviews/`).  
- Unit test procedures and results (`tests/`).  
- Coverage reports (`tests/coverage_report/`).  
- Updated Traceability Matrix (`docs/TraceabilityMatrix.xlsx`).  
- Verification Summary Report (future).  

---

## 8. Compliance Records
Verification activities will produce certification evidence, including:  
- Requirements/test traceability  
- Review logs  
- Coverage evidence  
- Test results  
