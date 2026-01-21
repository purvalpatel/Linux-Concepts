TCP Congestion control:
-----------------------
TCP Vegas is a **congestion avoidance algorithm** that tries to **detect congestion early and prevent packet loss**, instead of reacting after packets are dropped (like **TCP Reno**).

### Why TCP Vegas Was Introduced
#### Traditional TCP (Reno, Tahoe):
- Detects congestion after packet loss
#### Uses:
- Timeout
- Triple duplicate ACKs

❌ Packet loss = congestion already happened <br>

**👉 TCP Vegas was designed to be proactive.** <br>

**“If network delay increases, congestion is building up — slow down before packets drop.”** <br>
nstead of loss, Vegas uses: <br>
RTT (Round Trip Time) <br>
Queuing delay <br>

| Feature           | TCP Reno    | TCP Vegas        |
| ----------------- | ----------- | ---------------- |
| Congestion signal | Packet loss | RTT increase     |
| Reaction          | Reactive    | Proactive        |
| Packet loss       | High        | Very low         |
| Latency           | Higher      | Lower            |
| Fairness          | Dominates   | Can be dominated |

**TCP Vegas is a Linux kernel–level TCP congestion control algorithm.** <br>

### Check if TCP Vegas is available on your system
```
sysctl net.ipv4.tcp_available_congestion_control
```

If not enabled you can enable it via,
```
sudo sysctl -w net.ipv4.tcp_congestion_control=vegas
```
verify:
```
sysctl net.ipv4.tcp_congestion_control
```

Make it persistant on reboot:
```
sudo nano /etc/sysctl.conf
```
Add,
```
net.ipv4.tcp_congestion_control=vegas
```
Apply,
```
sudo sysctl -p
```

| Scenario                       | Best Choice             |
| ------------------------------ | ----------------------- |
| Low latency, private network   | **TCP Vegas**           |
| Public Internet, mixed traffic | **TCP Reno**            |
| Compatibility & fairness       | **TCP Reno**            |
| Research / controlled env      | **TCP Vegas**           |
| Production servers today       | **Reno (or CUBIC/BBR)** |

