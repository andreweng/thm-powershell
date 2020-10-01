# THM: Hacking with Powershell
> Andrew Eng | 2020-12-23

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
> get-Help

## 3/6 Basic Powershell Commands

### Notes:

#### Using Get-Command

#### Object Manipulation

#### Creating Objects From Previous cmdlets

#### Filtering Objects

#### Sort Objects

### Questions:
Q1. What is the location of the file "interesting-file.txt"
> notes: There has got ot be a better way thing brute forcing files for strings.
> cmd: Get-ChildItem C:\ -recurse | findstr interesting-file.txt
> C:\Program Files

Q2. Specify the contents of this file
> cmd: type "C:\Program Files\interesting-file.txt.txt"
> notsointerestingcontent

Q3. How many cmdlets are installed on the system(only cmdlets, not functions and aliases)?
> notes: initially ran get-command, then I started to filter out the columns and pipe it to measure-object
> cmd: Get-Command | Where-Object -Property CommandType -eq cmdlet | Measure-Object 
> 6638

Q4. Get the MD5 hash of interesting-file.txt
> notes: help hash followed by help Get-FileHash to give me to arguments I need for setting Algorithm to MD5
> cmd: Get-FileHash -Algorithm MD5 "C:\Program Files\interest-file.txt.txt"
> 49A586A2A9456226F8A1B4CEC6FAB329

Q5. What is the command to get the current working directory?
> Get-Location

Q6. Does the path "C:\Users\Administrator\Documents\Passwords" Exist(Y/N)?
> I didn't do this one in powershell....  Need to go back and look for how to do it in pwsh
> N

Q7. What command would you use to make a request to a web server?
> notes: help web
> Invoke-WebRequest

Q8. Base64 decode the file b64.txt on Windows.
> notes: I originally had an easier way to do this, but I can't seem to remember how.  So I'm doing this in a different way:
> $content = [IO.File]::ReadAllText("Desktop\b64.txt")

> ihopeyoudidthisonwindows

## 4/6 


