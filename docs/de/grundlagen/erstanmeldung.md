# Erstanmeldung

i-doit ist [installiert](../installation/manuelle-installation/index.md) und der erste Login steht an? Nichts leichter als das. Dennoch gibt es einige Dinge zu bedenken, weswegen sich das Lesen dieses Artikels lohnt.

Standard-Benutzer und -Gruppen
------------------------------
<!---Todo: Fixme--->
Für die Anmeldung gibt es einige Standard-Benutzer, die Standard-Gruppen zugewiesen sind und somit vordefinierte [Rechte](/display/de/Rechteverwaltung) besitzen:

| Benutzer | Passwort | Gruppe | Rechte (oberflächlich) |
| --- | --- | --- | --- |
| **admin** | **admin** | **Admin** | Alle Rechte, auch für die Verwaltung |
| **archivar** | **archivar** | **Archivar** | Lesen und ändern |
| **author** | **author** | **Author** | Anlegen, ändern, archivieren und ausführen |
| **editor** | **editor** | **Editor** | Lesen und ändern |
| **reader** | **reader** | **Reader** | Lesen |

Die aufgelisteten Benutzer erhalten ihre Rechte durch die gleichnamigen Gruppen.

Anmelden
--------

Aus den oben genannten Standard-Benutzern wählt man bestenfalls den Benutzer **admin** aus, der in der Funktionsweise nicht eingeschränkt ist.

[![login](../assets/images/grundlagen/erstanmeldung/1-erstanmeldung.png)](../assets/images/grundlagen/erstanmeldung/1-erstanmeldung.png)

Weitere Benutzer und Gruppen hinzufügen
---------------------------------------
<!---Todo: Fixme--->
Jeder Benutzer in i-doit ist ein [Objekt](struktur-it-dokumentation.md) vom Typ **Personen**. Es ist _dringend zu empfehlen_ nach dem ersten Login eine [LDAP-Kopplung](/pages/viewpage.action?pageId=9666615) oder weitere lokale Benutzer einzurichten _und_ den Login der oben genannten Benutzer zu ändern. Hierfür werden in der [Objekttypgruppe](struktur-it-dokumentation.md) **Kontakte** unter dem [Objekttyp](struktur-it-dokumentation.md) **Personen** der jeweilige Benutzer ausgewählt und in der Kategorie **Personen → Login** die Zugangsdaten geändert. Alternativ können die **Personen**\-Objekte [archiviert](lebens-und-dokumentationszyklus.md) werden. Dadurch wird der Login dieser Benutzer verweigert.

[![Benutzer-und-Gruppen](../assets/images/grundlagen/erstanmeldung/2-erstanmeldung.png)](../assets/images/grundlagen/erstanmeldung/2-erstanmeldung.png)

<!---Todo: Fixme--->
!!! success "Lokaler Administrator"

    Auch wenn ein [LDAP-Verzeichnisserver oder ein Active Directory (AD)](/pages/viewpage.action?pageId=9666615) zum Einsatz kommt, bietet es sich an, trotzdem einen lokalen Benutzer mit allen Rechten anzulegen. Falls nämlich der externe Dienst nicht erreichbar sein sollte, kann man sich immer noch mit dem lokalen Benutzer anmelden.

Begrüßungstext
--------------

Wer Benutzer direkt beim Login mit einem Text begrüßen möchte, kann dies tun: Der Text wird unter **Verwaltung → Systemeinstellungen → Login → Willkommensnachricht für Login** hinterlegt.

[![login-begruessungstext](../assets/images/grundlagen/erstanmeldung/3-erstanmeldung.png)](../assets/images/grundlagen/erstanmeldung/3-erstanmeldung.png)