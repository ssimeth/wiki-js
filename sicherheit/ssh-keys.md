---
title: SSH Keys
description: Unter **SSH Keys** hinterlegen Sie öffentliche Schlüssel für den Zugriff auf Ihre Server.
published: true
date: 2026-11-19T13:21:13.805Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T13:21:13.805Z
---

Unter **SSH Keys** hinterlegen Sie öffentliche Schlüssel für den Zugriff auf Ihre Server.

## Öffentlichen SSH Key hinzufügen

1. Öffnen Sie **Sicherheit** → **SSH Keys** und wählen Sie **SSH Key erstellen**.
2. Geben Sie unter **Name** eine Bezeichnung ein.
3. Fügen Sie unter **Public Key** Ihren öffentlichen SSH-Schlüssel ein.
4. Wählen Sie **SSH Key erstellen**.

Die Felder **Name** und **Public Key** sind erforderlich. Fügen Sie nur den öffentlichen Schlüssel ein.

## SSH Key beim Erstellen eines Servers verwenden

1. Wählen Sie beim [Erstellen eines Servers](/compute/server) im Abschnitt **SSH Key** den gewünschten Schlüssel. Es kann ein Schlüssel ausgewählt werden.
2. Schließen Sie die Server-Erstellung ab.

Wenn genau ein SSH Key vorhanden ist, wird er vorausgewählt. Für die Cloud-Images der Plattform ist ein SSH Key erforderlich, sofern Sie weder ein vorhandenes Startvolume noch einen Snapshot verwenden oder Cloud Config für den Zugang einrichten. Bei einem Snapshot ist ein zusätzlicher Schlüssel optional, da der Snapshot Benutzer und SSH Keys des ursprünglichen Servers enthält.

Wenn noch kein Schlüssel vorhanden ist, wählen Sie im Abschnitt **SSH Key** den Link **SSH Key hinzufügen**.

## SSH Key löschen

Wählen Sie in der Liste beim Schlüssel das Papierkorb-Symbol und bestätigen Sie **SSH Key löschen** im Dialog.
