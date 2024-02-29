1. SR-IOV: higher bandwidth PPS (packet per second), lower latency. 2 options: Elastic Network Adapter (ENA) up to 100 GBs, Legacy (Intel 82599 VF) up to 10 GBs
2. Elastic Fabric Adapter (EFA): improve ENA for HPC, Linux only, great for inter-node communications, tightly coupled workloads, leverage message passing interface (MPI) standard, bypass the underlying Linux OS to provide low-latency, reliable transport

 ==Noted these network is defined by instance type==