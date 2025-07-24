# 🧭 Mastering Nmap: The Complete Journey

<aside>
This roadmap guides you through learning Nmap from basics to advanced techniques, with practical exercises and resources at each stage.

</aside>

# 📊 Learning Path Overview

- [ ]  Beginner: Fundamentals & Basic Scanning
- [ ]  Intermediate: Advanced Scanning & NSE
- [ ]  Advanced: Stealth, Evasion & Automation
- [ ]  Expert: Integration & Ecosystem

# ⏱️ Quick Reference

```bash
# Basic scan
nmap 192.168.1.1

# Aggressive scan with OS/version detection
nmap -A 192.168.1.0/24

# Stealth SYN scan (requires root)
sudo nmap -sS 10.0.0.1

# Service version detection
nmap -sV -p 1-1000 target.com
```

# 📚 Phase 1: Fundamentals & Setup (Est. time: 1-2 weeks)

### 🔰 Getting Started

- [x]  Install Nmap and Zenmap on your system
- [x]  Read the official Nmap Book: [Nmap Network Scanning](https://nmap.org/book/)
- [ ]  TryHackMe: [Nmap Room](https://tryhackme.com/room/introtonmap)

- [ ]  Hack The Box Academy: [Network Enumeration with Nmap](https://academy.hackthebox.com/)
- [x]  Build a virtual lab using VirtualBox, EVE-NG, or Proxmox

<aside>
✅ Beginner Challenge: Install Nmap and scan your own network with different verbosity levels (-v, -vv)

</aside>

# 🔍 Phase 2: Scanning & Enumeration (Est. time: 2-3 weeks)

| **Technique** | **Command** | **Use Case** |
| --- | --- | --- |
| Host Discovery | `-sn`,`-Pn`,`-PR` | Identify live hosts without port scanning |
| Port Scanning | `-sS`,`-sT`,`-sU` | Discover open TCP/UDP ports |
| Service Detection | `-sV`,`-A` | Identify running services and versions |
| OS Fingerprinting | `-O`,`--osscan-guess` | Determine target operating system |
| Timing Templates | `-T0`to`-T5` | Control scan timing/aggressiveness |

### 🛠️ Practical Exercises

- [ ]  Practice Host Discovery: `nmap -sn 192.168.1.0/24`
- [ ]  Port Scanning: Compare TCP SYN vs Connect scans
- [ ]  Service & Version Detection: `nmap -sV -p 1-1000 target`
- [ ]  OS Fingerprinting: `sudo nmap -O target`
- [ ]  Customize Timing & Performance: Test different timing templates

<aside>
📝 Pro Tip: Always start with lighter scans and progressively move to more aggressive techniques.

</aside>

# 🎯 Phase 3: Stealth & Evasion Techniques (Est. time: 2-3 weeks)

### 🕵️ Advanced Scanning Techniques

### Stealthy Scans

- [ ]  Null Scan: `sudo nmap -sN target`
- [ ]  FIN Scan: `sudo nmap -sF target`
- [ ]  Xmas Scan: `sudo nmap -sX target`

### Evasion Methods

- [ ]  MAC Spoofing: `--spoof-mac`
- [ ]  Decoy Scanning: `-D decoy1,decoy2,ME`
- [ ]  Fragmentation: `-f` or `-ff`

### 🔬 Analysis & Testing

- [ ]  Analyze packets: `--packet-trace`, use Wireshark alongside
- [ ]  Test IDS/IPS evasion with `--badsum` and `--data-length`
- [ ]  Compare detection rates between stealth and regular scans

```bash
# Example: Scanning through a firewall with decoys and fragmentation
sudo nmap -sS -D 10.0.0.1,10.0.0.2,ME -f -v -n -Pn -p 80,443,8080 target.com

```

<aside>
⚠️ Warning: These techniques may be detected by modern security systems. Use responsibly and only on systems you have permission to scan.

</aside>

# ⚙️ Phase 4: NSE & Script Automation (Est. time: 3-4 weeks)

### 🧩 Nmap Scripting Engine (NSE)

NSE Categories:

- **auth**: Authentication related scripts
- **broadcast**: Network discovery via broadcast
- **brute**: Brute force attacks
- **default**: Default scripts (-sC)

- **discovery**: Host/service discovery
- **dos**: Denial of Service tests
- **exploit**: Exploit vulnerable services
- **vuln**: Check for specific vulnerabilities

### 🔧 Script Exercises

- [ ]  Run basic script scan: `nmap -sC target`
- [ ]  Try vulnerability scanning: `nmap --script vuln target`
- [ ]  Test specific scripts: `nmap --script smb-vuln-ms17-010 target`
- [ ]  Use script arguments: `--script-args` for customization
- [ ]  Update NSE database: `nmap --script-updatedb`

### 🐍 Python Integration

- [ ]  Install python-nmap: `pip install python-nmap`
- [ ]  Create basic automation script for recurring scans

```python
# Simple Python automation with python-nmap
import nmap

scanner = nmap.PortScanner()
targets = ['192.168.1.1', '192.168.1.2']

for host in targets:
    print(f"Scanning {host}...")
    scanner.scan(host, '22-443', arguments='-sV')
    
    for port in scanner[host]['tcp']:
        service = scanner[host]['tcp'][port]
        print(f"Port {port}: {service['name']} {service['version']}")

```

# 🤖 Phase 5: Integration & Automation (Est. time: 3-4 weeks)

### 🔄 CI/CD Integration

- [ ]  Create a GitHub Action workflow for scheduled scans
- [ ]  Set up GitLab CI pipeline for security testing

- [ ]  Use Dockerized Nmap in containers
- [ ]  Implement scan result difference detection

```yaml
# Example GitHub Actions workflow
name: Security Scan

on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly on Sundays
  workflow_dispatch:

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - name: Install Nmap
        run: sudo apt-get update && sudo apt-get install -y nmap
        
      - name: Run Scan
        run: nmap -sV -oX scan_results.xml example.com
        
      - name: Upload Results
        uses: actions/upload-artifact@v2
        with:
          name: scan-results
          path: scan_results.xml

```

### 📊 Output Processing

- [ ]  Parse XML output in scripts for reporting
- [ ]  Integrate with security dashboards (Elastic, Grafana)
- [ ]  Create custom alerting based on scan results

### 🔺 Adversary Emulation

- [ ]  Use [MITRE CALDERA](https://github.com/mitre/caldera) for adversary emulation
- [ ]  Test with [Atomic Red Team T1046](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1046/T1046.md)
- [ ]  Compare attack vs. defense perspectives

# 🧩 Phase 6: Nmap Ecosystem & Complementary Tools (Est. time: 2-3 weeks)

### 🛠️ Extended Toolkit

| **Tool** | **Purpose** | **Integration with Nmap** |
| --- | --- | --- |
| [Masscan](https://github.com/robertdavidgraham/masscan) | Fast port scanning at internet scale | Use for initial discovery, then Nmap for detailed analysis |
| [OWASP Amass](https://github.com/owasp-amass/amass) | Network mapping & external asset discovery | Feed Amass results to Nmap for deeper inspection |
| [EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness) | Website screenshots & analysis | Pipe Nmap web service results to EyeWitness |
| Nikto | Web server scanner | Use command:`nmap -p 80 -oG - | nikto -h -` |
- [ ]  Create a workflow combining multiple tools for comprehensive assessment
- [ ]  Build a custom Docker image with all tools pre-installed
- [ ]  Test integration scripts for automated reporting

```bash
# Example integration pipeline
#!/bin/bash
TARGET=$1

echo "Starting reconnaissance on $TARGET"
echo "1. Running Amass..."
amass enum -d $TARGET -o amass_results.txt

echo "2. Initial port discovery with Masscan..."
sudo masscan -p1-65535 --rate=1000 $(dig +short $TARGET) -oL masscan_results.txt

echo "3. Detailed scanning with Nmap..."
ports=$(cat masscan_results.txt | grep open | cut -d " " -f 3 | tr '\n' ',' | sed 's/,$//')
nmap -sV -sC -p$ports $TARGET -oX nmap_results.xml

echo "4. Web service analysis..."
cat nmap_results.xml | grep "service name=\"http" | grep -oP 'portid="\K[0-9]+' > http_ports.txt
for port in $(cat http_ports.txt); do
  echo "Running EyeWitness on port $port..."
  eyewitness --web --single http://$TARGET:$port -d eyewitness_$port
done

echo "Scan complete! Check reports for details."

```

# 📚 Resources & Community

### Essential Resources

- **📖 Documentation**: [Nmap Book](https://nmap.org/book/) (comprehensive guide)
- **🌐 Community**: [Nmap Hackers Mailing List](https://seclists.org/nmap-hackers/)
- **📝 Cheat Sheet**: [StationX Nmap Cheat Sheet](https://www.stationx.net/nmap-cheat-sheet/)

### Practice Labs

- **🏆 Platforms**: [HackTheBox](https://www.hackthebox.com/), [TryHackMe](https://tryhackme.com/), [VulnHub](https://www.vulnhub.com/)
- **🎓 Courses**: [HackTheBox Nmap Guide](https://www.hackthebox.com/blog/mastering-nmap)
- **🧪 CTFs**: Look for CTFs with network enumeration challenges

### Certification Alignment

Skills in this roadmap align with:

- OSCP (Offensive Security Certified Professional)
- PNPT (Practical Network Penetration Tester)
- eJPT (eLearnSecurity Junior Penetration Tester)

# 🏁 Mastery Milestones

<aside>
Track your progress through these key achievements:

</aside>

### 🔰 Beginner

- [ ]  Successfully scan and enumerate your lab network
- [ ]  Identify all running services on target hosts
- [ ]  Create basic scan automation scripts

### 🥋 Intermediate

- [ ]  Evade basic network security controls
- [ ]  Create custom NSE scripts for specific tasks
- [ ]  Build multi-tool scanning workflows

### 👑 Advanced

- [ ]  Implement enterprise-grade scan automation
- [ ]  Contribute to Nmap project or NSE scripts
- [ ]  Teach others through presentations/blogs

> Remember: The best network mappers are those who understand both the technical details and the strategic context of their scans. Always scan responsibly and with proper authorization.
>
