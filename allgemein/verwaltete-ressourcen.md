---
title: Von Kubernetes verwaltete Ressourcen
description: Die Plattform schützt Cluster-Ressourcen sowohl in der App als auch bei ausgewählten schreibenden Aufrufen über die OpenStack-API. Betroffene Änderungen werden mit HTTP 403 und einem Hinweis auf den zuständigen Kubernetes-Cluster abgewiesen. Die Open
published: false
date: 2026-11-19T11:51:10.154Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T11:51:10.154Z
---

Die Plattform schützt Cluster-Ressourcen sowohl in der App als auch bei ausgewählten schreibenden Aufrufen über die OpenStack-API. Betroffene Änderungen werden mit HTTP 403 und einem Hinweis auf den zuständigen Kubernetes-Cluster abgewiesen. Die OpenStack-API erlaubt weiterhin Leseaufrufe wie `GET`. Ein sicherer CLI-Leseaufruf ist zum Beispiel:

```sh
openstack server list --long
```

## Geschützte Ressourcen und Änderungen

Der Schutz gilt für Ressourcen, die die Plattform anhand ihrer Cluster-Zuordnung erkennt:

- **Kubernetes-Nodes (Server):** schreibende Server-Aufrufe einschließlich Aktionen an vorhandenen Node-Servern. Beim Anhängen eines Volumes wird zusätzlich geprüft, ob das Volume vom Cluster verwaltet wird.
- **Volumes:** Änderungen und Löschungen sowie Volume-Aktionen.
- **Netzwerke und Subnetze:** Änderungen und Löschungen.
- **Router:** Änderungen und Löschungen sowie das Hinzufügen oder Entfernen einer Router-Schnittstelle.
- **Sicherheitsgruppen:** Änderungen und Löschungen. Auch das Anlegen von Sicherheitsgruppenregeln wird geprüft; beim Löschen einer Regel wird die zugehörige Gruppe geprüft.
- **Floating IPs:** Änderungen und Löschungen, wenn die IP an einen Cluster-Node oder den erkannten Cluster-Load-Balancer gebunden ist.
- **Load Balancer:** Änderungen, Löschungen und Aktionen am Load Balancer.
- **Listener, Pools, Pool-Mitglieder und Health Monitors:** Änderungen und Löschungen vorhandener Unterressourcen werden gegen den übergeordneten Cluster-Load-Balancer geprüft. Änderungen an einem erkannten Cluster-Load-Balancer, einschließlich Aktionen über seine Unterpfade, werden ebenfalls abgewiesen.

Die Sperre greift für die beschriebenen Änderungen über die Console und für die vom Schutz erfassten OpenStack-API-Aufrufe, etwa über CLI, SDK oder Terraform. Nicht jede OpenStack-Schreiboperation wird pauschal blockiert: Der Schutz greift nur für die genannten Ressourcentypen und erkannten Zuordnungen. Erstellen Sie neue Ressourcen über die Kubernetes-Funktionen, wenn sie zum Cluster gehören sollen.

## Änderungen richtig vornehmen

1. Öffnen Sie **Kubernetes** und wählen Sie den zugehörigen Cluster.
2. Ändern Sie die Node-Anzahl unter **Node-Pools** mit **Skalieren**.
3. Verwalten Sie Services und PersistentVolumeClaims Ihrer Anwendungen über `kubectl`. Lassen Sie die daraus erzeugten Cloud-Ressourcen durch die jeweiligen Kubernetes-Controller verwalten.

Der Schutz setzt eine erkennbare Cluster-Zuordnung voraus. Er ist keine allgemeine Schreibsperre für sämtliche Ressourcen Ihres Projekts.
