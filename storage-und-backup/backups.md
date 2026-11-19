---
title: Backups für Server, Volumes und Datenbanken
description: Ein Backup eines Servers sichert seine **Systemplatte**. Weitere angehängte Volumes sind darin nicht enthalten; für jedes Volume erstellen Sie ein eigenes Backup. Das gilt auch für die Systemplatte, wenn Sie Backups direkt in den Volume-Details verwa
published: true
date: 2026-11-19T12:05:10.885Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T11:51:45.492Z
---

## Was wird gesichert?

Ein Backup eines Servers sichert seine **Systemplatte**. Weitere angehängte Volumes sind darin nicht enthalten; für jedes Volume erstellen Sie ein eigenes Backup. Das gilt auch für die Systemplatte, wenn Sie Backups direkt in den Volume-Details verwalten möchten. Ein Backup ist kein Ersatz für ein Datenbank-Backup: Für verwaltete Datenbanken verwenden Sie die Funktionen auf der jeweiligen Datenbank-Detailseite.

## Manuelles Backup einer Server-Systemplatte

1. Öffnen Sie **Server** und wählen Sie den gewünschten Server.
2. Scrollen Sie auf der Detailseite zum Abschnitt **Backups**.
3. Wählen Sie **Backup erstellen**, tragen Sie im Feld **Name** einen Namen ein und wählen Sie erneut **Backup erstellen**.
4. Warten Sie, bis das Backup in der Liste als verfügbar angezeigt wird. Während des Vorgangs aktualisiert sich die Liste automatisch.

Ein Backup eines laufenden Servers ist nur absturzkonsistent. Für einen konsistenten Stand fahren Sie den Server vor dem Erstellen herunter. Der Name darf 1 bis 255 Zeichen lang sein.

## Manuelles Backup eines Volumes

1. Öffnen Sie **Volumes** und wählen Sie das gewünschte Volume.
2. Scrollen Sie zum Abschnitt **Backups**.
3. Wählen Sie **Backup erstellen**, geben Sie einen Namen ein und bestätigen Sie mit **Backup erstellen**.
4. Prüfen Sie in der Liste den Status des Backups.

Backups angehängter Volumes sind nur absturzkonsistent. Für einen konsistenten Stand trennen Sie das Volume vorher vom Server oder fahren Sie den Server herunter. Kubernetes-verwaltete Volumes können in dieser Ansicht nur eingesehen werden.

## Datenbank-Backup erstellen

1. Öffnen Sie **Datenbanken** und wählen Sie die aktive Instanz.
2. Wählen Sie unter **Backups** die Aktion **Backup erstellen**.
3. Warten Sie, bis das Backup abgeschlossen ist. Über **Wiederherstellen** erstellen Sie daraus eine neue Instanz gleicher Größe.

Weitere Schritte finden Sie unter [Datenbanken](/datenbanken/datenbanken).

## Automatische nächtliche Backups

Server, Volumes und Datenbanken können einen automatischen Backup-Plan haben. Aktivieren Sie im jeweiligen Detailseitenabschnitt **Automatische Backups** den Schalter. Wählen Sie unter **Aufbewahren:** **3 Backups**, **5 Backups** oder **7 Backups**. Der Standard sind 7 Backups. Im Abschnitt erscheinen der Status sowie, wenn aktiviert, die Angaben **Nächster Lauf** und **Letzter Lauf**. Bei Servern umfasst auch der automatische Plan ausschließlich die Systemplatte; zusätzliche Volumes benötigen eigene Pläne. Die geplante Ausführung liegt nachts im Zeitfenster 01:00–04:59 Uhr (Europe/Berlin); die konkrete Uhrzeit variiert je Ressource und wird in der Oberfläche angezeigt.

Wenn Sie die Anzahl der aufzubewahrenden Backups verringern, werden ältere automatische Backups beim nächsten Lauf gelöscht. Zum Ausschalten betätigen Sie den Schalter und bestätigen Sie die Deaktivierung. Bereits erstellte Backups bleiben dabei erhalten.

## Wiederherstellen

- **Volume-Backup:** Öffnen Sie die Liste der Backups, wählen Sie im Aktionsmenü des verfügbaren Backups **Als neues Volume wiederherstellen**. Es wird ein neues Volume angelegt; das Quell-Volume bleibt bestehen. Bei einem bootfähigen Volume können Sie stattdessen **Server aus Backup erstellen** wählen.
- **Server-Backup:** Bei verfügbaren Backups zeigt das Aktionsmenü **Server aus Backup erstellen**. Für Systemplatten-Backups auf Volume-Basis wird zuerst ein neues Volume wiederhergestellt und anschließend der Dialog zum Erstellen eines Servers geöffnet. Folgen Sie dort den angezeigten Schritten.

Die Wiederherstellung überschreibt nicht das bestehende Volume oder den bestehenden Server. Das neue Volume wird erst nutzbar, wenn sein Status **Verfügbar** ist.

## Löschen

Öffnen Sie das Aktionsmenü des Backups und wählen Sie **Löschen**. Bestätigen Sie den Dialog **Backup löschen** mit **Löschen**. Das Löschen ist endgültig. Wenn Sie einen Server löschen, können dessen Systemplatten-Backups mitgelöscht werden; die Bestätigungsansicht nennt dies gegebenenfalls ausdrücklich. Angefügte Volumes und deren Backups verwalten Sie weiterhin separat unter **Volumes**.

## Backups per CLI auflisten

Mit einem für Ihr Projekt eingerichteten OpenStack-CLI-Zugang können Sie die Listen auch abfragen. S3 Access Keys eignen sich dafür nicht.

```sh
openstack volume backup list
openstack database backup list
```

## Kontingent und Grenzen

- Namen für Server- und Volume-Backups: 1 bis 255 Zeichen.
- Das zulässige Kontingent ist projektabhängig. Bei erschöpftem Volume-Backup-Kontingent löschen Sie nicht mehr benötigte Backups oder bitten den Support, das Kontingent zu erhöhen.
- Server-Backups sind nicht verfügbar, wenn die Konsole die Systemplatte des Servers nicht eindeutig zuordnen kann.
