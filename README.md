# PHP2Shell — Technical Security Analysis

**Analyzing CVE-2026-109618 and CVE-2026-109619**

A technical study of critical PHP runtime vulnerabilities conducted through an authorized TryHackMe lab.

---

## Table of Contents

- [Overview](#overview)
- [Objectives](#objectives)
- [Vulnerability Summary](#vulnerability-summary)
- [Lab Environment](#lab-environment)
- [Technical Analysis](#technical-analysis)
- [Conceptual Exploitation Chain](#conceptual-exploitation-chain)
- [Security Impact](#security-impact)
- [Detection and Investigation](#detection-and-investigation)
- [Mitigation and Remediation](#mitigation-and-remediation)
- [Key Takeaways](#key-takeaways)
- [References](#references)
- [Disclaimer](#disclaimer)

---

## Overview

This repository documents my analysis of **PHP2Shell**, a TryHackMe lab focused on two critical vulnerabilities affecting the PHP runtime.

The vulnerabilities studied are:

- **CVE-2026-109618:** Remote Code Execution (RCE) via out-of-memory corruption.
- **CVE-2026-109619:** Remote Code Execution (RCE) via stale global state.

Both vulnerabilities have been reported with a CVSS score of 9.2.

The purpose of this project is to understand the underlying technical concepts, investigate the potential impact, and explore defensive strategies for identifying and mitigating runtime vulnerabilities.

## Objectives

- Understand PHP runtime security and memory-management issues.
- Study the relationship between memory disclosure and ASLR.
- Understand how runtime vulnerabilities can lead to remote code execution.
- Identify potential indicators of exploitation.
- Document remediation and vulnerability management practices.

## Vulnerability Summary

| CVE | Description | Reported CVSS |
|---|---|---|
| CVE-2026-109618 | RCE associated with out-of-memory corruption | 9.2 |
| CVE-2026-109619 | RCE associated with stale global state | 9.2 |

These vulnerabilities have distinct root causes. Their exploitability depends on the affected PHP version, configuration, and deployment environment.

Consult the official advisories for authoritative details about affected versions and remediation.

## Lab Environment

| Component | Details |
|---|---|
| Platform | TryHackMe |
| Lab | PHP2Shell |
| Web server | Apache |
| PHP integration | mod_php |
| Focus | PHP runtime security |

All practical testing is restricted to the authorized lab environment.

## Technical Analysis

### 1. Root Cause and Memory Management

The lab explores how memory-management failures and exceptional request-handling paths can leave internal runtime state inconsistent.

Key concepts include:

- Out-of-memory conditions.
- Partially initialized internal state.
- Stale references to previously allocated memory.
- Unsafe cleanup and request lifecycle handling.

These concepts demonstrate why runtime security is an important part of application security.

### 2. Memory Disclosure

Memory disclosure occurs when a program unintentionally exposes information from its process memory.

Depending on the information exposed, this may reveal internal process details and make subsequent exploitation easier.

Memory disclosure alone does not necessarily result in remote code execution, but it can contribute to a larger exploitation chain.

### 3. Address Space Layout Randomization (ASLR)

ASLR is a security mechanism that randomizes memory locations to make exploitation more difficult.

If useful memory addresses are disclosed, an attacker may be able to reduce the uncertainty provided by ASLR.

The impact depends on the platform, configuration, and information exposed.

### 4. Remote Code Execution (RCE)

Remote Code Execution occurs when an attacker can cause a vulnerable system to execute code or commands remotely.

In a web server environment, the impact depends on the permissions and resources available to the affected process.

Potential consequences include unauthorized data access, exposure of application secrets, and modification of accessible files.

## Conceptual Exploitation Chain

The following diagram illustrates the general concepts explored in the lab. It is not a universal exploitation sequence for both CVEs.

```text
Vulnerable PHP Runtime
          |
          v
Memory Management Failure
          |
          v
Memory Disclosure or
Corrupted Internal State
          |
          v
Reduced ASLR Effectiveness
          |
          v
Potential Remote Code Execution
          |
          v
Impact on the Web Environment
```

## Security Impact

A successful RCE vulnerability in a web-serving runtime can affect the confidentiality, integrity, and availability of an application.

The potential impact depends on:

- PHP worker privileges.
- Access to application secrets and environment variables.
- Network connectivity to internal services.
- Container and process isolation.
- Permissions granted to the web server.

These are potential risks and do not imply that every deployment is exploitable in the same way.

## Detection and Investigation

Potential indicators worth investigating include:

- Unexpected multipart HTTP requests.
- Unusual session-cookie values or sizes.
- PHP worker crashes or unexpected restarts.
- Memory-related errors in PHP or web-server logs.
- Repeated malformed requests or abnormal application behavior.

These indicators are not conclusive evidence of exploitation. They should be correlated with other application, operating-system, and infrastructure logs.

## Mitigation and Remediation

### 1. Update PHP

Prioritize upgrading to a fixed PHP release using the official PHP project or the relevant vendor's security guidance.

The following fixed versions have been reported for the relevant supported branches:

| PHP Branch | Fixed Version |
|---|---|
| PHP 8.5 | 8.5.12 |
| PHP 8.4 | 8.4.27 |
| PHP 8.3 | 8.3.36 |
| PHP 8.2 | 8.2.35 |

Verify these versions against current official advisories and vendor guidance before applying remediation.

### 2. Apply Temporary Mitigations

Temporary configuration changes may reduce exposure when explicitly recommended by the official advisory.

Such measures do not replace upgrading to a fixed version.

### 3. Verify Remediation

After patching:

- Restart or redeploy affected PHP workers as required.
- Verify the effective runtime version.
- Check containers and application bundles.
- Review relevant logs for suspicious activity.
- Document the remediation and validation results.

### 4. Improve Vulnerability Management

Maintain an inventory of PHP runtimes, dependencies, containers, and exposed services.

Prioritize internet-facing assets and ensure that security updates reach every affected environment.

## Key Takeaways

- Runtime vulnerabilities can compromise applications independently of application-level code flaws.
- Memory disclosure can make additional exploitation steps easier.
- Error handling and memory lifecycle management are important security considerations.
- Vulnerability management must include runtimes and dependencies, not just application code.
- Patching should always be followed by deployment verification.

## References

- [TryHackMe — PHP2Shell](https://tryhackme.com/room/cvephp2shell)
- [PHP Official Website](https://www.php.net/)
- [PHP Source — GitHub Security Advisories](https://github.com/php/php-src/security/advisories)
- [CVE-2026-109618 — CVE.org](https://www.cve.org/)
- [CVE-2026-109619 — CVE.org](https://www.cve.org/)

Refer to the official advisories for the latest technical details and remediation guidance.

## Disclaimer

This repository is intended for educational purposes and documents learning from an authorized security lab.

It does not include the room flag, credentials, or sensitive session information.

All practical testing must remain within systems for which explicit authorization has been granted.

---

**Topics:** PHP Security · Application Security · Memory Safety · Vulnerability Management · TryHackMe
