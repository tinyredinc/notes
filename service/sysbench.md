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

SINGLE THREAD

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

MULTI THREADS

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