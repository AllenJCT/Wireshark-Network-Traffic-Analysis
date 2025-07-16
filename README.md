# Wireshark Network Traffic Analysis

This project demonstrates how to use Wireshark to analyze various types of network traffic in a lab environment running on a VirtualBox Ubuntu VM. The goal is to simulate real-world scenarios for blue team training and threat detection.

## Objective

To gain hands-on experience capturing and analyzing common protocols, observing network behavior, and detecting signs of scans, service use, and system communication.

---

## 🔧 Lab Setup

- **Host OS:** Windows  
- **Virtualization Tool:** VirtualBox  
- **Guest OS:** Ubuntu (with GUI)  
- **Wireshark**  

  ---

  # **STEP 1: INSTALL WIRESHARK ON UBUNTU VM**

- ''bash
- suduo apt update
- sudo apt install wireshark -y
- **wait for install**
- click **yes** when prompted
- sudo usermod -aG $USER
-   *this gives you root administrator access*

  ![Wireshark Install](./Wireshark%20Install.png)

---

# **STEP 2: START CAPTURING TRAFFIC**

- Open Wireshark
  *Open terminal and type in "Wireshark"*
- Select your primary network interface (should be enp0s3)
- Click on the blue "Shark fin" to start capturing network traffic on your Virtual Machine

(Wireshark Post-Installation.png)


---

# **STEP 3: SIMULATE REAL-WORLD TRAFFIC**

- **While the capture is running, open the terminal and run the following code**

- curl http://neverssl.com
    *This will generate http web traffic*

- **Open another window in your terminal**

- nslookup google.com
    *This will generate DNS Traffic

- **Open another window in your terminal**

- ping -c 4 8.8.8.8
    *This will ping external server*

- **Open another window in your terminal**

- sudo apt install ftp -y
- ftp dlptest.com
    *Simulate FTP Traffic*
- Login with a random UserName and Password
  *This demostrates that FTP Traffic can expose Cleartext passwords*

**Each of these creates unique packet types you can analyze in Wireshark**

(Web traffic.PNG)
(ICMP Ping.PNG)
(FTP Install.PNG)

---


# **STEP 4: ANALYZE TRAFFIC IN WIRESHARK**

**Lets now filter the packets that we just generated on Wireshark**

- **Filter:** DNS
    *Check what domains are being resolved — this could help catch command-and-control traffic in real-world cases*
- **Filter:** HTTP
    *Look at headers, plaintext URLs, and sometimes even passwords in the payload*
- **Filter:** FTP
   *You should see:*
    USER (Username you put)
    PASS (Password you created)
  *This shows you how unencrypted FTP can leak credentials*
- **Filter:** ICMP (Ping)
    *Useful for detecting ping sweeps or DoS attacks*
- **Filter:** tcp.flags.syn == 1 && tcp.flags.ack == 0
    *Shows you all SYN (connection start) packets — useful for spotting scans or malware beaconing out*

(Filtering DNS Traffic.PNG)
(Filtering HTTP Traffic.PNG)
(Filtering FTP Traffic.PNG)
(Filtering ICMP Traffic.PNG)
(TCP SYN Scan Detection.PNG)

  ---

  # **STEP 5: SAVE YOUR PCAP**

  - **Make sure that you 'stop' the live traffic capture**
  - Go to File > Save As
