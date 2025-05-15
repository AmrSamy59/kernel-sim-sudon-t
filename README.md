
## SUDON'T Kernal Simulator

This project simulates core OS kernel functionalities including:

- Process scheduling with multiple algorithms
- Memory management using buddy allocation
- Process creation and termination
- Inter-process communication (IPC)
- Performance monitoring and analysis

## Features

### Process Scheduling Algorithms

- **Round Robin (RR)**: Time-slice based scheduling with configurable quantum
- **Highest Priority First (HPF)**: Priority-based preemptive scheduling
- **Shortest Remaining Time Next (SRTN)**: Preemptive scheduling based on remaining execution time

### Memory Management

- **Buddy Memory Allocation**: Efficient memory allocation algorithm
- **Memory Mapping**: Tracks memory blocks assigned to processes
- **Wait Queue Management**: Handles processes waiting for memory

### Logging and Performance Metrics

- Detailed scheduler logs (`scheduler.log`)
- Memory allocation/deallocation logs (`memory.log`)
- Performance statistics (`scheduler.perf`)
  - CPU utilization
  - Average waiting time
  - Average weighted turnaround time
  - Standard deviation of weighted turnaround time

## Project Structure

```
kernel-sim/
├── src/                     # Source code
│   ├── Algorithms/          # Scheduling algorithm implementations
│   ├── DS/                  # Data structures (queues, linked lists, etc.)
│   ├── buddy_memory.c/h     # Buddy memory allocation implementation
│   ├── clk.c/h              # Clock simulation
│   ├── file_handlers.c/h    # File I/O and logging utilities
│   ├── memory_manager.c/h   # Memory management system
│   ├── process_generator.c  # Process generation and initialization
│   ├── process.c/h          # Process implementation
│   ├── scheduler.c/h        # Scheduler implementation
│   └── Makefile             # Build configuration
├── testcases/               # Test scenarios
│   ├── inputs/              # Input test files
│   │   ├── tc1_hpf.txt      # Test case for HPF
│   │   ├── tc3_srtn.txt     # Test case for SRTN
│   │   ├── tc4_rr2.txt      # Test case for RR (quantum=2)
│   │   └── ...              # Additional test cases
│   └── outputs/             # Output directory for test results
├── test_generator.c         # C test generator utility
└── test_generator.py        # Python test generator utility
```

## Building the Project

```bash
cd src
make
```

This will create the executable `bin/os-sim`.

## Running the Simulator

### Command Line Options

```bash
./bin/os-sim -s <scheduling-algorithm> [-q <quantum>] -f <processes-file>
```

Where:
- `<scheduling-algorithm>` can be: `rr` (Round Robin), `hpf` (Highest Priority First), or `srtn` (Shortest Remaining Time Next)
- `<quantum>` is required for Round Robin algorithm
- `<processes-file>` is a file containing process definitions

### Process File Format

```
#id arrival runtime priority memsize
1    1       12      3       64
2    2       8       8       128
3    12      9       3       32
...
```

### Generating Test Files

You can use the included test generators to create process files:

```bash
# Using Python generator
python test_generator.py

# Using C generator
gcc test_generator.c -o test_gen
./test_gen
```

## Running Test Cases

The project includes several predefined test cases in the inputs directory. The naming convention for test files is:

- `tc<num>_<algo><quantum>.txt`: where `<num>` is the test case number, `<algo>` is the algorithm (hpf, srtn, rr), and `<quantum>` is the quantum value for RR.

Following the instructions in README.md:

```bash
# Example: Run HPF test case 1
./bin/os-sim -s hpf -f testcases/inputs/tc1_hpf.txt

# Example: Run RR test case with quantum=2
./bin/os-sim -s rr -q 2 -f testcases/inputs/tc4_rr2.txt
```

The results will be stored in the outputs directory as `<num>.log` and `<num>.perf`.

## Implementation Details

### Simulated System Clock

The simulator uses a simulated system clock (clk.c) to synchronize all processes. Each process and the scheduler synchronize with this clock to maintain proper timing.

### Process Control Blocks (PCBs)

Each process is represented by a PCB that includes:
- Process ID
- Memory allocation
- Runtime information
- Remaining execution time
- Priority
- State information

### Memory Management

The buddy memory allocation system efficiently manages memory blocks, splitting and merging them as needed to satisfy process memory requirements.

## Cleaning Up

```bash
make clean
```

This will remove all build artifacts.
