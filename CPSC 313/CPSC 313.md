---
sticker: lucide//network
---
# CPSC 313: Computer Hardware and Operating Systems
**Instructors:** Norm Hutchinson, Thomas Pasquier  
**Coordinator:** Carol Feng  

> **Introductory exploration of computer architecture and operating systems**  
>  
> Examines how a **processor works** and how the **operating system** provides abstractions like **processes, memory, and files**. Builds on [[CPSC 213]] and [[CPSC 221]] to connect **hardware details** with **system-level behavior**.

---

## Class Outline
- Introduction to Systems  
- CPU Implementations & Instruction Execution  
- Pipelining and Hazards  
- Parallel Architectures & Branch Prediction  
- Memory Hierarchy & Caching  
- File Systems (APIs, design, naming, representation, case studies)  
- Processes, Virtual Memory, and Isolation  
- OS Abstractions: TLBs, Paging, and Faults  

---

## Topics
- CPU pipelines, instruction-level parallelism, hazards  
- Memory systems: caching, replacement, locality, coherence (MESI)  
- File systems: APIs, descriptors, block-based representation, directories  
- Operating systems: processes, isolation, paging, TLBs  
- Virtual machines and OS interactions  
- Performance optimization (C + Assembly)  

---

## Course Learning Outcomes
- Analyze and optimize pipeline timing and hazards  
- Evaluate tradeoffs in CPU, cache, and memory designs  
- Work with file system abstractions and implementations  
- Understand how OS schedules, isolates, and shares resources  
- Optimize C and Assembly for performance  

---

## Textbook
- *Computer Systems: A Programmer’s Perspective (3rd Edition)*  
  By Randal Bryant & David O’Hallaron  

---

## Assessments

| Activity              | Weight | Notes |
|-----------------------|--------|-------|
| Class Participation   | 6%     | Pre-class (2%), In-class (2%), Tutorials (2%) – lowest 3 dropped in each category |
| Quizzes (6 total)     | 36%    | 6% each, self-scheduled in CBTF |
| Labs (10 total)       | 20%    | Mix of individual, group, or hybrid (1/4 indiv, 3/4 group) |
| Final Exam            | 38%    | Self-scheduled, December |
| **Total**             | **100%** | Must pass both labs and quizzes/exams (≥50% each) |

---

## Key Dates – Quizzes
| Quiz | Dates        | Notes |
|------|-------------|-------|
| Quiz 0 | Sep 8–10  | Intro/review |
| Quiz 1 | Sep 22–24 | |
| Quiz 2 | Oct 6–8   | |
| Quiz 3 | Nov 3–5   | |
| Quiz 4 | Nov 24–26 | |
| Quiz 5 | Dec 3–5   | |
| Final Exam | After Dec 9 | Exact dates TBA |

---

## Labs  
*(Released Fridays 5 PM → Due following Sunday 11:59 PM, 9 days later)*  

| Lab   | Release | Due   | Notes |
|-------|---------|-------|-------|
| [[Lab 1]] | Sep 5  | Sep 14 | |
| [[Lab 2]] | Sep 12 | Sep 21 | |
| [[Lab 3]] | Sep 19 | Sep 28 | Individual + Partner |
| [[Lab 4]] | Sep 26 | Oct 5  | |
| [[Lab 5]] | Oct 1  | Oct 12 | |
| [[Lab 6]] | Oct 17 | Oct 26 | |
| [[Lab 7]] | Oct 24 | Nov 2  | |
| [[Lab 8]] | Oct 31 | Nov 9  | Individual + Partner |
| [[Lab 9]] | Nov 7  | Nov 16 | |
| [[Lab 10]]| Nov 14 | Nov 23 | |

---

## Weekly Highlights
[[School/CPSC 313/Introduction]]
SCHEDULE

| Week  | Topics                                                                         | In class          | Lecture |
| ----- | ------------------------------------------------------------------------------ | ----------------- | ------- |
| 1     | C refresher, Data Representation                                               |                   |         |
| 2–5   | [[y86 CPU]], [[Pipelining]], [[Hazards]], [[Branch Prediction]]                | [[Treasure Hunt]] |         |
| 6–8   | [[Memory hierarchy]], [[Caching]], [[Replacement]], [[Performance]]            |                   |         |
| 9–12  | [[File systems]] (APIs, Representation, Directories)                           |                   |         |
| 13–14 | [[Processes]], [[OS abstractions]], [[Virtual memory]], [[Paging]], [[Review]] |                   |         |

***Lecture note list***
[[1. Introduction to 313]]
[[1. Representation]]
[[2. Week 2]]
[[3. Week 3]]
[[4. Week 4 - Pipelining the CPU]]
[[5. Week 5 - Data Hazards - Control Flow]]
[[6. Week 6 - Guest Lecture on EBPF]]
[[7. Week 7 - Caching Intro]]
[[8. Week 8 - Caching Continued]] More caching
[[9. Week 9 - MSIMESI]] MSI/MESI + File Systems Intro
[[10. Week 10 File Descriptors]] The File Descriptor
[[11. Week 11 File Systems]]
[[12. The V6 File System]]
[[13. Process Abstractions and the OS]] - continued!
[[14. Trapping]]
[[15. Page Replacement and Clock]]


**Tutorials**
[[2. Tutorial 2]]
[[3. Tutorial 3]]
[[4. Tutorial 4]]
[[6. Tutorial 6 - Caching]]
[[7. Tutorial 7 - Caching pt 2]]
[[8. Tutorial 8 - File Descriptors]]
[[9. Tutorial 9 - File Systems]]
[[10. Tutorial 10 - Processes Things]]

---

## Course Staff

| Role        | Name            | Contact                   |
| ----------- | --------------- | ------------------------- |
| Instructor  | Norm Hutchinson | <norm@cs.ubc.ca>          |
| Instructor  | Thomas Pasquier | <tfjmp@cs.ubc.ca>         |
| Coordinator | Carol Feng      | <cpsc313-admin@cs.ubc.ca> |

**Teaching Assistants**:  
Hannah Baek, William Chow, Shreyas Goyal, Han Kim, Angela Li, Sina Mahdavi, Matthew Wan, Michael Wang, Naufal Wibawa, Qingyu Yan, Alex Zhang  

---

## Important Links
- [PrairieLearn]() → Assignments, quizzes, labs  
- [PrairieTest]() → Quizzes, final exam scheduling  
- [GitHub Repo]() → Pre-class & class code (VPN/UBCsecure required)  


