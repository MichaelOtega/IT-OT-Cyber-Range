## Architecture & Tech Stack
The testbed leverages VirtualBox for virtualization and is segmented into distinct IT, OT, and Attacker zones to simulate a standard Purdue Model architecture across a distributed hardware setup.

*   IT Zone (Monitoring, Defense & Endpoints): 
    *   Wazuh (SIEM & Endpoint Detection)
    *   Zeek (Network Security Monitoring)
    *   Ubuntu (Host OS for Manager/Sensor Nodes)
    *   Windows (Simulated Corporate Endpoint with Wazuh Agent)
*   OT Zone (Control & Visualization): 
    *   OpenPLC (Programmable Logic Controller)
    *   ScadaBR (Human Machine Interface / HMI)
*   Attacker Node: 
    *   Kali Linux

## Attack & Detection Scenarios
### Scenario 1: Initial IT Compromise
1.  The Attack: The Kali Linux node targets the Windows endpoint to simulate a first-stage breach of the corporate IT environment.
2.  The Telemetry: The Wazuh agent deployed on the Windows machine captures unauthorized process executions, file modifications, and system-level anomalies.
3.  The Detection: The Wazuh Manager ingests the endpoint data and triggers an initial alert for the compromised IT asset.

### Scenario 2: Lateral Movement & Modbus TCP Manipulation
1.  The Attack: Following the initial breach, a simulated Modbus TCP protocol manipulation is executed against the OpenPLC node, mimicking an attempt to alter operational logic.
2.  The Telemetry: *Zeek* actively monitors the network layer, capturing the anomalous traffic and unauthorized Modbus requests crossing the IT/OT boundary.
3.  The Detection: The Wazuh Manager aggregates the Zeek network logs alongside endpoint telemetry, triggering a high-severity alert for unauthorized lateral movement and OT manipulation.
4.  The Mitigation: Validated the necessity of network segmentation and strict firewall access control lists (ACLs) to drop unauthorized Modbus TCP requests before they reach the controller layer.
