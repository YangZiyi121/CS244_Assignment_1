# Assignment_1_Ziyi Yang

## **Environment**

All experiments are done under the following settings as client.

| Operating System |  macOS |
| --- | --- |
| Chip | Apple M1 Pro |
| Wireless Channel | Wi-Fi is connected to iCampus and has the IP address 10.85.38.200  |
| Wired Channel  |  LAN is active and has the IP address 10.68.203.5 with cable speed rate 1Gbps |

# iP**erf3 Tests**

This part includes a couple of tests using **iperf 3.11**.  My computer with the above settings is performing as client side. All public servers I used for testing are shown as follow.

| iPerf3 server | Localization | Speed | Port | IP version | IP address |
| --- | --- | --- | --- | --- | --- |
| https://speedtest.serverius.net/ | Netherlands | 10 Gbit/s | 5002 TCP/UDP | IPv4 and IPv6 | 178.21.16.76 |
| https://paris.testdebit.info/ | France Paris | 10 Gbit/s | 9200 to 9240 TCP/UDP | IPv4 and IPv6 | 89.84.1.194 |
| http://speedtest.uztelecom.uz/ | Uzbekistan Tashkent | 10 Gbit/s | 5201 TCP | IPv4 and IPv6 | 195.69.189.215 |
| http://iperf.biznetnetworks.com/ | Indonesia | 1 Gbit/s | 5201 to 5203 TCP | IPv4 and IPv6 | 117.102.109.186 |
1. **Wired/wireless throughput comparison**
- **Description**

For this experiment, I tested throughputs from both **wireless** and **wired** network interfaces of my computer to the public server [https://speedtest.serverius.net/](https://speedtest.serverius.net/). The cmd used is 

```bash
iperf3 -c 178.21.16.76 -p 5002 -O 1 -t time(s)
```

 -c  → setting my computer as client

178.21.16.76 → the ip address of the server I am trying to connect

-p → the **port number** that the server is **listening** for connection

-O  1 → omit the first second, since it is the warm up stage 

-t → time in seconds to transmit for, I set it to 60s for accuracy

**note**: iPerf3 is by default using TCP, no need for setting this flag

- **Results**

**60s Test**

**Wired** throughput. (local 10.68.203.5 → server 178.21.16.76)

```bash
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-60.00  sec  2.31 GBytes   331 Mbits/sec                  sender
[  5]   0.00-60.00  sec  2.31 GBytes   331 Mbits/sec                  receiver
```

**Wireless** throughput (local 10.85.38.200 → server 178.21.16.76)

```bash
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-60.00  sec   208 MBytes  29.1 Mbits/sec                  sender
[  5]   0.00-60.00  sec   208 MBytes  29.1 Mbits/sec                  receiver
```

Wired network is more than **10 times** faster than wireless one.

**Throughput changes according to time (wired and wireless)**

|  | 1s | 2s | 3s | 4s | 5s | 10s | 15s | 20s |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Wired | 117 Mbits/s | 243 Mbits/s | 292 Mbits/s | 310 Mbits/s | 325 Mbits/s | 325 Mbits/s | 332 Mbits/s | 333 Mbits/s |
| Wireless | 10.1 Mbits/s | 13.7 Mbits/s | 14.1 Mbits/s | 26.0 Mbits/s | 38.8 Mbits/s | 110 Mbits/s | 82.2 Mbits/s | 101 Mbits/s |

![troughput](https://github.com/YangZiyi121/CS244_Assignment_1/blob/main/Assignment_1_Ziyi%20Yang_figure/throughput_time.png)

- **Reasons**
    - Wired network has more **stable** connection, since it is **physical connection**
    - Wired network uses copper as **communication medium**, while it is air for wireless
    - Wireless connection is easier to be influenced by **obstacles**
    
2. **Different servers in different areas throughput comparison**

- **Description**

For this experiment, I will test the throughput from my computer wired network interface to **iPerf3** servers in **Netherlands, France Paris, Uzbekistan Tashkent** and **Indonesia.**  The first three of them have theoretical **10Gbit/s** speed, while the last one has **1Gbit/s** speed. The cmd used is the same as previously (with different port parameter), testing the **receiving** and **sending** throughput for 60s. 

- **Results**

From local to **Netherlands** (178.21.16.76)

```bash
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-60.00  sec  2.51 GBytes   360 Mbits/sec                  sender
[  5]   0.00-60.00  sec  2.52 GBytes   360 Mbits/sec                  receiver
```

From local to **Paris** (89.84.1.194)

```bash
[ ID] Interval           Transfer     Bitrate
[  7]   0.00-60.00  sec  2.79 GBytes   400 Mbits/sec                  sender
[  7]   0.00-60.08  sec  2.80 GBytes   400 Mbits/sec                  receiver
```

From local to **Uzbekistan Tashkent** (195.69.189.215)

```bash
[ ID] Interval           Transfer     Bitrate
[  7]   0.00-60.00  sec  1.40 GBytes   200 Mbits/sec                  sender
[  7]   0.00-60.00  sec  1.40 GBytes   201 Mbits/sec                  receiver
```

From local to **Indonesia** (117.102.109.186)

```bash
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-60.00  sec   949 MBytes   133 Mbits/sec                  sender
[  5]   0.00-60.00  sec   949 MBytes   133 Mbits/sec                  receiver
```

- **Reasons**
  
    There are mainly two things influencing the throughput
    
    - Server capacity
      
        The server in Indonesia has the lowest **1Gbps** speed, which results in the lowest throughput 113 Mbits/sec.
        
    - Routing bottleneck
      
        By running **traceroute**, we can get the info of each hop that taken. There may be some bottleneck affects throughputs that causes the differences.
        
3. **Multi threads**

- **Description**

I tested the throughput from local wired interface to server in Indonesia (117.102.109.186). The cmd used is 

```bash
iperf3 -c 117.102.109.186 -p 5202 -O 1 -t 60 -P 5
```

-P → The number of client streams to run

- **Results**

| Thread num | Sender throughput | Receiver throughput | Sending transfer | Receiving transfer |
| --- | --- | --- | --- | --- |
| 1 | 99.1 Mbits/sec | 99.0 Mbits/sec | 709 Mbytes | 708 Mbytes |
| 2 | 93.9 Mbits/sec | 93.8 Mbits/sec | 672 MBytes | 671 MBytes |
| 3 | 371 Mbits/sec | 371 Mbits/sec | 2.59 GBytes | 2.59 GBytes |
| 4 | 495 Mbits/sec | 494 Mbits/sec | 3.45 GBytes | 3.45 GBytes |
| 5 | 445 Mbits/sec | 444 Mbits/sec | 3.11 GBytes | 3.10 GBytes |
| 6 | 464 Mbits/sec | 462 Mbits/sec | 3.24 GBytes | 3.23 GBytes |
| 7 | 491 Mbits/sec | 489 Mbits/sec | 3.43 GBytes | 3.42 GBytes |
| 8 | 510 Mbits/sec | 509 Mbits/sec | 3.56 GBytes | 3.55 GBytes |
| 9 | 514 Mbits/sec | 512 Mbits/sec | 3.59 GBytes | 3.58 GBytes |
| 10 | 498 Mbits/sec | 495 Mbits/sec | 3.48 GBytes | 3.46 GBytes |
| 20 | 500 Mbites/sec | 498 Mbits/sec | 3.49 GBytes | 3.48 GBytes |

![Untitled](https://github.com/YangZiyi121/CS244_Assignment_1/blob/main/Assignment_1_Ziyi%20Yang_figure/throughput_client.png)

- **Reasons**

With the increasing of thread number, the sending and receiving throughput reaches the capacity of the channel at around 500 Mbits/sec and becomes nearly constant. Trade off between **throughput** and **latency**

 When the thread number is **small** → the throughput needed for sending and receiving is low → the channel is able to process fast with no queueing

When the thread number is **big** → the throughput needed used up the capacity of the channel → the jobs need to queue for processing, but there is no wasted capacity of the channel

4. **Reverse Test**

- **Description**
  
    For this part, I tried to do reverse test through wired interface, which means **server sends** and **client receives**. The server I used in this test is Netherland (178.21.16.76) with cmd 
    
    ```bash
    iperf3 -c 178.21.16.76 -p 5002 -t 10 -R
    ```
    

       -R means run in reverse mode

- **Results**

|  | 1s | 2s | 3s | 4s | 5s | 10s | 15s | 20s |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Normal | 161 Mbits/s | 217 Mbits/s | 299 Mbits/s | 305 Mbits/s | 325 Mbits/s | 321 Mbits/s | 347 Mbits/s | 296 Mbits/s |
| Reverse | 45.5 Mbits/s | 46.3 Mbits/s | 39.1 Mbits/s | 38.4 Mbits/s | 35.5 Mbits/s | 24.1 Mbits/s | 35.5 Mbits/s | 29.8 Mbits/s |

![Untitled](https://github.com/YangZiyi121/CS244_Assignment_1/blob/main/Assignment_1_Ziyi%20Yang_figure/reverse.png)

- **Reasons**
  
    Theoretically, no matter client sends or server sends, the throughputs should be similar since they have similar routing distance. However, in this experiment, it varies a lot. In my opinion, I feel it is because that the sever is public and busy, resulting in **queueing** for sending packets.
5. **Adjust window size**

* **Discription**

For this part, I did the experiment from my local wired interface to server in Indonesia (117.102.109.186). The window size is 	adjusted from **0.01M** to **1M**.  The cmd used is 

```bash
iperf3 -c 117.102.109.186 -p 5203 --window windowSize
```

* **Results**

  |          | 0.01M       | 0.05M        | 0.1M         | 0.15M        | 0.2M         | 0.5M         | 1M           |
  | -------- | ----------- | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ |
  | sender   | 239 kbits/s | 1.48 Mbits/s | 2.73 Mbits/s | 4.24 Mbits/s | 4.47 Mbits/s | 5.43 Mbits/s | 5.73 Mbits/s |
  | receiver | 239 kbits/s | 1.48 Mbits/s | 2.73 Mbits/s | 4.24 Mbits/s | 4.44 Mbits/s | 5.17 Mbits/s | 5.04 Mbits/s |

![windwSize.png](https://github.com/YangZiyi121/CS244_Assignment_1/blob/main/Assignment_1_Ziyi%20Yang_figure/windowSize.png)

6. **iPerf Server on U280**

- **Description**

Following the github library [https://github.com/fpgasystems/Vitis_with_100Gbps_TCP-IP#configure-tcp-stack](https://github.com/fpgasystems/Vitis_with_100Gbps_TCP-IP#configure-tcp-stack) , the iPerf kernel (User KRNL) with tcp-ip stack was ran on board U280. The architecture overview is shown as below. 

![Screen Shot 2022-09-12 at 12.13.16 PM.png](https://github.com/YangZiyi121/CS244_Assignment_1/blob/main/Assignment_1_Ziyi%20Yang_figure/architecture.png)

 I directly connected a **NVIDIA-Mellanox-ConnectX-6 SmartNIC to the** U280 board ****using **100Gbps DAC cable.** The NIC is acting as an **iPerf client** and the board is the **iPerf server**. 

**IP of NIC**: 10.72.138.19

**IPof U280**: 10.72.138.18

![U280_iperf.png](https://github.com/YangZiyi121/CS244_Assignment_1/blob/main/Assignment_1_Ziyi%20Yang_figure/U280_iperf.png)

- **Result**

```bash
------------------------------------------------------------
Client connecting to 10.72.138.18, TCP port 5001
TCP window size:  165 KByte (default)
------------------------------------------------------------
[  3] local 10.72.138.19 port 56248 connected with 10.72.138.18 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec  33.8 GBytes  29.1 Gbits/sec
```

# Owamp Tests

For this experiment, I deployed **owamp server** on our lab workstation. The setting is shown as follow.

| Hostname | kw61088 |
| --- | --- |
| IP Address  | 10.72.139.15 |
| OS | Ubuntu 18.04 |
- **Installation and setup**

The perfSonar public server has not been maintained for a long time and showed **denial-of-service,** I installed owamp according to [https://docs.perfsonar.net/release_candidates/4.0rc2/install_debian.html](https://docs.perfsonar.net/release_candidates/4.0rc2/install_debian.html)

The **owamp** server can be setup using cmd 

```bash
owampd -c /path/to/confdir
```

1. **Packet delay vs count**
- **Description**

For this part, I used my own laptop as **owamp** client (wired interface) and tried to test packet delay with different number of test packets sent. The cmd used is 

```bash
owping -c num 10.72.138.15
```

- **Results**

| packet Num | 1 | 10 | 20 | 100 | 300 | 500 |
| --- | --- | --- | --- | --- | --- | --- |
| Delay min/median/max (client → server) | 52.5/52.5/52.5 ms, (err=504 ms) | 50.2/51/53.3 ms, (err=504 ms) | 56.4/57.1/58.4 ms, (err=504 ms) | 57/57.6/61.2 ms, (err=504 ms) | 62.6/63.5/67.4 ms, (err=504 ms) | 63.9/64.9/68.8 ms, (err=504 ms) |
| Delay min/median/max (server → client) | -42.6/-42.5/-42.4 ms, (err=504 ms) | -49/-48.7/-47.1 ms, (err=504 ms) | -55.1/-54.7/-17 ms, (err=504 ms) | -55.9/-55.3/-18.8 ms, (err=504 ms) | -61.8/-61.3/-42.4 ms, (err=504 ms) | -63.4/-62.6/-28.3 ms, (err=504 ms) |

![Untitled](https://github.com/YangZiyi121/CS244_Assignment_1/blob/main/Assignment_1_Ziyi%20Yang_figure/delay.png)

- **Reasons**

The packet delay increases with the number of packet sent and becomes stable after around 300 packets. This because that when the packet number is small, the throughput is not used up and the server is able to process it immediately. When the number of packet sent is large, there is a **congestion** in the network and causes the delay to increase. 

2. **Different link rates vs delay**

- **Description**

For this part, I used my own laptop as **owamp client** and tried to test the **delay** using wired and wireless interfaces.   As tested previously, the wired interface has around 10 times link rate than the wireless one. The cmd used is 

```bash
owping -c 100 10.72.138.15
```

The default sending **100 packets** with **0.1 second** scheduled delay for testing

- **Results**

|  | Hops | One-way delay min/median/max (sending) | One-way delay min/median/max (receiving) | Jitter (sending) | Jitter(receiving) |
| --- | --- | --- | --- | --- | --- |
| Wired | 2 | 88.3/89.2/94.5 ms, (err=504 ms) | 87.4/-86.8/-62.2 ms, (err=504 ms) | 0.7 ms (P95-P50) | 3.7 ms (P95-P50) |
| Wireless | 2 | 88.2/89.2/140 ms, (err=504 ms) | 85.5/-84.9/-49.7 ms, (err=504 ms) | 1.7 ms(P95-P50) | 4.1 ms(P95-P50) |
- **Reasons**
    - There are two **layer 3 switches** between my laptop and kw61088, resulting in 2 hops
    - The jitter of **wired interface** is much smaller because of the **stable** connection
    - The delay is similar because that both of them are routing through **the same network** and testing with the same server
