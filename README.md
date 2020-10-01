# THM: Hacking with Powershell
> Andrew Eng | 2020-12-23

# Table of Contents
1. [Information](#Information)
2. [Objectives](#1/6 Objectives)
3. [What is Powershell](#2/6 What is Powershell)
4. [Basic Powershell Commands](#3/6 Basic Powershell Commands)

## Information
- IP Address: 10.10.206.6
- Username: Administrator
- Password: BHN2UVw0Q

## 1/6 Objectives
Q1. Deploy the machine

## 2/6 What is PowerShell
### Notes: 
Introduction to Powershell; concepts on Verb-Noun cmdlet usage (i.e. Get, Start, Stop, read, Write, New, Out).  
Additional read: https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7

### Questions:
Q1. What is the command to get help about a particular cmdlet(without any parameters)?
`Get-Help`

## 3/6 Basic Powershell Commands

### Cheat Sheets:

#### Using Get-Command

#### Object Manipulation

#### Creating Objects From Previous cmdlets

#### Filtering Objects

#### Sort Objects

### Questions:
**Q1.** What is the location of the file "interesting-file.txt"

Notes: There has got ot be a better way thing brute forcing files for strings.

`PS> Get-ChildItem C:\ -recurse | findstr interesting-file.txt`
> Answer: C:\Program Files

**Q2.** Specify the contents of this file

`PS> type "C:\Program Files\interesting-file.txt.txt"`
> Answer: notsointerestingcontent

**Q3.** How many cmdlets are installed on the system(only cmdlets, not functions and aliases)?

Notes: initially ran get-command, then I started to filter out the columns and pipe it to measure-object

`PS> Get-Command | Where-Object -Property CommandType -eq cmdlet | Measure-Object` 
> Answer: 6638

**Q4.** Get the MD5 hash of interesting-file.txt

Notes: help hash followed by help Get-FileHash to give me to arguments I need for setting Algorithm to MD5

`PS> Get-FileHash -Algorithm MD5 "C:\Program Files\interest-file.txt.txt"`
> Answer: 49A586A2A9456226F8A1B4CEC6FAB329

**Q5.** What is the command to get the current working directory?

> Answer: Get-Location

**Q6.** Does the path "C:\Users\Administrator\Documents\Passwords" Exist(Y/N)?

Notes: I didn't do this one in powershell....  Need to go back and look for how to do it in pwsh

> Answer: N

**Q7.** What command would you use to make a request to a web server?

Notes: help web

> Answer: Invoke-WebRequest

**Q8.** Base64 decode the file b64.txt on Windows.

Notes: I originally had an easier way to do this, but I can't seem to remember how.  So I'm doing this in a different way:

`PS> $content = [IO.File]::ReadAllText("Desktop\b64.txt")`
> Answer: ihopeyoudidthisonwindows

## 4/6 Enumeration
**Q1.** How many users are there on the machine

Notes: I used help user to find the cmdlet

`PS> Get-LocalUser`
> Answer: 5

**Q2.** Which local user does this SID(S-1-5-21-1394777289-3961777894-1791813945-501) belong to?

`PS> Get-LocalUser | Select-Object -Property Name,SID | findstr 501`
> Answer: Guest

**Q3.** How many users have their password required values set to False?

Notes:

`PS>`
> Answer: 4

**Q4.** How man local groups exist?

Notes: help groups

`Get-Localgroup`
> Answer: 24

**Q5.** What command did you use to get the IP address info?

> Answer: Get-NetIPAddress

**Q6.** How many ports are listed as listening?

Notes:

`PS>`
> Answer: 20

**Q7.** What is the remote address of the local port litsening on port 445?

Notes:

`PS>`
> Answer: ::

**Q8.** How many patches have been applied?

Notes:

`PS>`
> Answer: 20

**Q9.** When was the patch with ID KB4023834 installed?

Notes:

`PS>`
> Answer: 6/15/2017 12:00:00 AM

**Q10.** Find the contents of a backup file

Notes:

`PS>`
> Answer:

**Q11.** Search for all files containing API_KEY

Notes:

`PS>`
> Answer:

**Q12.** What command do you do to list all the running processes?

Notes:

`PS>`
> Answer: Get-Process

**Q13.** What is the path of the scheduled task called new-sched-task?

Notes:

`PS>`
> Answer /

**Q14.** Who is the owner of the C:\

Notes:

`PS>`
>

## 5/6: basic Scripting Challenge
**Q1.** What file contains the password?

Notes:

`PS>`
> Answer:

**Q2.** What is the password?

Notes:

`PS>`
> Answer:

**Q3.** What files contains an HTTPS link?

Notes:

`PS>`
> Answer:

## 6/6: Intermediate Scripting

**Q1.** How many open ports did you find between 130 and 140 (inclusive of those two)?

Notes:

`PS>`
> Answer:

