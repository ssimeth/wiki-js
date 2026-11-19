---
title: Security Groups
description: Security Groups steuern den ein- und ausgehenden Netzwerkverkehr Ihrer Server. Regeln legen Richtung, Protokoll, Ports und einen IP-Bereich in CIDR-Schreibweise fest.
published: true
date: 2026-11-19T13:21:08.797Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T13:21:08.797Z
---

Security Groups steuern den ein- und ausgehenden Netzwerkverkehr Ihrer Server. Regeln legen Richtung, Protokoll, Ports und einen IP-Bereich in CIDR-Schreibweise fest.

## Security Group erstellen

1. Öffnen Sie **Sicherheit** → **Security Groups** und wählen Sie **Security Group erstellen**.
2. Geben Sie unter **Name** einen Namen ein. **Beschreibung** ist optional.
3. Wählen Sie **Security Group erstellen**.

## Standardgruppe prüfen

Falls eine Gruppe `default` vorhanden ist, öffnen Sie diese, um ihre Regeln und zugeordneten Server zu prüfen. Die Konsole zeigt die bestehenden Regeln des Projekts an. Jede Security Group – die Gruppe `default` und jede neu angelegte – enthält ab Werk Regeln für ausgehenden Verkehr (IPv4 und IPv6) sowie eingehendes ICMPv6. ICMPv6 wird für IPv6 und Neighbor Discovery (NDP) benötigt; entfernen Sie diese Regel nicht, wenn Ihr Server öffentliches IPv6 nutzt. Alle anderen eingehenden Verbindungen, etwa SSH, geben Sie mit eigenen Regeln frei.

## Regel hinzufügen

1. Öffnen Sie die gewünschte Security Group und wechseln Sie zum Tab **Regeln**.
2. Wählen Sie **Regel hinzufügen**.
3. Wählen Sie unter **Richtung** **Eingehend** oder **Ausgehend**.
4. Wählen Sie unter **Protokoll** **TCP**, **UDP** oder **ICMP**.
5. Geben Sie für TCP oder UDP **Von Port** und **Bis Port** ein. Für einen einzelnen Port tragen Sie in beide Felder denselben Wert ein. Bei ICMP werden keine Portfelder angezeigt.
6. Tragen Sie unter **Quelle (CIDR)** den gewünschten IPv4- oder IPv6-Bereich ein, zum Beispiel `192.0.2.0/24` oder `2001:db8::/32`. `0.0.0.0/0` steht für alle IPv4-Adressen, `::/0` für alle IPv6-Adressen.
7. Bestätigen Sie mit **Regel hinzufügen**.

Die Plattform leitet IPv4 oder IPv6 aus dem CIDR-Bereich ab. Für ICMP mit einem IPv6-Bereich wird automatisch das IPv6-Protokoll ICMPv6 verwendet. Das Feld **Quelle (CIDR)** bezeichnet bei eingehenden Regeln den Quellbereich, bei ausgehenden Regeln den Zielbereich. Die Regelübersicht zeigt Richtung, Protokoll, Port und Quelle. Löschen Sie eine einzelne Regel über das Papierkorb-Symbol.

## Security Group einem Server zuweisen oder entfernen

1. Öffnen Sie die Security Group und wechseln Sie zum Tab **Ressourcen**.
2. Wählen Sie **Ressource hinzufügen** und anschließend beim gewünschten Server **Anwenden**.
3. Zum Entfernen wählen Sie beim Server **Von Security Group entfernen**.

Weisen Sie die Gruppe nach dem [Erstellen des Servers](/compute/server) wie oben beschrieben zu.

## Security Group löschen

Wählen Sie in der Liste das Papierkorb-Symbol der Gruppe und bestätigen Sie den Dialog **Security Group löschen** mit **Security Group löschen**.
