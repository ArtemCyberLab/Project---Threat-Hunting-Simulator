Threat Hunting Report: Project "Health Hazard"
Project Objective
The main objective of this project was to investigate suspicious activity on an organization's workstation and verify a potential supply chain attack. My goal was to identify the initial access vector, analyze the execution of malicious scripts, and uncover how the adversary maintained persistence within the system.

Investigation Process
I started by examining the Splunk logs for any unusual third-party package installations. During this phase, I identified that the user itadmin-tom had installed an npm package called healthchk-lib@1.0.1.

I then analyzed the process execution chain and discovered a suspicious sequence: node.exe spawning cmd.exe, which in turn launched a hidden PowerShell process.

I decoded the Base64 command found in the logs using CyberChef. This allowed me to reveal the attacker's true intent: downloading a malicious executable named SystemHealthUpdater.exe from a typosquatted domain (wlndows.thm).

I investigated the system's registry and confirmed that the script established persistence by creating a "Windows Update Monitor" key in the Registry Run keys.

Conclusion and Findings
Based on my analysis, I have concluded that the system was compromised through a Supply Chain Attack (T1195). The attacker successfully bypassed initial defenses by weaponizing a legitimate development tool.

I have confirmed the following key indicators of compromise (IoCs):

Malicious Domain: global-update.wlndows.thm

Persistence Mechanism: Registry Run Key (HKCU\...\Run)

Payload: SystemHealthUpdater.exe

As a result of this investigation, I recommended immediate host isolation and the implementation of stricter dependency auditing for all future development projects to prevent similar incidents.
