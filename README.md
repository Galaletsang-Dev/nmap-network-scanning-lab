# Nmap Network Scanning Lab

## Overview

This project demonstrates a basic network security assessment using Nmap in an isolated VirtualBox lab environment.

I configured two virtual machines on the same Host-Only network:

- **Lubuntu** – Nmap scanning machine
- **Metasploitable 2** – Target machine

The objective was to establish connectivity between the two systems, identify open TCP ports, enumerate running services and service versions, and analyse the security significance of the findings.

![Nmap Installation and Version Verification](Nmap%20Installation%20and%20Version%20Verification.png)

## Lab Environment

| Component | Configuration |
|---|---|
| Virtualization | Oracle VirtualBox |
| Scanner | Lubuntu Linux |
| Target | Metasploitable 2 |
| Security Tool | Nmap 7.98 |
| Network Type | Host-Only Adapter |
| Scanner IP | 192.168.56.103 |
| Target IP | 192.168.56.101 |

## Lab Architecture

Lubuntu (192.168.56.103)
        |
        | Nmap Scan
        v
Metasploitable 2 (192.168.56.101)

Both virtual machines were placed on an isolated VirtualBox Host-Only network.

## 1. Connectivity Testing

Before performing the security scan, I verified that the Lubuntu machine could communicate with the target.

```bash
ping -c 4 192.168.56.101
```

The test returned:

* 4 packets transmitted
* 4 packets received
* 0% packet loss

This confirmed successful communication between the two virtual machines.

![Successful Connectivity Test](https://github.com/Galaletsang-Dev/nmap-network-scanning-lab/blob/main/Successful%20Connectivity%20Test.png?raw=true)

## 2. Basic Port Scan

I performed an initial Nmap scan against the Metasploitable target:

```bash
nmap 192.168.56.101
```

The scan identified **23 open TCP ports**, while 977 of the default 1,000 scanned TCP ports were closed.

Some of the discovered services included:

| Port | Service    |
| ---- | ---------- |
| 21   | FTP        |
| 22   | SSH        |
| 23   | Telnet     |
| 25   | SMTP       |
| 53   | DNS        |
| 80   | HTTP       |
| 139  | NetBIOS    |
| 445  | SMB        |
| 3306 | MySQL      |
| 5432 | PostgreSQL |
| 5900 | VNC        |
| 6667 | IRC        |
| 8009 | AJP13      |
| 8180 | HTTP       |

![Basic Nmap Scan Results](Basic%20Nmap%20Scan%20Results.png)

## 3. Service Version Detection

I then performed service and version detection:

```bash
sudo nmap -sV 192.168.56.101
```

This provided additional information about the software running behind the open ports.

Some notable findings included:

* **vsftpd 2.3.4** on port 21
* **OpenSSH 4.7p1** on port 22
* **Apache HTTP Server 2.2.8** on port 80
* **Samba** on ports 139 and 445
* **Metasploitable root shell** on port 1524
* **MySQL** on port 3306
* **PostgreSQL** on port 5432
* **VNC** on port 5900
* **UnrealIRCd** on port 6667
* **Apache Tomcat** on port 8180

* ![Nmap Service and Version Detection Results](Nmap%20Service%20and%20Version%20Detection%20Results%20(...).png)

## 4. Security Analysis

The scan demonstrated how exposed network services increase a system's attack surface.

For example, Telnet provides unencrypted remote communication, while database services such as MySQL and PostgreSQL may expose sensitive resources if they are incorrectly configured.

The presence of a root shell on port 1524 was particularly significant because a root-level shell represents highly privileged system access.

Metasploitable is intentionally vulnerable and was used only as a controlled target within the isolated lab environment.

![Two Virtual Machines and IP Address Configuration](Two%20Virtual%20Machines%20and%20IP%20Address%20Configuration.png)

## What I Learned

Through this lab, I gained practical experience with:

* Configuring VirtualBox Host-Only networking
* Establishing communication between Linux virtual machines
* Using Nmap for host and port discovery
* Performing service and version enumeration
* Interpreting Nmap scan results
* Identifying exposed network services
* Understanding attack surface and service exposure
* Documenting security assessment findings

## Disclaimer

This project was conducted in a controlled and isolated lab environment using systems that I own and an intentionally vulnerable Metasploitable virtual machine. The techniques demonstrated here are for educational and authorised security testing purposes only.
