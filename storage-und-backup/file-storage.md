---
title: File Storage
description: File Storage stellt eine gemeinsame NFS-Freigabe bereit, die mehrere Server und Kubernetes-Pods gleichzeitig einhängen können. Der Dienst verwendet CephFS über OpenStack Manila und stellt den Zugriff per NFS bereit. Öffnen Sie in der Honeycomb Cloud 
published: true
date: 2026-11-19T12:05:15.781Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T11:51:50.688Z
---

File Storage stellt eine gemeinsame NFS-Freigabe bereit, die mehrere Server und Kubernetes-Pods gleichzeitig einhängen können. Der Dienst verwendet CephFS über OpenStack Manila und stellt den Zugriff per NFS bereit. Öffnen Sie in der Honeycomb Cloud Console **Storage → File Storage**.

## Speicher erstellen

1. Wählen Sie **File Storage erstellen**.
2. Geben Sie einen Namen und die Größe in GB an. Die Oberfläche erlaubt 1 bis 10.000 GB.
3. Wählen Sie im Bereich **Zugriff** die Router aus, hinter denen Ihre Server oder Ihr Kubernetes-Cluster liegen. Legen Sie unter **Berechtigung** **Lesen und Schreiben** oder **Nur lesen** fest.
4. Wählen Sie **Erstellen** und warten Sie, bis der Status **Verfügbar** ist.

Falls die Freigabe beim Erstellen noch nicht aktiv werden konnte, öffnen Sie die Details und fügen Sie den Router im Abschnitt **Zugriff** hinzu, sobald der File Storage **Verfügbar** ist. Eine angelegte Freigabe kann während der Aktivierung einen Zwischenstatus zeigen; prüfen Sie den Status, bevor Sie den Mount verwenden.

Die Freigabe wird anhand der externen IPv4-Adresse des Router-Gateways erteilt. Alle Server und Kubernetes-Cluster hinter einem freigegebenen Router können darauf zugreifen. Geben Sie nur Router frei, deren dahinterliegende Systeme Zugriff erhalten sollen.

## Auf einem Linux-Server einhängen

Öffnen Sie den File Storage in der Liste, kopieren Sie den angezeigten **Export-Pfad** und geben Sie unter **Zugriff** den Router frei, hinter dem der Server liegt. Sobald der Status **Verfügbar** ist und die Freigabe aktiv ist, verwenden Sie den Export-Pfad aus der Detailansicht:

```sh
EXPORT_PFAD='EXPORT_AUS_DER_KONSOLE'
sudo apt install nfs-common
sudo mkdir -p /mnt/daten
sudo mount -t nfs4 -o vers=4.1 "$EXPORT_PFAD" /mnt/daten
```

Auf RHEL-Systemen installieren Sie stattdessen `nfs-utils`. Der konkrete Export-Pfad wird pro File Storage angezeigt; übernehmen Sie ihn unverändert aus der Console.

Für einen dauerhaften Mount kann die Console eine passende Zeile für `/etc/fstab` anzeigen. Das Format lautet:

```text
<export-pfad> /mnt/daten nfs4 vers=4.1,_netdev 0 0
```

## Größe und Zugriff verwalten

1. Wählen Sie in der Detailansicht unter **Zugriff** den Router und die **Berechtigung** aus. Klicken Sie auf **Freigeben**.
2. Zum Widerrufen entfernen Sie die entsprechende Freigabe und bestätigen **Entfernen**. Damit endet der Zugriff für die Systeme hinter diesem Router.
3. Wählen Sie unter **Speicher** zunächst **Vergrößern**, geben Sie die neue Größe an und klicken Sie auf **Übernehmen**. Bestätigen Sie den Dialog mit **Vergrößern**. Eine Verkleinerung ist nicht möglich.

Beim Erstellen können Sie bis zu 20 Router auswählen. Name und optionale Beschreibung dürfen jeweils höchstens 255 Zeichen lang sein.

Mit **Löschen** wird der File Storage einschließlich aller darauf gespeicherten Dateien gelöscht. Server, die ihn eingehängt haben, verlieren den Zugriff. Sichern Sie benötigte Daten vorher an einem anderen Ort.
