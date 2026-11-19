---
title: Netzwerke, Router und Floating IPs
description: Private Netzwerke verbinden Ihre Server. Ein Subnetz legt den IP-Bereich fest. Ein Router verbindet angeschlossene Subnetze mit dem externen Netzwerk. Eine Floating IP stellt eine öffentliche Adresse für einen Server oder Load Balancer bereit.
published: false
date: 2026-11-19T11:51:34.793Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T11:51:34.793Z
---

Private Netzwerke verbinden Ihre Server. Ein Subnetz legt den IP-Bereich fest. Ein Router verbindet angeschlossene Subnetze mit dem externen Netzwerk. Eine Floating IP stellt eine öffentliche Adresse für einen Server oder Load Balancer bereit.

## Privates Netzwerk erstellen

1. Öffnen Sie im gewünschten Projekt **Netzwerk** → **Netzwerke**.
2. Klicken Sie auf **Netzwerk erstellen**.
3. Geben Sie **Netzwerk-Name** und **IP-Bereich** in CIDR-Schreibweise an. Wählen Sie einen privaten IPv4-Bereich, der sich nicht mit Ihren anderen Netzen überschneidet.
4. Klicken Sie auf **Netzwerk erstellen**.

Die Konsole erstellt das Netzwerk und ein IPv4-Subnetz mit DHCP. Falls nur das Netzwerk angelegt werden konnte, erscheint ein Hinweis. Prüfen Sie dann den IP-Bereich in der Netzwerkansicht, bevor Sie Server anschließen.

## Router verbinden

1. Öffnen Sie **Netzwerk** → **Router** und klicken Sie auf **Router erstellen**.
2. Geben Sie **Router Name** ein und bestätigen Sie mit **Router erstellen**.
3. Die Plattform versucht, ein externes Gateway zuzuweisen. Beachten Sie gegebenenfalls die Warnung und prüfen Sie **Externes Gateway** in den Router-Details.
4. Öffnen Sie den Router und wählen Sie unter **Verbundene Netzwerke** die Aktion **Netzwerk hinzufügen**.
5. Wählen Sie unter **Subnet auswählen** Ihr Subnetz und klicken Sie auf **Hinzufügen**.

Wählen Sie dieses private Netzwerk beim Erstellen Ihrer Server aus. Ein bereits mit dem Router verbundenes Subnetz wird nicht erneut zur Auswahl angeboten.

## Floating IP zuweisen

1. Öffnen Sie **Netzwerk** → **Floating IPs**.
2. Klicken Sie auf **Floating IP erstellen** und bestätigen Sie den Dialog mit **Floating IP erstellen**.
3. Öffnen Sie bei der freien Adresse das Aktionsmenü und wählen Sie **Ressource zuweisen**.
4. Wählen Sie Ihren Server oder Load Balancer und klicken Sie auf **Zuweisen**.
5. Erlauben Sie die gewünschten Verbindungen in den Security Groups und gegebenenfalls in der Firewall des Servers.

Bei Servern weist die Plattform die Floating IP einem privaten Netzwerkinterface zu. Das Subnetz muss über einen Router mit externem Gateway erreichbar sein. Das öffentliche IPv6-Interface wird hierfür nicht verwendet.

Zum Wechseln der Zielressource wählen Sie zuerst **Ressource trennen** und anschließend **Ressource zuweisen**. Zum Freigeben einer unbenutzten Adresse wählen Sie **IP löschen**. Eine zugewiesene Adresse müssen Sie vorher trennen.

## Verbindung auf dem Server prüfen

```sh
ip address show
ip route show
```

Bei einer Floating IP erfolgt die Zuordnung außerhalb des Betriebssystems. Tragen Sie die Floating IP nicht als zusätzliche lokale Adresse auf dem Server ein.

## DNS und Grenzen

Wenn ein privates Subnetz keine eigenen DNS-Server vorgibt, verwendet die Plattform standardmäßig Quad9 `9.9.9.9` und `149.112.112.112`.

Floating IPs stammen aus dem öffentlichen IPv4-Pool und sind ausschließlich für IPv4 verfügbar. Öffentliche IPv6-Adressen sind direkt erreichbar und verwenden kein NAT und keine Floating IP.

Die verfügbare Anzahl von Netzwerken, Routern und Floating IPs hängt von Ihren Projektkontingenten und freien Adressen ab. Der Code legt dafür keine einheitlichen Kundenlimits fest.

Wenn ein Router noch für File-Storage-Zugriffsregeln verwendet wird, weist die Konsole beim Löschen darauf hin. Prüfen Sie die betroffenen Freigaben: Mit dem Widerruf der Regeln endet auch der entsprechende NFS-Zugriff.
