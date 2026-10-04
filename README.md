# Cloud Computing - Experiment 1: Performance Analysis of Type-1 vs Type-2 Hypervisors

**Author:** Nupur Bagave  
**USN:** 01FE24BCI029 | **Division:** A | **Roll No:** 125  
**Repository:** [cloudcomputing_exp01](https://github.com/Nupur922/cloudcomputing_exp01)

---

## 📌 Overview

This experiment evaluates and compares the performance of **Type-1 (Bare-Metal)** and **Type-2 (Hosted)** hypervisors running an Ubuntu 22.04.5 LTS guest OS. The evaluation is conducted using **Sysbench** for CPU stress testing and performance benchmarking.

---

## 📁 Repository Structure

```
cloudcomputing_exp01/
├── Performance Analysis Using Type-1 Hypervisor.docx   # Detailed report for Proxmox VE
├── Performance Analysis Using Type-2 Hypervisor.docx   # Detailed report for VMware Workstation
└── README.md                                          # Experiment documentation & summary
```

---

## ⚡ Part 1: Type-1 Hypervisor (Proxmox VE)

### 1. Objective
To deploy, configure, and inspect an Ubuntu virtual machine directly on the **Proxmox VE (Type-1 bare-metal hypervisor)** environment, monitor guest resources, and execute a CPU benchmark using `sysbench`.

### 2. Experimental Configuration
| Parameter | Configured / Observed Value |
| :--- | :--- |
| **Hypervisor** | Proxmox VE (Type-1 Bare-Metal) |
| **Guest OS** | Ubuntu 22.04.5 LTS (x86_64) |
| **CPU Allocation** | 1 Socket, 2 Cores = 2 vCPUs |
| **CPU Model** | QEMU Virtual CPU version 2.5+ |
| **Memory Allocation** | 2048 MiB (2 GB) |
| **Virtual Disk** | 20 GB SCSI/VirtIO |
| **Network Bridge** | `vmbr0` (VirtIO) |
| **Virtualization Vendor** | KVM |

### 3. Key Commands Executed
```bash
# System and resource verification
hostnamectl
lscpu
free -h
df -h
top

# Benchmark installation and execution
sudo apt update
sudo apt install sysbench -y
sysbench --version
sysbench cpu --cpu-max-prime=20000 run

# Shutdown guest VM
sudo poweroff
```

### 4. Benchmark Results (Type-1 Proxmox VE)
- **Execution Time:** `10.0005 seconds`
- **Total Events Processed:** `15,877`
- **Throughput:** `1,587.47 events/sec`
- **Average Latency:** `0.63 ms`

---

## 💻 Part 2: Type-2 Hypervisor (VMware Workstation)

### 1. Objective
To deploy, configure, and inspect an Ubuntu virtual machine on **VMware Workstation (Type-2 hosted hypervisor)**, monitor guest resources, and run the identical `sysbench` CPU benchmark for comparative analysis.

### 2. Experimental Configuration
| Parameter | Configured / Observed Value |
| :--- | :--- |
| **Hypervisor** | VMware Workstation (Type-2 Hosted) |
| **Guest OS** | Ubuntu 22.04.5 LTS (x86_64) |
| **CPU Allocation** | 1 Processor, 2 Cores = 2 vCPUs |
| **CPU Model Reported** | 13th Gen Intel Core i5-13450HX |
| **Memory Allocation** | 2048 MB (~2 GB) |
| **Virtual Disk** | 20 GB Virtual Disk |
| **Network Adapter** | NAT |
| **Virtualization Mode** | VMware / Full Virtualization |

### 3. Key Commands Executed
```bash
# System & resource inspection
hostnamectl
lscpu
free -h
df -h
top

# Sysbench benchmark execution
sudo apt update
sudo apt install sysbench -y
sysbench cpu --cpu-max-prime=20000 run

# Shutdown
sudo poweroff
```

### 4. Benchmark Results (Type-2 VMware Workstation)
- **Execution Time:** `10.0004 seconds`
- **Total Events Processed:** `14,410`
- **Throughput:** `1,440.80 events/sec`
- **Average Latency:** `0.69 ms` (Min: `0.65 ms`, Max: `1.89 ms`)

---

## 📊 Performance Comparison & Summary

| Metric | Type-1 Hypervisor (Proxmox VE) | Type-2 Hypervisor (VMware Workstation) | Difference / Impact |
| :--- | :--- | :--- | :--- |
| **Hypervisor Layer** | Direct Hardware (Bare-Metal) | Runs on Host OS (Hosted) | Type-1 eliminates host OS overhead |
| **Virtualization Tech** | KVM / QEMU | VMware VMM / Host OS API | Direct kernel integration in Type-1 |
| **Execution Time** | `10.0005 s` | `10.0004 s` | Benchmark duration fixed ~10s |
| **Total Events** | **15,877** | **14,410** | **+10.18% higher events on Type-1** |
| **Events per Second** | **1,587.47** | **1,440.80** | **+10.18% higher throughput on Type-1** |
| **Average Latency** | **0.63 ms** | **0.69 ms** | **~8.7% lower latency on Type-1** |

---

## 🎯 Key Conclusions

1. **Architecture & Efficiency:** Type-1 hypervisors (Proxmox VE / KVM) run directly on physical host hardware, providing lower abstraction overhead compared to Type-2 hypervisors (VMware Workstation) which must pass virtualization calls through the underlying host OS.
2. **Benchmark Throughput:** Under identical vCPU (2 cores) and memory (2 GB) allocations with `sysbench cpu --cpu-max-prime=20000 run`, the Type-1 hypervisor achieved **1,587.47 events/sec** compared to **1,440.80 events/sec** on the Type-2 hypervisor.
3. **Use-Case Recommendation:**
   - **Type-1 (Proxmox VE):** Ideal for enterprise production environments, server virtualization, high performance, and minimal latency overhead.
   - **Type-2 (VMware Workstation):** Ideal for desktop software development, testing, multi-OS workstation environments, and educational labs.

---

## 📖 Reports Included

For full detailed reports including step-by-step screenshots and detailed execution logs:
- Refer to `Performance Analysis Using Type-1 Hypervisor.docx`
- Refer to `Performance Analysis Using Type-2 Hypervisor.docx`
