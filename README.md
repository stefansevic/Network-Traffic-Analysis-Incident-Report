# Network Traffic Analysis: Incident Report

## 1. Executive Summary
During a routine security audit, a brute-force attack was identified targeting a web server's authentication mechanism. The attacker successfully compromised the account `lilith` after multiple failed attempts.

* **Status:** Resolved
* **Severity:** High (Credential Theft)
* **Attacker IP:** 192.168.1.237
* **Target IP:** 192.168.1.18

---

## 2. Technical Analysis
The analysis was conducted using **Wireshark** on a captured `.pcap` file. The attack targeted **HTTP Basic Authentication**, which transmits credentials in plaintext (Base64 encoded).

### Attack Pattern
The attacker followed a classic Brute Force pattern:
1. **Discovery:** Initial GET request without credentials, receiving a `401 Unauthorized` response.
2. **Iteration:** Multiple GET requests with different credential combinations.
3. **Success:** Packet #26 contained the correct credentials, leading to a `200 OK` response in Packet #27.

### Evidence Table
| Packet | Username | Password | Outcome |
| :--- | :--- | :--- | :--- |
| 10 | lilith | i narius | 401 Unauthorized |
| 13 | paris | ThatsHot! | 401 Unauthorized |
| 16 | alucard | Dracula | 401 Unauthorized |
| 23 | lilith | !narius! | 401 Unauthorized |
| 26 | **lilith** | **Inarius** | **200 OK (Success)** |

---

## 3. Screenshots
![Wireshark Analysis](screenshots/successful_login.png)
*Figure 1: Identification of successful login credentials in Packet #26.*

---

## 4. Mitigation Recommendations
To harden the infrastructure against such attacks, the following measures are recommended:
1. **Implement HTTPS (TLS):** Upgrade from HTTP to HTTPS to encrypt traffic and prevent plaintext credential sniffing.
2. **Account Lockout Policy:** Implement a policy that locks accounts after 3-5 failed login attempts to stop brute-force tools.
3. **MFA:** Enable Multi-Factor Authentication to add a layer of security beyond just passwords.
