<h1>Cisco Router Radius AAA</h1>


<h2>TLDR Description</h2>
Configuring a Windows Server 2019 RADIUS server to authenticate management logins on a Cisco router.
<br />

<h2>Purpose</h2>
The purpose of this lab is to configure Radius Authentication using Windows Server 2019 to authenticate logins for a management user on a cisco router.
<br />

<h2>Background Info</h2>
RADIUS or Remote Authentication Dial-In User Service is a service which allows for AAA (authentication, authorization, and accounting) management and it was first introduced in 1991. The basic idea of RADIUS authentication is that the radius client sends a authentication request to the server which holds the database of accounting, the RADIUS server then replies with one of 3 things, access reject, access challenge, access accept. Access reject and accept are exactly how they sound, within these replies the RADIUS server either rejects or accepts the request sent by the client. Access challenge is when the RADIUS server replies with a request back to the client for more information such as a secondary access password.
<br />

<h2>Lab Summary</h2>
I began this lab by configuring my windows server to utilize the NPAS service which allows for radius authentication requests from clients. After my configurations on the windows server side were complete I configured the router to utilize my radius server as the primary authentication database, and finally tested it by attempting to log in with the credentials of a user only on the active directory on my windows radius server.
<br />

<h2>Lab Commands</h2>
  -  aaa new-model – verifying aaa model.
<br />
  -  aaa group server radius radius-server1 – starts the radius connection config.
<br />
  -  server-private &lt;your-windows-radius-server-ip&gt; key &lt;your-preset-radius-key&gt; – specifies the location of the intended radius server.
<br />
  -  ip radius source-interface &lt;the vlan or interface you want radius to send FROM on the cisco switch&gt; – configures the source interface of a radius request from the router.
<br />
  -  aaa authentication login default group radius-server1 local
<br />
  -  aaa authentication login console group radius-server1 local
<br />
  -  aaa authorization console
<br />
  -  aaa authorization exec default group radius-server1 local – these 4 commands configure the router to use the radius before local database for authentication.
<br />
  -  ip host &lt;your-AD-domain&gt; &lt;your DC ip address&gt; – specifies directory used for accounting.
<br />

<h2>Network Diagram</h2>
<p align="center">
  <img src="images/img1.png" height="60%" width="60%" alt="Cisco Router Radius AAA"/>
</p>
<br />

<h2>Walk-Through:</h2>
<br/>
<h4>Configurations on Windows Server:</h4>

<p align="center">
<img src="images/img2.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img3.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img4.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Selected Active Directory Domain Services and Network Policy and Access Services.<br/>
<img src="images/img5.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Then selecting install the required services.<br/>
<img src="images/img6.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Then I had to configure the Active Directory to be used.<br/>
<img src="images/img7.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img8.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img9.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img10.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img11.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Registering the NPS server to the active directory.<br/>
<img src="images/img12.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img13.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Creating a security group to hold our radius accounts.<br/>
<img src="images/img14.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img15.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Then creating a test user to verify configurations later on.<br/>
<img src="images/img16.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Configuring the router as a RADIUS Client<br/>
<img src="images/img17.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img18.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Adding windows groups as a condition.<br/>
<img src="images/img19.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Then setting our configured security group to be used as the main accounting source for the router.<br/>
<img src="images/img20.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Granting access to correct credentials.<br/>
<img src="images/img21.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img22.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img23.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img24.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Configuring cisco vendor specific attributes.<br/>
<img src="images/img25.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img26.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Verify configs and finish.<br/>
<img src="images/img27.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Enabled ignore user account dial-in properties.<br/>
<img src="images/img28.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
</p>

<h4>Configurations on Cisco Router:</h4>
<p align="center">
<img src="images/img29.png" height="60%" width="60%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img30.png" height="60%" width="60%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img31.png" height="60%" width="60%" alt="Cisco Router Radius AAA"/>
<br />
<br />
</p>

<h4>Proof of Functionality:</h4>
<br/>
<p align="center">
Verifying configurations:<br/>
<img src="images/img32.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img33.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img34.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img35.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Creating test user and adding it to radius user group.<br/>
<img src="images/img36.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Successful login using user testfinal from radius server<br/>
<img src="images/img37.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img38.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<img src="images/img39.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Seeing successful radius request and authentication through windows event viewer.<br/>
<img src="images/img40.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
Users password is encrypted as seen in wireshark.<br/>
<img src="images/img41.png" height="80%" width="80%" alt="Cisco Router Radius AAA"/>
<br />
<br />
</p>

<h2>Problems</h2>
When configuring the Active Directory Domain Services as I attempted to configure my NetBIOS name I encountered an error telling me the name is already being used. I realized that as I have another virtual machine running windows server I must’ve configured with the name I was trying to configure on my new machine. I simply added a 0 to the end of the name and it worked fine and I was able to save my configurations.
<br/>
<br/>

<h2>Conclusion</h2>
In this lab I configured a Windows Radius Server fully from scratch to allow for radius authentication for access into our router. I had to configure security groups and users within windows active directory for the router to draw from. I configured the connections between the router and my radius server through the NPAS service within windows server and then I configured the router to draw user authentication from my radius server prior to trying its internal local database. Once all the configurations were finished, I verified the configurations by creating a new user in active directory associated with my cisco security group and attempted to access my router using that users credentials, which it did. I also used Wireshark to visually see the process and verify that the users credentials are not insecurely available in clear text.
<br />
