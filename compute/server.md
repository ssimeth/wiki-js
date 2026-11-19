---
title: Server erstellen und verwalten
description: Ein Server startet von einer Systemplatte auf einem Volume. Die Größe der neu angelegten Systemplatte richtet sich nach dem gewählten Server-Typ und den Anforderungen des Images. Zusätzliche Daten-Volumes verwalten Sie separat unter [Volumes](/comput
published: true
date: 2026-11-19T13:20:52.297Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T13:20:52.297Z
---

Ein Server startet von einer Systemplatte auf einem Volume. Die Größe der neu angelegten Systemplatte richtet sich nach dem gewählten Server-Typ und den Anforderungen des Images. Zusätzliche Daten-Volumes verwalten Sie separat unter [Volumes](/compute/volumes).

## Server erstellen

1. Öffnen Sie im gewünschten Projekt **Compute** → **Server** und klicken Sie auf **Server erstellen**.
2. Tragen Sie unter **Server Name** einen Namen ein und wählen Sie den **Standort**.
3. Wählen Sie unter **Image** → **Betriebssysteme** das gewünschte Betriebssystem und die Version. Bereits erstellte Snapshots finden Sie unter **Eigene Snapshots**.
4. Wählen Sie unter **Server Typ** die gewünschte Größe. Die Auswahl zeigt vCPU, RAM und Plattengröße.
5. Wählen Sie unter **Netzwerk** → **Private Netzwerke** mindestens ein privates Netzwerk. Falls noch keines vorhanden ist, folgen Sie [Netzwerke, Router und Floating IPs](/netzwerk/netzwerke-router-floating-ips).
6. Falls angeboten, aktivieren oder deaktivieren Sie **Öffentliches IPv6**. Die Option ist zunächst aktiviert. Für eingehende Verbindungen benötigen Sie passende Regeln in den Security Groups. Siehe [Öffentliches IPv6 für Server](/netzwerk/oeffentliches-ipv6).
7. Wählen Sie unter **SSH Key** Ihren Schlüssel. Falls noch keiner hinterlegt ist, verwenden Sie **SSH Key hinzufügen**. Siehe [SSH Keys](/sicherheit/ssh-keys).
8. Klicken Sie auf **Server erstellen** und prüfen Sie den Status in der Serverübersicht.

Bei einem neuen Betriebssystem ist ein SSH Key erforderlich, sofern Sie keinen Zugang über Cloud Config einrichten. Ein Snapshot enthält bereits die Benutzer und Schlüssel seines Quellservers. Weitere [Security Groups](/sicherheit/security-groups) weisen Sie nach dem Erstellen über die Gruppenverwaltung zu; das Serverformular bietet dafür keine Auswahl.

## Details und öffentliche Erreichbarkeit

Klicken Sie in der Serverübersicht auf den Server. Die Detailseite zeigt unter anderem **Hardware**, **System**, **Netzwerke** und **Volumes**. Unter **System** finden Sie den SSH Key und die **Security Groups**.

Für öffentlichen IPv4-Zugriff weisen Sie dem Server eine Floating IP zu. Folgen Sie dazu [Netzwerke, Router und Floating IPs](/netzwerk/netzwerke-router-floating-ips).

## Starten, stoppen und neu starten

1. Öffnen Sie **Server** und das Aktionsmenü des gewünschten Servers.
2. Wählen Sie bei einem gestoppten Server **Starten**.
3. Bei einem aktiven Server stehen **Stoppen** und **Reboot** zur Verfügung.
4. Warten Sie, bis die Statusanzeige die Aktion abgeschlossen anzeigt.

**Starten** und **Stoppen** erreichen Sie auch auf der Detailseite über **Aktionen**.

## Snapshot erstellen und verwenden

1. Öffnen Sie die Detailseite des Servers und scrollen Sie zu **Snapshots**.
2. Klicken Sie auf **Snapshot erstellen** und prüfen oder ändern Sie den vorgeschlagenen **Name**.
3. Bestätigen Sie mit **Snapshot erstellen**. Der Name darf 1 bis 255 Zeichen lang sein.
4. Sobald der Snapshot verfügbar ist, wählen Sie in seinem Aktionsmenü **Server aus Snapshot erstellen** und folgen dem Serverformular.

Bei einem von einem Volume gestarteten Server umfasst der Snapshot die Systemplatte und alle angehängten Volumes. Der daraus erstellte Server erhält Kopien dieser Volumes. Ein Snapshot eines laufenden Servers ist nur absturzkonsistent; fahren Sie den Server für einen konsistenten Stand vorher herunter.

Zum Entfernen wählen Sie im Aktionsmenü des Snapshots **Löschen** und bestätigen **Snapshot löschen** mit **Löschen**. Die zugehörigen Volume-Snapshots werden dabei ebenfalls gelöscht.

## Backups der Systemplatte

Im Abschnitt **Backups** erstellen und verwalten Sie Sicherungen der Systemplatte. Zusätzliche Volumes benötigen eigene Backups. Manuelle und automatische Backups sowie die Wiederherstellung beschreibt [Backups für Server, Volumes und Datenbanken](/storage-und-backup/backups).

## Server löschen

1. Öffnen Sie in der Serverübersicht das Aktionsmenü und wählen Sie **Löschen**.
2. Prüfen Sie den Dialog **Server löschen**. Bei neu erstellten Servern bleibt die Systemplatte standardmäßig mit ihren Snapshots und Backups unter **Volumes** erhalten und belegt weiterhin Speicher.
3. Wenn Sie auch die Systemplatte entfernen möchten, aktivieren Sie **Systemplatte ebenfalls löschen**. Der Dialog nennt die Snapshots und Backups, die dabei ebenfalls gelöscht werden.
4. Bestätigen Sie mit **Server löschen**. Prüfen Sie anschließend mögliche Hinweise, falls die Systemplatte oder Sicherungen nicht vollständig gelöscht werden konnten.

Bei älteren Servern kann die Systemplatte zusammen mit dem Server gelöscht werden. Vorhandene Snapshots können dies verhindern; beachten Sie dann die Option **Snapshots ebenfalls löschen** und den Hinweis im Dialog. Zusätzliche Daten-Volumes verwalten Sie weiterhin separat unter **Volumes**.

Für Kubernetes-Nodes gelten die Einschränkungen unter [Von Kubernetes verwaltete Ressourcen](/allgemein/verwaltete-ressourcen).
