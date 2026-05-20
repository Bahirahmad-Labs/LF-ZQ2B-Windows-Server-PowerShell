# Tag 16 & 17: Hyper-V Netzwerkvirtualisierung & Statische IP-Infrastruktur

## 📌 1. Ausgangslage & Zielsetzung
Für das standortübergreifende Enterprise-Szenario aus dem Lernfeld LF-ZQ2B wurde eine vollständig isolierte Testumgebung (Sandboxed Lab) aufgebaut. Das Ziel an Tag 16 und 17 bestand darin, die physische Trennung der beiden Firmenstandorte **Berlin** und **Hamburg** auf Hypervisor-Ebene zu simulieren. Dabei wurde sichergestellt, dass die virtuellen Maschinen untereinander kommunizieren können, jedoch kein unkontrollierter Zugriff auf das reale Produktionsnetzwerk oder das Internet möglich ist.

Hierzu wurden isolierte, private virtuelle Switches implementiert und die Netzwerkkarten der Server-Systeme (insbesondere des primären Domain Controllers `DC01`) mit festen, statischen IPv4-Adressen konfiguriert.

---

## 🛠️ 2. Hyper-V Netzwerkkonfiguration (Umsetzung in der GUI)

Um die strikte Isolation der beiden Standorte zu gewährleisten, wurden private virtuelle Switches im Hyper-V Manager angelegt. Ein privater Switch blockiert jeglichen externen Datenverkehr und erlaubt ausschließlich die Kommunikation der virtuellen Maschinen untereinander.

### Durchgeführte Schritte:
1. **Aufrufen des Hyper-V Managers** auf dem Windows-Hostsystem.
2. Navigieren zum rechten Aktionsmenü und **Öffnen des Managers für virtuelle Switches**.
3. Auswahl des Switch-Typs **Privat** im linken Auswahlmenü.
4. Betätigen der Schaltfläche **Virtuellen Switch erstellen**.
5. **Konfiguration der Eigenschaften** des ersten Switches mit folgenden Parametern:
   * **Name:** `vSwitch_Berlin`
   * **Verbindungstyp:** Privates Netzwerk
6. Speichern der Einstellungen über die Schaltfläche **Übernehmen**.
7. **Wiederholung des Vorgangs** (Schritte 3 bis 6) für den zweiten Standort-Switch mit folgenden Parametern:
   * **Name:** `vSwitch_Hamburg`
8. Abschluss der Konfiguration durch Klicken auf **OK**.

![Hyper-V Manager - Erstellung der privaten virtuellen Switches](images/01_hyperv_vswitch.png)

### 💻 Automatisierung via PowerShell:
Um den Bereitstellungsprozess zu beschleunigen und menschliche Fehler auszuschließen, wurde dieser Vorgang zusätzlich über die administrative PowerShell-Konsole des Hyper-V Hosts abgebildet:

```powershell
# Erstellung der isolierten virtuellen Switches für beide Standorte
New-VMSwitch -Name "vSwitch_Berlin" -SwitchType Private -Confirm:$false
New-VMSwitch -Name "vSwitch_Hamburg" -SwitchType Private -Confirm:$false

# Validierung der erstellten Switches
Get-VMSwitch | Select-Object Name, SwitchType
🔑 3. Statische IPv4-Konfiguration (Umsetzung in der GUI)
Nachdem die virtuellen Maschinen den jeweiligen virtuellen Switches zugewiesen wurden, erfolgte die Konfiguration der IP-Infrastruktur auf Betriebssystemebene. Da es sich um Server-Systeme und Domänencontroller handelt, wurde eine feste IP-Vergabe erzwungen.

Durchgeführte Schritte auf dem Server DC01:
Anmeldung am Windows Server 2022 mit administrativen Rechten.

Öffnen des Ausführen-Dialogs über die Tastenkombination Win + R, Eingabe von ncpa.cpl und Bestätigen mit Enter, um direkt die Netzwerkverbindungen aufzurufen.

Rechtsklick auf den aktiven Netzwerkadapter und Auswahl der Eigenschaften.

Selektieren des Eintrags Internetprotokoll, Version 4 (TCP/IPv4) und Öffnen der dazugehörigen Eigenschaften.

Aktivieren der Option Folgende IP-Adresse verwenden und Eintragen der definierten statischen Werte für den Standort Berlin:

IP-Adresse: 192.168.1.10

Subnetzmaske: 255.255.255.0

Standardgateway: 192.168.1.1 (Die IP-Adresse des zukünftigen Routers)

Aktivieren der Option Folgende DNS-Serveradresse verwenden:

Bevorzugter DNS-Server: 127.0.0.1 (Zuweisung der Loopback-Adresse, da dieser Server nachfolgend die DNS-Rolle für das Active Directory übernimmt)

Speichern und Schließen aller Dialogfenster über OK.

💻 Automatisierung via PowerShell:
Für eine effiziente Konfiguration – insbesondere im Hinblick auf zukünftige Windows Server Core-Installationen ohne grafische Oberfläche – wurde die IP-Einrichtung über die PowerShell realisiert:

PowerShell
# 1. Ermitteln des korrekten Netzwerk-Interface-Index (IfIndex)
$netAdapter = Get-NetAdapter | Where-Object Status -eq "Up"

# 2. Umbennung des Adapters für eine eindeutige Systemdokumentation
Rename-NetAdapter -Name $netAdapter.Name -NewName "NIC_LAN_Berlin"

# 3. Zuweisung der statischen IP-Adresse, Subnetzmaske (Präfix 24 = 255.255.255.0) und des Gateways
New-NetIPAddress -InterfaceAlias "NIC_LAN_Berlin" -IPAddress 192.168.1.10 -PrefixLength 24 -DefaultGateway 192.168.1.1

# 4. Konfiguration des primären DNS-Eintrags auf die eigene Loopback-Adresse
Set-DnsClientServerAddress -InterfaceAlias "NIC_LAN_Berlin" -ServerAddresses "127.0.0.1"

# 5. Überprüfung und Verifizierung der angewendeten Einstellungen
Get-NetIPConfiguration -InterfaceAlias "NIC_LAN_Berlin"
🔍 4. Troubleshooting & Funktionstest
Zur abschließenden Qualitätskontrolle und zur Sicherstellung einer fehlerfreien Netzwerkkonfiguration wurden folgende Überprüfungen in der PowerShell durchgeführt:

PowerShell
# Verifizieren, ob die statische IP-Adresse im lokalen Netzsegment fehlerfrei antwortet
Test-Connection -ComputerName 192.168.1.10 -Count 2
