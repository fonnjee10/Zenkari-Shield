# Zenkari-Shield
Zenkari Shield v7.0 —/ Antivirus Service
Zenkari Shield is a fully-featured, open-source endpoint security suite built entirely in Python. It combines real antivirus scanning, behavioral analysis, AES-256-GCM encryption, VPN management, network monitoring, and forensic logging into a single desktop application with a polished dark-mode UI.

What it does
Zenkari Shield covers every layer of endpoint protection that professional security tools offer — without subscriptions, telemetry, or cloud dependencies. Everything runs locally on your machine.
Antivirus Engine
Integrates directly with ClamAV via Unix socket or subprocess, with full signature-based detection and real-time database updates via freshclam. When ClamAV is not installed, a built-in heuristic engine scores files based on extension, PE magic bytes, executable bit, and name patterns.
Real-Time Protection
A watchdog filesystem observer monitors your Downloads, Desktop, and Temp folders. Every file created or modified is automatically scanned. Threats are logged, quarantined, and surfaced with a toast notification — all without user intervention.
Behavioral Analysis & Ransomware Shield
A sliding-window counter tracks filesystem write events per second across all monitored processes. If more than 15 files are modified within 8 seconds — the signature of ransomware encryption activity — the offending process is automatically killed via psutil and a critical alert is raised.
Real Quarantine (AES-256-GCM)
Files are encrypted with AES-256-GCM using a PBKDF2-HMAC-SHA256 derived key (200,000 iterations, 32-byte random salt). The original file is overwritten in 3 passes (0x00, 0xFF, random) before deletion. Quarantine records are stored in SQLite with SHA-256 hashes for integrity verification. Restoration includes optional re-scan with ClamAV before the file is returned.
VPN Manager
Supports WireGuard (via wireguard.exe /installtunnelservice on Windows, wg-quick on Linux) and OpenVPN. Auto-detects installed tunnels by querying wg show interfaces and the Windows service registry. Includes an integrated .conf file editor with syntax highlighting, save/load, and a new-file template. A network Kill Switch blocks all traffic outside the VPN interface using netsh advfirewall (Windows) or iptables (Linux).
DNS over HTTPS
Resolves DNS queries via HTTPS using Cloudflare (1.1.1.1), Google (8.8.8.8), Quad9, or AdGuard. Includes a latency benchmark across all four providers and a DNS leak test to verify that queries are not escaping the VPN tunnel.
System Integrity (Tamper Protection)
Builds a SHA-256 baseline of system directories (/bin, /usr/bin, System32) and performs periodic verification. Any modified, deleted, or timestamp-changed binary triggers a CRITICAL alert and a forensic log entry.
Process Manager
Full process control: list all running processes with CPU, RAM, thread count, and open connection metrics. Kill (SIGKILL), terminate (SIGTERM), suspend, resume, and change priority — all from the UI. Each process receives a risk score out of 100 based on name, location, I/O rate, network connections, and suspicious port usage.
Startup Manager
Reads Windows startup entries from HKCU/HKLM Run and RunOnce registry keys, scheduled tasks via schtasks, and Linux XDG autostart .desktop files. Entries can be disabled directly from the UI.
Firewall Manager
Adds, removes, and toggles Windows Firewall rules via netsh advfirewall. Block a program or an IP address in one click. Reads the active firewall profile and lists existing rules.
Forensic Logging
Every event — file scan, process alert, C2 connection, quarantine action, integrity change, encryption operation — is recorded in a local SQLite database with timestamp, severity, PID, process name, and file path. The log supports filtering by event type and full-text search. Export to JSON for external analysis.
AES-256 File Encryption
Standalone file encryption and decryption using AES-256-GCM with PBKDF2-SHA256 key derivation. Encrypted files use the .zenk extension. Includes SHA-256, SHA-512, MD5, BLAKE2b hash computation and HMAC-SHA256 generation.
Network Scanner
Real-time connection table with C2/botnet detection against a blacklist of known malicious IPs. Multi-threaded port scanner (up to 500 threads). ICMP ping with raw socket and TCP fallback. All connections are checked against a list of suspicious ports.
15 Languages
Full interface localization: French, English, Spanish, German, Italian, Portuguese, Russian, Chinese, Japanese, Arabic, Polish, Dutch, Turkish, Korean, Romanian.

Technical details

Language: Python 3.8+
UI: tkinter + ttk (no external UI framework)
Encryption: cryptography library (AES-256-GCM, PBKDF2-HMAC-SHA256, Scrypt)
Process monitoring: psutil
Filesystem watching: watchdog
Database: SQLite3 (forensic log + quarantine index)
Antivirus: ClamAV (optional) + heuristic fallback
VPN: WireGuard + OpenVPN (system binaries)
Lines of code: ~6,000
Classes: 21
Methods: 316
Dependencies: pip install psutil cryptography watchdog
ClamAV (optional): sudo apt install clamav or the Windows installer
WireGuard (optional): wireguard.com/install
Platforms: Windows 10/11, Linux, macOS

2026 Zenkari Technonology Copyright
