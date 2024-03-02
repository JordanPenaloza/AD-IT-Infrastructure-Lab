# AD-IT-Infrastructure-Lab
Network hosted on windows server with 1000 users created with powershell, managed my Server Manager and Active Directory

To create this lab setup, I utilized virtual box to create my windows server machine with a client machine. The setup for this lab infrastructure can be found below.

![image](https://github.com/JordanPenaloza/AD-IT-Infrastructure-Lab/assets/113396128/97c2f98e-15cd-43b0-a46f-eb6276a54122)

I had to make it possible for this infrastructure to be isolated but also be able to use the internet
My server machine needed to have 2 network adapters, NAT for internet and INT for all other machines to connect to and use the server as an internet provider
As shown in the diagram, our host machine will connect to the server internally and backpack off of it to connect to the internet

I used the server manager to install all necesarry tools such as DHCP and AD 
The powershell script to create users is shown below
In unison with a wordlist, this can be used to create as many users as the wordlist porvides

# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$USER_FIRST_LAST_LIST = Get-Content .\names.txt
# ------------------------------------------------------ #

$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
}

![image](https://github.com/JordanPenaloza/AD-IT-Infrastructure-Lab/assets/113396128/a9cb1c81-70de-4fef-b10d-34e5593cdb1e)

Logging in on our client machine, we can confirm that the client is on the domain and on a domain account

![image](https://github.com/JordanPenaloza/AD-IT-Infrastructure-Lab/assets/113396128/80789908-9bf8-4f28-85c2-7c00431d8049)

Pinging our domain and google confirms that we are able to connect to both the internal network and the internet from the client machine

![image](https://github.com/JordanPenaloza/AD-IT-Infrastructure-Lab/assets/113396128/7415e512-bdf4-436c-a1de-995bd792a6c1)

I also experimented a bit with how groups and I created a few groups myself

![image](https://github.com/JordanPenaloza/AD-IT-Infrastructure-Lab/assets/113396128/2f3b59d4-47c9-4036-8141-f1e43a545d6c)

![image](https://github.com/JordanPenaloza/AD-IT-Infrastructure-Lab/assets/113396128/bf5375e9-041a-44ca-85a8-d77d4563a48c)

For example, I created an IT Team group with me and another user as a member, this IT Team group is a security group with Domain Admin privelages, this group can be used to add IT team members to be able to manage all of the users

![image](https://github.com/JordanPenaloza/AD-IT-Infrastructure-Lab/assets/113396128/12fc426c-215d-4324-bfb1-a0852a361e9c)

The marketing team should only be a member of domain users, they should not be able to manage any user privelages for AD therefore they are given the domain user group.

These security privelages can further be tweaked based on the requirements of the company.'

This project was very informational and fun to do and I can't wait to experiment more with AD




