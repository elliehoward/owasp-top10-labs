# OWASP Lab: Broken Access Control (IAAA)

Goal: Understand and exploit Identification, Authentication, Authorization, and Accountability (IAAA) failures.

## Environment

* Platform: TryHackMe
* Lab URL:https://tryhackme.com/room/owasptopten2025one
* Date Completed:7/15/2026

\---

## Objective



## This lab demonstrates how identification, authentication, authorization, and accountability (IAAA) failures can allow attackers to access unauthorized data, impersonate privileged users, and avoid detection due to insufficient logging and monitoring.

## Steps Taken

1. Examined account functionality and URL parameters.
2. Tested whether object references could be manipulated.
3. Attempted account registration using a privileged username variant.
4. Reviewed authentication logs for suspicious activity.
5. Analyzed the impact of authorization and authentication weaknesses.

\---

## Evidence

* Modified ?id=5 to other values and successfully accessed different users' account data.
* Registered the username aDmiN and obtained access associated with the administrator account due to flawed account validation.
* Identified successful brute-force activity against the administrator account in the application logs.

## Exploit Explanation

The application had an insecure direct object reference within the URL and lacked authentication check on server side for every request. This allows threat actors to access information that should be confidential and protected with authentication and authorization. The application also lacked protection against brute force attempts such as rate limiting or limiting login attempts. There was also insecure logic within the registration, not validating that a username was already taken, the account was allowed to access the admin account by registering with the same username. User accounts should be associated with a unique immutable identifier (such as a user ID). Usernames should be validated for uniqueness during registration, and authentication should reference the unique account record rather than relying solely on user-supplied identifiers.

\---

## Vulnerability Type

* OWASP Category: A01 Broken Access Control, A07 Authentication Failures, and A09 Logging \& Alerting Failures
* Specific Type: IDOR, Logic flaws.

\---

## Why This Is Dangerous

* Unauthorized access to accounts
* Data exposure
* Privilege escalation for attackers

\---

## Remediation / Fix

* Enforce server-side authentication and authorization checks for every request.
* Replace direct object references with indirect references where possible and verify resource ownership before granting access.
* Enforce unique username constraints and associate accounts with immutable unique identifiers.
* Implement rate limiting, account lockout policies, and MFA to mitigate brute-force attacks.
* Rotate session tokens after authentication and privilege-level changes.
* Centralize logging and monitor authentication events for suspicious activity.

\---

## Key Takeaways

* Never trust client-side authorization decisions; authentication and authorization checks must be enforced server-side for every request.
* User identities should be tied to unique immutable identifiers rather than user-controlled values such as usernames.
* Authentication systems should include protections against brute-force attacks, including rate limiting and account lockout mechanisms.
* Effective logging and monitoring are critical for detecting suspicious activity and supporting incident response.

\---

## How This Applies to DevSecOps

This vulnerability could be identified through automated DAST testing, API security testing, and penetration testing incorporated into a CI/CD pipeline. Access control unit tests and integration tests should verify that users can only access resources they own. Security reviews should also validate registration, authentication, and logging functionality before deployment.

