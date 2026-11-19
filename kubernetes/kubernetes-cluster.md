---
title: Kubernetes-Cluster
description: Unter **Kubernetes** verwalten Sie Cluster für Ihr Projekt. Die Plattform erstellt und betreibt die Kubernetes-Infrastruktur; Sie verwalten Ihre Anwendungen im Cluster.
published: false
date: 2026-11-19T11:51:29.800Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T11:51:29.800Z
---

Unter **Kubernetes** verwalten Sie Cluster für Ihr Projekt. Die Plattform erstellt und betreibt die Kubernetes-Infrastruktur; Sie verwalten Ihre Anwendungen im Cluster.

## Cluster erstellen

1. Öffnen Sie **Kubernetes** und wählen Sie **Cluster erstellen**.
2. Tragen Sie unter **Name** einen Namen ein und wählen Sie bei **Kubernetes-Version** das bereitgestellte Template.
3. Wählen Sie unter **Netzwerk** ein Projektnetzwerk. Es benötigt einen Router mit Internetzugang. Falls mehrere Subnetze vorhanden sind, wählen Sie optional eines aus; andernfalls wird ein Subnetz automatisch gewählt.
4. Wählen Sie optional eine **Größe der Worker Nodes**. Ohne Auswahl gilt der Standard des Templates.
5. Legen Sie die Anzahl bei **Worker Nodes** fest. Sie können diese später ändern.
6. Aktivieren Sie bei Bedarf **Hochverfügbare Control Plane**. Dadurch werden drei statt eines Master-Knotens verwendet.
7. Aktivieren Sie bei Bedarf **Ingress (Traefik, Gateway API)**, um Webdienste und zusätzliche Ports über die Cluster-IP bereitzustellen.
8. Wählen Sie **Cluster erstellen**. Die Erstellung läuft im Hintergrund; der Status erscheint in der Übersicht.

## Cluster verwalten

Öffnen Sie den Cluster in der Übersicht **Kubernetes**, um Details und Aktionen zu sehen.

- Wählen Sie **Kubeconfig**. Speichern Sie die heruntergeladene Datei sicher, zum Beispiel in `~/Downloads/mein-cluster-kubeconfig.yaml`. Um sie für `kubectl` zu verwenden, setzen Sie in einem Terminal:

  ```sh
  export KUBECONFIG="$HOME/Downloads/mein-cluster-kubeconfig.yaml"
  kubectl get nodes
  ```

  `kubectl get nodes` zeigt den Status der Cluster-Nodes an.
- Um einen Pool hinzuzufügen, wählen Sie unter **Node-Pools** **Node-Pool hinzufügen**. Geben Sie **Name**, **Größe** und **Nodes** an und wählen Sie **Anlegen**. Zum Skalieren ändern Sie die Node-Zahl beim betreffenden Pool und wählen **Skalieren**. Die Plattform prüft bei zusätzlichen Nodes das Projektkontingent für Server, vCPU und RAM. Bei fehlendem Kontingent reduzieren Sie die Node-Anzahl oder wählen kleinere Nodes.
- Unter **Kubernetes-Version** wählen Sie bei einem verfügbaren Update **Update starten** und bestätigen im Dialog **Kubernetes-Update starten** mit **Update starten**. Updates erfolgen jeweils höchstens um eine Minor-Version. Master und Worker werden nacheinander ersetzt. Workloads mit nur einer Replica sind währenddessen kurzzeitig nicht erreichbar.
- Zum Entfernen eines Clusters wählen Sie **Cluster löschen**. Dabei werden die Nodes entfernt; sichern Sie benötigte Daten vorher.

Die verfügbaren Versionen und Node-Größen hängen von den für Ihr Projekt bereitgestellten Templates und Kontingenten ab.
