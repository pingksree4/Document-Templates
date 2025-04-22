# Sample & Reference Diagrams



```mermaid
mindmap
Root (Ensurity's XSense IdP Solution)
  Flexible Deployment
    OnPremises
      DataCenter
      id)Own Cloud(
    id)SaaS(
      Tenant Access
  **User Sync**
    AD
    LDAP
    AAD
  Supported Authenticators
    FIDO2 Key
    XSense Verify
    TOTP
    SMS
  Applications for Secure Authentication
    CP Plugin
      AD joined Windows Workstations
      AD joined Windows Servers
      AD joined RDP systems
    SAML / OIDC
      Web Applications
    SDK / API
      Home grown Applications

```


```mermaid
---
title: XSense IdP Solution
---
erDiagram
    "XSense MFA" ||--|{ Deployment : setup
    Deployment ||--|{ "Ensurity Cloud" : SaaS
    Deployment ||--|{ On-Premises : flexible
    On-Premises ||--|{ Data-Center : own
    On-Premises ||--|{ Own-Clod : own
    "XSense MFA" ||--|{ "Secure Authenticators" : supports
    "Secure Authenticators" ||--|{ ThinC-AUTH : FIDO2
    "Secure Authenticators" ||--|{ "XSense Verify" : Patented
    "XSense Verify" ||--|{ "Android" : Smartphone
    "XSense Verify" ||--|{ "iOS" : Smartphone
    "Secure Authenticators" ||--|{ TOTP : OATH
    TOTP ||--|{ Mobile : "iOS/Android"
    TOTP ||--|{ Desktop : "Windows"
```

---



```mermaid
sequenceDiagram
autonumber
    participant AD as Active<br>Directory
    actor USER as Enterprise<br> User
    participant SYS as Windows <br>Workstations <br>/ Servers
    participant CPP as XSense<br> CP Plugin
    participant XS as XSense <br>IdP Server
    participant MFA as Secure <br>Authenticators
    
    note over AD,SYS: "Existing Enterprise Infrastructure"
    AD -->> USER: Domain Users
    AD -->> SYS: Domain joined
    
    note over CPP,MFA: Ensurity Authentication Solution for<br> AD-joined Windows systems
    AD -) XS: User sync from AD to XSense Server
    CPP ->> SYS: Deploying <br>CPP (Credential Provider Plugin)
    USER ->> MFA: Registration of <br>Multi-Factor Authenticators
    Note right of MFA: • Biometric FIDO2<br>• XSense Verify App<br>• OATH-TOTP Codes
    
    note over AD,MFA: "Secure Authentication Process"
    USER ->> SYS: Initiating<br>Authentication process
    activate USER
    activate SYS
    SYS ->> CPP: Initiating<br>Authentication process
    activate CPP
    CPP ->> AD: User validation<br>(Username/Password)
    activate AD
    AD --) AD: User validation
    AD ->> CPP: Successful validation
    deactivate AD
    CPP ->> MFA: Seeking Multi-factor authentication
    activate MFA
    Note right of MFA: • Biometric FIDO2<br>• XSense Verify App<br>• OATH-TOTP Codes
    MFA ->> USER: Seeking User Approval for Authentication
    USER ->> MFA: Validation Approval
    MFA ->> CPP: Validation Successful
    deactivate MFA
    CPP ->> SYS: Authentication successful
    deactivate CPP
    USER ->> SYS: Secure Accessibility
    deactivate SYS
    deactivate USER
```

---

```mermaid
sequenceDiagram
autonumber
    actor USER as User Activity
    participant ICICILA as ICICI Lombard<br>Application
    participant XIB as XSense<br>Identity Broker
    participant XIDAM as XSense<br>IDAM
    
    note over USER,XIDAM: Global Admin (ICICI Lombard) creates Tenants for every Vendor
    USER -->> USER: Global Admin<br>Login to XSense IDAM<br>with Global Admin Credentials
    activate USER
    USER ->> XIDAM: Global Admin creates a Tenant-ID (with Credentials) for each of their Vendor
    deactivate USER
    
    note over USER,XIDAM: Every Vendor's Admin creates their Users
    USER -->> USER: Admin @ each Vendor<br>Login with <br> Vendor Tenant-ID Credentials
    activate USER
    USER ->> XIDAM: Each Vendor's Admin create their Users (short-term) with user-wise credentials
    deactivate USER
    
    note over USER,XIDAM: Users (@ every Vendor) Login Process
    USER ->> ICICILA: Requests Access to Application
    activate USER
    activate ICICILA
    ICICILA ->> XIB: Request Authentication
    activate XIB
    XIB ->> XIDAM: Authentication Request
    activate XIDAM
    XIDAM ->> USER: Challenge User Credential / Consent
    USER ->> XIDAM: Approve User Credential
    XIDAM ->> XIB: Authentication Response
    deactivate XIDAM
    XIB ->> XIB: Local Authentication / <br>Identity Federation
    XIB ->> ICICILA: Authentication Response
    deactivate XIB
    ICICILA ->> USER: Allow Access to <br>the requested resource
    deactivate ICICILA
    deactivate USER
    
    
```

---

```mermaid
sequenceDiagram
autonumber
    participant APP1 as Client<br>Application 1
    participant EnSafe as Ensurity<br>EnSafe
    participant APP2 as Client<br> Application 2
    
    APP1 ->> EnSafe: Send payload to encrypt
    EnSafe -->> EnSafe: Check the Client1 identity token <br>& authorization policy,<br> if not present, redirect to authentication
    EnSafe -->> EnSafe: Verify the Key and encrypt the data
    EnSafe ->> APP1: Issue encrypted payload to Client1
    APP1 ->> APP2: Send the encrypted payload to Client2
    APP2 ->> EnSafe: Send encrypted payload for decryption
    EnSafe -->> EnSafe: Check the Client2 identity token <br>& authorization policy,<br> if not present, redirect to authentication
    EnSafe -->> EnSafe: Verify the Key and decrypt the data
    EnSafe ->> APP2: Send the decrypted data to Client2
```

---

```mermaid
sequenceDiagram
autonumber
    participant XSDB as XSense IdP <br>Database Server
    participant XSID as XSense <br>Identity Server
    participant SYS as User <br> System
    actor User
    participant XSMV as XSense <br>Mobile App<br>@User

    User ->> XSID: Initiate User Registration
    XSID -->> XSDB: Update DB
    XSID ->> User: Mail
    
```


----

```mermaid
flowchart TD
    A(User Interface) --> B(Relying Party)
    B --> C(Platform)
    C --> D(Authenticator)
    D --> E(FIDO2 Server)
    E --> F(Platform)
    F --> B
```

---


```mermaid
%%{init: {"pie": {"textPosition": 0.5}, "themeVariables": {"pieOuterStrokeWidth": "1px"}} }%%
pie showData

    "Dogs" : 86
    "Cats" : 85
    "Rats" : 15
```
pie title Pets adopted by volunteers


---

```mermaid
---
config:
  layout: dagre
---
flowchart TD
    n1(["N1"]) --> n2["N2"]
    n2 --> n4["N4"] & n5["N5"]
    n2 ==> n3["N3"]
    n4 --> n6["N6"] & n7["N7"]
    n5 --> n8["N8"]
    n3 --> n8
    n6 --> n8 & n9["N9"] & n10["N10"]
    n7 --> n9
    n10 --> n11["N11"] & n12["N12"]
    n11:::Aqua
    classDef Aqua stroke-width:1px, stroke-dasharray:none, stroke:#46EDC8, fill:#DEFFF8, color:#378E7A
    style n2 stroke-width:4px,stroke-dasharray: 0,fill:#757575
    
```

---

```mermaid
quadrantChart
    title Reach and engagement of campaigns
    x-axis Low Reach --> High Reach
    y-axis Low Engagement --> High Engagement
    quadrant-1 We should expand
    quadrant-2 Need to promote
    quadrant-3 Re-evaluate
    quadrant-4 May be improved
    Campaign A: [0.3, 0.6]
    Campaign B: [0.45, 0.23]
    Campaign C: [0.57, 0.69]
    Campaign D: [0.78, 0.34]
    Campaign E: [0.40, 0.34]
    Campaign F: [0.35, 0.78]
```

---




```mermaid
sequenceDiagram
autonumber
    participant User
    participant macOS
    participant Smartcard
    participant Keychain
    participant AuthenticationService

    User->>macOS: Insert Smartcard
    macOS->>Smartcard: Detect and read certificates
    Smartcard-->>macOS: Return certificate data
    macOS->>Keychain: Verify certificate validity
    Keychain-->>macOS: Certificate is valid
    macOS->>AuthenticationService: Authenticate user with Smartcard certificate
    AuthenticationService-->>macOS: Authentication success/failure
    macOS->>User: Login successful or denied
```

---

# FAQ for CPP

## How do i disable the "Other user" option when logging into Windows on a PC joined to AD?

To use a Group Policy Object (GPO) to disable the "Other Users" option in the Windows logon screen when the PC is joined to Active Directory, you can follow these steps:

- Open the Group Policy Management Console.
- Create a new Group Policy Object or select an existing Group Policy Object to edit.
- Right-click the selected GPO and select Edit to open the Group Policy Management Editor.
- Navigate to the following location in the editor: **`Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options`.**
- Find the policy setting called "**`Interactive logon: Do not display last user name`**" and double-click it.
- Select the Enabled option to prevent the last logged in user name from being displayed on the login screen.
- Click Apply, then OK to save your changes.

By enabling the "**`Interactive logon: Do not display last user name`**" policy, the "**`Other users`**" option on the Windows logon screen is disabled. This policy ensures that only the last logged-in user's username is displayed, preventing other users from entering their credentials directly on the login screen.

## 

---

<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUA
AAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO
9TXL0Y4OHwAAAABJRU5ErkJggg==" alt="Red dot">

<img alt="Ensurity Logo" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAKcAAAAeCAYAAABJ0w5RAAAACXBIWXMAACxLAAAsSwGlPZapAAAGlmlUWHRYTUw6Y29tLmFkb2JlLnhtcAAAAAAAPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgOS4xLWMwMDIgNzkuYTZhNjM5NjhhLCAyMDI0LzAzLzA2LTExOjUyOjA1ICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOmRjPSJodHRwOi8vcHVybC5vcmcvZGMvZWxlbWVudHMvMS4xLyIgeG1sbnM6cGhvdG9zaG9wPSJodHRwOi8vbnMuYWRvYmUuY29tL3Bob3Rvc2hvcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RFdnQ9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZUV2ZW50IyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgMjUuMTEgKE1hY2ludG9zaCkiIHhtcDpDcmVhdGVEYXRlPSIyMDI0LTA0LTAzVDEzOjMyOjUyKzA1OjMwIiB4bXA6TW9kaWZ5RGF0ZT0iMjAyNS0wMi0yMFQxMzoxMDowMCswNTozMCIgeG1wOk1ldGFkYXRhRGF0ZT0iMjAyNS0wMi0yMFQxMzoxMDowMCswNTozMCIgZGM6Zm9ybWF0PSJpbWFnZS9wbmciIHBob3Rvc2hvcDpDb2xvck1vZGU9IjMiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6MzI1MzYzZTMtMThiNC00MmM4LWFjOWItYTBmYzE3NjA2Y2YzIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOmJmOTlkZTRiLTBmODMtNDRlYi05MTU4LTIyMjFjNjM1MGViMSIgeG1wTU06T3JpZ2luYWxEb2N1bWVudElEPSJ4bXAuZGlkOmJmOTlkZTRiLTBmODMtNDRlYi05MTU4LTIyMjFjNjM1MGViMSI+IDx4bXBNTTpIaXN0b3J5PiA8cmRmOlNlcT4gPHJkZjpsaSBzdEV2dDphY3Rpb249ImNyZWF0ZWQiIHN0RXZ0Omluc3RhbmNlSUQ9InhtcC5paWQ6YmY5OWRlNGItMGY4My00NGViLTkxNTgtMjIyMWM2MzUwZWIxIiBzdEV2dDp3aGVuPSIyMDI0LTA0LTAzVDEzOjMyOjUyKzA1OjMwIiBzdEV2dDpzb2Z0d2FyZUFnZW50PSJBZG9iZSBQaG90b3Nob3AgMjUuMTEgKE1hY2ludG9zaCkiLz4gPHJkZjpsaSBzdEV2dDphY3Rpb249InNhdmVkIiBzdEV2dDppbnN0YW5jZUlEPSJ4bXAuaWlkOjM5MTg3YTZmLWNjNmItNGMzMi05NjQwLTNmZjg5ZjdiOThhYiIgc3RFdnQ6d2hlbj0iMjAyNS0wMi0yMFQxMzowMDo0NSswNTozMCIgc3RFdnQ6c29mdHdhcmVBZ2VudD0iQWRvYmUgUGhvdG9zaG9wIDI1LjExIChNYWNpbnRvc2gpIiBzdEV2dDpjaGFuZ2VkPSIvIi8+IDxyZGY6bGkgc3RFdnQ6YWN0aW9uPSJzYXZlZCIgc3RFdnQ6aW5zdGFuY2VJRD0ieG1wLmlpZDozMjUzNjNlMy0xOGI0LTQyYzgtYWM5Yi1hMGZjMTc2MDZjZjMiIHN0RXZ0OndoZW49IjIwMjUtMDItMjBUMTM6MTA6MDArMDU6MzAiIHN0RXZ0OnNvZnR3YXJlQWdlbnQ9IkFkb2JlIFBob3Rvc2hvcCAyNS4xMSAoTWFjaW50b3NoKSIgc3RFdnQ6Y2hhbmdlZD0iLyIvPiA8L3JkZjpTZXE+IDwveG1wTU06SGlzdG9yeT4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7zrBaSAAAIOklEQVR42u1cZ3BVRRQOCUVABDSAwQQDGAQBy2BBMYIgqIQyDDIiExuCMhQx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" />

---

```mermaid
flowchart LR
    %% Define subgraphs for each branch

    subgraph PrimaryBranch[Primary Branch]
    PBAD[Primary<br>AD<br>.]
    PBSQL[XSense IP &<br> SQL Server<br>.]
    PBClients[Systems with <br>XSense CPP <br>.]
    PBAD ---> PBClients
    PBAD ---> PBSQL
    PBClients ---> PBSQL
    end
    
    subgraph ChildBranch1[Child Branch 1]
    CB1AD[Branch <br>AD<br>.]
    CB1SQL[XSense IP &<br> SQL Server<br>.]
    CB1Clients[Systems with <br>XSense CPP <br>.]
    CB1AD --> CB1Clients
    CB1AD --> CB1SQL
    CB1Clients --> CB1SQL
    end
    
    subgraph ChildBranch2[Child Branch 2]
    CB2AD[Branch <br>AD<br>.]
    CB2SQL[XSense IP &<br> SQL Server<br>.]
    CB2Clients[Systems with <br>XSense CPP <br>.]
    CB2AD --> CB2Clients
    CB2AD --> CB2SQL
    CB2Clients --> CB2SQL
    end
    
    %% Show site-to-site VPN links (conceptual)
    PrimaryBranch <=======> ChildBranch1
    PrimaryBranch <==> ChildBranch2
    
    %% Indicate Peer-to-Peer Replication among SQL Servers
    PBSQL -- Peer-to-Peer <--> CB1SQL
    PBSQL -- Peer-to-Peer <--> CB2SQL
    CB1SQL -- Peer-to-Peer <--> CB2SQL
    
    %% Optional styling
    linkStyle default stroke:#999,stroke-width:4px,fill:none
    style PrimaryBranch stroke:#009900,stroke-width:4px,stroke-dasharray: 5 5
    style ChildBranch1 stroke:#FF9933,stroke-width:4px,stroke-dasharray: 5 5
    style ChildBranch2 stroke:#0066CC,stroke-width:4px,stroke-dasharray: 5 5
```

---

```mermaid
---
config:
  layout: dagre
  look: classic
  theme: default
---

flowchart LR
A[Start] --> B{Choose Path}
B -->|Option 1| C[Path 1]
B -->|Option 2| D[Path 2]
```

---

- What is PIV / Certificate-based Authentication?
  - Supported scenarios				
  - Unsupported scenarios				

- Pre-requisites for PIV	
  - Certificate Requirements and Enumeration
	 - Windows Server			
	   - CA Service for Domain Controller
	     - Generating a Certificate for AD User
	       - Configuration Parameters
		  - Export Public Certificate 
		  - Export Public+Private Certificate 
		  - Delete/Revoke Certificate	
		- Configure Systems to enable Smart-card login
	- Entra ID			

- Loading Certificate onto BioPro Key	

    
 
---


```mermaid
stateDiagram 
    [*] --> First
    state First {
        [*] --> second
        second --> alternative
        second --> [*]
        alternative --> [*]
    }
    First --> Second
```

---

```mermaid
flowchart LR
    n1((("Certificate <br>Authority")))
    n2[["Active<br>Directory"]]
    n3["AD<br>Users"]
    n4{{"PKI<br>Certificates"}}
    n5[["Windows<br>Workstations"]]
    n6(["AUTH BioPro<br>Security Keys"])

    n1 <--> n2
    n2 --> n3
    n3 <-- "Enroll Fingerprints" --> n6
    n3 --> n4
    n4 --> n6
    n2 <-- "Domain Joined" --> n5
    n6 -- "PIV Login" --> n5 
    n1 --> n4
    
```

---

```mermaid
flowchart LR
  A e1@==> B
  e1@{ animate: true }
```

---

```mermaid
flowchart LR
    HW[["DL160 Server HW"]]
    OS(["Win SRV 2016"])
    IIS(["IIS Service"])
    SQL(["MS SQL DB"])
    XS(["XSense MFA Server"])
    VM(["HyperV"])
    LX(["Ubuntu Linux"])
    DK(["Docker"])
    AX(["AUTHx Server"])
    MDB(["Mongo DB"])
    AD(["Active Directory"])
    
    AD --> XS
    HW --> OS
    OS --> AD
    OS --> IIS
    IIS --> XS
    XS --> SQL
    OS --> SQL
    OS --> VM
    VM --> LX
    LX --> DK
    DK --> AX
    DK --> MDB
    
```


