# iLO 5 Hardening & Management Settings

## Credentials & Access
*   **Default Password:** Changed from factory default to a secure custom password.
*   **iLO RBSU Login:** `Require Login for iLO RBSU` set to `Enabled`.
    *   *Why:* Prevents unauthorized users with physical access to the server from altering network settings or resetting the iLO chip during the boot POST phase.

## Protocols & Network Security
*   **Web Server Non-SSL Port (HTTP Port 80):** `Disabled`.
    *   *Why:* Enforces strict SSL/TLS encryption (HTTPS on Port 443) for all web management sessions.
*   **SNMP Settings:**
    *   `SNMPv1 Request` and `SNMPv1 Trap` set to `Disabled`.
    *   `SNMPv3 Request` and `SNMPv3 Trap` set to `Enabled`.
    *   *Why:* SNMPv1 transmits community strings (passwords) in cleartext over the network. SNMPv3 enforces encrypted communication, crucial for security in a shared datacenter network.
*   **IPMI/DCMI over LAN:** `Disabled`.
    *   *Why:* IPMI has historical vulnerabilities; out-of-band management is exclusively handled via modern iLO RESTful APIs and HTML5 web access.
*   **Host FQDN/IP Address:** Dynamic DHCP binding on home subnet (`192.168.1.2`).
    *   *Note:* The default SSL certificate remains active for home staging. Once deployed in the datacenter with a static IP or designated subdomain, a self-signed certificate mapped to the public address will be generated via `Customize Certificate`.
