# Easy Ipxfer



## What

Easy Ipxfer is an application that **integrates** our official tool  "**ipxfer**." And it is a command-line application.  It can also deployed via GPO, SCCM. The app's PE file name is "**easyIpxfer.exe**." The latest version is 2.0.

The purpose of "easyIpxfer" is the same as "ipxfer" to migrate a security agent from one management server to another. However, "easyIpxfer" is focusing on migrating a security agent from one management server (Source Server) to another **"Apex One"** server (Destination Server), and it is more easier and powerful than the official 'ipxfer'.

SHA256 of the application:

Download link: 



## Why 

Easy Ipxfer is designed to simplify using "**ipxfer**" and make it smarter and safer to use.

##### Simplified

- Despite the ipxfer has ipxfer.exe and ipxfer_x64.exe. "easyIpxfer.exe" can be run on both x86 and x64 platforms.

- In contrast with ipxfer's compatibility with different versions of security agents. "easyIpxfer" is compatible to migrate 11.0, XG and Apex One security agents.

- Weak coupling between different parameters. Regardless of the relationship of the parameters. 
  For example, It is mandatory to attach "-s" and "-p" parameters in OfficeScan 11.0 and XG GM version ipxfer. In XG SP1 and Apex One version. "-s," "-p" and "-sp" are the mandatory parameters. With Apex One's ipxfer, you need to consider when to attach the "-off" parameter.

  But in "easyIpxfer," you ONLY need to provide the attribute you want customized.

  **Exception:** "-pwd" is the mandatory parameter in both 'ipxfer' and easyIpxfer regarding the security concerns.

##### Smart

- exit with a specified return code for different results
- will not migrate an agent not compatible with Apex One. Those agent platforms are Windows XP, Windows Server 2003, Windows Server 2008, Windows Vista, Windows 7 without SP, and Windows 8
- Just provide what attribute you want to customize is enough: The server name, port, NTSG certificate etc.
- Validate the parameters: Server IP or Name, Port number, Domain, Domain hierarchy, and NTSG certificate file

##### Safe

- When we use "ipxfer," the agent's unload password is visible without encryption in the command line. It is unsafe. "easyIpxfer" provides a way to attach an encrypted unload password in the command line.

##### Enhanced

- In "easyIpxfer," you can designate the agent(s) to conduct the migration procedure while the other agent(s) remain untouched. ([Need the assistance from China Core Team Solution Center Platform](https://tmsc.trendmicro.com/armory/#/login))
- More sources for fetching "OfficeScan Server NTSG"
  - "easyIpxfer" resource folder ([Need the assistance from China Core Team Solution Center Platform](https://tmsc.trendmicro.com/armory/#/login))
  - A user-defined URL in the command line
  - (By default) The migration destination server (preferably an Apex One server)



## How

1. To use "easyIpxfer"

   ```
   > easyIpxfer.exe -h
   ```

   ```
   # Agent Scope Parameters
       -all        Set the scope to all agents
       -domainID   Set the scope to agents with the domain UID 
                   matched in the pattern file
       -guid       Set the scope to agents with the agent GUID
                   matched in the pattern file
       -host       Set the scope to agents with the hostname
                   matched in the pattern file
       -ssrv       Set the scope to agents with the management
                   srver's name or IP address
       -serverID   Set the scope to agents with the management
                   server's server UID
   ```

   ```
   # Agent Attribue Parameters
       -dsrv       The destination management server
       -dport      The destination management server port for 
                   non-SSL communication
       -dsslport   The destination management server port for
                   SSL communication
       -dlsp       Agent listening port
       -dcer       To customize the certificate file source
       -dcerUrl    Fetch certificate file from a designate URL
       -domain     Reloacte an agent to the domain or a domain 
                   hierarchy
       -passwd     Agent unload password 
       -epasswd    The encrypted agent unload password
   ```

   ```
   # Password Encyption Parameter
       -enc        Encrypt the agent unload password
   ```

   ```
   # Command line example
   [Example]
       easyIpxfer.exe [-all] [-domainID] [-guid] [-host] [-ssrv] 
                      [-dsrv SERVER] [-dport PORT -dsslport PORT]
                      [-dlsp PORT] [-domain DomainName\SubDomain] 
                      [-passwd TEXT -epasswd BASE64] [-enc]  
                      [-dcer] [-dcerUrl URL] [ServerID]
   ```

   

2. Explanation of each parameter

   "-**passwd**"

   ```
   The agent's unload password.
   ```

   "-**epasswd**"

   ```
   The enhanced feature for the agent's unload password. This parameter accepts an encrypted password in the command line.
   You can get the encrypted password by the command:
   > easyIpxfer.exe -enc <Plaintext password>
   
   For example, my security agent's "unload password" is "P@ssw0rd"
   You can get the encrypted password by:
   > easyIpxfer.exe -enc P@ssw0rd
   > Yjg3NjZmMGM2NDE4YTQ4N2Y1MjRiYTYzOGNmOWI1Yzc=
   
   Then you can attach the ciphertext password to easyIpxfer command line by:
   > easyIpxfer.exe -epasswd Yjg3NjZmMGM2NDE4YTQ4N2Y1MjRiYTYzOGNmOWI1Yzc= <other parameters>
   ```

   "-**enc**"

   ```
   The parameter is used for encrypting a plaintext password. Please refer to "-epasswd" for more info.
   ```

   "-**all**"

   ```
   Let all security agents perform the migration action. It is not including the agents on which OS platforms are incompatible with Apex One.
   ```

   "-**domainID**"

   ```
   Executing the migration procedure for an agent when the agent's domain UID is contained in the application's pattern file. (The pattern file can be created by China Core Team Solution Center Platform.)
   
   You can get the domain UID from the following 2 locations:
       - Management server Database (Recommend)
       - HKEY_LOCAL_MACHINE\SOFTWARE{\WOW6432Node}\TrendMicro\PC-cillinNTCorp\CurrentVersion@DomainID
   ```

   "-**guid**"

   ```
   Executing the migration procedure for an agent when the agent itself's UID is contained in the application's pattern file.
   
   You can get the agent UID from the following 2 locations:
       - Management server Database (Recommend)
       - HKEY_LOCAL_MACHINE\SOFTWARE{\WOW6432Node}\TrendMicro\PC-cillinNTCorp\CurrentVersion@GUID
   ```

   "-**host**"

   ```
   Executing the migration procedure for an agent when the agent's hostname is contained in the application's pattern file.
   
   You can get the domain UID from the following 2 locations:
       - Management server Database (Recommend)
       - Type "hostname" in the command line
   ```

   "-**ssrv**"

   ```
   Executing the migration procedure for an agent when the agent's current management server's server name or IP address is contained in the application's pattern file.
   
   You can get the server name from the following 2 locations:
       - HKEY_LOCAL_MACHINE\SOFTWARE{\WOW6432Node}\TrendMicro\PC-cillinNTCorp\Server@GUID
   ```

   **"-serverID"**

   ```
   Executing the migration procedure for an agent when the agent's current management server's UID is contained in the application's pattern file.
   
   You can get the server UID from
   	- HKEY_LOCAL_MACHINE\SOFTWARE{\WOW6432Node}\TrendMicro\PC-cillinNTCorp\CurrentVersion\Misc.@ServerID
   ```

   "-**dsrv**"

   ```
   Migrate a security agent from the current management server to another designated Apex One server followed after "-dsrv"
   
   Example:
   > easyIpxfer.exe -epasswd {ciphertext password} -dsrv {Apex One Server}
   OR
   > easyIpxfer.exe -passwd {plaintext password} -dsrv {Apex One Server}
   ```

   "-dport"

   ```
   Update the current management server's HTTP communication port (TCP) for a security agent. The port number is followed after "-dport"
   
   Example:
   > easyIpxfer.exe -epasswd {ciphertext password} -dport {Port Number}
   OR
   > easyIpxfer.exe -passwd {plaintext password} -dport {Port Number}
   ```

   "-**dsslport**"

   ```
   Update the current management server's HTTPS communication port (TCP) for a security agent. The port number is followed after "-dsslport"
   
   Example:
   > easyIpxfer.exe -epasswd {ciphertext password} -dsslport {Port Number}
   OR
   > easyIpxfer.exe -passwd {plaintext password} -dsslport {Port Number}
   ```

   "-**dlsp**"

   ```
   Update a security agent's listening port, which is used for the management server connecting back to this agent for notification and task assignment. The port number is followed after "-dlsp."
   
   Example:
   > easyIpxfer.exe -epasswd {ciphertext password} -dlsp {Port Number}
   OR
   > easyIpxfer.exe -passwd {plaintext password} -dlsp {Port Number}
   ```

   "-**domain**"

   ```
   While migrating a security agent, you can designate a domain\subdomain on the destination server's agent tree showing in the web console.
   
   Example:
   > easyIpxfer.exe -epasswd {ciphertext password} -domain {Domain\SubDomain}
   OR
   > easyIpxfer.exe -passwd {plaintext password} -domain {Domain\SubDomain}
   ```

   "-**dcer**"

   ```
   Tell easyIpxfer where to update the NTSG certificate for a security agent while performing a migration.
   While enclosing "-dcer" in the command line, the expected NTSG certificate file should be saved in the easyIpxfer's resource folder. You need to login China Core Team Solution Center Platform to pack the file into the easyIpxfer app.
   
   Example:
   > easyIpxfer.exe -epasswd {ciphertext password} -dcer
   OR
   > easyIpxfer.exe -passwd {plaintext password} -dcer
   
   If you do not enclose "-dcer" in the command line, easyIpxfer will try to get the NTSG file from the management server, or the server followed after "-dsrv."
   In some special conditions, for example:
   A security agent cannot reach the management server, or the server followed after "-dsr," but the customer still wants to migrate the agent right away. At this moment, you can attach "-dcer" in the command line after repacking the easyIpxfer app from China Core Team Solution Center Platform, or add one more parameter "-dcerUrl" which will be introduced later on this page.
   ```

   "-**dcerUrl**"

   ```
   This parameter expects to work with "-dcer."
   The parameter is used to tell easyIpxfer to update the NTSG certificate for a security agent while performing a migration. And easyIpxfer will try to download the expected NTSG certificate file from the URL followed after "-dcerUrl." This is a workaround while you are inconvenient to reach China Core Team Solution Center Platform to create a new easyIpxfer package with the right NTSG file.
   
   Example:
   > easyIpxfer.exe -epasswd {ciphertext password} -dcer -dcerUrl https://2.2.2.2/download/ofcNTCer.dat
   OR
   > easyIpxfer.exe -passwd {plaintext password} -dcer -dcerUrl https://2.2.2.2/download/ofcNTCer.dat
   ```

   

3. Example of some scenarios to use "easyIpxfer" to simplify the agent migration procedure

   **Example 1**

   ```
   There is an Apex One server administrator. He needs to change the Apex One server's HTTP and HTTPS ports from 8080 and 4343 to 80 and 443.
   1. Login to the Apex One server's management console
   2. Change the ports in the console
   3. Deploy easyIpxfer via GPO by running the command:
      > easyIpxfer.exe -epasswd {ciphertext password} -dport 80 -dsslport 443 -all
   ```

   **Example 2**

   ```
   There is an Apex One server which is managing around 50,000 security agents. With the concern on server workload, the administrator wants to add a new Apex One server. And each of the servers will manage 25,000 agents.
   The administrator can achieve the request by:
   1. Create a new domain in the old Apex One server's management console
   2. Move 25,000 security agents to the newly created domain.
   3. Get the new domain UID from the database, for example, 11111111-1111-1111-1111-111111111111
   4. Login China Core Team Solution Center Platform and file a request to repack the easyIpxfer app by adding    11111111-1111-1111-1111-111111111111 to the pattern file.
   5. Deploy easyIpxfer via GPO by running the command:
      > easyIpxfer.exe -epasswd {encrypted password} -dsrv {New Apex One Server} -dport {Port number} -dsslport {Port number} -dlsp {Port number} -domainUID
   6. The agents having domain UID 11111111-1111-1111-1111-111111111111 in its registry will perform migration.
   ```

   **Example 3**

   ```
   There is an administrator who is managing 20 OfficeScan/ApexOne servers. Because the company brought some new powerful servers, and the current servers' OS has reached EOL. The administrator wants to combine 11 servers' agents to a new Apex One server.
   The administrator can achieve the request by:
   1. Login China Core Team Solution Center Platform and file a request to repack the easyIpxfer app by adding ten servers, which will later be retired, to the pattern file. (The servers' information can be found in the ofcscan.ini file in PCCSRV\ofcscan.ini@Master_DomainName)
   2. Deploy easyIpxfer via GPO by running the command:
      > easyIpxfer.exe -epasswd {encrypted password} -dsrv {New Apex One Server} -dport {Port number} -dsslport {Port number} -dlsp {Port number} -ssrv
   3. The agents with the same Server Name in the pattern file and in its registry will perform the migration.
   ```

   

## Exit Code

| Code | Meaning                                                      |
| ---- | ------------------------------------------------------------ |
| 0    | Exit without any error                                       |
| 1    | Exit with no need to perform migration                       |
| 2    | Exit with performing migration failed                        |
| 3    | Exit with not performing migration because of some unexpected reason |
| 11   | Exit with printing help message                              |
| 12   | Exit with printing ciphertext agent unload password by parameter "-enc" |
| 21   | Failed to encrypt a string                                   |
| 22   | Failed to get the product registry information               |
| 23   | Product not installed                                        |
| 24   | Failed to get the product service                            |
| 25   | Product installation concern                                 |
| 26   | Unsupported product version                                  |
| 27   | Agent unload password not provided                           |
| 28   | Provided destination server information invalid in the command line |
| 30   | Invalid port number: server http port                        |
| 31   | Invalid port number: server https port                       |
| 32   | invalid port number: local server port                       |
| 33   | invalid URL                                                  |
| 34   | Certificate file download failed from the designate URL      |
| 35   | Invalid certificate file                                     |
| 36   | Invalid domain and its hierarchy                             |
| 37   | File downloading failed: package.ini                         |
| 38   | File downloading failed: settingProfile.ini                  |
| 39   | Get ofcscan.ini file failed                                  |
| 40   | The destination server version unsupported                   |
| 41   | The destination server version unknown                       |
| 42   | command line incomplete                                      |
| 50   | OS non-supporting Apex One                                   |





## Who

This app is created by Client Server Group of China Core Team. 



## Further Plan

"easyIpxfer" with UI