---
title: "Parallel Replica Algorithm"
excerpt: "The Parallel Replica Method (ParRep) is a computational technique designed to accelerate molecular dynamics simulations by leveraging parallel computing to explore rare events and long timescales. Traditional molecular dynamics often struggle with simulating processes separated by long waiting times due to time step limitations. As part of our coursework, I implemented ParRep to simulate rare transitions between metastable states efficiently. During the implementation, I used a parallelized approach, utilizing GPU threading to distribute computational workloads across multiple processors. I used the Julia programming language, leveraging its capabilities for high-performance computing, including GPU acceleration. Through benchmarking, I evaluated both performance and efficiency to select the optimal threading configuration, ensuring maximum speedup and resource utilization.

In the image below, the system begins at the top of the energy landscape and undergoes random motion. Over time, it becomes trapped in the circled region, representing a metastable state. ParRep helps accelerate the process of escaping such states, which would take much longer using traditional molecular dynamics methods. This method allows for the efficient simulation of rare events without modifying the potential energy surface, making it highly effective for studying processes like phase transitions or defect migrations.  <br/><img src='/images/trajectory_animation.mp4'>
"
collection: portfolio
---
