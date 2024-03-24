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
Prime numbers limit: 10000

CPU speed:
    events per second:  1125.14

General statistics:
    total time:                          10.0007s
    total number of events:              11255
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
Prime numbers limit: 10000

CPU speed:
    events per second: 15793.27

General statistics:
    total time:                          10.0011s
    total number of events:              157975
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
block size: 1KiB
total size: 524288MiB
operation: read
scope: global

Total operations: 536870900 (95827999.02 per second)
524287.99 MiB transferred (93582.03 MiB/sec)
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
block size: 1KiB
total size: 524288MiB
operation: write
scope: global

Total operations: 172613733 (17258186.48 per second)
168568.10 MiB transferred (16853.70 MiB/sec)
```

## DISK BENCHMARK

### DROP CACHE
Linux system caching can significantly affect your benchmarks, especially read speeds. If you're seeing unbelievably high numbers, consider dropping the cache before running benchmarks.
```
#Drop Page Cache: This will clear out the page cache, which includes files and other data cached from disk reads.

echo 1 | sudo tee /proc/sys/vm/drop_caches


# Drop Dentries and Inodes: This clears out the dentry and inode caches, which are used by the kernel to keep track of directories and files.

echo 2 | sudo tee /proc/sys/vm/drop_caches


# Drop Page Cache, Dentries, and Inodes: This clears out all of the above, providing a more thorough cache cleanup.

echo 3 | sudo tee /proc/sys/vm/drop_caches
```

### DISK SEQ READ/WRITE

```
sysbench --threads=20 --file-total-size=8G --file-num=1 --file-block-size=16K fileio prepare

sysbench --threads=20 --file-total-size=8G --file-num=1 --file-block-size=16K --file-fsync-freq=0 --file-test-mode=seqrd fileio run
sysbench --threads=20 --file-total-size=8G --file-num=1 --file-block-size=16K --file-fsync-freq=0 --file-test-mode=seqwr fileio run

sysbench --threads=20 --file-total-size=8G --file-num=1 --file-block-size=16K fileio cleanup
```

- Reference result for the E5-2666v3 (10 cores, 20 threads), with 64GB DDR3 ECC RAM at 1866MHz and a 512GB NVMe SSD

```
sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 20
1 files, 8GiB each
8GiB total file size
Block size 16KiB
Calling fsync() at the end of test, Enabled.
Using synchronous I/O mode

File operations:
    reads/s:                      1482339.01
    writes/s:                     79356.41
    fsyncs/s:                     1.52

Throughput:
    read, MiB/s:                  23161.55
    written, MiB/s:               1239.94
```

### DISK RND READ/WRITE

```
sysbench --threads=20 --file-total-size=8G --file-num=128 --file-block-size=4K fileio prepare

sysbench --threads=20 --file-total-size=8G --file-num=128 --file-block-size=4K --file-fsync-freq=0 --file-test-mode=rndrw fileio run

sysbench --threads=20 --file-total-size=8G --file-num=128 --file-block-size=4K fileio cleanup
```

- Reference result for the E5-2666v3 (10 cores, 20 threads), with 64GB DDR3 ECC RAM at 1866MHz and a 512GB NVMe SSD

```
sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 20
128 files, 64MiB each
8GiB total file size
Block size 4KiB
Number of IO requests: 0
Read/Write ratio for combined random IO test: 1.50
Calling fsync() at the end of test, Enabled.
Using synchronous I/O mode
Doing random r/w test

File operations:
    reads/s:                      620197.28
    writes/s:                     413464.85
    fsyncs/s:                     182.83

Throughput:
    read, MiB/s:                  2422.65
    written, MiB/s:               1615.10

```

## MYSQL READ/WRITE

```
CREATE SCHEMA `sbtest` ;

sysbench --threads=64 --table_size=1000000 --tables=8 --mysql-host='' --mysql-user='' --mysql-password='' oltp_read_write prepare

sysbench --threads=64 --table_size=1000000 --tables=8 --mysql-host='' --mysql-user='' --mysql-password='' oltp_read_write run

sysbench --threads=64 --table_size=1000000 --tables=8 --mysql-host='' --mysql-user='' --mysql-password='' oltp_read_write cleanup
```
- Reference result for the E5-2666v3 (10 cores, 20 threads), with 64GB DDR3 ECC RAM at 1866MHz and a 512GB NVMe SSD
```
sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 64

SQL statistics:
    queries performed:
        read:                            571228
        write:                           163208
        other:                           81604
        total:                           816040
    transactions:                        40802  (4050.74 per sec.)
    queries:                             816040 (81014.78 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)
```