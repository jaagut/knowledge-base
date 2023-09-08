# JDisc Discovery

[JDisc Discovery](http://www.jdisc.com/de/) inventarisiert gesamte Netzwerke und erkennt alle wichtigen Betriebssysteme (einschließlich HP-UX, Solaris und AIX). Es erkennt Hardware- und Software, IP-Netzwerke, Windows-Domänen und Active Directory. Darüber hinaus identifiziert JDisc Discovery alle wichtigen Virtualisierungstechnologien, sowie viele Cluster-Umgebungen.

Ein Praxisbeispiel finden Sie auf unserem [Blog](https://www.i-doit.com/blog/it-service-management-mit-dem-discovery-tool-jdisc-und-i-doit/)

## Installation

Für den Betrieb ist die Installation der JDisc-Server-Komponenten auf einem Microsoft Windows (bestenfalls in der Server-Variante) erforderlich Die Installationsdateien hierfür können unter folgender Adresse heruntergeladen werden:

[http://www.jdisc.com/de/downloads.html](https://jdisc.com/de/download/)

Während des Setups wird abgefragt, welche Passwörter für das im Hintergrund verwendete PostgreSQL-DBMS verwendet werden sollen. Dies betrifft den administrativen Account **postgres** und den Account **postgresro**, der lediglich über Leserechte auf die JDisc-Datenbank verfügt. Letztere Account wird für die Schnittstelle zu i-doit benötigt. Zudem muss beim Setup angegeben werden, dass die PostgreSQL-Instanz nach außen hin erreichbar sein soll. Diese hört standardmäßig auf den Port **25321**. Der Port ist in der Windows Firewall freizuschalten.

Bei Fragen zur Installation oder Konfiguration von JDisc hilft der Support des Herstellers:

[http://www.jdisc.com/de/support.html](https://jdisc.com/de/support/faq/)

Dieser Artikel widmet sich im Folgenden der Schnittstelle zwischen i-doit und JDisc Discovery.

## Konfiguration

Die Konfiguration der Schnittstelle befindet sich unter **Verwaltung → Schnittstellen / externe Daten → Import → JDISC → JDisc-Konfiguration**. Dort können beliebig viele Instanzen von JDisc Discovery angegeben werden. Dies ermöglicht komplexe Inventarisierungs-Szenarien mit voneinander getrennten Netzen.

[![JDisc Konfiguration](../assets/images/de/daten-konsolidieren/jdisc/1-jdisc.png)](../assets/images/de/daten-konsolidieren/jdisc/1-jdisc.png)

### Allgemeine Einstellungen

| Parameter | Anmerkung |
| --- | --- |
| **Standard Server** | Handelt es sich um diejenige Instanz, die bei einem manuellen Import vorgeschlagen werden soll? |
| **Host** | Hostname/FQDN oder IP-Adresse der JDisc-Instanz; diese muss von der i-doit-Instanz aus erreichbar sein |
| **Port** | Offener Port der PostgreSQL-Instanz, die im Hintergrund von JDisc Discovery läuft |
| **Datenbank** | JDisc Discovery verwendet standardmäßig die Datenbank **inventory**. |
| **Benutzername** | Hier wird einer der Accounts angegeben, die beim Setup abgefragt wurden. Es genügt der Account **postgresro** mit eingeschränkten Rechten, da i-doit lediglich lesend auf die Datenbank zugreift. |
| **Passwort** | Das beim Setup vergebene Passwort, das zum verwendeten Account gehört |
| **Import älterer JDisc Versionen erlauben?** | Abfrage, die nur für ältere Versionen von JDisc Discovery relevant ist |

Nach dem Speichern der Parameter kann die Verbindung zur JDisc-Datenbank über den Button **Verbindung prüfen** getestet werden.

### Discovery Einstellungen

Über i-doit kann der Benutzer einen Scan bei der konfigurierten JDisc-Instanz auslösen. Dazu wird der Web Service von JDisc Discovery verwendet, der installiert und aktiviert sein muss.

| Parameter | Anmerkung |
| --- | --- |
| **Benutzername** | Account, der für den Web Service genutzt werden soll |
| **Passwort** | Zum Account passendes Passwort |
| **Port** | Standard: **9000** |
| **Protokoll** | Standard: **http** |

Auch hier kann nach dem Speichern der Parameter über den Button **Verbindung prüfen** getestet werden, ob die Konfiguration erfolgreich ist.

Damit die Verbindung zu JDisc Discovery funktioniert, muss auf dem Host von i-doit die PHP-Erweiterung SOAP installiert sein. Auf einem Debian-basierten Betriebssystem ist dies mit der Installation von PHP bereits geschehen, unter anderen Betriebssystemen sollte es ggf. ein passendes Distributionspaket geben.

## Profile

Die JDisc-Profile innerhalb von i-doit geben die Möglichkeit, die Verknüpfung von Objekttypen und ihren Attributen zwischen JDisc und i-doit zu definieren. Unter **Verwaltung → Schnittstellen / externe Daten → Import → JDISC → JDisc-Profile** kann festgelegt werden, in welcher Weise die durch JDisc gefundenen Objekte ihren Weg in die IT-Dokumentation finden.

[![JDisc Profile](../assets/images/de/daten-konsolidieren/jdisc/2-jdisc.png)](../assets/images/de/daten-konsolidieren/jdisc/2-jdisc.png)

In der Standard-Installation von i-doit sind bereits vordefinierte Profile vorhanden. Darunter befindet sich das Profile **Complete import**, dass darauf ausgelegt ist, alle Daten in i-doit zu importieren, die JDisc inventarisiert hat.
Falls bestimmte Objekte nicht importiert werden sollte das Profil auf den entsprechenden Objekttyp angepasst werden. Dazu muss der JDisc-Typ ausgewählt werden

### Allgemein Einstellungen

| Option | Beschreibung |
| --- | --- |
| **JDisc Server** | Welche Instanz von JDisc soll beim Import vor-selektiert werden? Siehe Abschnitt "Konfiguration" weiter oben. |
| **Titel** | Name des Profils |
| **Beschreibung** | Beschreibung des Profils |

### Objekttyp-Zuweisungen

Für jeden Typ in JDisc kann ein äquivalenter Objekttyp in i-doit ausgewählt werden. Geschieht dies nicht, werden Devices diesen Typs nicht importiert. Die Reihenfolge der Zuweisungen wird beim Import beachtet. Dieser geht die Liste von oben nach unten durch, um eine Zuweisung zu identifizieren. Die Matrix ist pro Zeile folgendermaßen aufgebaut:

| JDisc-Typ | JDisc-Betriebssystem | Objektnamen modifizieren | FQDN Zusatz | Port Filter | Objekttyp | Standort | Aktionen |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Device Type aus JDisc, der beim Import berücksichtigt werden soll.<br><br>Die Liste wird aus der angegebenen JDisc-Instanz ausgelesen und zwischengespeichert. | Betriebssysteme, die JDisc bereits inventarisiert hat.<br><br>Oftmals reicht der Device Type für eine Zuweisung zu einem Objekttyp nicht aus, weswegen optional das Betriebssystem angegeben werden kann.<br><br>Das Feld kann mit Wildcards (*) versehen werden. | Wandelt den Objekttitel um.<br><br>**Unverändert**: Der Objektname wird nicht verändert<br><br>**Großbuchstaben**: Der komplette Objektname wird in Großbuchstaben geändert.<br><br>**Kleinbuchstaben**: Der komplette Objektname wird in Kleinbuchstaben geändert. | Hängt an den Objekttitel  einen FQDN Zusatz an. | Welche Netzwerk-Ports sollen importiert werden?<br><br>**Normaler Import**: Alle physikalischen und logischen Ports werden importiert.<br><br>**Kein Import**: Im Textfeld können Port-Namen angegeben werden, die ignoriert werden sollen.  <br>Das Feld kann mit Wildcards (*) genutzt werden. (Beispiel: Port-Name: Loopback → Ports mit dem Namen Loopback werden nicht importiert)<br><br>**Logischer Port**/**Physikalischer Port**/**FC-Port**: Es werden nur die Ports importiert, deren Name im Textfeld zu finden ist.<br><br>Die Kriterien können miteinander kombiniert werden, sodass nur bestimmte physikalische und nur bestimmte logische Ports berücksichtigt werden. | Diesem Objekttyp werden importierte Devices zugeordnet.<br><br>Wird kein Objekttyp ausgewählt, werden Devices diesen Types ignoriert. | Beim Import werden Objekte, die den hier genannten Kriterien entsprechen, einem bestimmten Standort zugeordnet. Dies geschieht über die Kategorie **Standort**. | Neue Zuweisung hinzufügen, eine bestehende duplizieren oder löschen. |

### Zusätzliche Optionen

| Option | Beschreibung |
| --- | --- |
| **Kategorien auswählen** | Welche [Kategorien](../grundlagen/struktur-it-dokumentation.md) sollen beim Import befüllt werden? Es werden nur Kategorien gelistet, die der Import behandeln kann. |
| **Netzwerk Interfaces importieren als** | Inventarisierte Netzwerk-Schnittstellen können in unterschiedlichen Kategorien abgebildet werden. |
| **Software beim Import berücksichtigen** | Soll inventarisierte Software als Objekttyp **Anwendungen** importiert werden? |
| **Softwarelizenzen beim Import berücksichtigen** | Sollen inventarisierte Softwarelizenzen als Objekttyp **Lizenzen** importiert werden? |
| **Systemdienste importieren** | Sollen inventarisierte Systemdienste als Objekttyp **Systemdienst** importiert werden? |
| **Cloud Subscriptionen importieren** | Sollen Daten aus der JDisc Kategorie Cloud importiert werden?<br>Wird in die Kategorie **Zugewiesene Abonnente** importiert. Die Rückwärtige Kategorie **Zugewiesene Benutzer** ist bei Objekten vom Typ Lizenzen zu finden. |
| **Verbindungs Endpunkte importieren** | Die Verbindungen werden nicht in die Verkabelung, sondern in die Kategorie "Verbindungs Endpunkte" importiert. Das ermöglicht zwischen der manuellen Verkabelung und der automatischen Verkabelung durch JDisc zu unterscheiden. |
| **Einfache Datenbank Modellierung verwenden?** | Soll die neue oder die alte [Datenbanklogik](../anwendungsfaelle/dokumentation-von-datenbanken.md) verwendet werden? |
| **Layer-3-Netze beim Import berücksichtigen** | Sollen inventarisierte IP-Netze als Objekttyp **Layer-3-Netze** importiert werden? |
| **IP Adressentypen beibehalten** | Sollen **IPv4 Adressen**, **IPv6 Adressen**, **Loopback Adressen**, **Virtuelle Adressen** importiert werden? |
| **Importtyp für DHCP IP Adressen** | Sollen per DHCP zugewiesen Adressen überschrieben werden? |
| **VLans beim Import berücksichtigen** | Sollen inventarisierte VLANs als Objekttyp **Layer-2-Netze** importiert werden? |
| **Cluster beim Import berücksichtigen** | Sollen (Virtualisierungs-)Umgebungen als Objekttyp **Cluster** importiert werden? |
| **Blade/Chassis Verbindungen beim Import berücksichtigen** | Sollen Objekte vom Typ **Blade Server** zu Objekten vom Typ **Blade Chassis** beim Import hinzugefügt werden? |
| **Objekttyp der zugewiesenen Module innerhalb eines Blade/Chassis Gerätes** | Ist ein Blade oder Switch Chassis inventarisiert worden, können die eingesteckten Module einem bestimmten Objekttyp beim Import zugeordnet werden. |
| **Objekttyp der zugewiesenen Module aktualisieren** | Sollen die Objekttypen der zugewiesenen Module eines Blade/Chassis Gerätes aktualisiert werden? |
| **Custom attributes importieren** | Wenn in JDisc Discovery benutzerdefinierte Attribute (**Custom Attributes**) gepflegt werden, können diese in i-doit importiert werden. Diese werden nach dem Import in der Kategorie **JDisc Custom Attributes** angezeigt. |
| **Standard Templates aus Objekttypen berücksichtigen (nur bei Neuanlage)** | Wird ein neues Objekt erstellt, kann automatisch ein [Template](../effizientes-dokumentieren/templates.md) berücksichtigt werden. Die Auswahl des Templates findet in der Objekttyp-Konfiguration statt. |
| **CMDB-Status der Objekte ändern auf** | Bereits vorhandene Objekte können beim Aktualisieren einen bestimmten **[CMDB-Status](../grundlagen/lebens-und-dokumentationszyklus.md)** erhalten. Soll der **CMDB-Status** nicht geändert werden, ist in der Auswahl **CMDB-Status beibehalten** zu wählen. |
| **Software Filter** | Entweder kann eine White- oder eine Blacklist an Software-Applikationen angegeben werden, welche importiert werden sollen (Whitelist) oder eben nicht (Blacklist). Es können Wildcards (\*) angegeben werden. Die Liste von Titeln wird durch Kommas separiert. |
| **Filter als regexp gebrauchen** | Den Software Filter mit Strings oder mit Regulären Ausdrücken verwenden? Da das regex direkt an die JDisc Datenbank weitergeleitet wird, sollten die passenden regex paramter verwendet werden.  <br>Diese sind z.B. unter [h](https://www.postgresql.org/docs/9.3/functions-matching.html)[ttps://www.postgresql.org/docs/9.3/functions-matching.html](https://www.postgresql.org/docs/9.3/functions-matching.html) zu finden.<br><br>Hinweis eines Anwenders:<br><br>\s kann nicht verwendet werden. Hier wird mit Leerzeichen gearbeitet.  <br>\d kann nicht verwendet werden. Hier muss mit [a-zA-Z] gearbeitet werden. |
| **Benutze OS-Familie (falls verfügbar) anstatt OS-Version als Objekttitel** | Für den Softwareimport anstatt der Softwareversion die Softwarefamilie als Objekttitel verwenden. Z.B. anstatt "Windows Server 2008 Standard" nur "Windows" mit "Server 2008 Standard" als Variante. |
| **Objekt-Matching Profil** | [Welche Strategie soll erfolgen](objekt-identifizieren-bei-importen.md), um bereits in i-doit dokumentierte Objekte durch einen Import zu aktualisieren?<br> Siehe auch [custom identifier](#custom-identifier)|
| **Objekttyp aktualisieren** | Ist das Device bereits als Objekt in i-doit vorhanden, kann hierüber bestimmt werden, ob der Objekttyp anhand der Zuweisung (siehe oben) aktualisiert werden soll oder nicht. |
| **Objekt-Titel aktualisieren** | Soll der Objekttitel durch den Import aktualisiert werden? |

Kategorien die für JDisc erstellt wurden sind:

-   [Support Entitlements](../grundlagen/kategorien-und-attribute.md)
-   [Custom Identifier](../grundlagen/kategorien-und-attribute.md)
-   [JDisc Custom Attributes](../grundlagen/kategorien-und-attribute.md)
-   [JDisc Discovery](../grundlagen/kategorien-und-attribute.md)
-   [Netzwerk > Verbindungs Endpunkte](../grundlagen/kategorien-und-attribute.md)
-   [Zugewiesene Abonnente](../grundlagen/kategorien-und-attribute.md)

### Custom identifier

!!! attention "Matching Regel"
    Die `devideid` wird immer für die Identifizierung von Objekten verwendet, wenn vorhanden.

JDisc device ids sollen bei Anlage des Objekts in der **Custom identifier** Kategorie mit type JDisc zugeordnet werden. Hierher sollen die Objekte im nach hinein dann auch identifiziert werden.
Wenn keine Daten vorhanden sind wird auf das Objekt-Matching Profil zurück gegriffen.

## Import von Daten über die Web GUI

Der Import von Inhalten aus JDisc erfolgt über **Extras → CMDB → Import → JDisc**.

[![JDisc Import GUI](../assets/images/de/daten-konsolidieren/jdisc/3-jdisc.png)](../assets/images/de/daten-konsolidieren/jdisc/3-jdisc.png)

Vor dem Ausführen des Imports müssen die Import-Parameter festlegt sein.<br>
Da es möglich ist, dass mehr als ein JDisc-Server eingesetzt wird, muss im ersten Schritt der JDisc-Server ausgewählt werden. Das Profil, welches zuvor definiert wurde, kann ebenfalls gewählt werden um die importierten Inhalte und deren Handhabung zu beeinflussen. Zusätzlich kann der Modus des Imports angegeben werden. Der Import-Modus **Erstellen** wird alle gefundenen Objekte erstellen, ohne zu prüfen, ob diese bereits existieren. Der Import-Modus **Aktualisieren** wird nur Objekte erstellen, die im i-doit-Datenbestand nicht gefunden werden. Kategorien von bereits existierenden Objekten werden (wenn nötig) um neue Daten ergänzt. Der Import-Modus **Überschreiben** verhält sich im Prinzip wie der Modus **Aktualisieren** mit dem Unterschied, dass Listen-Kategorien erst geleert und dann neu angelegt werden.

| **Import Modus** | **Beschreibung** |
| --- | --- |
| **Erstellen** | Der Import-Modus **"Erstellen"** wird alle gefundenen Objekte erstellen, ohne zu prüfen, ob diese bereits existieren. |
| **Aktualisieren** | Der Import-Modus **"Aktualisieren"** wird nur Objekte erstellen, die in i-doit nicht gefunden werden konnten. Kategorien von bereits existierenden Objekten werden (wenn nötig) um neue Daten ergänzt. |
| **Aktualisieren (Neu inventarisiert)** | Mit dem Zusatz **"(Neu inventarisiert)"** wird die `Custom identifier` Kategorie gelöscht und feste idoit-zu-jdisc-device Verknüpfungen verworfen und die Objektzugehörigkeiten neu errechnet. |
| **Erstelle nur neu gescannte Geräte** | Der Modus "**Erstelle nur neu gescannte Geräte**" erstellt nur Objekte die nicht in i-doit existieren. |
| **Überschreiben** | Der Import-Modus **"Überschreiben"** verhält sich genauso wie der Modus "Aktualisieren" mit dem Unterschied, das Listen-Kategorien erst geleert und dann neu angelegt werden. |
| **Überschreiben (Neu inventarisiert)** | Mit dem Zusatz **"(Neu inventarisiert)"** wird die `Custom identifier` Kategorie gelöscht und feste idoit-zu-jdisc-device Verknüpfungen verworfen und die Objektzugehörigkeiten neu errechnet. |

In der Protokollierung kann der Umfang der geschriebenen Logs beeinflusst werden. Ein umfangreicheres Logging erhöht die Dauer des Imports.<br>
Wenn die Einstellungen vollständig vorgenommen wurden, kann der Import über **Importvorgang starten** angestoßen werden. Bitte beachte, dass die Dauer des Imports sowohl von der Größe der JDisc-Datenbank, als auch von der verwendeten Hardware abhängig ist.<br>
Wenn der Import abgeschlossen wurde, wird eine Zusammenfassung im Bereich **Ergebnis** angezeigt.<br>
Beim Import erzeugte Logs findest du im i-doit-Verzeichnis log/.

## Import über die i-doit Console

Der Import der Daten aus JDisc nach i-doit ist nicht nur manuell über die Oberfläche möglich. Er kann ebenfalls über die i-doit [Console](../automatisierung-und-integration/cli/console/index.md) ausgeführt und somit **automatisiert** werden. Wie der entsprechende Aufruf erzeugt wird, finden Sie im [zugehörigen Artikel](../automatisierung-und-integration/cli/console/optionen-und-parameter-der-console.md#import-jdisc) mit einem Beispiel für die Option **import-jdisc**.

Für den Import lässt sich die ID des gewünschten Profils angeben. Die ID ist in der Liste der Profile zu finden:

[![JDisc Profile](../assets/images/de/daten-konsolidieren/jdisc/4-jdisc.png)](../assets/images/de/daten-konsolidieren/jdisc/4-jdisc.png)

Ebenfalls kann die ID des zu verwendenden JDisc Servers angegeben werden. Diese ist in der Konfiguration zu finden:

[![JDisc Konfiguration](../assets/images/de/daten-konsolidieren/jdisc/5-jdisc.png)](../assets/images/de/daten-konsolidieren/jdisc/5-jdisc.png)

Eine jdisc.ini könnte so aussehen mehr Informationen zu .ini Dateien sind [hier](../automatisierung-und-integration/cli/console/verwendung-von-konfigurationsdateien-fuer-console-commands.md) zu finden.

```ini
ini
[commandArguments]
[commandOptions]
user=admin
password=admin
tenantId=6
profile=1
group=
mode=1
server=2
overwriteHost
detailedLogging=3
regenerateSearchIndex
[additional]
```

Aufruf aus dem i-doit Verzeichnis:

```shell
sudo -u www-data php console.php import -jdisc -c jdisc.ini
```
