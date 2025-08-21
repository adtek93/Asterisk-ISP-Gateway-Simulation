📌 Project: Asterisk ISP Gateway Simulation
1. Description

This project simulates a Telco/ISP environment using Asterisk to:

Provide a simulated mobile extension (e.g., 0909123123).

Provide a fixed-line DID number (e.g., 02873001234).

Connect to FreePBX via IP Trunk (PJSIP) without SIP accounts.

Support two-way call routing:

Mobile 0909123123 → DID 02873001234 → FreePBX → Agent 123

Agent 123 (FreePBX) → Mobile 0909123123 → Softphone

👉 The goal is to emulate a real carrier call flow and validate inbound/outbound communication between FreePBX and Asterisk.

2. Architecture
[Softphone: 0909123123] 
        |
        v
+----------------------+
| Asterisk (ISP/Carrier) |
|  - Extension 0909      |
|  - IP Trunk to FreePBX |
+----------------------+
        |
        v
+----------------------+
| FreePBX              |
|  - DID: 02873001234  |
|  - Extension: 123    |
+----------------------+
        |
        v
[Agent 123 (Softphone)]

3. Components

Asterisk (ISP/Carrier)

Manages extension 0909123123 (registered via softphone).

Provides an IP-based trunk (PJSIP) to FreePBX.

Dialplan routing:

Inbound: DID 02873001234 → FreePBX.

Outbound: number 0909123123 → extension inside Asterisk.

FreePBX

Configured with IP Trunk to Asterisk.

Inbound Route: 02873001234 → Extension 123.

Outbound Route: 0909123123 → Asterisk trunk.

4. Testing Procedure
🟢 Inbound Call Test

Register softphone Zoiper/X-Lite to Asterisk with user 0909123123.

Dial 02873001234.

Asterisk receives the call → sends it to FreePBX trunk.

FreePBX inbound route for 02873001234 routes to extension 123.

Softphone registered as Agent 123 rings → PASS.

🔵 Outbound Call Test

Register softphone to FreePBX as extension 123.

Dial 0909123123.

FreePBX matches outbound route → sends call to Asterisk trunk.

Asterisk routes the call to extension 0909123123.

Softphone for 0909123123 rings → PASS.

📌 SIP Signaling Verification

On Asterisk, run:

pjsip show endpoints → verify endpoint/trunk registrations.

pjsip set logger on → trace SIP INVITE, 100 Trying, 180 Ringing, 200 OK.

Use Wireshark to capture SIP packets for protocol-level validation.
