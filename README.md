# Project Instructions

## Steps to Run the Project

Save the `run_all_tests.sh` file inside the same directory where `rl_sched_mod.ko` is present (`~/Design_Project_RL_Scheduler
` folder`).  
The script expects to find `rl_sched_mod.ko` in the current working directory.

### 1. Navigate to the work directory
```bash
cd ~/Design_Project_RL_Scheduler
make
make latency_probe
chmod +x throughput_worker.sh run_single_test.sh run_both.sh
```

### 2. Execute test scripts
```bash
./run_both.sh
```

### 3. Analyze results
```bash
python3 analyze_compare.py rl_test_results/
```

---

# Reinforcement Learning Aided OS Scheduler

## Overview
This project implements a **Reinforcement Learning (RL)–driven CPU scheduler** as a Linux kernel module (`rl_sched_mod.ko`).  
The scheduler dynamically adjusts process priorities using **Q-learning**, enhancing system responsiveness and throughput while maintaining fairness.

---

## Features

- Adaptive Q-learning agent integrated directly into the Linux kernel.
- Real-time modification of process priorities (`nice` values) based on workload behavior.
- Reduction in tail latency by **12.4%**, throughput increase by **1.93%**, and CPU utilization reduction by **2.8%**.
- Minimal kernel overhead (<1% CPU usage).

---

## System Requirements

- **Linux Kernel:** 5.15 or later (tested on Ubuntu 22.04)
- **Tools Required:**
  - `make`
  - `gcc`
  - `python3`
  - `bash`
- Root privileges (for kernel module insertion/removal).

---

## Module Details

### Files
| File | Description |
|------|--------------|
| `rl_sched_mod.c` | Core kernel module implementing Q-learning agent |
| `throughput_worker.sh` | CPU stress and throughput measurement script |
| `latency_probe` | Latency testing binary (compiled from C source) |
| `analyze_compare.py` | Python script for post-run result analysis |
| `run_single_test.sh` | Executes individual latency/throughput tests |
| `run_both.sh` | Runs both latency and throughput benchmarks sequentially |

---

## Implementation Summary

- **Q-Learning Parameters**
  | Parameter | Default | Description |
  |------------|----------|-------------|
  | αpermille | 200 | Learning rate (0.2) |
  | γpermille | 900 | Discount factor (0.9) |
  | ϵpermille | 200 | Exploration probability (0.2) |
  | Interval (ms) | 1000 | Sampling interval |
  | Action Step | 5 | Nice value delta per action |

- **Key APIs Used:**
  - `<linux/sched.h>`, `<linux/timer.h>`, `<linux/rcupdate.h>`, `<linux/module.h>`
  - `set_user_nice()` for modifying process priorities safely.

- **Concurrency:**  
  Uses RCU locking (`rcu_read_lock() / unlock()`) for safe task traversal.

---

## Experiment Setup

- **Workloads:**
  - CPU Workers: 4 busy-loop tasks.
  - Latency Probe: Sleep-interval-based task for responsiveness testing.
- **Phases:**
  - *Episode 1:* Exploration phase.
  - *Episode 2:* Exploitation using learned Q-table.

## Results Summary

| Metric | Baseline | RL (EP2) | Improvement |
|---------|-----------|-----------|--------------|
| Median Latency | 65.51 µs | 63.37 µs | ↓ 4.42% |
| Throughput | 89.31 iter/s | 91.03 iter/s | ↑ 1.93% |
| Avg CPU Usage | 95.45% | 92.75% | ↓ 2.82% |

---

## Repository

📦 **GitHub Link:**  
[https://github.com/Naman-ghost/Design_Project_RL_Scheduler](https://github.com/Naman-ghost/Design_Project_RL_Scheduler)

## Authors:
### 1.NAMAN SINGH
### 2.KAVYANSH KHANDELWAL
### 3.VISVIJIT KUMAR SINGH
### 4.CHARITRA JAIN


