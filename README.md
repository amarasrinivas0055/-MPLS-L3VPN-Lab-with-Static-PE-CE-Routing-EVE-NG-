#🚧 MPLS L3VPN Lab with Static PE-CE Routing (EVE-NG)
This lab demonstrates an MPLS Layer 3 VPN setup in EVE-NG, where static routing is used between Provider Edge (PE) and Customer Edge (CE) routers instead of dynamic routing protocols.

🔍 A key learning from this lab was understanding how MPLS treats static routes and how that impacts label assignment and end-to-end reachability.

🧱 Topology Summary
🖧 CE Routers

CE1 (R5)
→ Loopback IP: 192.168.1.1/32

CE2 (R7)
→ Loopback IP: 192.168.2.1/32

🏢 PE Routers

PE1 ↔ CE1
→ Connected using static routing

PE2 ↔ CE2
→ Connected using static routing

🌐 Core P Routers

IGP Protocol:

→ OSPF (with full mesh topology inside MPLS core)

Label Distribution:

→ LDP enabled across all core-facing interfaces

MPLS:

→ mpls ip enabled on all OSPF interfaces

🔄 PE-to-PE Communication

MP-BGP (VPNv4) used between PE routers

Update Source:

→ Loopback interfaces used for BGP peering

VRF Segregation:

Route Distinguisher (RD)

   → Used to uniquely identify customer prefixes across VRFs

Route Target (RT)

  → Used to control import/export of routes between VRFs

⚙️ Technologies Used

✅ OSPF — IGP for the MPLS backbone

✅ MPLS + LDP — Label switching across P and PE routers

✅ MP-BGP (VPNv4) — PE-to-PE communication

✅ VRF Configuration — Customer traffic isolation

✅ Static Routing (PE-CE) — Simplified customer edge setup

Troubleshooting using ping source, mpls forwarding-table, ip cef, and bgp vpnv4

🔍 Problem Faced

Despite correct routing and label propagation across the core, end-to-end ping between CE loopbacks failed.

Reason: The ping test from CE1 (R5) used the physical interface IP as the source instead of the loopback.

Since CE2 had no route to that physical IP (only to 192.168.1.0/24), the return packet couldn't reach back.

✅ Final Fix
Used this ping command from R5:

ping 192.168.2.1 source 192.168.1.1

With this, the MPLS LSP and routing paths worked perfectly, and end-to-end reachability was successful.

📸 Screenshots

Topology Diagram :

<img width="1650" height="690" alt="Image" src="https://github.com/user-attachments/assets/394e4111-09e1-4c09-bea5-cf421e1ef44b" />

Routing Table Outputs :

<img width="1777" height="725" alt="Image" src="https://github.com/user-attachments/assets/368aae64-8349-478a-bf59-48b67ab23e7c" />

<img width="1801" height="586" alt="Image" src="https://github.com/user-attachments/assets/3ddae003-9931-4429-b013-fe415e5fdd5d" />

MPLS Forwarding Tables :

<img width="1830" height="632" alt="Image" src="https://github.com/user-attachments/assets/9f59a6b9-618c-4de0-92f9-6e42a84d44ac" />

PE to CE Reachability :

<img width="1864" height="742" alt="Image" src="https://github.com/user-attachments/assets/312fc979-8f44-4299-8160-c8dfe72ca52b" />

Failed Ping Test :

<img width="1695" height="575" alt="Image" src="https://github.com/user-attachments/assets/394824aa-74ef-46cf-819d-ef6dee3bb893" />

Final Successful Ping Test :

<img width="1804" height="847" alt="Image" src="https://github.com/user-attachments/assets/77e3247d-f154-48a7-80d1-44ce1d795942" />

<img width="763" height="487" alt="Image" src="https://github.com/user-attachments/assets/1dc8201f-7eef-4254-95ef-9190074d69b2" />


💡 Key Takeaways

Static routing between PE and CE requires careful source IP selection during testing.

Always verify return path reachability in L3VPN labs.

Understand how MPLS label assignment works with connected vs static vs dynamic routes.

🧠 Lab Time

Lab Build Time: ~3 hours

Troubleshooting Time: ~3 hours

Outcome: Learned practical MPLS behavior and enhanced troubleshooting skills in VPNv4/MPLS environments.

