# ThreatResearch-Reporting-Guide

Offensive Research Guide to Help Defense Improve Detection

Whenever you research a certain vulnerability ask yourself these questions and please answer them for us

## Process Patterns

### Which is the exploited service?

- Please provide the full and absolute path on the system.

e.g. C:\ManageEngine\ServiceDesk\jre\bin\java.exe

### Does the exploited service / application spawn a certain sub process? 



## Log Patterns

Please include an example log lines of the exploited service or system services generated during or after the exploitation.

### Does the exploited service write a log?

Commands that can be used on Linux systems:

```bash
ls -lrt /var/log
lsof +D /var/log/ 
lsof | grep servicename
```
 
### Does a system service write a log?

e.g. check with 

```bash
tail -f /var/log/messages
```

Commands that can be used on Windows systems:
 
```powershell
Get-EventLog -List `
| %{Get-EventLog -LogName $_.Log -After (Get-Date).AddMinutes(-5) -ErrorAction Ignore} `
| Sort-Object TimeGenerated | Format-Table -AutoSize -Wrap `
| Out-File new-log-entries-last5min.txt
```



### Does it write an event in that log for an exploitation attempt?

### Does additional logging/configuration requires enabling?

e.g. access logs need to be configured to include uri_query

### Does it write an event in case of successful exploitation?

### Does that log line contain specific values that shouldn't normally appear in similar log lines?

e.g. empty source address, uncommon characters

## File Traces

### Does the exploitation create temporary files?

e.g. an XML in a temp folder

### Does the exploitation create permanent files? 

## Other Traces

### Does exploitation generate other events that are not directly caused by your actions?

e.g. user login

### Does the exploitation modify or create any additional registry keys? (Windows)

## Optional: Provide Simple Detection Methods

### Could you provide simple shell commands to check if someone has previously exploited that vulnerability?

e.g. egrep "specific-url" /var/log/service.log, zgrep "specific-url" /var/log/service/*.gz
  
### Could you provide a quick fix that can block exploits until the vendor provides a solution?

e.g. add line in server-side script to drop all requests that contain ":;" in their User-Agent field
  

## Hall of Fame

The hall of fame lists good examples of published offensive research which included useful data which helped in creating detection rules. Follow the authors, they might do it again.

| Author | Source | Resulting Rules |
|---|---|---|
| [James Horseman](https://twitter.com/JamesHorseman2) | <https://twitter.com/Horizon3Attack/status/1616062915097886732>  | [Manage Engine Java Suspicious Sub Process](https://github.com/SigmaHQ/sigma/blob/master/rules/windows/process_creation/proc_creation_win_susp_manageengine_pattern.yml)