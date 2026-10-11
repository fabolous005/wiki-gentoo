<!-- source: https://wiki.gentoo.org/wiki/Nmap | group: Gentoo Wiki (Main) | wiki-title: Nmap -->
---
title: Nmap
url: https://wiki.gentoo.org/wiki/Nmap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
fingerprint: c640c91cb4e284ee
license: CC BY-SA 4.0
---

# Nmap

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**


Nmap (**N**etwork **Map**per) is an open-source network exploration and security auditing tool for host discovery, port scanning, service identification, and OS fingerprinting.

Gordon Lyon (Fyodor) wrote Nmap.

## Installation

### USE flags


### USE flags for
            [net-analyzer/nmap](https://packages.gentoo.org/packages/net-analyzer/nmap)
            
            Network exploration tool and security / port scanner

| [+nse](https://packages.gentoo.org/useflags/+nse) | Include support for the Nmap Scripting Engine (NSE) | 
| [libssh2](https://packages.gentoo.org/useflags/libssh2) | Enable SSH support through net-libs/libssh2 | 
| [ncat](https://packages.gentoo.org/useflags/ncat) | Install the ncat utility | 
| [ndiff](https://packages.gentoo.org/useflags/ndiff) | Install the ndiff utility | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [nping](https://packages.gentoo.org/useflags/nping) | Install the nping utility | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [symlink](https://packages.gentoo.org/useflags/symlink) | Install symlink to nc | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [zenmap](https://packages.gentoo.org/useflags/zenmap) | Install the GTK+ based nmap GUI, zenmap | 

### Emerge

Install Nmap with Portage:

`root #``emerge --ask net-analyzer/nmap`
The default USE flags support basic port scanning. Review the available flags to enable optional features.

## Configuration

### Environment variables

| Variable | Purpose | 
|---|---|
| `NMAPDIR` | Adds a directory to the search path for Nmap data files and NSE scripts. | 
| `NMAP_PRIVILEGED` | Makes Nmap assume raw-packet and packet-capture privileges are available; equivalent to --privileged. | 
| `NMAP_UNPRIVILEGED` | Makes Nmap assume raw-packet privileges are unavailable; equivalent to --unprivileged. | 
| `HOME` | Supplies the user's home directory, where Nmap can look for \~/.nmap/. It is a general environment variable, not an Nmap-specific setting. | 

### Files

**Nmap** does not use a single configuration file for ordinary scans.

Command-line options control most scan behavior.

Data files supply service mappings and fingerprint databases.

#### Runtime data files

Runtime data files defaults to the /usr/share/nmap directory.

| File | Purpose | 
|---|---|
| nmap-services | Maps port numbers and transport protocols to service names; also supplies port-frequency data for common-port selection. | 
| nmap-service-probes | Contains probes and response signatures for service and version detection (-sV). | 
| nmap-os-db | Contains TCP/IP fingerprints for OS detection (-O). | 
| nmap-protocols | Maps IP protocol numbers to protocol names. | 
| nmap-rpc | Maps SunRPC program numbers to names. | 
| nmap-mac-prefixes | Maps MAC-address prefixes to vendor names. | 
| nmap-payloads | Contains payload data used by certain UDP and other protocol probes. | 

#### Data file search order

Nmap searches for each data file independently. The main search order is:

- Directory specified by --datadir.
- Directory specified by **`NMAPDIR`** envvar.
- User-specific \~/.nmap/ directory.
- Directory of the Nmap executable (compatibility, debugging)
- Executable directory's ../share/nmap/ directory on applicable Unix platforms. (compatibility)
- Compiled-in data directory based on `NMAPDATADIR` C macro, defaults to /usr/share/nmap

An explicitly selected file, such as one supplied through --servicedb or --versiondb, takes precedence for that database.

## Usage

Nmap scans hosts and reports port states, detected services, and other network characteristics. See the [Nmap reference guide](https://nmap.org/book/man.html) for details.

### Port scanning

The `-p` option selects ports to scan.

Scan TCP port 80:

`user $``nmap -p 80 example.com`
Scan multiple ports:

`user $``nmap -p 80,443,8080 example.com`
Scan common database ports:

`user $``nmap -p 1433,3306,5432 example.com`
Scan a port range:

`user $``nmap -p 1-1000 example.com`
Combine multiple port ranges with commas:

`user $``nmap -p 6660-6670,6690-6700 example.com`
Nmap reports each port's state. The main states are:

- \`open\` — an application accepts connections or packets.
- \`closed\` — the host responds, but no application listens on the port.
- \`filtered\` — packet filtering prevents Nmap from determining the port state.
- \`unfiltered\` — the port is accessible, but Nmap cannot determine whether it is open or closed.
- \`open|filtered\` — Nmap cannot distinguish an open port from a filtered port.
- \`closed|filtered\` — Nmap cannot distinguish a closed port from a filtered port.

### Service detection

The `-sV` option probes open ports to identify services and, when possible, their software versions.

An example of IRC service detection:

`user $``nmap -sV -p 6660-6670,6690-6700 irc.libre.chat`
Starting Nmap 7.991 ( https://nmap.org ) at 2026-10-10 16:06 -0500
Warning: Hostname irc.libre.chat resolves to 4 IPs. Using 91.216.248.20.
Nmap scan report for irc.libre.chat (91.216.248.20)
Host is up (0.13s latency).
Other addresses for irc.libre.chat (not scanned): 2a00:f48:2000:affe::50 91.216.248.21 91.216.248.23
rDNS record for 91.216.248.20: frontend.lima-city.de
PORT     STATE  SERVICE      VERSION
6660/tcp closed unknown
6661/tcp closed unknown
6662/tcp closed radmind
6663/tcp closed unknown
6664/tcp closed unknown
6665/tcp closed irc
6666/tcp closed irc
6667/tcp closed irc
6668/tcp closed irc
6669/tcp closed irc
6670/tcp closed irc
6690/tcp closed cleverdetect
6691/tcp closed unknown
6692/tcp closed unknown
6693/tcp closed unknown
6694/tcp closed unknown
6695/tcp closed unknown
6696/tcp closed babel
6697/tcp closed ircs-u
6698/tcp closed unknown
6699/tcp closed napster
6700/tcp closed carracho
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3.91 seconds


Service detection matches protocol responses against known fingerprints. Results may identify a specific application, a software family, or only a probable service. A port number alone does not identify the running daemon.

### Web services

The `-sV` option identifies HTTP services and may report the server implementation and version.

`user $``nmap -sV -p 80,443 google.com`
Starting Nmap 7.991 ( https://nmap.org ) at 2026-10-10 16:04 -0500
Warning: Hostname google.com resolves to 10 IPs. Using 142.251.116.139.
Nmap scan report for google.com (142.251.116.139)
Host is up (0.0030s latency).
Other addresses for google.com (not scanned): 2607:f8b0:4023:1009::8b 2607:f8b0:4023:1009::71 2607:f8b0:4023:1009::65 2607:f8b0:4023:1009::66 142.251.116.101 142.251.116.102 142.251.116.138 142.251.116.113 142.251.116.100
rDNS record for 142.251.116.139: rt-in-f139.1e100.net
PORT    STATE SERVICE   VERSION
80/tcp  open  http      gws
443/tcp open  ssl/https gws
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.991%I=7%D=10/10%Time=6ACAA861%P=x86\_64-pc-linux-gnu%r(Ge
SF:tRequest,160ED,"HTTP/1\.0\x20200\x20OK\r\nContent-Type:\x20text/html;\x
SF:20charset=ISO-8859-1\r\nDate:\x20Sat,\x2010\x20Oct\x202026\x2021:04:32\
SF:x20GMT\r\nExpires:\x20-1\r\nCache-Control:\x20private,\x20max-age=0\r\n
SF:Content-Security-Policy-Report-Only:\x20object-src\x20'none';base-uri\x
SF:20'self';script-src\x20'nonce-uAdKcA3UjVV1hkR9fFCm4A'\x20'strict-dynami
SF:c'\x20'report-sample'\x20'unsafe-eval'\x20'unsafe-inline'\x20https:\x20
SF:http:;report-uri\x20https://csp\.withgoogle\.com/csp/gws/other-hp\r\nP3
SF:P:\x20CP=\"This\x20is\x20not\x20a\x20P3P\x20policy!\x20See\x20g\.co/p3p
SF:help\x20for\x20more\x20info\.\"\r\nServer:\x20gws\r\nX-XSS-Protection:\
SF:x200\r\nX-Frame-Options:\x20SAMEORIGIN\r\nSet-Cookie:\x20\_\_Secure-STRP=
SF:ABNhA6gXIuhsPdlc-SkKNZ2hiCvaka1oDeN3AtrynFh4Q-WVpJm-3rAQG9YJArZ4ABEzIfT
SF:daoQyfw9JmfTNt0sbcVi\_50-LZlPV;\x20expires=Sat,\x2010-Oct-2026\x2021:09:
SF:32\x20GMT;\x20path=/;\x20domain=\.google\.com;\x20Secure;\x20SameSite=s
SF:trict\r\nSet-Cookie:\x20AEC=Aaa9EJpSXYeDbONDFyyAqGFkJksFwa99vYJBXXpXi-G
SF:V0IfYS5RtvEZ3vw;\x20expires=Thu,\x2008-Apr-2027\x2021:04:32\x20GMT;\x20
SF:path=/;\x20domain=\.google\.com;\x20Secure;\x20HttpO");
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.07 seconds

Service detection does not, by itself, perform OS detection.

### OS fingerprinting

The `-O` option probes TCP/IP behavior to identify the remote operating system.

`root #``nmap -O -v localhost````
Starting Nmap 7.991 ( https://nmap.org ) at 2026-10-10 16:11 -0500
Initiating Parallel DNS resolution of 1 host. at 16:11
Completed Parallel DNS resolution of 1 host. at 16:11, 0.00s elapsed
Warning: Hostname localhost resolves to 2 IPs. Using 127.0.0.1.
Initiating Parallel DNS resolution of 1 host. at 16:11
Completed Parallel DNS resolution of 1 host. at 16:11, 0.00s elapsed
Initiating SYN Stealth Scan at 16:11
Scanning localhost (127.0.0.1) [1000 ports]
Discovered open port 22/tcp on 127.0.0.1
Completed SYN Stealth Scan at 16:11, 0.06s elapsed (1000 total ports)
Initiating OS detection (try #1) against localhost (127.0.0.1)
Retrying OS detection (try #2) against localhost (127.0.0.1)
Retrying OS detection (try #3) against localhost (127.0.0.1)
Retrying OS detection (try #4) against localhost (127.0.0.1)
Retrying OS detection (try #5) against localhost (127.0.0.1)
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000099s latency).
Other addresses for localhost (not scanned): ::1
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.991%E=4%D=10/10%OT=22%CT=1%CU=31678%PV=Y%DS=0%DC=L%G=Y%TM=6ACAA
OS:9F7%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=10D%TI=Z%CI=Z%TS=22)SEQ(S
OS:P=105%GCD=1%ISR=10B%TI=Z%CI=Z%TS=21)SEQ(SP=107%GCD=1%ISR=103%TI=Z%CI=Z%I
OS:I=I%TS=21)SEQ(SP=107%GCD=1%ISR=10B%TI=Z%CI=Z%TS=22)SEQ(SP=109%GCD=1%ISR=
OS:10D%TI=Z%CI=Z%TS=22)OPS(O1=MFFD7ST11NW9%O2=MFFD7ST11NW9%O3=MFFD7NNT11NW9
OS:%O4=MFFD7ST11NW9%O5=MFFD7ST11NW9%O6=MFFD7ST11)WIN(W1=FFCB%W2=FFCB%W3=FFC
OS:B%W4=FFCB%W5=FFCB%W6=FFCB)ECN(R=Y%DF=Y%T=40%W=FFD7%O=MFFD7NNSNW9%CC=Y%Q=
OS:)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W
OS:=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
OS:T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S
OS:+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUC
OS:K=G%RUD=G)IE(R=N)IE(R=Y%DFI=N%T=40%CD=S)
Uptime guess: 0.000 days (since Sat Oct 10 16:11:16 2026)
Network Distance: 0 hops
TCP Sequence Prediction: Difficulty=263 (Good luck!)
IP ID Sequence Generation: All zeros
Read data files from: /usr/bin/../share/nmap
OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.19 seconds
           Raw packets sent: 1134 (56.802KB) | Rcvd: 2259 (106.768KB)
```
OS detection depends on available responses and matching fingerprints. Results may be approximate or inconclusive.

Combine OS and service detection with both options:

`root #``nmap -O -sV example.com`
Starting Nmap 7.991 ( https://nmap.org ) at 2026-10-10 16:14 -0500
Warning: Hostname example.com resolves to 4 IPs. Using 104.20.23.154.
Nmap scan report for example.com (104.20.23.154)
Host is up (0.0049s latency).
Other addresses for example.com (not scanned): 2606:4700:10::6814:179a 2606:4700:10::ac42:93f3 172.66.147.243
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          cloudflare
443/tcp  open  ssl/https     cloudflare
8080/tcp open  http-proxy
8443/tcp open  ssl/https-alt cloudflare
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port80-TCP:V=7.991%I=7%D=10/10%Time=6ACAAABC%P=x86\_64-pc-linux-gnu%r(Ge
SF:tRequest,166,"HTTP/1\.1\x20403\x20Forbidden\r\nDate:\x20Sat,\x2010\x20O
SF:ct\x202026\x2021:14:36\x20GMT\r\nContent-Length:\x2057\r\nConnection:\x
SF:20close\r\nCache-Control:\x20private,\x20max-age=0,\x20no-store,\x20no-
SF:cache,\x20must-revalidate,\x20post-check=0,\x20pre-check=0\r\nReferrer-
SF:Policy:\x20same-origin\r\nExpires:\x20Thu,\x2001\x20Jan\x201970\x2000:0
SF:0:01\x20GMT\r\nCF-RAY:\x20a488a2bb4a471234-DFW\r\n\r\nCloudflare\x20enc
SF:ountered\x20an\x20error\x20processing\x20this\x20request:\x20");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port8080-TCP:V=7.991%I=7%D=10/10%Time=6ACAAABC%P=x86\_64-pc-linux-gnu%r(
SF:GetRequest,166,"HTTP/1\.1\x20403\x20Forbidden\r\nDate:\x20Sat,\x2010\x2
SF:0Oct\x202026\x2021:14:36\x20GMT\r\nContent-Length:\x2057\r\nConnection:
SF:\x20close\r\nCache-Control:\x20private,\x20max-age=0,\x20no-store,\x20n
SF:o-cache,\x20must-revalidate,\x20post-check=0,\x20pre-check=0\r\nReferre
SF:r-Policy:\x20same-origin\r\nExpires:\x20Thu,\x2001\x20Jan\x201970\x2000
SF::00:01\x20GMT\r\nCF-RAY:\x20a488a2bb49c9eb02-DFW\r\n\r\nCloudflare\x20e
SF:ncountered\x20an\x20error\x20processing\x20this\x20request:\x20");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 45.61 seconds

### Invocation

`user $``nmap --help````
Nmap 7.95 ( https://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN, TCP ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <port ranges>: Exclude the specified ports from scanning
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports sequentially - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma-separated list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
  --data <hex string>: Append a custom payload to sent packets
  --data-string <string>: Append a custom ASCII string to sent packets
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --noninteractive: Disable runtime interactions via keyboard
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE MAN PAGE (https://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES
wolfe@mensa:~/.local$ 
```
## Easter eggs

The `-oS` option selects Nmap's deliberately stylized output format.

`user $``nmap -oS - example.com`
The hyphen directs output to standard output. The stylized format is intended for amusement, not machine parsing.

## Troubleshooting

### My custom script doesn't fire

I inserted a custom-made script into /usr/share/nmap/scripts directory, but nmap won't pick it up.

Workaround: scripts/script.db is an index of script names and categories, not a library or executable script. Use nmap --script-updatedb to regenerate it after adding or removing scripts

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-analyzer/nmap`
## See also

- [Wireshark](https://wiki.gentoo.org/wiki/Wireshark) — a free and open-source packet analyzer.
