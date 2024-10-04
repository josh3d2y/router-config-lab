# Router Configuration Lab

## Objective
In this lab, we're learning how to connect two PCs using switches and a router. The router allows two different networks to talk to each other. We’ll assign IP addresses to the PCs and router interfaces and then test if the PCs can communicate with each other.

## Devices Used:
- **PC0**: 1 computer (connected to Switch0)
- **PC1**: 1 computer (connected to Switch1)
- **Switch0**: Connected to PC0 and Router (interface 0/0/0)
- **Switch1**: Connected to PC1 and Router (interface 0/0/1)
- **Router (ISR4331)**: Used to connect both switches and PCs

### IP Addresses:
- **PC0**: 192.168.1.2 / 255.255.255.0
- **PC1**: 192.168.2.2 / 255.255.255.0
- **Router GigabitEthernet0/0/0** (connected to PC0): 192.168.1.1 / 255.255.255.0
- **Router GigabitEthernet0/0/1** (connected to PC1): 192.168.2.1 / 255.255.255.0

## Problem We Encountered:
At first, we tried to give both router interfaces IP addresses from the **same network** (192.168.1.x). But this caused a problem called **IP overlap**, where the router couldn't tell which network the devices were on.

### Explanation of the Problem:
Routers connect **different networks** together. Each interface on the router (like 0/0/0 and 0/0/1) must belong to **separate networks**. If both interfaces have IP addresses from the same network, the router gets confused, and devices won’t be able to communicate across the router.

**Example of What Went Wrong:**
- We tried to assign 192.168.1.1 to **GigabitEthernet0/0/0** (this was fine).
- But we also tried to assign 192.168.1.2 to **GigabitEthernet0/0/1**, which caused the overlap problem. These two interfaces must be on different networks.

## Solution:
1. **Assign a unique network** to each interface on the router.
    - **GigabitEthernet0/0/0**: We gave this interface an IP from **192.168.1.0** network, specifically `192.168.1.1/24`.
    - **GigabitEthernet0/0/1**: We gave this interface an IP from a **different network**, the **192.168.2.0** network, specifically `192.168.2.1/24`.

2. This way, the router can now route traffic between these two networks, allowing **PC0** and **PC1** to communicate.

## How to Reproduce the Lab:

### Step 1: Add Devices
- Place **two PCs** and **two switches** on the workspace.
- Place a **router** to connect the two switches.

### Step 2: Connect the Devices
- Connect **PC0** to **Switch0** and **PC1** to **Switch1**.
- Connect **Switch0** to **Router GigabitEthernet0/0/0**.
- Connect **Switch1** to **Router GigabitEthernet0/0/1**.

### Step 3: Configure IP Addresses
1. **For PC0**:
   - IP Address: `192.168.1.2`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.1.1`

2. **For PC1**:
   - IP Address: `192.168.2.2`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.2.1`

3. **For the Router**:
   - **GigabitEthernet0/0/0**:
     ```bash
     ip address 192.168.1.1 255.255.255.0
     no shutdown
     ```
   - **GigabitEthernet0/0/1**:
     ```bash
     ip address 192.168.2.1 255.255.255.0
     no shutdown
     ```

### Step 4: Test Connectivity
1. On **PC0**, open the command prompt and type:
   ```bash
   ping 192.168.2.2

![image](https://github.com/user-attachments/assets/d5481458-3e63-49e8-94fb-6c711dadffad)
