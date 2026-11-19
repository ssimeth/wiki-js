---
title: Load Balancer einrichten
description: Ein Load Balancer verteilt Verbindungen über Listener (in der Oberfläche **Services** genannt) an Backend-Server (**Member**). Die Backend-Server müssen ein Netzwerk mit dem VIP-Netzwerk des Load Balancers gemeinsam nutzen. Eine Floating IP macht den
published: true
date: 2026-11-19T13:21:03.879Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T13:21:03.879Z
---

Ein Load Balancer verteilt Verbindungen über Listener (in der Oberfläche **Services** genannt) an Backend-Server (**Member**). Die Backend-Server müssen ein Netzwerk mit dem VIP-Netzwerk des Load Balancers gemeinsam nutzen. Eine Floating IP macht den Load Balancer von außen erreichbar.

## Load Balancer erstellen

1. Öffnen Sie **Netzwerk** → **Load Balancer** und wählen Sie **Load Balancer erstellen**.
2. Geben Sie einen **Name** ein. Ergänzen Sie optional eine **Beschreibung**.
3. Wählen Sie unter **Netzwerk** das Subnetz für die virtuelle IP-Adresse (VIP) aus.
4. Wählen Sie **Load Balancer erstellen** und warten Sie, bis der Load Balancer in der Übersicht erscheint.

## Service (Listener) hinzufügen

1. Öffnen Sie den Load Balancer und wählen Sie im Abschnitt **Services** die Aktion **Service hinzufügen**.
2. Geben Sie unter **Name** einen Namen ein.
3. Wählen Sie unter **Protokoll** **TCP** oder **UDP** und geben Sie unter **Port** den eingehenden Port an (1 bis 65535).
4. Wählen Sie **Service hinzufügen**. Der Service erscheint anschließend in der Liste.

Die Plattform legt dabei automatisch einen Backend-Pool an und verknüpft ihn mit dem Service. Beachten Sie mögliche Warnungen, falls dies nicht vollständig gelingt.

## Backend-Server als Member hinzufügen

1. Öffnen Sie den gewünschten Service.
2. Wählen Sie **Member hinzufügen**.
3. Wählen Sie einen oder mehrere Server aus. Auswählbar sind Server, die ein Netzwerk mit dem VIP-Netzwerk des Load Balancers gemeinsam nutzen.
4. Geben Sie unter **Port** den Port des Dienstes auf den Servern an (1 bis 65535). Passen Sie bei Bedarf **Gewichtung** an (1 bis 256; Standardwert 1).
5. Bestätigen Sie mit **1 Member hinzufügen** beziehungsweise der angezeigten Anzahl. Wenn für den Service noch kein Backend-Pool besteht, versucht die Konsole, ihn dabei anzulegen.

Der Status der Member wird in den Service-Details angezeigt und regelmäßig aktualisiert.

## Öffentliche Floating IP zuweisen

1. Öffnen Sie **Netzwerk** → **Floating IPs** und wählen Sie **Floating IP erstellen**. Bestätigen Sie mit **Floating IP erstellen**.
2. Öffnen Sie **Netzwerk** → **Load Balancer** und wählen Sie beim gewünschten Load Balancer im Aktionsmenü **Öffentliche IP zuweisen**.
3. Wählen Sie eine freie Floating IP aus diesem Projekt und bestätigen Sie mit **Öffentliche IP zuweisen**.

Alternativ können Sie den Load Balancer öffnen und bei **Öffentliche IP** die Aktion **Öffentliche IP zuweisen** wählen. Zum Entfernen der Zuordnung wählen Sie dort **Trennen**. Die Floating IP selbst bleibt dabei bestehen und kann erneut zugewiesen oder unter **Netzwerk** → **Floating IPs** freigegeben werden.

## Pools und Member

Der Backend-Pool fasst die Zielserver eines Services zusammen. Zum Entfernen eines Members öffnen Sie die Service-Detailseite, wählen beim Member **Member löschen** und bestätigen den Dialog mit **Member löschen**.

## Automatische Zustandsprüfung

Die Plattform richtet für den Pool automatisch einen Health Monitor ein. Für TCP-Services verwendet sie TCP-Prüfungen, für UDP-Services UDP-CONNECT. Beim Hinzufügen eines Members wird geprüft, ob der Pool einen Health Monitor hat; ein fehlender Monitor wird dabei eingerichtet.

Die Prüfungen verwenden ein Intervall von 10 Sekunden, einen Timeout von 5 Sekunden und 3 Wiederholungen. Den Zustand der Member sehen Sie auf der Service-Detailseite.

## Kubernetes-verwaltete Load Balancer

Load Balancer, die zu einem Kubernetes-Cluster gehören, werden durch den Cluster verwaltet. Weitere Informationen finden Sie unter [Verwaltete Ressourcen](/allgemein/verwaltete-ressourcen).
