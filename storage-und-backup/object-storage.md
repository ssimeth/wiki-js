---
title: Object Storage (S3)
description: 
published: true
date: 2024-10-21T10:49:35.536Z
tags: 
editor: markdown
dateCreated: 2024-10-21T09:57:13.257Z
---

# Object Storage (S3)
Im der Honeycomb Cloud Object Storage Service werden Daten als Objekte gespeichert, was als Object Storage bezeichnet wird. Diese Technologie bietet nahezu unbegrenzte Möglichkeiten zur Speicherung großer Datenmengen. Sie gewährleistet die Sicherheit von Ressourcenplanungssystemen und ermöglicht einen schnellen und unmittelbaren Zugriff auf die benötigte Datenmenge. Block Storage in der Honeycomb Cloud zeichnet sich durch seine Flexibilität und Zuverlässigkeit aus, was es zu einer idealen Lösung für Unternehmen mit wachsenden Datenspeicheranforderungen macht.
<br />
<!--
## Einsatzzwecke für Object Storage
<br />
 
**Datensicherung und Wiederherstellung**
- Backup kritischer Datenbanken und Daten
- Disaster Recovery dank hoher Verfügbarkeit und Redundanz
- Langzeitarchivierung

**Medien- und Inhaltsspeicherung**
- Hosting von Website-Assets (Bilder, Videos, Downloads)
- Speicherung von Multimedia-Inhalten für Content Delivery Networks
- Verwaltung großer Mengen unstrukturierter Daten (z.B. für Medienunternehmen)

**Cloud-native Anwendungen**
- Persistenter Datenspeicher für containerisierte und serverlose Anwendungen
- Skalierbare Speicherlösung für dynamische Websites und mobile Apps
- Artifact-Speicherung für Entwickler (z.B. Log-Daten, Software-Builds)


**Big Data und Analysen**
- Aufbau von Data Lakes für umfangreiche Analysen
- Speicherung von IoT-Gerätedaten
- Verarbeitung großer Datenmengen in der Cloud

**Compliance und Datenhaltung**
- Erfüllung gesetzlicher Aufbewahrungspflichten mit S3 Object Lock
- Sichere Speicherung sensibler Daten (z.B. im Gesundheitswesen)
- Verwaltung von Kundenakten und Produktdaten im E-Commerce

**Spezielle Anwendungsfälle**
- On-Premises-Objektspeicherung mit S3 Outposts
- Hosting statischer Websites
- Software-Verteilung und -Hosting
<br />
-->

## Voraussetzungen
<br />
Um die S3 API nutzen zu können, installieren Sie sich einen S3 kompatiblen CLient Ihrer Wahl. Wir verwenden in den folgendne Beispielen die AWS CLI, welche Sie unter folgendem Link installierne können: 

[Install or update to the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Sobald Sie den Client installiert haben können Sie aws configure ausführen, um ihn zu konfigurieren. Außerdem benötigen Sie einen Access- und Secretkey. Im Folgenden finden Sie die nötigen Schritte, um diese zu erstellen: 
<br />

**Weg über die Console**

Einloggen in https://dashboard.honeycomb-cloud.de/

Access &#8594; S3/EC2 Credentials &#8594; Create S3/EC2 Credential
> **_Hinweis:_** Speichern Sie sich den Access- und Secretkey an einem sicheren Ort.

![console_access-key.png](/storage-und-backup/console_access-key.png =1000x)

**Openstack Client**
```
openstack ec2 credentials create
```
<br />

## S3 API 
Konfigurieren Sie Ihren S3 kompatiblen Client entsprechend den folgenden Daten:
<br />
```
endpoint_url = https://s3.honeycomb-cloud.de
aws_access_key_id = <aws_access_key_id>
aws_secret_access_key = <aws_secret_access_key>
```
<br />

## Object Lock
Dieser Abschnitt enthält Anweisungen zur Verwaltung von Object Lock mithilfe der CLI.
<br />

### Bucket mit Object Lock erstellen
Mit folgendem Befehl erstellen Sie einen Bucket mit Object Lock:
```
aws s3api create-bucket \
  --bucket object-lock \
  --object-lock-enabled-for-bucket \
  --endpoint-url https://s3.honeycomb-cloud.de
```
<br />

### Object Lock mit Compliance Mode
Die Objektsperre im Compliance-Modus für einen Bucket gewährleistet eine strenge Kontrolle durch eine entsprechende Aufbewahrungsrichtlinie. Sobald dieser Modus aktiviert ist, kann die Aufbewahrungsfrist für ein Objekt weder verkürzt noch anderweitig geändert werden. Dieser Modus eignet sich besonders gut für die Erfüllung regulatorischer Anforderungen, da er garantiert, dass Objekte unverändert bleiben. Die Sperre kann vor Ablauf der Aufbewahrungsfrist nicht aufgehoben werden. Dies gewährleistet einen konsistenten Datenschutz.

Um den Compliance-Modus für den Bucket "object-lock" mit einer Standard-Aufbewahrungsfrist von 15 Tagen zu konfigurieren, verwenden Sie folgenden Befehl:
```
aws s3api put-object-lock-configuration \
    --bucket object-lock \
    --object-lock-configuration '{ "ObjectLockEnabled": "Enabled", "Rule": { "DefaultRetention": { "Mode": "COMPLIANCE", "Days": 15 }}}' \
    --endpoint-url https://s3.honeycomb-cloud.de
```
<br />

### Object Lock mit Governance Mode
Eine Objektsperre mit Governance Modus für einen Bucket bietet dem Bucket-Besitzer mehr Flexibilität als der Compliance Modus. Sie ermöglicht die Aufhebung der Objektsperre vor Ablauf der festgelegten Aufbewahrungsfrist, so dass das Objekt später ersetzt oder gelöscht werden kann. 
Wenden Sie die Konfiguration des Governance Modus auf den Bucket "object-lock" mit einer standardmäßigen Aufbewahrungsfrist von 15 Tagen an:
```
aws s3api put-object-lock-configuration \
    --bucket object-lock \
    --object-lock-configuration '{ "ObjectLockEnabled": "Enabled", "Rule": { "DefaultRetention": { "Mode": "GOVERNANCE", "Days": 15 }}}' \
    --endpoint-url https://s3.honeycomb-cloud.de
```
<br />

### Object Lock Konfiguration abfragen
Abrufen der Object Lock-Konfiguration eines Buckets:
```
aws s3api get-object-lock-configuration \
  --bucket object-lock \
  --endpoint-url https://s3.honeycomb-cloud.de
```
Beispielantwort:
```
{
    "ObjectLockConfiguration": {
        "ObjectLockEnabled": "Enabled",
        "Rule": {
            "DefaultRetention": {
                "Mode": "COMPLIANCE",
                "Days": 15
            }
        }
    }
}
```
<br />

### Hochladen von Dateien in einen Bucket mit Object Lock
Laen Sie die Datei ```object-1``` in den Bucket ```object-lock```:
```
aws s3api put-object \
   --bucket object-lock \
   --key object-1 \
   --endpoint-url https://s3.honeycomb-cloud.de
```
Beispielantwort:
```
{
    "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
    "VersionId": "dNiUNvvWD6HCABzkdH9i8FPN5i3yLYP"
}
```
> **_Hinweis:_**  Da keine Policy angegeben wurde die Standardaufbewahrungskonfiguration des Buckets angewendet.

Laden Sie nun erneut ```object-1``` in den Bucket ```object-lock``` aber dieses Mal überschreiben Sie die Object Lock Konfiguration:
```
aws s3api put-object \
      --bucket object-lock \
      --key object-1 \
      --object-lock-mode GOVERNANCE \
      --object-lock-retain-until-date 2024-10-30T10:11:12Z \
      --endpoint-url https://s3.honeycomb-cloud.de
```
> **_Hinweis:_**  Es ist möglich geschützte Objekte zu überschreiben. Da die Versionierung für den Bucket aktiviert ist, werden mehrere Versionen des Objekts gespeichert. Das Löschen von Objekten ist ebenfalls möglich, da bei diesem Vorgang lediglich ein Löschmarker zur Version des Objekts gesetzt wird.
<br />

### Löschen von Dateien in einen Bucket mit Object Lock
Das permanente Löschen der Objektversion ist nicht möglich, das System erstellt lediglich einen Löschmarker für das Objekt. Wenn Sie das Objekt über die API oder per GUI anzeigen wollen, sieht es dennoch so aus als wäre das Objekt gelöscht worden. Sie können die Löschmarkierungen und andere Versionen eines Objekts nur mit Hilfe des API-Aufrufs ```ListObjectVersions``` auflisten:
```
aws s3api list-object-versions --bucket object-lock --endpoint-url https://s3.honeycomb-cloud.de
```
> **_Hinweis:_** Löschmarkierungen sind nicht WORM-geschützt, unabhängig von der Aufbewahrungsfrist oder dem rechtlichen Schutz des zugrunde liegenden Objekts.
<br />

### Legal Hold
Aktivieren Sie den ```legal-hold``` Status auf dem ```object-1``` im Bucket ```object-lock```:
```
aws s3api put-object-legal-hold \
    --bucket object-lock \
    --key object-1.pdf \
    --legal-hold Status=ON \
    --endpoint-url https://s3.honeycomb-cloud.de
```
Nutzen Sie den ```Status=OFF``` um den ```legal-hold``` Status zu deaktivieren.
<br />

### Anzeigen der Object Lock Konfiguration für ein Objekt
Um den Status der Objektsperre für eine bestimmte Version eines Objekts zu überprüfen, können Sie entweder die Befehle ```GET Object``` oder ```HEAD Object``` verwenden. Beide Befehle liefern Informationen über den Aufbewahrungsmodus, das angegebene 'Retain Until Date' und den Status des ```legal-hold``` für die gewählte Objektversion.
<br />

### Setzen von Retention Limits
Wenn mehrere Benutzer die Berechtigung haben, Objekte in Ihren Bucket hochzuladen, besteht die Gefahr, dass zu lange Aufbewahrungsfristen festgelegt werden. Dies kann zu erhöhten Speicherkosten und Herausforderungen bei der Datenverwaltung führen. Obwohl das System mit Hilfe des Bedingungsschlüssels ```s3:object-lock-remaining-retention-days``` eine Aufbewahrungsdauer von bis zu 100 Jahren zulässt, kann die Implementierung von Beschränkungen besonders in Umgebungen mit mehreren Benutzern von Vorteil sein.

Legen Sie eine maximale Aufbewahrungsfrist von 30 Tagen fest:
```
{
    "Version": "2024-11-09",
    "Id": "Set Retention Limits",
    "Statement": [
        {
            "Sid": "Set Retention Period",
            "Effect": "Deny",
            "Principal": "*",
            "Action": [
                "s3:PutObjectRetention"
            ],
            "Resource": "arn:aws:s3:::my-bucket-with-object-lock/*",
            "Condition": {
                "NumericGreaterThan": {
                    "s3:object-lock-remaining-retention-days": "30"
                }
            }
        }
    ]
}
```
Speichern Sie diese Richtlinie nach ```policy.json``` ab und wenden Sie sie mit diesem Befehl an:
```
aws s3api put-bucket-policy --bucket object-lock --policy file://policy.json
```

























