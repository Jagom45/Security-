# Wazuh Incident Repsonse Detection Report

Incident/Ticket ID: 1

Alert/Detection: Rule ID 60122, 60106, 92652 and then 63103

Date/Time: 8/2/2025 9:55PM

Severity: High

Affected Host: DC10 hosting windows Server 2019

Affected User: Administrator account

Source IP: 10.1.16.242 

Detection Source: SIEM generated logon failed alert on Wazuh

Initial Alert: 60122 Logon failure - Unknown user or bad password

Investigation: There where 57 60122 Logon failed login attempts, the 58th was a successful logon attempt

Raw Event Evidence: Logon failure - Unknown or bad password

IOC Analysis: There where 57 60122 Logon failed login attempts, the 58th was a successful logon attempt

MITRE ATT&CK Mapping: Mapped to T1550.002 but that is a Pass the Hash Attack, Wazuh incorrectly mapped it to that, and this is not correct because there was multiple failed attempts and then a successful one on an high level administrator account. T1078 and T1531 are also mapped to 60122 for using a legitimate account to gain/access resource and removes/block legitimate user's account access. Also 63103 because the audit logs was cleared, this maps to TA0005 & T1070 which falls under Stealth and Indicator of removal, adversaries may selectively deleted or modified artifacts generated to reduce indications of their presence and blend in with legitmate activity

Scope/Impact: High because a high level administrator account was compromised and can add users to maintain access and access any resource available

Containment: Remove the compromised host from the network, disable all incoming and outgoing communication to prevent lateral movement or further damage

Remediation: Remove the affected host from the network, record generated reports, check other hosts for IoC's or evidence of lateral movement, reset password for the compromised account, follow password policy to make sure that a strong password is being used, backup generated security events incase of adversaries deleting security logs

Escalation: Yes to allow in house user take over the report and follow necessary remediation

Final Disposition: Incident contained and closed, the suspicious authentication activity was detected and investigated through wazuh, windows security logs deletion was also detected as potential anti-forensic activity. No additional systems where affected.

Analyst Notes: Wazuh successfully detected repeated logon failures, with a successful logon, and the clearing of local security alerts on the windows machine. During the investigation the default Wazuh MITRE ATT&CK mapping for Rule 92652 identified T1550.002 which is pass the hash; however, review of the raw event data and the simulated attack showed that the activity was password guessing rather than Pass the Hash. This shows the importance of validating automated SIEM output against raw event data before determining the attack technique.

Lessons Learned / Detection Improvement: Automated SIEM detection can identify suspicious authentication and anti-forensic activity. Detection coverage can be improved by improving correlating repeated logon failures with a successful logon and detecting windows event log clearing. A place to forward event logs should be implemented in case of evidence of an attacker clearing local security logs on a machine, a backup of them will still exist somewhere else.

