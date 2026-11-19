---
title: Object Storage
description: Object Storage speichert Dateien als Objekte in Buckets und ist über eine S3-kompatible API erreichbar. Öffnen Sie in der Honeycomb Cloud Console **Storage → Object Storage**. Dort können Sie Buckets und ihre Objekte verwalten.
published: false
date: 2026-11-19T11:51:56.086Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T11:51:56.086Z
---

Object Storage speichert Dateien als Objekte in Buckets und ist über eine S3-kompatible API erreichbar. Öffnen Sie in der Honeycomb Cloud Console **Storage → Object Storage**. Dort können Sie Buckets und ihre Objekte verwalten.

## Bucket erstellen

1. Wählen Sie **Bucket erstellen**.
2. Tragen Sie einen **Bucket-Name** ein.
3. Wählen Sie **Bucket erstellen**.

Öffnen Sie den Bucket über die Liste, um Dateien hochzuladen, herunterzuladen oder zu löschen. Der Browser-Upload benötigt einen Access Key des ausgewählten Projekts.

## Access Key erstellen

1. Öffnen Sie **Sicherheit → Access Keys** und wählen Sie **Access Key erstellen**.
2. Bestätigen Sie die Erstellung für das ausgewählte Projekt mit **Erstellen**.
3. Kopieren Sie die **Access Key ID** und den **Secret Access Key** direkt aus dem Bestätigungsdialog. Der Secret Access Key wird danach nicht erneut angezeigt.
4. Bewahren Sie den Secret Access Key sicher auf und löschen Sie nicht mehr benötigte Schlüssel.

Ein Access Key gilt für alle Buckets seines Projekts und ausschließlich für Object Storage. Er kann nicht verwendet werden, um Zugang zur OpenStack-API zu erhalten. Projekte sind voneinander isoliert: Schlüssel eines Projekts sehen die Buckets anderer Projekte nicht.

## Zugriff für Dritte begrenzen

Bucket-Richtlinien können einzelne Access Keys desselben Projekts nicht unterscheiden. Erstellen Sie deshalb für Partner oder Backuptools ein eigenes, getrenntes Projekt. Erstellen Sie die benötigten Buckets und Access Keys in diesem Projekt und geben Sie nur diese Zugangsdaten weiter.

## S3-kompatiblen Client verwenden

Verwenden Sie den Endpunkt `https://s3.honeycomb-cloud.de`. Der Browser-Upload nutzt Path-Style-Adressen, bei denen der Bucket im URL-Pfad steht. Der Hostname wird in der Object-Storage-Oberfläche angezeigt.

Richten Sie AWS CLI ein. Ersetzen Sie den Beispielnamen für den Bucket durch Ihren eigenen Namen. Die Profilkonfiguration fragt die Schlüssel interaktiv ab. Geben Sie den Secret Access Key nicht in Skripten oder Versionsverwaltung ein.

```sh
aws configure --profile honeycomb
aws configure set s3.addressing_style path --profile honeycomb
aws configure set region us-east-1 --profile honeycomb

ENDPOINT='https://s3.honeycomb-cloud.de'
BUCKET='mein-bucket'
aws --profile honeycomb --endpoint-url "$ENDPOINT" s3api list-buckets
aws --profile honeycomb --endpoint-url "$ENDPOINT" s3 ls "s3://$BUCKET/"
aws --profile honeycomb --endpoint-url "$ENDPOINT" s3 cp ./datei.zip "s3://$BUCKET/datei.zip"
```

## Bucket-Richtlinie

Bucket-Richtlinien werden unterstützt. Ein typischer Anwendungsfall ist, die Objekte eines Buckets öffentlich lesbar zu machen, zum Beispiel für Bilder oder Downloads. Ersetzen Sie `<bucket-name>` vor dem Verwenden:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::<bucket-name>/*"
    }
  ]
}
```

Speichern Sie die Richtlinie als `bucket-policy.json` und setzen Sie sie mit dem zuvor eingerichteten Profil:

```sh
aws --profile honeycomb --endpoint-url "$ENDPOINT" s3api put-bucket-policy \
  --bucket "$BUCKET" --policy file://bucket-policy.json
```

Öffentliche Objekte erreichen Sie anschließend ohne Anmeldung unter folgender Adresse. Vor dem Bucket-Namen steht die Projekt-ID, getrennt durch einen Doppelpunkt:

```text
https://s3.honeycomb-cloud.de/<projekt-id>:<bucket-name>/<objekt>
```

Das Auflisten des Buckets bleibt dabei gesperrt, solange Sie `s3:ListBucket` nicht freigeben. Mit `aws ... s3api delete-bucket-policy --bucket "$BUCKET"` entfernen Sie die Freigabe wieder.

`NotAction` wird akzeptiert, hat aber keine Wirkung. Verwenden Sie in Richtlinien explizite `Action`-Listen.

## Object Lock

Object Lock schützt Objektversionen während einer Aufbewahrungsfrist vor dem Löschen. Aktivieren Sie Object Lock beim Erstellen eines neuen Buckets mit `--object-lock-enabled-for-bucket`; die Versionierung wird dabei automatisch eingeschaltet. Legen Sie anschließend mit `put-object-lock-configuration` eine Standardaufbewahrung im Modus `GOVERNANCE` oder `COMPLIANCE` fest. `TAGE=30` ist ein frei gewähltes Beispiel, kein Dienstlimit.

```sh
ENDPOINT='https://s3.honeycomb-cloud.de'
BUCKET='mein-bucket'
TAGE=30
aws --profile honeycomb --endpoint-url "$ENDPOINT" s3api create-bucket \
  --bucket "$BUCKET" \
  --object-lock-enabled-for-bucket
```

Governance-Modus:

```sh
aws --profile honeycomb --endpoint-url "$ENDPOINT" s3api put-object-lock-configuration \
  --bucket "$BUCKET" \
  --object-lock-configuration "{\"ObjectLockEnabled\":\"Enabled\",\"Rule\":{\"DefaultRetention\":{\"Mode\":\"GOVERNANCE\",\"Days\":${TAGE}}}}"
```

Compliance-Modus:

```sh
aws --profile honeycomb --endpoint-url "$ENDPOINT" s3api put-object-lock-configuration \
  --bucket "$BUCKET" \
  --object-lock-configuration "{\"ObjectLockEnabled\":\"Enabled\",\"Rule\":{\"DefaultRetention\":{\"Mode\":\"COMPLIANCE\",\"Days\":${TAGE}}}}"
```

Im Governance-Modus wird das Löschen einer gesperrten Objektversion verweigert, sofern Sie nicht ausdrücklich `--bypass-governance-retention` verwenden.

**Achtung: Im Compliance-Modus lässt sich die Aufbewahrung einer gesperrten Objektversion bis zum Ende der Frist weder verkürzen noch umgehen – auch nicht durch den Support.** Die Version kann bis dahin nicht gelöscht werden. Prüfen Sie Modus und Dauer sorgfältig, bevor Sie Objekte mit Aufbewahrung speichern.
