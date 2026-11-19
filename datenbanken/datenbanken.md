---
title: Datenbanken erstellen und verwalten
description: Der verwaltete Datenbankdienst basiert auf OpenStack Trove. In der Konsole verwalten Sie MySQL-, MariaDB- und PostgreSQL-Instanzen. Die im Erstellungsdialog verfügbaren Engines und Versionen hängen vom aktuellen Angebot ab. Eine Instanz ist nur in de
published: false
date: 2026-11-19T11:55:02.164Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T11:51:19.193Z
---

## Unterstützte Datenbanken

Der verwaltete Datenbankdienst basiert auf OpenStack Trove. In der Konsole verwalten Sie MySQL-, MariaDB- und PostgreSQL-Instanzen. Die im Erstellungsdialog verfügbaren Engines und Versionen hängen vom aktuellen Angebot ab. Eine Instanz ist nur in dem Netzwerk erreichbar, das Sie beim Erstellen auswählen; achten Sie darauf, dass dieses Netzwerk mit einem Router verbunden ist.

## Datenbank-Instanz erstellen

1. Öffnen Sie **Datenbanken** und wählen Sie **Datenbank erstellen**.
2. Geben Sie einen **Name** ein und wählen Sie **Engine**, **Version**, **Größe** und **Speicher (GB)**.
3. Wählen Sie das **Netzwerk**. Der Dialog weist darauf hin, wenn es offenbar mit keinem Router verbunden ist.
4. Optional können Sie unter **Erster Zugang** eine erste Datenbank und einen Benutzer angeben. Für diesen Benutzer wird ein sicheres Passwort erzeugt und genau einmal angezeigt. Kopieren und speichern Sie es direkt.
5. Optional wählen Sie **Mit Replikat (Hochverfügbarkeit)**. Das Replikat wird nach Bereitstellung automatisch angelegt und benötigt dieselbe Größe zusätzlich.
6. Wählen Sie **Erstellen**. Die Instanz erscheint in der Übersicht; Datenbanken, Benutzer und Backups können Sie verwalten, sobald sie aktiv ist.

Namen dürfen mit einem Buchstaben oder einer Ziffer beginnen und danach Buchstaben, Ziffern, Bindestriche oder Unterstriche enthalten; maximal 64 Zeichen. Der Speicher kann 1 bis 1000 GB betragen. Welche Größen, Versionen, Netzwerke und Kontingente für Ihr Projekt verfügbar sind, zeigt der Dialog.

## Datenbanken und Benutzer verwalten

1. Öffnen Sie **Datenbanken** und wählen Sie die Instanz.
2. Im Abschnitt **Datenbanken** geben Sie den Datenbanknamen ein und wählen **Anlegen**. Eine Datenbank löschen Sie mit dem Papierkorb in ihrer Zeile und bestätigen anschließend den Dialog.
3. Im Abschnitt **Benutzer** geben Sie einen Namen ein, wählen optional unter **Zugriff auf Datenbank** eine Datenbank und wählen **Anlegen**.
4. Kopieren und speichern Sie das erzeugte Benutzerpasswort sofort. Es wird einmalig angezeigt. Mit dem Papierkorb entfernen Sie einen Benutzer; der Zugriff wird damit entzogen.

Von der Plattform verwaltete Systembenutzer sind nicht für Kunden bestimmt und lassen sich nicht anlegen oder löschen.

## Verbindung zur Instanz

Auf der Instanz-Detailseite zeigt der Abschnitt **Verbindung** Host und Port sowie einen Client-Befehl, den Sie kopieren können. Das Format ist beispielsweise:

```sh
DB_HOST='HOST_AUS_DER_KONSOLE'
DB_USER='IHR_BENUTZER'
DB_NAME='IHRE_DATENBANK'
mysql -h "$DB_HOST" -P 3306 -u "$DB_USER" -p "$DB_NAME"
```

Für MariaDB wird `mariadb` statt `mysql` angezeigt. Für PostgreSQL zeigt die Konsole einen Befehl im Format:

```sh
psql -h "$DB_HOST" -p 5432 -U "$DB_USER" "$DB_NAME"
```

Setzen Sie die Variablen auf Ihre Zugangsdaten und den in der Konsole angezeigten Host. Die Befehle entsprechen dem Verbindungsbeispiel der Konsole. Der Client muss das ausgewählte Instanznetzwerk erreichen können.

## Manuelle Datenbank-Backups

1. Öffnen Sie die Instanz-Detailseite und den Abschnitt **Backups**.
2. Wählen Sie **Backup erstellen**. Die Konsole startet das Backup und zeigt es mit Status, Erstellzeit und Größe in der Liste.
3. Sobald ein Backup den Status **Fertig** erreicht hat, können Sie **Wiederherstellen** wählen. Bestätigen Sie den Dialog.

Die Wiederherstellung legt eine **neue Instanz** mit gleicher Größe an. Die bestehende Instanz bleibt erhalten. Zum Löschen eines Backups wählen Sie das Papierkorb-Symbol in seiner Zeile und bestätigen **Löschen**; das Backup wird endgültig gelöscht.

## Automatische Backups

Im Abschnitt **Automatische Backups** auf der Instanz-Detailseite können Sie den Schalter aktivieren und unter **Aufbewahren:** **3 Backups**, **5 Backups** oder **7 Backups** auswählen. Standardmäßig werden 7 Backups aufbewahrt. Der Lauf findet nachts im Zeitfenster 01:00–04:59 Uhr (Europe/Berlin) statt; die konkrete Uhrzeit wird in der Oberfläche als **Nächster Lauf** angezeigt. Status und letzter Lauf werden ebenfalls angezeigt.

Wenn Sie die Aufbewahrung verringern, werden ältere automatische Backups über der ausgewählten Anzahl beim nächsten Lauf gelöscht. Beim Ausschalten bleiben bereits vorhandene Backups erhalten.

## Kontingente und Hinweise

Das Projektkontingent kann die Zahl der Instanzen, den gesamten Datenbankspeicher oder Backups begrenzen. Der Erstellungsdialog zeigt bekannte Kontingente und den verfügbaren Verbrauch an; das System prüft sie beim Erstellen erneut. Ein Replikat belegt eine zusätzliche Instanz und die gleiche Speichermenge. Bei ausgeschöpftem Kontingent löschen Sie nicht mehr benötigte Ressourcen oder lassen Sie das Projektkontingent erhöhen.

Das Löschen einer Instanz entfernt ihre Daten. Bereits vorhandene Backups bleiben laut Bestätigungsdialog erhalten.
