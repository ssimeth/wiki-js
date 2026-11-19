---
title: Erste Schritte in der Honeycomb Cloud Console
description: Die Konsole zeigt die Ressourcen des ausgewählten Projekts. Prüfen Sie vor dem Erstellen oder Ändern einer Ressource oben unter **Projekt**, ob das richtige Projekt aktiv ist.
published: true
date: 2026-11-19T15:33:27.302Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T13:20:47.148Z
---

Die Konsole zeigt die Ressourcen des ausgewählten Projekts. Prüfen Sie vor dem Erstellen oder Ändern einer Ressource oben unter **Projekt**, ob das richtige Projekt aktiv ist.

## Anmelden

1. Öffnen Sie die Honeycomb Cloud Console.
2. Geben Sie unter **Domain** Ihre Domain ein und wählen Sie **Weiter**. Bei einer bereits gespeicherten Domain beginnt die Anmeldung direkt mit den Zugangsdaten; über **ändern** wechseln Sie die Domain.
3. Geben Sie im Feld **E-Mail oder Benutzername** Ihre E-Mail-Adresse und Ihr **Passwort** ein. Klicken Sie auf **Anmelden**. Ihre Zugangsdaten erhalten Sie per E-Mail; dort ist auch der Anmeldename angegeben.
4. Falls die Zwei-Faktor-Authentifizierung abgefragt wird, geben Sie den **Bestätigungscode** ein und klicken Sie auf **Bestätigen**.

## Projekt auswählen und wechseln

1. Bei mehreren verfügbaren Projekten öffnet sich nach der Anmeldung die Projektübersicht. Wählen Sie beim gewünschten Projekt **Öffnen**. Bei genau einem Projekt gelangen Sie direkt zum Dashboard.
2. Zum Wechseln öffnen Sie oben die Auswahl **Projekt** und wählen das gewünschte Projekt.
3. Über **Alle Projekte anzeigen** gelangen Sie zur Projektübersicht zurück.

## Dashboard und Navigation

Unter **Dashboard** sehen Sie die Ressourcen Ihres Projekts. Die Kacheln führen zur jeweiligen Ressourcenübersicht. Darunter finden Sie **Kontingente**, **Letzte Aktivitäten** und **Hinweise**.

Die Navigation ist in folgende Gruppen aufgeteilt:

- **Compute:** **Server** und **Volumes**.
- **Managed Services:** **Kubernetes** und **Datenbanken**.
- **Netzwerk:** **Topologie**, **Netzwerke**, **Router**, **Load Balancer** und **Floating IPs**.
- **Storage:** **Object Storage** und **File Storage**.
- **Sicherheit:** **Security Groups**, **SSH Keys** und **Access Keys**.

Auf kleinen Bildschirmen öffnen Sie das Menü über **Navigation öffnen**.

## Verbindungen in der Topologie prüfen

1. Öffnen Sie **Netzwerk** → **Topologie**.
2. Wählen Sie **Netzwerk**, um Netzwerke, Router und angeschlossene Server zu überblicken. Unter **Load Balancer** sehen Sie die zugehörigen Verbindungen.
3. Laden Sie die Ansicht bei Bedarf mit **Aktualisieren** neu.

## Schnellstart: erster erreichbarer Server

1. Hinterlegen Sie unter **Sicherheit** → **SSH Keys** Ihren öffentlichen Schlüssel. Die Schritte finden Sie unter [SSH Keys](/sicherheit/ssh-keys).
2. Erstellen Sie ein privates Netzwerk mit Subnetz und verbinden Sie es über einen Router mit externem Gateway. Folgen Sie [Netzwerke, Router und Floating IPs](/netzwerk/netzwerke-router-floating-ips).
3. Erstellen Sie unter **Compute** → **Server** einen Server mit Image, Server-Typ, privatem Netzwerk und SSH Key. Siehe [Server erstellen und verwalten](/compute/server).
4. Weisen Sie für öffentlichen IPv4-Zugriff eine Floating IP zu. Alternativ aktivieren Sie beim Erstellen des Servers **Öffentliches IPv6**, sofern die Option angeboten wird. Details finden Sie unter [Öffentliches IPv6 für Server](/netzwerk/oeffentliches-ipv6).
5. Erlauben Sie die benötigten eingehenden Verbindungen, etwa SSH, in den [Security Groups](/sicherheit/security-groups).

Zusätzlichen Speicher verwalten Sie unter [Volumes](/compute/volumes), Sicherungen unter [Backups für Server, Volumes und Datenbanken](/storage-und-backup/backups). Für die Verteilung von Anfragen auf mehrere Ziele verwenden Sie [Load Balancer](/netzwerk/load-balancer).
