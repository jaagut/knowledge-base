**Inhaltsverzeichnis**

*   1[Vorbereitungen und Annahmen](#UmzugeinerInstallationunterGNU/Linux-VorbereitungenundAnnahmen)
*   2[Neues System vorbereiten](#UmzugeinerInstallationunterGNU/Linux-NeuesSystemvorbereiten)
*   3[Altes System außer Betrieb nehmen](#UmzugeinerInstallationunterGNU/Linux-AltesSystemaußerBetriebnehmen)
*   4[Dateien und Verzeichnisse umziehen](#UmzugeinerInstallationunterGNU/Linux-DateienundVerzeichnisseumziehen)
*   5[Datenbanken umziehen](#UmzugeinerInstallationunterGNU/Linux-Datenbankenumziehen)
    *   5.1[\*Wenn beim einspielen der Datenbank diese Fehlermeldung erscheint "Can't create table \`idoit\_data\`.\`table\_name\` (errno: 140 "Wrong create options")". Ist die Lösung dazu HIER zu finden.](#UmzugeinerInstallationunterGNU/Linux-*WennbeimeinspielenderDatenbankdieseFehlermeldungerscheint"Can'tcreatetable`idoit_data`.`table_name`(errno:140"Wrongcreateoptions")".IstdieLösungdazuHIERzufinden.)
*   6[Nachbereitung](#UmzugeinerInstallationunterGNU/Linux-Nachbereitung)

In diesem Artikel beschreiben wir die generelle Vorgehensweise, um eine Installation von i-doit von einem GNU/Linux zu einem anderen umzuziehen. Der Umzug umfasst sowohl die Datenbanken, als auch die Dateien und Verzeichnisse.

Vorbereitungen und Annahmen
---------------------------

Es sind ein paar Dinge zu beachten, um einen möglichst reibungslosen Umzug zu gewährleisten:

1.  i-doit und optional installierte [i-doit pro Add-ons](/display/de/i-doit+pro+Add-ons) sollten auf einem [aktuellen Stand](/display/de/Update+einspielen) sein.
2.  Wir verändern das alte System nicht, um im Fall der Fälle schnell wieder in den Ursprungszustand zurückkehren zu können.
3.  Die gezeigten Befehle passen zu einem aktuellen [Debian GNU/Linux](/pages/viewpage.action?pageId=10223831) und sollten an die entsprechende Umgebung angepasst werden. Blindes Ausführen der Befehle sollten vermieden werden.

Neues System vorbereiten
------------------------

Zunächst gilt es, das neue System so weit wie möglich vorzubereiten:

1.  Das neue Betriebssystem entspricht den [Systemvoraussetzungen](/display/de/Systemvoraussetzungen) und ist auf einem aktuellen Stand.
2.  Auf dem neuen Betriebssystem sind die [Systemeinstellungen](/display/de/Systemeinstellungen) konfiguriert worden.
3.  Gängige [Sicherheitsmaßnahmen](/display/de/Sicherheit+und+Schutz) wurden durchgeführt.

Altes System außer Betrieb nehmen
---------------------------------

Das alte System sollte bereits während des Umzugs nicht mehr produktiv verwendet werden:

1.  Downtimes sind nervig – vor allem unerwartete. Die Benutzer von i-doit sollten vorab informiert worden sein, dass die Installation umzieht und in welchem Zeitraum sie nicht erreichbar ist.
2.  [Automatisierte Zugriffe von Drittsystemen](/display/de/Automatisierung+und+Integration) sollten deaktiviert werden.
3.  [Cronjobs](/display/de/CLI) sollten ebenfalls deaktiviert werden. Hierzu reicht es meist, die Befehlszeilen auszukommentieren.
4.  Nach Sicherstellung der obigen Punkte sollte der Apache Webserver gestoppt werden:
    
    [?](#)
    
    `sudo` `systemctl stop apache2.service`
    

Dateien und Verzeichnisse umziehen
----------------------------------

1.  Wir kopieren das gesamte Installationsverzeichnis von i-doit vom alten auf das neue System. Das Verzeichnis befindet sich in vielen Fällen unter `/var/www/html/`. Beispiel mit SSH, wobei i-doit im Verzeichnis `/var/www/html/i-doit/` zu finden ist:
    
    [?](#)
    
    `scp` `-r user@oldsystem:``/var/www/html/i-doit/` `/tmp/`
    
    `scp` `-r` `/tmp/i-doit/` `user@newsystem:``/tmp/`
    
    `ssh` `user@newsystem`
    
    `sudo` `-u www-data` `cp` `-r` `/tmp/i-doit/` `/var/www/html/`
    
2.  Nach dem Kopieren sollte sichergestellt werden, dass die Dateisystemrechte richtig gesetzt sind. Der Apache Webserver benötigt Lese- und Schreibrechte auf das vollständige Installationsverzeichnis. Weitere Hinweise gibt der Artikel "[Setup](/display/de/Setup)". Beispiel:
    
    [?](#)
    
    `cd` `/var/www/html/i-doit/`
    
    `sudo` `chown` `www-data:www-data -R .`
    
    `sudo` `find` `. -``type` `d -name \* -``exec` `chmod` `775 {} \;`
    
    `sudo` `find` `. -``type` `f -``exec` `chmod` `664 {} \;`
    
3.  Interne Caches speichert i-doit unterhalb des `temp/`\-Verzeichnisses. Die Inhalte sollten komplett entfernt werden. Bei der ersten Verwendung von i-doit werden die Caches automatisch erstellt:
    
    [?](#)
    
    `sudo` `rm` `-r temp/*`
    
4.  Es sollte kontrolliert werden, ob die Datei `.htaccess` kopiert wurde:
    
    [?](#)
    
    `ls` `-lha` `/var/www/html/i-doit/``.htaccess`
    

Datenbanken umziehen
--------------------

1.  i-doit benötigt mindestens 2 [Datenbanken](/display/de/Datenbank-Modell). Von jeder einzelnen sollte auf dem alten System ein Dump erstellt werden:
    
    [?](#)
    
    `mysqldump -uroot -p idoit_system >` `/tmp/idoit_system``.sql`
    
    `mysqldump -uroot -p idoit_data >` `/tmp/idoit_data``.sql`
    
    ###### \*Wenn beim einspielen der Datenbank diese Fehlermeldung erscheint "Can't create table \`idoit\_data\`.\`table\_name\` (errno: 140 "Wrong create options")". Ist die Lösung dazu [HIER](https://kb.i-doit.com/pages/viewpage.action?pageId=97288438) zu finden.
    
2.  Diese Dumps kopieren wir auf das neue System:
    
    [?](#)
    
    `scp` `user@oldsystem:``/tmp/idoit_system``.sql` `/tmp/`
    
    `scp` `user@oldsystem:``/tmp/idoit_data``.sql` `/tmp/`
    
    `scp` `/tmp/idoit_system``.sql user@newsystem:``/tmp/`
    
    `scp` `/tmp/idoit_data``.sql user@newsystem:``/tmp/`
    
3.  Diese Dumps spielen wir im neuen System ein:
    
    [?](#)
    
    `mysql -uroot -p`
    
    `CREATE DATABASE idoit_system;`
    
    `CREATE DATABASE idoit_data;`
    
    `exit`
    
    `mysql -uroot -p idoit_system <` `/tmp/idoit_system``.sql`
    
    `mysql -uroot -p idoit_data <` `/tmp/idoit_data``.sql`
    
4.  Beim ursprünglichen [Setup](/display/de/Setup) von i-doit wurde ein MySQL-Benutzer erstellt (Standard: `idoit`). Dieser muss mit denselben Berechtigungen und demselben Passwort auf dem neuen System vorhanden sein. Dazu melden wir uns mit dem Superuser von MySQL an:
    
    [?](#)
    
    `mysql -uroot -p`
    
    Nun führen wir die nötigen SQL-Befehle aus:
    
    [?](#)
    
    `GRANT` `ALL` `PRIVILEGES` `ON` `idoit_system.*` `TO` `'idoit'``@``'localhost'` `IDENTIFIED` `BY` `'mypasswd'``;`
    
    `GRANT` `ALL` `PRIVILEGES` `ON` `idoit_data.*` `TO` `'idoit'``@``'localhost'` `IDENTIFIED` `BY` `'mypasswd'``;`
    
    `exit;`
    
    Für einen Test melden wir uns mit diesem Benutzer an:
    
    [?](#)
    
    `mysql -uidoit -p`
    
    Wenn wir schon einmal dabei sind, können wir die Credentials für die Mandanten-Datenbanken überprüfen:
    
    [?](#)
    
    `SELECT` `*` `FROM` `idoit_system.isys_mandator;`
    
    `exit;`
    
    Das obige Passwort für den Benutzer `idoit` sollte deckungsgleich mit den Angaben in der Datei `/var/www/html/i-doit/src/config.inc.php` sein.
    

Nachbereitung
-------------

1.  DNS-Einträge, IP-Adressen, Hostnames etc. sollten im Nachgang angepasst werden, damit i-doit wie gewohnt erreichbar ist.
2.  Schnittstellen zu Drittsystemen können nun wieder aktiviert werden. Die Funktionen sollten geprüft werden.
3.  Cronjobs sollten wieder aktiviert und getestet werden.
4.  [Backups](/display/de/Daten+sichern+und+wiederherstellen) sollten eingerichtet und getestet werden.
5.  Wenn auch die WebGUI wie gewohnt reagiert und alle Daten in i-doit augenscheinlich vorhanden sind, ist der Umzug erfolgreich verlaufen.