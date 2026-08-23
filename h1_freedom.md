# h1 Freedom of Action, Control, and Risk Mitigation

## a) Definition of ISMS (Information Security Management System) for home network

<br>

### a1) What is included in the scope:

**Basic network infrastructure:**
- Home router with Wi-Fi and firewall functions

**Devices used for the exercises:**
- Windows desktop PC
- Windows laptop
- Linux VMs (Kali/Debian)
- Android phone (often connected to the same network through Wi-Fi. Also used for MFA.)

**Information and data:**
- Online course materials
- Personal study notes stored locally, in GitHub repositories and on OneDrive
- Course-related source code and other exercise files
- Credentials and authentication information used for course-related platforms (logins, MFA, possible SSH/API keys)

<br>

### a2) What is excluded from the scope and why:

**Exclusions:**
> - Devices belonging to other family members because they are not owned or managed by me.
> - Smart TV because it is not relevant to the course exercises and not part of the study environment.
> - ISP's network and infrastructure because it is outside of my control and management.
> - The infrastructure of cloud/web service providers (GitHub, OneDrive, course homepage) because they are managed by their respective service providers and not me.

<br>

### a3) Key interfaces and boundaries:

**Home network:**
- The home router and firewall form the main boundary between my home network and the internet.
- The ISP provides the internet connection service and is outside the scope.

**Cloud services:**
- GitHub, OneDrive and the course homepage are external services used to store and access course-related information.
- The services themselves are outside the scope, but my personal accounts and data stored on them are relevant to it.

**Remote connections:**
- SSH may be used to connect to/from virtual machines.
- VPN may be used to connect to external networks or services.

**Suppliers and service providers:**
- The main external service providers are my ISP, cloud service providers, and device manufacturers/vendors.
- Their infrastructure and services are outside my direct control, forming an external boundary to my environment.

<br>

### Deliverables:

**Diagram of the network and interface:**

<img width="831" height="710" alt="ISMS Scope Diagram" src="https://github.com/user-attachments/assets/e71812b2-5bce-44de-b6a2-f52690f4fb45" />

**What evidence could I provide?**

Home router:
- Screenshot of the router's configuration page showing Wi-Fi and firewall settings.

Physical devices and virtual machines:
- A device inventory
- Screenshots of devices connected to the same network

Course materials and notes:
- Screenshots of the relevant OneDrive folders and GitHub repositories (including contents).

MFA:
- Screenshots of the relevant accounts' security settings showing that MFA is enabled.
- Screenshot of the MFA app from the phone (with sensitive information hidden).

Network boundaries:
- Screenshot of router's configuration showing the connection between home network and ISP/internet.
