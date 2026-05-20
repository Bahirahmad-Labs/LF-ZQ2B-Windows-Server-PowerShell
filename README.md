Lernfeld LF-ZQ2B: Microsoft Windows Server & PowerShell Automation
Dieses Repository enthält die vollständige, tiefgehende Projektdokumentation für das Lernfeld LF-ZQ2B im Rahmen meiner Ausbildung zum Fachinformatiker für Systemintegration bei Bahirahmad-Labs.

Ziel dieses Projekts war der eigenhändige Aufbau, die Absicherung und standortübergreifende Vernetzung einer simulierten Enterprise-Infrastruktur (Standorte Berlin und Hamburg) unter Windows Server 2022 und der intensiven Nutzung von PowerShell-Skripting zur Automatisierung.

🗺️ Gesamtarchitektur des Labs
Physische Isolation: Private Hyper-V Switches zur vollständigen Netztrennung.

Zentraler Verzeichnisdienst: Active Directory Domain Services (AD DS) mit der Gesamtstruktur gfnlab.test.

Zweigstellennetzwerk: Anbindung des Standorts Hamburg über Software-Routing und WAN-Optimierung via BranchCache.

Sicherheits-Härtung: Implementierung domänenweiter AppLocker-Sicherheitsrichtlinien, Fine-Grained Password Policies (PSOs) und einer eigenen Public-Key-Infrastruktur (PKI).

📚 Ausführliches Inhaltsverzeichnis (Schritt-für-Schritt-Berichte)
Hier sind die einzelnen Phasen und Projekttage detailliert dokumentiert. Die Links führen direkt zu den jeweiligen Fachberichten in den Unterordnern:

🔹 Phase 1: Virtualisierung & Basis-Netzwerkinfrastruktur
Tag 16 & 17: Hyper-V Netzwerkvirtualisierung & Statische IP-Infrastruktur

Kernthemen: Private vSwitches im Hyper-V Manager, statische IPv4-Konfiguration auf DC01, PowerShell-Äquivalente für Server-Core.

🔹 Phase 2: Active Directory & Identitätsverwaltung (Dokumentation folgt)
Tag 20 & 21: AD DS Forest Setup & OU-Struktur

Kernthemen: Heraufstufen zum Domain Controller, logische Strukturierung nach OUs.

Tag 22: Gruppen & Benutzerverwaltung im Enterprise-Umfeld

Kernthemen: AGDLP-Prinzip, Massen-Userimport via PowerShell.

🔹 Phase 3: Gruppenrichtlinien & Erweiterte Sicherheit (Dokumentation folgt)
Tag 25 & 26: GPO Central Store & AppLocker Endpunktsicherheit

Kernthemen: ADMX-Templates zentralisieren, Software-Whitelisting für Clients.

Tag 27: Granulare Passwortrichtlinien (PSOs)

Kernthemen: Password Settings Objects für administrative Konten.

🔹 Phase 4: Public Key Infrastructure & Verschlüsselung (Dokumentation folgt)
Tag 30 & 31: Active Directory Certificate Services (AD CS)

Kernthemen: Aufbau einer Enterprise Root-CA, EFS-Datenverschlüsselung und Key Recovery Agenten.

🔹 Phase 5: WAN-Optimierung & Infrastrukturdienste (Dokumentation folgt)
Tag 33 & 34: LAN-Routing & BranchCache für Zweigstellen

Kernthemen: Windows Server als Router, Zwischenspeicherung im Hosted Cache Mode zur WAN-Entlastung.

🛠️ Einsehbarer technologischer Stack
Betriebssysteme: Windows Server 2022 (GUI & Core), Windows 11 Pro

Hypervisor: Microsoft Hyper-V

Automatisierung: Windows PowerShell 5.1 / 7
