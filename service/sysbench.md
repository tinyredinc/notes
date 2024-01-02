# Sysbench Benchmark Test

Unless you're using a standard product from a well-known cloud computing provider, it's always advisable to benchmark your system to check if its performance meets your expectations, particularly with new builds.

## Install Sysbench

```
sudo apt install sysbench
```

```
root@redhs88:/var/app# sysbench --version
sysbench 1.0.20
```

## CPU BENCHMARK

### SINGLE THREAD

```
sysbench --threads=1 --cpu-max-prime=10000 cpu run
```
- Reference result for the E5-2666v3 (10 cores, 20 threads), with 64GB DDR3 ECC RAM at 1866MHz and a 512GB NVMe SSD

```
sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 1
Initializing random number generator from current time


Prime numbers limit: 10000

Initializing worker threads...

Threads started!

CPU speed:
    events per second:  1125.14

General statistics:
    total time:                          10.0007s
    total number of events:              11255

Latency (ms):
         min:                                    0.88
         avg:                                    0.89
         max:                                    2.58
         95th percentile:                        0.90
         sum:                                 9999.04

Threads fairness:
    events (avg/stddev):           11255.0000/0.00
    execution time (avg/stddev):   9.9990/0.00

```

### MULTITHREADS

```
sysbench --threads=20 --cpu-max-prime=10000 cpu run
```

- Reference result for the E5-2666v3 (10 cores, 20 threads), with 64GB DDR3 ECC RAM at 1866MHz and a 512GB NVMe SSD

```
sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 20
Initializing random number generator from current time


Prime numbers limit: 10000

Initializing worker threads...

Threads started!

CPU speed:
    events per second: 15793.27

General statistics:
    total time:                          10.0011s
    total number of events:              157975

Latency (ms):
         min:                                    1.05
         avg:                                    1.27
         max:                                   10.05
         95th percentile:                        1.27
         sum:                               199972.46

Threads fairness:
    events (avg/stddev):           7898.7500/17.96
    execution time (avg/stddev):   9.9986/0.00

```

## MEMORY BENCHMARK

### MEMORY READ
```
sysbench --threads=20 --memory-block-size=1K --memory-total-size=512G --memory-oper=read memory run
```

- Reference result for the E5-2666v3 (10 cores, 20 threads), with 64GB DDR3 ECC RAM at 1866MHz and a 512GB NVMe SSD

```
sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 20
Initializing random number generator from current time


Running memory speed test with the following options:
  block size: 1KiB
  total size: 524288MiB
  operation: read
  scope: global

Initializing worker threads...

Threads started!

Total operations: 536870900 (95827999.02 per second)

524287.99 MiB transferred (93582.03 MiB/sec)


General statistics:
    total time:                          5.5987s
    total number of events:              536870900

Latency (ms):
         min:                                    0.00
         avg:                                    0.00
         max:                                    1.37
         95th percentile:                        0.00
         sum:                                35569.52

Threads fairness:
    events (avg/stddev):           26843545.0000/0.00
    execution time (avg/stddev):   1.7785/0.01

```

### MEMORY WRITE

```
sysbench --threads=20 --memory-block-size=1K --memory-total-size=512G --memory-oper=write memory run
```

- Reference result for the E5-2666v3 (10 cores, 20 threads), with 64GB DDR3 ECC RAM at 1866MHz and a 512GB NVMe SSD

```
sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 20
Initializing random number generator from current time


Running memory speed test with the following options:
  block size: 1KiB
  total size: 524288MiB
  operation: write
  scope: global

Initializing worker threads...

Threads started!

Total operations: 172613733 (17258186.48 per second)

168568.10 MiB transferred (16853.70 MiB/sec)


General statistics:
    total time:                          10.0001s
    total number of events:              172613733

Latency (ms):
         min:                                    0.00
         avg:                                    0.00
         max:                                   10.89
         95th percentile:                        0.00
         sum:                               173725.36

Threads fairness:
    events (avg/stddev):           8630686.6500/93153.38
    execution time (avg/stddev):   8.6863/0.03

```