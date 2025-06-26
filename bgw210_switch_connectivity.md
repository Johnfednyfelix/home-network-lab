# Cisco 2960 ↔ BGW210-700 WiFi Gateway: Layer 2 Integration Test

## 🎯 Objective

Connect a Cisco 2960 Catalyst switch to the AT&T BGW210-700 gateway for wired device access through the home network.

---

## ⚙️ Setup

- **Gateway IP:** 192.168.1.254
- **Switch Management VLAN IP:** 192.168.1.39 (`interface vlan 1`)
- **Uplink:** Copper Ethernet (FastEthernet)
- **Cable:** Switch FastEthernet0/1 to BGW210 LAN port

---

## ❗ Problem

After connecting the switch to the BGW210:
- The switch **did not appear** in the BGW210 device list.
- No devices plugged into the switch could access the internet initially.

---

## 🔍 Root Cause Analysis

- The switch itself does **not initiate DHCP or ARP traffic**, so it won’t be listed in the gateway's UI.
- BGW210 shows only **active hosts** that request an IP or send data through it.
- Without an end device behind the switch, no DHCP traffic is generated.

---

## ✅ Resolution & Validation Steps

1. Set up VLAN 1:
   ```bash
   interface vlan 1
    ip address 192.168.1.39 255.255.255.0
    no shutdown
   ```

2. Set default gateway:
   ```bash
   ip default-gateway 192.168.1.254
   ```

3. Plugged a laptop into the switch (access port in VLAN 1).

4. Laptop received IP via DHCP from BGW210.

5. Laptop **successfully appeared** in BGW210's connected devices list.

---

## 📌 Key Takeaways

- A switch alone doesn’t generate traffic—devices behind it must send DHCP/ARP.
- BGW210 tracks only **active endpoints**, not passive network gear.
- This setup confirmed successful **Layer 2 bridging** between the switch and the home network.

---

## 🧪 Next Steps

- Document port security, VLAN trunking, or static IP test cases.
- Add topology diagram under `/diagrams`.
