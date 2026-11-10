API - Websites for machines

Classic Cyber Attack

Recon -> Infiltration -> Weaponize -> Lateral Movement -> Privilege Escalation -> BREACH

API Attack

Find vulnerability -> BREACH

  

Regulatory compliance all boils down to 3 things. Security, Privacy, and Accessibility.

  

OWASP API TOP 10

API1 Broken Object Level Authorization

API2 Broken Authentication

API3 Broken Object Property Level Authorization

API4 Unrestricted resource Consumption

API5 Broken Function Level Authorization

API6 Unrestricted Access to Sensitive Business Flows

API7 Server Side Request Forgery

API8 Security Misconfiguration

API9 Improper Inventory Management

API10 Unsafe Consumptions of APIs

  

  

Broken Object Level Authorization

When a user is able to perform actions they otherwise should not be able to, this can be accessing other user’s data, changing information about the actions they’re performing, etc.

Solutions

- Discuss authorization rules during API design phase
- Review business requirements and define data access policies
- Entire authorization controls at application logic layer
- Implement automated, pre-production testing to find BOLA flaws

  

Broken Authentication

An API vulnerability due to weak or poor authentication. Often as a result of missing or poorly implemented security controls such as not using OAuth, no 2FA, no captcha, brute forcing, credential stuffing, etc.

Solutions

- Define authentication policies based on business requirements.
- Consider data sensitivity in policies.
- Implement continuous testing to identify gaps, weaknesses.
- Don’t assume APIs are hidden or won’t be found. Lock the door.

  

Broken Object Property Level Authorization

This is the exploit of endpoints by reading and/or modifying values of the objects. They can update the object elements, this is known as mass assignment, this means the attacker can set ‘account-type=premium’. They can also reveal unnecessary sensitive data known as excessive data exposure.

Solutions

- Ensure user can only access legitimate, permitted fields.
- Return only minimum amount of data required for the use case.
- Define data requirements in AOI specifications.
- Test to validate policy compliance.
- Implement proper controls enforced to prevent mass assignment exploits.
- Test controls to identify logic flaws.

  

Unrestricted resource Consumption

Abuse of APIs due to high volumes of API calls, large requests, etc. Refers to any kind of volumetric or brute forcing of APIs.

Solutions

- Implement traffic controls.
- Test effectiveness of controls
- Note: rate limiting is a velocity control, not a volume control.

- Attackers are clever and determined
- They’ll evade detection by limiting requests velocity
- Breaches can take months.

  

Broken Function Level Authorization

Compared to Object level authorization that looked at what type of data a user could access, here we’re looking at what type of functions they can perform.

It is an abuse of functions, API functionality to improperly modify objects (create, update, delete). e.g being able to reset another user’s password. It often involves replacing passive methods like GET with active methods like PUT, DELETE.

Solutions

- Identify and prioritize functions that expose high sensitivity capability.
- Develop controls to limit access.
- Implement continuous release testing to ensure proper behavior.
- Review RBAC permissions across all user types and detect drift.

  

Unrestricted Access to Sensitive Business Flows/logic

As developers, we tend to think in terms of if the application is working as designed, can it perform as expected to. With this vulnerability we’re asking if that business logic can be manipulated, can it work as not designed, be misused, etc.

It is an abuse of a legitimate business workflow, usually a result of application logic flaw.

Solutions

- Do not just think of how your application is meant to work, also consider how it can be misused.
- Avoid use of incremental IDs.
- Think like a hacker - how can your app be abused/misused.

  

Server Side Request Forgery

This is an attack in which a server can be tricked into running commands that it shouldn’t have been permitted to run.

The goal of SSRF is to trick your API Server to go somewhere it shouldn’t, somewhere it doesn’t belong. It involves exploiting URL inputs to make a request to a malicious 3rd party server, access internal resources that external users shouldn’t have access to.

It often involves exploiting flaws in the permissions of the server, leveraging excessive permissions on the server to target another system.

Examples

- Creates a channel for malicious requests, data access, or other fraudulent activity.
- Potential for data leaks.
- User inputs malicious destination like a malware site, /localhost/etc/passwd

Solutions

- Utilize least privilege model.
- Don’t trust any inputs without inout validation and sanitization.
- Validate all user-supplied information, including URL parameters.
- Simulate SSRF attacks during QA/testing to identify any vulnerabilities.

  

Security Misconfiguration

This is a broad category encompassing a server’s lack of hardening to unnecessary services. It can involve the use of bots to scan, detect, and exploit these misconfigurations.

It involves the lack of following standards best practices for running and operating your API servers.

Examples

- Lack of security hardening.
- Missing security patches.
- Unnecessary features enabled.
- Missing TLS.
- Improperly configured permissions.
- Missing headers: Rate Limit, HSTS, CORS policy missing/improperly set.
- Verbose, revealing error messages.
- Misconfigurations can expose sensitive user data.
- Potential for full server compromise.

Solutions

- Implement hardening procedures.
- Enforce proper headers and policies: CORS, HSTS, Rate limit.
- Ensure error messages are helpful, but not revealing.
- Prevent oath traversal, server information leakage.
- Routinely review configurations.
- Test configurations to ensure proper settings; discover drift.

  

Improper Inventory Management

Its two main aspects are Awareness & Visibility: Do you know all the APIs running in your environment? And Control: Should all these APIs be running, are they up to date?

It is the lack of awareness of all APIs and the improper version control of these APIs.

Examples

- Zombie, shadow, rogue APIs.
- Old versions of APIs, unpathced endpoints.
- Data/account theft via unreturned, unsupported APIs.
- Endpoint with weaker security.
- Outdated documentation.
- Unnecessarily exposed endpoints.

Solutions

- Define common, standard processes for API development.
- Create a complete inventory of APIs.

- Traffic-based discovery
- Code-based discovery
- App crawling discovery
- Dictionary discovery
- Go talk to Engineering discovery

- Identify internally developed APIs, as well as 3rd party APIs.
- Deploy/manage all APIs in Gateway.
- Define versioning rules and retirement.
- make sure all parties upgrade to latest version.
- Periodically audit 3rd party access.

  

Unsafe Consumptions of APIs

This is about the API risk from using 3rd party APIs as they may have vulnerabilities or be integrated insecurely.

Examples

- Data theft.
- Breach.
- Account takeover.

Solutions

- Maintain accurate inventory of 3rd party APIs.
- Don’t assume that they’re safe, secure them like your own APIs.
- Validate data returned by 3rd party APIs.
- Test authorization rules.
- Request vulnerability testing reports from vendors.
- Evaluate security controls of 3rd party API

  

**API Attack Analysis**

OWASP 1, 2, 3 are responsible for 90% of API breaches. Being able to address these 3 vulnerabilities can hugely impact the security of your APIs.

Once you’ve identified the vulnerabilities causing the most damage, you want to conduct threat modeling and risk assessment.

Figure out where your weakest links are 

Asks this questions

Why do you care about API Security?

How important is API Security to the business?

Why API Security

  

Threat Modeling

Identify: Know your footprint, your attack surface area. e.g APIs, business flows, data, access paths.

Assess: Vulnerabilities, logic flaws, access controls, 3rd party risk.

Probability: examine the likelihood of an attack

Impact: Understand the damage, loss, consequences of an attack.

Mitigation: Develop a plan to address the risk.

Risk = Threat * Vulnerability * Likelihood * Impact

  

**The 3 Pillars of API Security**

**Governance**

Developing secure APIs. The goal of governance is to establish consistency, consistency in their development, deployment, testing, etc. Set expectations for your development team, let them know what is expected when developing these APIs, the documentation requirement, the authentication policies.

  

Awareness

Having proper visibilities of your APIs. Know your APIs, know your data, know your risks.

Get full inventory APIs, know your infrastructure, standardize API deployment process, mandate API documentation.

The best way to enforce a standardize API deployment is through the use of an API gateway, a central control point where all APIs are being managed and go through proper validation steps.

  

Policy & process

All about how you develop your apis, how you deploy it, it’s documentation, the style guide requirements, etc.

  

**Monitoring**

Deleting threats in production. It’s all about understanding how your APIs are performing and operating in production.

Runtime Protection

Active real time protection of your APIs. This includes things like policy enforcement, requiring authentication, & traffic filtering.

Threat Detection

Analyzing the traffic coming in to look for things like fraudulent traffic, volumetric attacks, incident response.

Control Validation

Validate the controls you have in place ensuring they are working as intended.

Monitoring Approaches

- Proactive Blocking

This can be done using API gateways & web app firewalls.

- Reactive Alerting

Capturing traffic, logging it, ingesting into a SIEM, a runtime API threat management, etc. Alerting is important for when we cannot immediately have context to understand whether the traffic is legitimate or not, storing it and looking through it later allows us to identify threats that might have tried to slip through.

Monitoring can aid in API discovery, it can assist in identifying endpoints in use, discover undocumented and unknown APIs, etc. But comprehensive API discovery requires more sources than just monitoring. For API discovery, look at the API gateway, the web app firewall, code repository, app testing, crawling, etc.

  

The key challenge with monitoring is that most API attacks pass through our defenses because they look legitimate.

  

**Testing**

Ensuring APIs are free of flaws.

The vast majority of testing organizations do is positive testing. Confirming if the application works as designed, is it working as expected, free of bugs, etc. We need to do more negative testing, does the application not do what it shouldn’t do, 

Types of Testing

- Security - Looking for things like Unsecured Endpoints, Incremental IDs, Injection, XSS, Fuzzing, Input validation, Error handling.
- Data - Focusing on what kind of data your API is accessing and returning. Excessing data exposure, Sensitive data exposure, Personal, health, bank data, File, Directory exposure, Data exfiltration.
- Logic - These are your unique vulnerabilities. They do not get captured in SAST or DAST tool, you have to check for it yourself. Object ID manipulation, Cross-account access, API function abuse, Role-based access control, Authorization gaps.

API Testing approaches

- Do nothing.
- Do it yourself.
- Pay someone else to do it.
- Automate it. APIsec scanner from APIsec

  

  

Cybersecurity Landscape

Network - Firewall, NAC, Threat Protection, Deception.

Endpoint - EDR, Anti Malware/Virus, Vulnerability Management.

Application - SAST, DAST, SCA, Container Security, Web App Firewall, API Security.

Data - Encryption, DLP, Access Control.

Web/Messaging - Email Gateway, Web Gateway.

Identity - Authentication, PIM, Identity Governance.

Detection - SIEM, SOAR, Incident Response.

Cloud - CASB, Infrastructure Security.

  

Application Security Technology

Static code analyzers / Static application security testing (SAST)

Dynamic application security testing  (DAST)

Software Composition Analysis (SCA)

Container Security

Web App Firewall

Web Scanners

API Security

SAST and DAST look for common vulnerabilities, CVEs, but APIs rarely get exploited via CVEs as they’re easily detected and fixed.

Attackers are looking for unique flaws in your APIs, your logic flaws, gaps in your authorization rules, ways to abuse your API functions, etc

  

Application Lifecycle and Security Tools & Process we can Implement Along the Way

- API Definition

Secure design, Governance, Policies.

- Development

Code reviews, Static code analysis, Composition analysis.

- Testing

Legacy DAST, Pen-testing, Bug bounty

- Deployment

Web server, API Gateway, WAF / SIEM

- Retirement

Governance, Versioning, Deprecation.

  

  

Common Sources of API Risks

- Unknown APIs

- Lack of complete API inventory
- Shadow/zombie APIs

- Vulnerable APIs

- Cross-account access
- Logic flaws
- Data exposure

- Weak runtime defenses

- Impro perly configured WAF
- Lack of rate-limiting

- 3rd party APIs

- Unknown 3rd party APIs
- Untested/vulnerable APIs
- Lack of input validation

- Weak governance

- Lack of standards
- Inconsistent processes
- Patchy documentation

- Poor collaboration

- Security and Engineering siloed/working together.
- Incomplete/inefficient info sharing

  

Top 10 API Do’s and Don’ts

1. Don’t trust anything (inputs/network)
2. Do validate all inputs (not just user)
3. Don’t reveal useful info in error messages, it should be helpful but not revealing.
4. Do expect attackers to find your APIs
5. Don’t have hidden/unadvertised features
6. Don’t filter data in UI - control at app level
7. Do use Gateways to control access, traffic
8. Don’t forget authentication & authorization
9. Do require API documentation
10. Do continuously test, pre-production

  

  

A limitation with API security monitoring tools is that they often lack context to distinguish malicious from legitimate requests. We need a tool that integrates AI to do this……

  

  

  

  

  

find / -user relay08 2>/dev/null - Find starting from root, all files and directories associated with the user relay08, send any errors to /dev/null

  

relay08 password: N3xtH0p_08

  

  

What is threat modeling, what is a risk profile, what is business logic abuse?

How to decode rot13(A-Z) - erynl06 cnffjbeq: Fg3cHc_06

Hexadecimal- 72656c617930372070617373776f72643a20536b317041683361645f3037

0a