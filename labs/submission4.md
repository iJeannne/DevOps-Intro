# Lab 4 --- Operating System & Networking Analysis (macOS Version)

> Note: The original assignment is designed for Linux (systemd-based
> systems). I am using macOS, which does not use systemd. Therefore,
> equivalent macOS/BSD commands were used and differences are documented
> below.

------------------------------------------------------------------------

# Task 1 --- Operating System Analysis

## 1.1 Boot Performance (macOS alternative)

### Command: uptime

    17:21  up 35 days, 17:17, 2 users, load averages: 1.96 1.62 1.66

### Command: sysctl kern.boottime

    kern.boottime: { sec = 1769029461, usec = 93286 } Thu Jan 22 00:04:21 2026

### Analysis

The system has been running for **35 days and 17 hours**, indicating
high stability and no recent reboots.

Load averages: - 1-minute: 1.96 - 5-minute: 1.62 - 15-minute: 1.66

Values are stable over time, which suggests consistent background
workload rather than a sudden spike.

Since macOS does not use `systemd`, the `systemd-analyze` command is
unavailable. Boot time was determined using `sysctl kern.boottime`.

------------------------------------------------------------------------

## 1.2 Process Forensics

### Top Memory-Consuming Processes

Command used:

    ps aux | sort -nrk 4 | head -6

    Telegram – 6.1% memory
    WebKit WebContent – ~5%
    WebKit GPU – ~2%
    Safari – ~1.4%

### Top CPU-Consuming Processes

Command used:

    ps aux | sort -nrk 3 | head -6

    WebKit WebContent – 25.1% CPU
    WindowServer – 13.6% CPU
    Terminal – 3.7% CPU
    Telegram – 3.1% CPU
    bluetoothd – 1.5% CPU

### Analysis

The top memory-consuming process is:

**Telegram (6.1% of RAM)**

WebKit processes consume both memory and CPU, which is expected due to
browser tab isolation in macOS.

WindowServer usage is normal during graphical interaction.

No suspicious or unknown processes were detected.

### Required Answer

**Top memory-consuming process: Telegram**

------------------------------------------------------------------------

## 1.3 Service Management (launchd)

Command used:

    launchctl list

Partial output:

    com.apple.cloudphotod
    com.apple.Finder
    com.apple.mediaremoteagent
    com.apple.nsurlsessiond
    com.openssh.ssh-agent
    homebrew.mxcl.mongodb-community
    application.ru.keepcoder.Telegram
    application.com.google.Chrome

### Analysis

macOS uses **launchd** instead of systemd.

The system runs: - Core Apple services - User application agents - SSH
agent - MongoDB via Homebrew

Status code: - 0 → running normally - -9 → not currently active

All services appear legitimate and expected.

------------------------------------------------------------------------

## 1.4 User Sessions

### Command: who

    jeanne console 22 Jan 00:05
    jeanne ttys003 9 Feb 17:06

### Command: last -5

    jeanne ttys004 Feb 9 17:07 - 17:07
    jeanne ttys003 Feb 9 17:06 still logged in

### Analysis

Only one user account is active.

Console session corresponds to system boot date.

No unknown or suspicious login activity observed.

------------------------------------------------------------------------

## 1.5 Memory Analysis

### Command: sysctl hw.memsize

    hw.memsize: 17179869184

System RAM: **16 GB**

### Command: vm_stat

    Pages free: 18662
    Pages active: 400194
    Pages inactive: 334070
    Pages wired down: 149128
    Pages stored in compressor: 1096696
    Swapouts: 1947751

### Analysis

-   16 GB physical memory installed
-   Active memory usage is normal for desktop workload
-   Memory compression is active
-   Swap usage exists but not excessive

System memory usage appears healthy.

------------------------------------------------------------------------

# Task 2 --- Networking Analysis

## 2.1 Network Path Tracing

### Command

    traceroute -m 15 github.com

    1 172.20.10.XXX
    2 10.215.67.XXX
    3 10.215.225.XXX
    ...
    9 104.44.15.XXX
    10 104.44.41.XXX
    11 104.44.11.XXX
    12 51.10.43.XXX

### Analysis

-   Initial hops are private IP ranges
-   Traffic routes through ISP infrastructure
-   Final hops are within Microsoft Azure (GitHub hosting)
-   Some hops returned \* \* \* due to ICMP filtering

------------------------------------------------------------------------

## 2.2 DNS Resolution

### Command

    dig github.com

    github.com. 75 IN A 20.205.243.XXX
    Query time: 48 ms

### Analysis

-   Domain resolved correctly
-   DNS response time is normal
-   DNS server is local gateway (router forwarding)

------------------------------------------------------------------------

## 2.3 Packet Capture (DNS)

### Command

    sudo tcpdump -i any port 53

    0 packets captured
    100 packets received by filter
    0 packets dropped by kernel

### Analysis

No DNS packets were captured during the observation window.

Possible reasons: - DNS result cached locally - No new DNS queries
generated during capture

To demonstrate DNS functionality, manual query with `dig github.com` was
analyzed instead.

------------------------------------------------------------------------

## 2.4 Reverse DNS Lookup

### Command

    dig -x 8.8.4.4

Result:

    dns.google.

### Command

    dig -x 1.1.2.2

No PTR record returned.

### Analysis

-   8.8.4.4 resolves to dns.google (Google DNS)
-   1.1.2.2 has no public PTR record
-   Not all IP addresses have reverse DNS configured

------------------------------------------------------------------------

# Final Conclusion

Although the assignment is Linux-oriented, macOS equivalents were used
appropriately.

System characteristics: - High uptime - Stable load averages - Normal
desktop workload - Functional DNS resolution - Typical internet routing
path

No abnormal system or network behavior detected.
