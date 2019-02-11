# XCASH CPU Miner

This program is based on XMRIG
* This is the **CPU-mining** version, there is also a [NVIDIA GPU version](https://github.com/X-CASH-official/XCASH_Nvidia_Miner) and [AMD GPU version](https://github.com/X-CASH-official/XCASH_AMD_Miner).

#### Table of contents
* [Features](#features)
* [Download](#download)
* [Usage](#usage)
* [Example Usage](#example-usage)
* [Algorithm variations](#algorithm-variations)
* [Build](#build-instructions)
* [Common Issues](#common-issues)

## Features
* High performance.
* Official Windows support.
* Small Windows executable, without dependencies.
* x86/x64 support.
* Support for backup (failover) mining server.
* keepalived support.
* Command line options compatible with cpuminer.
* It's open source software.

## Download
* Binary releases: https://github.com/X-CASH-official/XCASH_CPU_Miner/releases
* Git tree: https://github.com/X-CASH-official/XCASH_CPU_Miner.git
* Clone with `git clone hhttps://github.com/X-CASH-official/XCASH_CPU_Miner.git`

## Usage

### Options
```
  -o, --url=URL            URL of mining server
  -O, --userpass=U:P       username:password pair for mining server
  -u, --user=USERNAME      username for mining server
  -p, --pass=PASSWORD      password for mining server
      --rig-id=ID          rig identifier for pool-side statistics (needs pool support)
  -t, --threads=N          number of miner threads
  -v, --av=N               algorithm variation, 0 auto select
  -k, --keepalive          send keepalived packet for prevent timeout (needs pool support)
      --nicehash           enable nicehash.com support
      --tls                enable SSL/TLS support (needs pool support)
      --tls-fingerprint=F  pool TLS certificate fingerprint, if set enable strict certificate pinning
  -r, --retries=N          number of times to retry before switch to backup server (default: 5)
  -R, --retry-pause=N      time to pause between retries (default: 5)
      --cpu-affinity       set process affinity to CPU core(s), mask 0x3 for cores 0 and 1
      --cpu-priority       set process priority (0 idle, 2 normal to 5 highest)
      --no-huge-pages      disable huge pages support
      --no-color           disable colored output
      --variant            algorithm PoW variant
      --donate-level=N     donate level, default 5% (5 minutes in 100 minutes)
      --user-agent         set custom user-agent string for pool
  -B, --background         run the miner in the background
  -c, --config=FILE        load a JSON-format configuration file
  -l, --log-file=FILE      log all output to a file
  -S, --syslog             use system log for output messages
      --max-cpu-usage=N    maximum CPU usage for automatic threads mode (default 75)
      --safe               safe adjust threads and av settings for current CPU
      --asm=ASM            ASM code for cn/2, possible values: auto, none, intel, ryzen.
      --print-time=N       print hashrate report every N seconds
      --api-port=N         port for the miner API
      --api-access-token=T access token for API
      --api-worker-id=ID   custom worker-id for API
      --api-id=ID          custom instance ID for API
      --api-ipv6           enable IPv6 support for API
      --api-no-restricted  enable full remote access (only if API token set)
      --dry-run            test configuration and exit
  -h, --help               display this help and exit
  -V, --version            output version information and exit
```

## Example Usage
In this example, we are:
* Using the highest CPU priority `--cpu-priority 5`
* Using 4 CPU threads `--threads 4`
* Using keepalive for TCP packets to prevent a timeout `--keepalive`
* Connecting to the official X-CASH mining pool `--url minexcash.com:3333`
* Mining using "YOUR_XCASH_XCA_OR_XCB_ADDRESS" (replace YOUR_XCASH_XCA_OR_XCB_ADDRESS with your address) `--user YOUR_XCASH_XCA_OR_XCB_ADDRESS`
* Using a password of YOUR_MINING_COMPUTER_NAME (replace YOUR_MINING_COMPUTER_NAME with any name for your mining computer) `--pass YOUR_MINING_COMPUTER_NAME`  

`./XCASH_CPU_Miner --cpu-priority 5 --threads 4 --keepalive --url minexcash.com:3333 --user YOUR_XCASH_XCA_OR_XCB_ADDRESS --pass YOUR_MINING_COMPUTER_NAME`  

## Algorithm variations

- `av` option used for automatic and simple threads mode (when you specify only threads count).
- For [advanced threads mode](https://github.com/xmrig/xmrig/issues/563) each thread configured individually and `av` option not used.

| av | Hashes per round | Hardware AES |
|----|------------------|--------------|
| 1  | 1 (Single)       | yes          |
| 2  | 2 (Double)       | yes          |
| 3  | 1 (Single)       | no           |
| 4  | 2 (Double)       | no           |
| 5  | 3 (Triple)       | yes          |
| 6  | 4 (Quard)        | yes          |
| 7  | 5 (Penta)        | yes          |
| 8  | 3 (Triple)       | no           |
| 9  | 4 (Quard)        | no           |
| 10 | 5 (Penta)        | no           |

## Build Instructions

### Linux (Ubuntu)
* Install dependencies  
`sudo apt install git build-essential cmake libuv1-dev libmicrohttpd-dev libssl-dev`

* Clone the repository  
`git clone https://github.com/X-CASH-official/XCASH_CPU_Miner.git`

* Create a build folder and build the program  
`cd XCASH_CPU_Miner && mkdir build && cd build && cmake .. && make`

If you need a static build run  
`cd XCASH_CPU_Miner && mkdir build && cd build && cmake .. -DBUILD_STATIC=ON && make`

### Windows
* Install dependencies
Make sure you install Visual Studio 2017 Community Edition
https://visualstudio.microsoft.com/downloads/

Download prebuilt dependencies from XMRig and then unzip them anywhere
https://github.com/xmrig/xmrig-deps/releases
Note: If you just installed Visual Studio 2017 you can use 3.3, If you already had it installed try 3.1 instead

* Clone the repository   
`git clone https://github.com/X-CASH-official/XCASH_CPU_Miner.git`

* Create a build folder and build the program  
Replace PREBUILT_DEPENDENCIES_DIRECTORY with the directory location of the prebuilt dependencies  
`cd XCASH_CPU_Miner && mkdir build && cd build && cmake .. -G "Visual Studio 15 2017 Win64" -DXMRIG_DEPS=PREBUILT_DEPENDENCIES_DIRECTORY && cmake --build . --config Release`

If you get errors about redefintion of structs you might need to build without HTTPD and TLS (meaning the API and TSL will not work)  
`cd XCASH_CPU_Miner && mkdir build && cd build && cmake .. -G "Visual Studio 15 2017 Win64" -DXMRIG_DEPS=PREBUILT_DEPENDENCIES_DIRECTORY -DWITH_HTTPD=OFF -DWITH_TLS=OFF && cmake --build . --config Release`

If you need a static build run  
`cd XCASH_CPU_Miner && mkdir build && cd build && cmake .. -G "Visual Studio 15 2017 Win64" -DXMRIG_DEPS=PREBUILT_DEPENDENCIES_DIRECTORY -DBUILD_STATIC=ON && cmake --build . --config Release`

## Common Issues
### HUGE PAGES unavailable
* Run as Administrator.

## Other information
* No HTTP support, only stratum protocol support.
* Default donation is 0%


### CPU mining performance
It will be roughly around half of what you were mining at with Cryptonight V8 X-CASH

Please note performance is highly dependent on system load. Tasks heavily using a processor cache, such as video playback, can greatly degrade hashrate. Optimal number of threads depends on the size of the L3 cache of a processor, 1 thread requires 2 MB of cache.

### Maximum performance checklist
* Idle operating system.
* Do not exceed optimal thread count.
* Use modern CPUs with AES-NI instruction set.
* Try setup optimal cpu affinity.
* Enable fast memory (Large/Huge pages).
