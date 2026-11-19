---
title: Volumes erstellen und verwalten
description: Volumes sind persistente Datenträger für Ihre Server. Unter **Compute** → **Volumes** finden Sie zusätzliche Daten-Volumes und die Systemplatten Ihrer Server.
published: true
date: 2026-11-19T13:20:58.503Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T13:20:58.503Z
---

Volumes sind persistente Datenträger für Ihre Server. Unter **Compute** → **Volumes** finden Sie zusätzliche Daten-Volumes und die Systemplatten Ihrer Server.

## Volume erstellen

1. Öffnen Sie **Compute** → **Volumes** und klicken Sie auf **Volume erstellen**.
2. Geben Sie **Volume-Name** und **Größe (GB)** ein. Das Formular erlaubt 10 bis 1024 GB.
3. Tragen Sie bei Bedarf eine **Beschreibung (optional)** ein.
4. Bestätigen Sie mit **Volume erstellen** und warten Sie auf den Status **Verfügbar**.

## An einen Server anhängen

1. Öffnen Sie beim verfügbaren Volume das Aktionsmenü und wählen Sie **An Server anhängen**.
2. Wählen Sie im Dialog **Volume anhängen** den gewünschten Server.
3. Klicken Sie auf **Anhängen** und warten Sie, bis das Volume als **In Verwendung** angezeigt wird.

## Vom Server trennen

1. Öffnen Sie beim angehängten Volume das Aktionsmenü.
2. Wählen Sie **Von Server trennen**.
3. Warten Sie, bis der Status wieder **Verfügbar** ist.

## Volume-Details öffnen

Klicken Sie auf den Namen des Volumes. Die Detailseite zeigt **Größe**, **Typ**, **Bootfähig** und **Angehängt an**. Bei einer erkannten Systemplatte steht stattdessen **Systemplatte von**; der Servername führt zur Server-Detailseite.

Über **Aktionen** können Sie das Volume anhängen, trennen, vergrößern oder löschen. Bei einem verfügbaren, bootfähigen Volume erscheint außerdem **Server erstellen**. Damit öffnen Sie das Serverformular mit diesem Volume als Systemplatte. Siehe [Server erstellen und verwalten](/compute/server).

## Volume vergrößern

1. Öffnen Sie die Detailseite des Volumes.
2. Wählen Sie **Aktionen** → **Volume vergrößern**.
3. Geben Sie im Dialog **Volumegröße anpassen** unter **Neue Größe (GB)** einen Wert ein, der größer als die aktuelle Größe ist.
4. Klicken Sie auf **Speichern** und warten Sie, bis die Anpassung abgeschlossen ist. Der Vorgang kann einige Minuten dauern.

Eine Verkleinerung ist nicht vorgesehen.

## Snapshots und Backups

Im Abschnitt **Backups** der Volume-Detailseite erstellen Sie manuelle Sicherungen, verwalten automatische Backups und stellen Sicherungen als neue Volumes wieder her. Folgen Sie [Backups für Server, Volumes und Datenbanken](/storage-und-backup/backups).

Snapshots erstellen Sie auf der Detailseite eines Servers. Bei einem von einem Volume gestarteten Server umfassen sie auch die angehängten Volumes. Die Schritte finden Sie unter [Server erstellen und verwalten](/compute/server).

## Volume löschen

1. Trennen Sie ein angehängtes Volume zuerst vom Server und warten Sie auf das Ende laufender Aktionen.
2. Öffnen Sie auf der Volume-Detailseite **Aktionen** → **Löschen**.
3. Bestätigen Sie den Dialog **Volume löschen** mit **Volume löschen**. Das Löschen ist endgültig.

Kubernetes-verwaltete Volumes können Sie auf der Detailseite nur einsehen. Ändern Sie diese über den zugehörigen Cluster; siehe [Von Kubernetes verwaltete Ressourcen](/allgemein/verwaltete-ressourcen).
