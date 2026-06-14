# soc-home-lab
Date:         Sunday 14th June 2026. <br>
Analyst:      Adediran Gbolahan Adisa <br>
Lab session:  Day 1 of 30<br>
Phase:        1 — VM Setup and Network Configuration<br>

VM DETAILS<br>
VM name:        Ubuntu-SOC-Lab<br>
OS:             Ubuntu  [26.04 LTS]<br>
Hypervisor:     VirtualBox on Windows 11<br>
RAM assigned:   [4GB]<br>
Storage:        [40GB]<br>

<img width="810" height="689" alt="aaa" src="https://github.com/user-attachments/assets/e47bcfb0-aedb-4b73-80a8-988123edae46" />

NETWORK CONFIGURATION <br>
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

TASKS COMPLETED TODAY<br>
<ul>
    <li> Adapter 1 (NAT) confirmed active — enp0s3</li>
    <li> Adapter 2 (Host-only) enabled and configured — enp0s8</li>
    <li> sudo apt update && sudo apt upgrade -y — completed, no errors</li>
    <li> Snapshot taken — name: clean-base</li>
    <li> Ran: ip a — confirmed both interfaces visible </li>
    </ul>

Commands run:<br>
  ip a<br>
  sudo apt update && sudo apt upgrade -y<br>
  sudo apt install -y curl wget git net-tools ufw vim<br>

ISSUE ENCOUNTERED AND RESOLVED<br>
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

EVIDENCE / SCREENSHOTS
- Screenshot of: ip a output showing enp0s3 and enp0s8
   <img width="791" height="674" alt="day01-network-interfaces" src="https://github.com/user-attachments/assets/5e7c8a22-4106-4716-bd0b-65c0fe33f1df" />

- Screenshot of: VirtualBox Settings showing both adapters
    <img width="804" height="698" alt="day01-vbox-adapter1" src="https://github.com/user-attachments/assets/90dafeda-6bd6-4042-9c38-36f63e4e5f95" />
    <img width="798" height="695" alt="day01-vbox-adapter2" src="https://github.com/user-attachments/assets/04005693-e714-429c-bdfe-73151df25506" />

- Screenshot of: apt upgrade completion (no errors)
    <img width="678" height="548" alt="day01-apt-upgrade png" src="https://github.com/user-attachments/assets/ea93cad0-28cb-4b51-aa8e-909ec79f1aad" />

SNAPSHOT LOG<br>
Snapshot 1:   clean-base<br>
Taken after:  Network config complete, apt upgrade done,
              essentials installed<br>
Purpose:      Clean restore point before any tools installed.
              Roll back here if anything breaks in Week 1.<br>

PACKAGES INSTALLED<br>
curl, wget, git, net-tools, ufw, vim<br>
(installed via: sudo apt install -y curl wget git net-tools ufw vim)<br>

PENDING FOR DAY 2<br>
- Set hostname to: soc-analyst-lab
- Enable and configure UFW firewall
- Create lab folder structure: ~/soc-lab/{logs,captures,reports,scripts}
- Take snapshot: post-hardening

END OF DAY 1 ENTRY
