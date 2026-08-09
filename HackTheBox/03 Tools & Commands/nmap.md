nmap \[Scan Type(s)] \[Options] {target specification}

-sV   version detection
-sC   scripts
-sS	TCP SYN (stealth) scan
-sT	TCP connect scan
-sU	UDP scan
-sA	ACK scan
-sW	Window scan
-sM	Maimon scan
-sN	TCP NULL scan
-sF	FIN scan
-sX	Xmas scan
-sI	Idle (zombie) scan
-sO	IP protocol scan
-sV   Detect service versions
-O    OS detection
-p-   **scan all 65,535 TCP ports**.
-Pn   Treat all hosts as online -- skip host discovery
-A    Enable OS detection, version detection, script scanning, and traceroute

|Flag|Description|
|---|---|
|`-p`|Specify ports (e.g., `-p 80`, `-p 1-1000`, `-p 22,80,443`)|
|`-iL file`|Input target list from file|
|`-iR num`|Scan random hosts|
|`--exclude`|Exclude hosts|
|`--exclude-file`|Exclude hosts from file|

| Flag                   | Description                       |
| ---------------------- | --------------------------------- |
| `-F`                   | Fast scan (top 100 ports)         |
| `-r`                   | Scan ports in sequential order    |
| `--top-ports <n>`      | Scan most common n ports          |
| `--port-ratio <ratio>` | Scan ports more common than ratio |

| Flag              | Description                                           |
| ----------------- | ----------------------------------------------------- |
| `-O`              | OS detection                                          |
| `-A`              | Aggressive scan (OS + version + scripts + traceroute) |
| `-sC`             | Default script scan                                   |
| `--script=<name>` | Run specific NSE script                               |
| `--script-args`   | Provide script arguments                              |

| Flag            | Description                    |
| --------------- | ------------------------------ |
| `-f`            | Fragment packets               |
| `--mtu <val>`   | Set custom MTU                 |
| `-D`            | Decoy scan                     |
| `-S`            | Spoof source IP                |
| `-e`            | Use specific network interface |
| `--source-port` | Specify source port            |
| `--data-length` | Append random data             |0,443`) |
| `-iL file`       | Input target list from file                                |
| `-iR num`        | Scan random hosts                                          |
| `--exclude`      | Exclude hosts                                              |
| `--exclude-file` | Exclude hosts from file                                    |

|Flag|Description|
|---|---|
|`-oN`|Normal output file|
|`-oX`|XML output|
|`-oG`|Grepable output|
|`-oA`|Output in all major formats|
|`-v`|Increase verbosity|
|`-vv`|Very verbose|
|`-d`|Debug mode|