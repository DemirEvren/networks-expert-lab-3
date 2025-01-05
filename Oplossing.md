# PE 2 Networks Expert LAB 3

## Part 1: Install the Virtual lab environment
### Task Preparation and Implementation:
- Installeren VM > Downloaden van DEVASC.OVA > Importeren en stappen volgen  
### Task Troubleshooting:
- Geen problemen tegengekomen. Alles werkte prima.
### Task Verification:
- Openen Nieuwe VM. Aanmelden op Packet tracer op VM. 
- Door GUI verkennen. (Linux commando's ingeven op terminal...)

## Part 2: install the CSR1000v VM
### Task Preparation and Implementation:
- Installeren .OVA en .ISO file
- Importeren .OVA 
- .ISO importeren op DISK 1 van VM settings.
- VM opstarten
### Task Troubleshooting:
- Geen problemen tegengekomen
### Task Verification:
- De router start op.
- SSH connectie gemaakt en verbonden via Client Laptop met router voor betere bediening en scrolling van CLI. 

## Part 3: Python Network automation with NETMIKO 
### 3a: Connecting to an IOS-XE Device
Start with a simple script and gradually expand it with additional features.

• Send show commands to a single device
``` python3
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_xe",
    "host": "192.168.0.50",  # Router's IP
    "username": "pxl",  # Username
    "password": "pxl",  # Password
    "secret": "pxl",  # Enable password
}

connection = ConnectHandler(**device)
connection.enable()

print("Connected to Router 1 Successfully")
connection.disconnect()
```
• Send configuration commands to a single device
``` python3
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_xe",
    "host": "192.168.0.50",  # Router's IP
    "username": "pxl",  # Username
    "password": "pxl",  # Password
    "secret": "pxl",  # Enable password
}

connection = ConnectHandler(**device)
connection.enable()

config_commands = ["interface loopback0", "ip address 10.1.1.1 255.255.255.0", "no shutdown"]
connection.send_config_set(config_commands)


connection.disconnect()
```
<!-- !['send_cmd'](/images/send_command.png)  -->
• Run show commands and save the output
``` python3
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_xe",
    "host": "192.168.0.50",  # Router's IP
    "username": "pxl",  # Username
    "password": "pxl",  # Password
    "secret": "pxl",  # Enable password
}

connection = ConnectHandler(**device)
connection.enable()

output = connection.send_command("show running-config")
with open("running_config.txt", "w") as file:
    file.write(output)


connection.disconnect()
```
• Backup the device configurations to an external file
``` python3
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_xe",
    "host": "192.168.0.50",  # Router's IP
    "username": "pxl",  # Username
    "password": "pxl",  # Password
    "secret": "pxl",  # Enable password
}

connection = ConnectHandler(**device)
connection.enable()

output = connection.send_command("show running-config")
with open("running_config.txt", "w") as file:
    file.write(output)


connection.disconnect()
```
• Send device configuration using an external file
``` python3
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_xe",
    "host": "192.168.0.50",  # Router's IP
    "username": "pxl",  # Username
    "password": "pxl",  # Password
    "secret": "pxl",  # Enable password
}

connection = ConnectHandler(**device)
connection.enable()

with open("config.txt") as file:
    config_commands = file.read().splitlines()
connection.send_config_set(config_commands)

connection.disconnect()
```
* config.txt file aanmaken en de volgende commando's intypen
``` kotlin
interface loopback1
ip address 10.2.2.2 255.255.255.0
no shutdown
```

• Configure a subset of interfaces
``` python3
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_xe",
    "host": "192.168.0.50",  # Router's IP
    "username": "pxl",  # Username
    "password": "pxl",  # Password
    "secret": "pxl",  # Enable password
}

connection = ConnectHandler(**device)
connection.enable()

interfaces = ["GigabitEthernet0/1", "GigabitEthernet0/2"]
for interface in interfaces:
    config = [
        f"interface {interface}",
        "description Configured by Netmiko",
        "no shutdown"
    ]
    connection.send_config_set(config)


connection.disconnect()
```
• Connect using a Python Dictionary

• Execute a script with functions or classes
``` python3
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_xe",
    "host": "192.168.0.50",  # Router's IP
    "username": "pxl",  # Username
    "password": "pxl",  # Password
    "secret": "pxl",  # Enable password
}

def send_command(device, command):
    conn = ConnectHandler(**device)
    conn.enable()
    output = conn.send_command(command)
    conn.disconnect()
    return output

print(send_command(device, "show version"))

connection.disconnect()
```
• Execute a script with conditional statements (if, else)
``` python3
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_xe",
    "host": "192.168.0.50",  # Router's IP
    "username": "pxl",  # Username
    "password": "pxl",  # Password
    "secret": "pxl",  # Enable password
}
connection = ConnectHandler(**device)
connection.enable()

output = connection.send_command("show ip interface brief")
if "up" in output:
    print("Interfaces are up!")
else:
    print("Interfaces are down!")

connection.disconnect()
```
• Send show commands to multiple devices
``` python3 
from netmiko import ConnectHandler

# List of devices
devices = [
    {
        "device_type": "cisco_xe",
        "host": "192.168.0.50",  # Router 1 IP
        "username": "pxl",  # Username
        "password": "pxl",  # Password
        "secret": "pxl",  # Enable password
    },
    {
        "device_type": "cisco_xe",
        "host": "192.168.0.51",  # Router 2 IP
        "username": "pxl",  # Username
        "password": "pxl",  # Password
        "secret": "pxl",  # Enable password
    }
]

# Send show commands to multiple devices
for device in devices:
    connection = ConnectHandler(**device)
    connection.enable()
    output = connection.send_command("show ip interface brief")
    print(f"Device: {device['host']}\n{output}")
    connection.disconnect()
```

• Send configuration commands to multiple devices
``` python3
from netmiko import ConnectHandler

# List of devices
devices = [
    {
        "device_type": "cisco_xe",
        "host": "192.168.0.50",  # Router 1 IP
        "username": "pxl",  # Username
        "password": "pxl",  # Password
        "secret": "pxl",  # Enable password
    },
    {
        "device_type": "cisco_xe",
        "host": "192.168.0.51",  # Router 2 IP
        "username": "pxl",  # Username
        "password": "pxl",  # Password
        "secret": "pxl",  # Enable password
    }
]

# Send configuration commands to multiple devices
config_commands = ["hostname NetmikoRouter", "interface loopback0", "ip address 10.10.10.1 255.255.255.0", "no shutdown"]

for device in devices:
    connection = ConnectHandler(**device)
    connection.enable()
    connection.send_config_set(config_commands)
    print(f"Configuration sent to device: {device['host']}")
    connection.disconnect()
```
#### Task Preparation and Implementation:
- ### 1. Install Python 3 and Netmiko
Installation of python3 and Netmiko:
``` bash 
sudo apt update
sudo apt install python3 python3-pip -y
pip3 install netmiko
```
#### Task Troubleshooting:
1) Probleem: Onjuist IP-adres of Verkeerde Inloggegevens
Het script kon geen verbinding maken door een fout IP-adres of verkeerde gebruikersnaam/wachtwoord.  
**Oplossing:** Controleer IP en inloggegevens met:
```bash
ping 192.168.0.50
ssh -oHostKeyAlgorithms=+ssh-rsa -oKexAlgorithms=+diffie-hellman-group14-sha1 pxl@192.168.178.50
```

2) Probleem: Ongeldige of Verkeerd Getypte Commando’s, De commando's werkten niet zoals verwacht op de router.
**Oplossing:** Controleer de juistheid van de commando's handmatig op de CLI van de router met:

```plaintext
show ip interface brief
```

3) Probleem: Time-outs bij Verbinding

Probleem: Langdurige scripts faalden door time-out fouten.
**Oplossing:** Voeg een time-outparameter toe in het device woordenboek:

```python
"timeout": 10
```
#### Task Verification:
• Controles werden gedaan door de scripts telkens in een .py file te plaatsen en deze te runnen door gebruik te maken van volgende commando:
``` python3
micro script.py --> (script hierin schrijven)
python3 script.py
```
![run](/images/running-script.png)



### 3b: Create a Challenging and useful Script for a network engineer.
• Create an exciting and challenging script that a network engineer in a programmable era
would use every day. Surprise your lecturer!
Document your findings and important commands

``` python3
from netmiko import ConnectHandler

# Router connection details
device = {
    "device_type": "cisco_ios",
    "host": "192.168.0.50",  # Router's IP
    "username": "pxl",  # Username
    "password": "pxl",  # Password
    "secret": "pxl",  # Enable password
}

# List of IPs to block (replace these with actual suspicious IPs)
intruder_ips = ["192.168.0.100", "192.168.0.101"]

def block_intruders():
    """Block predefined intruder IPs."""
    connection = ConnectHandler(**device)
    connection.enable()
    for ip in intruder_ips:
        commands = [
            "ip access-list extended BLOCKED_IPS",
            f"deny ip host {ip} any",
            "exit",
        ]
        connection.send_config_set(commands)
        print(f"Blocked IP: {ip}")
    connection.disconnect()

block_intruders()

```
#### Task Preparation and Implementation:
1) verificatie door controle te doen of the juiste versies zijn geinstalleerd
``` python3 
python3 --version
pip3 show netmiko
```

2) Router configuraties: 
- Een test ACL aangemaakt om functionaliteit te controleren:
``` plaintext
ip access-list extended BLOCKED_IPS
permit ip any any
```
Implementatiestappen:
1) Verbind met de router:

Gebruik Netmiko om een SSH-verbinding te maken en de geprivilegieerde modus te activeren.

2) ACL-commando's versturen:

Voeg voor elk IP-adres in intruder_ips een deny-regel toe aan de ACL BLOCKED_IPS.

3) Verbinding verbreken:

Sluit de verbinding nadat de configuraties zijn toegepast.

#### Task Troubleshooting:
Problemen tegengekomen:

1) Verkeerd IP in het device woordenboek:

Probleem: Het script kon geen verbinding maken vanwege een verkeerd IP-adres.
Oplossing: Controleer het juiste IP met ping en update de host-waarde in het device woordenboek.

2) Onvolledige ACL-configuratie:

Probleem: De ACL-regels werkten niet omdat de ACL niet was toegepast op een interface.
Oplossing: Pas de ACL handmatig toe op de juiste interface:
``` plaintext
interface GigabitEthernet0/0
ip access-group BLOCKED_IPS in
```
3) Netmiko verbindingsfout:

Probleem: Het script kreeg een time-out door verkeerde inloggegevens.
Oplossing: Controleer de username, password en secret en test handmatige SSH-toegang.
#### Task Verification:
Controle stappen:
Script uitvoeren:

Sla het script op als block_intruders.py en voer het uit:
``` bash

python3 block_intruders.py
```
Controleer ACL-regels:

Controleer de ACL-configuratie van de router:
``` plaintext
show access-lists BLOCKED_IPS
```
Bevestig dat deny ip host-regels zijn toegevoegd voor de IP's in intruder_ips.
Test geblokkeerde IP's:

Probeer vanaf de geblokkeerde IP's de router te pingen en controleer of de pings mislukken:
``` bash
ping 192.168.0.50
```
Bevestig toegang vanaf toegestane IP's.
Controleer interfaceconfiguratie:

Controleer of de ACL is toegepast op de juiste interface:
``` plaintext
show running-config | include access-group
```
## Part 4: Explore YANG Models
Perform the following lab on Cisco NetAcademy within the DEVNET Associate course.
8.3.5 Lab - Explore YANG Models
Document your findings and important commands.
### Task Preparation and Implementation:
•Launch de DEVASC VM:

Start de DEVASC Virtual Machine op via VirtualBox of VMware.
Zorg ervoor dat de VM correct werkt en toegang heeft tot een terminal en internet.
Benodigde Tools:
**pyang:** Dit is een command-line tool voor het verwerken en visualiseren van YANG-bestanden.
**Chromium-browser:** Wordt gebruikt om YANG-modellen op GitHub te bekijken.
**Visual Studio Code:** Voor tekstbewerking en terminaltoegang.
Installatie van pyang:
``` bash
pyang -v
pip3 install pyang --upgrade
```
Controleer of pyang al is geïnstalleerd
### Task Troubleshooting:
Geen problemen tegengekomen 
### Task Verification:
Bij het ingeven van volgende commando wordt de onderstaande output gegeven
``` bash
pyang -f tree ietf-interfaces.yang
```
![run](/images/ietf-output-tree.png)

## Part 5: Use NETCONF to Access an IOS XE Device
Perform the following lab on Cisco NetAcademy within the DEVNET Associate course.
8.3.6 Lab - Use NETCONF to Access an IOS XE Device
Document your findings and important commands.

## Gebruikte tools + implementatie LAB : 
1) DEVASC VM
2) CRS1000 Cisco Router VM 
3) installeer NCCLIENT
    ``` bash
    pip3 install ncclient
    ```

**Verbinden met NETCONF via python:** 
    ``` python3
    from ncclient import manager

m = manager.connect(
    host="192.168.0.50",
    port=830,
    username="pxl",
    password="pxl",
    hostkey_verify=False
)
print("Connected to NETCONF.")
    ```
**Verzamel apparaatcapaciteiten:**

Toon ondersteunde YANG-modellen:
```python3
for capability in m.server_capabilities:
    print(capability)
```
**Configuratie ophalen:**

Gebruik get_config() om de huidige configuratie op te halen:
``` python3
netconf_reply = m.get_config(source="running")
print(netconf_reply)
```
**Prettify XML-output:**

Gebruik Python om de XML leesbaar te maken:
``` python3
import xml.dom.minidom
print(xml.dom.minidom.parseString(netconf_reply.xml).toprettyxml())
```
**Configuratie aanpassen:**

Wijzig de hostname met een configuratiescript:
```python3
netconf_hostname = """
<config>
  <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
    <hostname>MyRouter</hostname>
  </native>
</config>
"""
m.edit_config(target="running", config=netconf_hostname)
```

**Maak een nieuwe Loopback-interface:**

Voeg een Loopback toe:
```python
netconf_loopback = """
<config>
  <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
    <interface>
      <Loopback>
        <name>1</name>
        <description>NETCONF Loopback</description>
        <ip>
          <address>
            <primary>
              <address>10.2.2.2</address>
              <mask>255.255.255.0</mask>
            </primary>
          </address>
        </ip>
      </Loopback>
    </interface>
  </native>
</config>
"""
m.edit_config(target="running", config=netconf_loopback)
```
### Troubleshooting:
- Geen problemen tegengekomen - alles werkte door de labo te volgen 
### Taakverificatie
Verbind met de router:
Gebruik SSH om te controleren of wijzigingen zijn toegepast:**
``` bash
ssh cisco@192.168.0.50
```
**Controleer de configuratie:**

Controleer hostname:
``` plaintext
show running-config | include hostname
```
**Controleer interfaces:**
```plaintext
show ip interface brief
```
**Test configuratie:**

Probeer de loopback te pingen:
```bash
ping 10.2.2.2
```
**Scriptuitvoer:**
Controleer of m.get_config() en m.edit_config() geen fouten geven

### Task Verificatie met images 
![yang](/images/netconf-running.png)
![yang](/images/session-netconf.png)
![yang](/images/get-yang.png)
![yang](/images/pretty-xml.png)
![yang](/images/close-yang-session.png)





## Part 6: Use RESTCONF to Access an IOS XE Device
Perform the following lab on Cisco NetAcademy within the DEVNET Associate course.
8.3.7 Lab - Use RESTCONF to Access an IOS XE Device
Document your findings and important commands.
### Task Preparation and Implementation:

1) DEVASC VM 
2) CSR1000 VM.
**Verifieer netwerkconnectiviteit:*
``` bash
ping -c 5 192.168.0.50
ssh pxl@192.168.0.50
```
**Zorg ervoor dat RESTCONF is ingeschakeld op de CSR1000 met:**
```plaintext
show platform software yang-management process
```
**Als het niet actief is, schakel RESTCONF in:**
```plaintext
config t
restconf
ip http secure-server
ip http authentication local
```
**Benodigde Tools:**
Postman: Gebruik voor het versturen van RESTCONF-verzoeken.
Python: Gebruik scripts met requests voor automatisering. Installeer met:
```bash
pip3 install requests
```
**Setup van mappen en bestanden:**

Maak een map aan in de VM:
```bash
mkdir ~/labs/devnet-src/restconf
```
### Implementation:

1) Gebruik Postman voor GET-verzoeken:

Open Postman en schakel SSL-certificaatcontrole uit:
```plaintext
File > Settings > SSL Certificate Verification: OFF
```
* Configureer een GET-verzoek:
    * URL: https://192.168.0.50/restconf/data/ietf-interfaces:interfaces
    * Headers:
        * Content-Type: application/yang-data+json
        * Accept: application/yang-data+json
    * Auth: Kies Basic Auth met:
        * Gebruikersnaam: pxl
        * Wachtwoord: pxl
* Klik op Send en controleer de JSON-uitvoer.

2) Gebruik Postman voor PUT-verzoeken:

    * Dupliceer het GET-verzoek en wijzig het naar een PUT-verzoek.
    * Voeg in de Body de configuratie toe om een nieuwe Loopback-interface te maken:
```json
{
  "ietf-interfaces:interface": {
    "name": "Loopback2",
    "description": "My first RESTCONF loopback",
    "type": "iana-if-type:softwareLoopback",
    "enabled": true,
    "ietf-ip:ipv4": {
      "address": [
        { "ip": "10.3.3.3", "netmask": "255.255.255.0" }
      ]
    },
    "ietf-ip:ipv6": {}
  }
}
```
    * Klik op Send en controleer of de Status 201 Created is.

3) Automatiseer met Python GET-scripts:

* Maak een bestand restconf-get.py:
```python
import json
import requests
requests.packages.urllib3.disable_warnings()

api_url = "https://192.168.0.50/restconf/data/ietf-interfaces:interfaces"
headers = {
    "Accept": "application/yang-data+json",
    "Content-type": "application/yang-data+json"
}
basicauth = ("pxl", "pxl")

resp = requests.get(api_url, auth=basicauth, headers=headers, verify=False)
print(json.dumps(resp.json(), indent=4))
```
4) Automatiseer met Python PUT-scripts:

* Maak een bestand restconf-put.py:
```python
import json
import requests
requests.packages.urllib3.disable_warnings()

api_url = "https://192.168.0.50/restconf/data/ietf-interfaces:interfaces/interface=Loopback2"
headers = {
    "Accept": "application/yang-data+json",
    "Content-type": "application/yang-data+json"
}
basicauth = ("pxl", "pxl")

yangConfig = {
    "ietf-interfaces:interface": {
        "name": "Loopback2",
        "description": "My second RESTCONF loopback",
        "type": "iana-if-type:softwareLoopback",
        "enabled": True,
        "ietf-ip:ipv4": {
            "address": [{ "ip": "10.3.3.3", "netmask": "255.255.255.0" }]
        },
        "ietf-ip:ipv6": {}
    }
}

resp = requests.put(api_url, data=json.dumps(yangConfig), auth=basicauth, headers=headers, verify=False)
print(f"STATUS: {resp.status_code}")
```

### Task Troubleshooting:
**Probleem:** Geen JSON-uitvoer in Postman
**Oplossing:** Controle headers (Content-Type en Accept) en URL.

**Probleem:** Fout bij Python-scripts
**Oplossing:** Controle imports (requests en json) en zorg dat SSL-verificatie is uitgeschakeld met verify=False.

### Task Verification:
1) Controleer configuraties in de router:

Controleer of de nieuwe Loopback-interface is gemaakt:
``` plaintext
show ip interface brief
```
2) Valideer Postman-verzoeken:
    * Verifieer de Status (200 voor GET, 201 voor PUT).
3) Scriptuitvoer controleren:
    * Controleer de JSON-uitvoer van de Python-scripts en vergelijk met de verwachte configuraties.
4) Testconnectiviteit:

Ping de nieuwe interface om te verifiëren dat deze actief is:
``` bash
ping 10.4.4.4
```

![restconf](/images/postman-get.png)
![restconf](/images/postman-put.png)
![restconf](/images/loopback2-check.png)

**static routes** niet vergeten!! om de loopback adres te kunnen pingen voor verificatie

## Part 7: Getting started with NETCONF/YANG – Part 1
### Task Preparation and Implementation:
• Beschrijf de voorbereiding en implementatie van de taak.
• Leg uit welke stappen, tools of methoden je hebt gebruikt om het onderdeel te voltooien.
### Task Troubleshooting:
• Noteer eventuele problemen die je bent tegengekomen en hoe je deze hebt opgelost.
• Vermeld welke oplossingen of benaderingen je hebt geprobeerd.
### Task Verification:
• Leg uit hoe je hebt gecontroleerd of de taak succesvol was.
• Beschrijf de testmethoden of technieken die je hebt gebruikt om de taak te verifiëren.

## Part 8: Getting started with NETCONF/YANG – Part 2
### Task Preparation and Implementation:
• Beschrijf de voorbereiding en implementatie van de taak.
• Leg uit welke stappen, tools of methoden je hebt gebruikt om het onderdeel te voltooien.
### Task Troubleshooting:
• Noteer eventuele problemen die je bent tegengekomen en hoe je deze hebt opgelost.
• Vermeld welke oplossingen of benaderingen je hebt geprobeerd.
### Task Verification:
• Leg uit hoe je hebt gecontroleerd of de taak succesvol was.
• Beschrijf de testmethoden of technieken die je hebt gebruikt om de taak te verifiëren.


# Extra Information
## login via SSH connection to the router via windows powershell: 
- `Command:` ssh -oHostKeyAlgorithms=+ssh-rsa -oKexAlgorithms=+diffie-hellman-group14-sha1 pxl@192.168.0.50