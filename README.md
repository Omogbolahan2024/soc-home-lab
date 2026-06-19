# soc-home-lab
Date:         Sunday 14th June 2026. <br>
Analyst:      Adediran Gbolahan Adisa <br>
Lab session:  Day 1 of 30<br>
Phase:        1 — VM Setup and Network Configuration<br>

<b>VM DETAILS</b><br>
VM name:        Ubuntu-SOC-Lab<br>
OS:             Ubuntu  [26.04 LTS]<br>
Hypervisor:     VirtualBox on Windows 11<br>
RAM assigned:   [4GB]<br>
Storage:        [40GB]<br>

<img width="810" height="689" alt="aaa" src="https://github.com/user-attachments/assets/e47bcfb0-aedb-4b73-80a8-988123edae46" />

<b>NETWORK CONFIGURATION</b> <br>
Adapter 1:      NAT (enp0s3)<br>
Purpose:        Internet access — downloading tools,
                apt updates, Wazuh installer, Suricata<br>
IP:             10.x.x.x (assigned automatically by VirtualBox NAT)<br>

Adapter 2:      Host-only (enp0s8)<br>
Purpose:        Isolated lab traffic between VMs only —
                no internet access on this adapter<br>
Subnet:         192.168.56.101/24<br>
Ubuntu SOC IP:  192.168.56.255<br>
DHCP:           Enabled via VirtualBox Host Network Manager<br>

<b>TASKS COMPLETED TODAY</b><br>
<ul>
    <li> Adapter 1 (NAT) confirmed active — enp0s3</li>
    <li> Adapter 2 (Host-only) enabled and configured — enp0s8</li>
    <li> sudo apt update && sudo apt upgrade -y — completed, no errors</li>
    <li> Snapshot taken — name: clean-base</li>
    <li> Ran: ip a — confirmed both interfaces visible </li>
    </ul>

<b>COMMANDS RUN</b>:<br>
  ip a<br>
  sudo apt update && sudo apt upgrade -y<br>
  sudo apt install -y curl wget git net-tools ufw vim<br>

<b>ISSUE ENCOUNTERED AND RESOLVED</b><br>
Issue:      Adapter 2 was greyed out and could not be clicked
            in VirtualBox Network Settings.<br>
Root cause: The VM was still running. VirtualBox locks network
            adapter settings while a VM is powered on.<br>
Resolution: I performed a full shutdown of the Ubuntu VM from
            within the OS (not just closing the VirtualBox window).
            After confirming VM status showed "Powered Off" in
            VirtualBox Manager, Adapter 2 became fully clickable.
            I Enabled it, set to Host-only Adapter, applied settings,
            then rebooted. Both adapters confirmed working via ip a.<br>
Lesson:     Always fully shut down a VM before changing hardware
            settings in VirtualBox. Suspend/save state is not enough.<br>

<b>EVIDENCE / SCREENSHOTS</b><br>
- Screenshot of: ip a output showing enp0s3 and enp0s8
   <img width="791" height="674" alt="day01-network-interfaces" src="https://github.com/user-attachments/assets/5e7c8a22-4106-4716-bd0b-65c0fe33f1df" />

- Screenshot of: VirtualBox Settings showing both adapters
    <img width="804" height="698" alt="day01-vbox-adapter1" src="https://github.com/user-attachments/assets/90dafeda-6bd6-4042-9c38-36f63e4e5f95" />
    <img width="798" height="695" alt="day01-vbox-adapter2" src="https://github.com/user-attachments/assets/04005693-e714-429c-bdfe-73151df25506" />

- Screenshot of: apt upgrade completion (no errors)
    <img width="678" height="548" alt="day01-apt-upgrade png" src="https://github.com/user-attachments/assets/ea93cad0-28cb-4b51-aa8e-909ec79f1aad" />

<b>SNAPSHOT LOG</b><br>
Snapshot 1:   clean-base<br>
Taken after:  Network config complete, apt upgrade done,
              essentials installed<br>
Purpose:      Clean restore point before any tools installed.
              Roll back here if anything breaks in Week 1.<br>

<b>PACKAGES INSTALLED</b><br>
curl, wget, git, net-tools, ufw, vim<br>
(installed via: sudo apt install -y curl wget git net-tools ufw vim)<br>

<b>PENDING FOR DAY 2</b><br>
- Set hostname to: soc-analyst-lab
- Enable and configure UFW firewall
- Create lab folder structure: ~/soc-lab/{logs,captures,reports,scripts}
- Take snapshot: post-hardening<br>

<b>DAY 2 DOCUMENTATION ENTRY</b> <br>
Date:         18th June 2026 <br>
Analyst:      Adediran Gbolahan Adisa <br>
Lab session:  Day 2 of 30 <br>
Phase:        1 — VM Hardening and Lab Structure

<b>TASKS COMPLETED TODAY</b> <br>
<ul>
    <li>Hostname set to: soc-analyst-lab</li>
    <li>UFW firewall enabled</li>
    <li>SSH port 22 allowed through UFW</li>
    <li>OpenSSH server installed and running</li>
    <li>Lab folder structure created: ~/soc-lab/</li>
    <li>Snapshot taken: post-hardening</li>
</ul>
<b>COMMANDS RUN TODAY</b><br>

# Set hostname
sudo hostnamectl set-hostname soc-analyst-lab

# Verify hostname applied
hostnamectl

# Enable firewall
sudo ufw enable

# Allow SSH through firewall
sudo ufw allow 22/tcp

# Confirm firewall rules
sudo ufw status verbose

# Install OpenSSH server
sudo apt install openssh-server -y

# Confirm SSH service is running
sudo systemctl status ssh

# Create lab folder structure
mkdir -p ~/soc-lab/{logs,captures,reports,scripts}

# Verify folders were created
ls ~/soc-lab/

<b>SYSTEM STATE AFTER TODAY</b><br>
- **Hostname:** soc-analyst-lab
- **UFW status:** Active
- **UFW rules active:** 22/tcp (SSH) — ALLOW IN
- **SSH service:** active (running)

<b>Lab folder structure:</b><br>
- **~/soc-lab/**
  - **logs/** — for auth.log and syslog copies
  - **captures/** — for Wireshark .pcapng files
  - **reports/** — for Nmap output and incident reports
  - **scripts/** — for automation scripts later

<b>ISSUE ENCOUNTERED AND RESOLVED</b></br>
Issue:      Received an apt error when trying to install
            OpenSSH server — package was not found.</br>
Root cause: Typographical error in the install command.
            Misspelled 'openssh-server' when typing the
            command manually into the terminal.</br>
Command typed (wrong):
            sudo apt install [misspelled version]
Resolution: After reading the error message carefully — apt reported
            that the package could not be found. The
            command was retyped with the correct spelling:
            sudo apt install openssh-server -y
            Installation completed successfully on second attempt.
Lesson:     Always read the full error message before assuming
            something is broken. In this case the error was a
            spelling mistake, not a system problem. When apt says
            "unable to locate package", check spelling first.
            Use TAB autocomplete after typing the first few letters
            of a package name to avoid this in future:
            sudo apt install opens[TAB]

<b>EVIDENCE / SCREENSHOTS TO SAVE</b><br>
[ ] Screenshot of: hostnamectl output showing soc-analyst-lab
   <img width="786" height="674" alt="day02-hostname png" src="https://github.com/user-attachments/assets/3e4923cd-0e19-4561-a039-54ea09d7111e" />

[ ] Screenshot of: sudo ufw status verbose
  <img width="792" height="712" alt="day02-ufw-rules png" src="https://github.com/user-attachments/assets/4d297d7e-8eee-44e6-b8ae-d8a2a9a6fdb4" />

[ ] Screenshot of: sudo systemctl status ssh (green active)
<img width="789" height="677" alt="day02-ssh-running" src="https://github.com/user-attachments/assets/4e2a6705-6461-4af9-80cd-718604d56c08" />

[ ] Screenshot of: ls ~/soc-lab/ showing all four folders
  <img width="797" height="688" alt="day02-folder-structure" src="https://github.com/user-attachments/assets/c6ac049f-ddad-406e-8bf3-aa045526131a" />

<b>SNAPSHOT LOG</b><br>
Snapshot 1:   clean-base         (taken Day 1)<br>
Snapshot 2:   post-hardening     (taken today)

Taken after:  Hostname set, UFW configured, SSH installed,<br>
              folder structure created.
Purpose:      Restore point after initial hardening.<br>
              Roll back here if any tool installation in<br>
              Week 1 causes system instability.<br>

<b>SECURITY POSTURE AFTER DAY 2</b><br>
- Firewall:   UFW active, default deny incoming,
              only port 22 explicitly allowed<br>
- SSH:        Installed and running — needed for
              remote access and log testing later<br>
- Hostname:   Clearly identifies machine in logs
              (soc-analyst-lab will appear in syslog,
              auth.log, and Wazuh agent name)<br>
- Folders:    Organised structure ready to receive
              evidence files from Day 3 onward

<b>PENDING FOR DAY 3</b><br>
- Download Metasploitable 2 from SourceForge<br>
- Import into VirtualBox as second VM<br>
- Set Metasploitable to Host-only adapter ONLY<br>
- Boot and confirm it gets a 192.168.56.x IP<br>
- Ping test: Ubuntu → Metasploitable<br>
- Document both IPs in lab network diagram<br>
