Vulnerability submitted by Shuning Xu

Supplier:https://www.atlassian.com/software/jira

Software download link: https://www.atlassian.com/software/jira/update

Affected software :jira software

Vulnerability plugin name: Time to SLA

Vulnerability plugin Version: Version 10.13.5•Jira Data Center 8.0.0 - 9.10.1

Description :For jira software download, select HOSTING TYPES as the data center and download the latest version 9.10.1 locally. Install the latest version 10.13.5 of the Time to SLA plugin, which is applicable to Jira Data Center 8.0.0 - 9.10.1. The Time to SLA plugin has XSS Vulnerabilities, that is, cross-site scripting attacks, attackers will insert malicious JavaScript codes into web pages, and the payload is <ScRiPt>alert(1)</ScRiPt>, which will trigger when administrators/users access, so as to achieve the purpose of the attack. The essential reason is that the server does not strictly filter the data submitted by the user, causing the browser to treat the user's input as JS code and directly return it to the client for execution. What's happening here is reflected XSS. The attacker can steal the cookies of users such as administrators, steal clipboard content, change web page content (such as download links) and so on by using reflected cross-site scripting attacks.
