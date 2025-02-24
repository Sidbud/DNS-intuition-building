<p align="center">
<img src="https://i.imgur.com/MwrkwEQ.png" alt="DNS Photo"/>
</p>

<h1>Understanding DNS</h1>
This lab focuses on DNS and how it is used. DNS is a fundamental concept in IT and many sources go over the theory behind it. I will be configuring DNS records and seeing how it works in practice. This is building up from a previous lab where I have a client joined to my domain mydomain.com. I am logged in as Jane Doe (an admin account) on both the client and domain controller VMs. In order for the lab to work, you need to be logged in as an administrator. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>DNS Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/af01ab64-20d7-4ee7-987e-dca3c37d5d6c)

![image](https://github.com/user-attachments/assets/3c5a38f2-7587-4ab0-83ed-eb2a9837a390)

![image](https://github.com/user-attachments/assets/7c449410-3d76-4693-b6d3-455abb822175)

![image](https://github.com/user-attachments/assets/a64b595b-72a9-4446-9d49-85207bccf282)

<p>
While logged in to the client as an admin, open the command prompt. When we ping "mainframe" it will fail. Nslookup "mainframe" provides similar results and shows that there is no DNS record for it. A DNS A-record will have to be created for mainframe on the domain controller. On the domain controller, open the DNS Manager and go to the domain you created within the Forward Lookup Zones tab. In my case, it is mydomain.com. Right click on the page and create a New Host. For name, input mainframe and the IP address should be the same IP as the domain controller so that ping can resolve. Refresh the DNS server so that the new record can be updated. Upon returning to the client, ping mainframe once again to see that it will now resolve.
</p>
<br />


![image](https://github.com/user-attachments/assets/821e7e82-40b1-4773-91a4-90e16ea9cccb)
![image](https://github.com/user-attachments/assets/ac511966-8573-45f8-819a-937bb1b43c5b)
![image](https://github.com/user-attachments/assets/f9ced43f-c67f-423e-9b54-0b5b7c5b7019)
![image](https://github.com/user-attachments/assets/847d5a94-f381-448f-8a46-6b951cc26967)

<p>
This next exercise will showcase the DNS cache. On the domain controller, I changed mainframe's record address to 8.8.8.8 (Google) and refreshed the DNS server. When pinging mainframe on the client, it will still ping the old IP address. When the command ipconfig /displaydns is run, it will reveal that the DNS cache still has the old IP. In order to update the cache, we need to clear it. The command ipconfig /flushdns will empty the cache so that when we ping mainframe again, the IP address will be updated to the new one on the client side. When pinging mainframe, the new IP address of the record will show.
</p>
<br />


![image](https://github.com/user-attachments/assets/7220b29e-bde0-43e4-8d83-be7f4481fcee)
![image](https://github.com/user-attachments/assets/dd622c22-5c7c-42b9-a17a-eb27de20dc7c)
![image](https://github.com/user-attachments/assets/9a563c83-ffba-4273-b027-5d43b86bab7f)

<p>
A CNAME record will now be made on the DNS server that will point "search" to Google. On the Forward Lookup Zones tab in the DNS Manager, open the tab that has the domain. Create a new CNAME record called search and point it to Google. Refresh the server to save the changes. On the client, pinging search and using nslookup will return the results of the CNAME record.
</p>
<br />

<h2>Lessons Learned </h2>

This lab made me understand how DNS directly works on a computer and the network. DNS records can change and sometimes this can cause connectivity issues. The DNS cache may have the old records and needs to be cleared out to update to the new records. The ipconfig /flushdns command is a common troubleshooting tool that has been referenced a lot in IT programs and I did not get a complete understanding how and why this works until I have done this lab. In the context of pinging "mainframe" at the start of the lab, the DNS cache gets checked first. If there is no result, the host file will be checked. The DNS server will be checked if there are no results in the host file. I made a record on the DNS server so a ping to mainframe can resolve. A CNAME record maps an alias name to another domain name. In this case, "search" was another name for Google.
