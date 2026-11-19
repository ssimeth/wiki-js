---
title: Öffentliches IPv6 für Server
description: Mit öffentlichem IPv6 ist Ihr Server direkt über eine globale IPv6-Adresse erreichbar. Zusätzlich zu den ausgewählten privaten Netzwerken erhält er ein Interface im öffentlichen Netzwerk. Bei einem privaten Netzwerk ist dies die zweite Netzwerkkarte.
published: true
date: 2026-11-19T12:05:06.474Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T11:51:40.079Z
---

Mit öffentlichem IPv6 ist Ihr Server direkt über eine globale IPv6-Adresse erreichbar. Zusätzlich zu den ausgewählten privaten Netzwerken erhält er ein Interface im öffentlichen Netzwerk. Bei einem privaten Netzwerk ist dies die zweite Netzwerkkarte.

## Server erstellen

1. Wählen Sie Ihr Projekt und öffnen Sie **Server** → **Server erstellen**.
2. Wählen Sie Image, Server-Typ, SSH Key und mindestens ein privates Netzwerk.
3. Lassen Sie **Öffentliches IPv6** aktiviert. Die Option ist standardmäßig eingeschaltet. Sie erscheint nur, wenn ein geeignetes öffentliches IPv6-Netzwerk verfügbar ist.
4. Klicken Sie auf **Server erstellen**.
5. Öffnen Sie den Server. Unter **Öffentliches Netzwerk** finden Sie die Adresse mit der Kennzeichnung **Öffentliches IPv6**.

## Eingehende Verbindungen erlauben

Eingehender ICMPv6-Verkehr aus `::/0` ist in allen Security Groups standardmäßig erlaubt, sowohl in der Standardgruppe als auch in selbst erstellten Gruppen. ICMPv6 wird unter anderem für die Nachbarerkennung (Neighbor Discovery) benötigt.

Für SSH und andere Dienste legen Sie selbst passende Regeln an:

1. Öffnen Sie die am Server verwendete Gruppe unter **Sicherheit** → **Security Groups**.
2. Wählen Sie im Bearbeitungsdialog unter **Regeln** die Aktion **Regel hinzufügen**.
3. Setzen Sie für SSH **Richtung** auf **Eingehend**, **Protokoll** auf **TCP**, **Von Port** und **Bis Port** auf `22` und **Quelle (CIDR)** auf `::/0`.
4. Klicken Sie auf **Regel hinzufügen**. Damit erlauben Sie SSH aus dem gesamten IPv6-Internet. Beschränken Sie die Quelle bei Bedarf auf Ihr eigenes IPv6-Netz.

Prüfen Sie zusätzlich die Firewall im Betriebssystem.

## Verbindung prüfen

Führen Sie auf dem Server aus:

```sh
ip -6 address show scope global
ip -6 route show
```

Von einem Rechner mit IPv6-Verbindung können Sie die in der Konsole angezeigte Adresse verwenden:

```sh
SERVER_IPV6='IHRE_IPV6_ADRESSE'
SSH_BENUTZER='IHR_IMAGE_BENUTZER'
ping -6 "$SERVER_IPV6"
ssh -6 "${SSH_BENUTZER}@${SERVER_IPV6}"
```

## Adressierung und Hinweise

Das öffentliche IPv6-Netz ist `2a01:4f8:11a:900::/64`. Server erhalten Adressen aus dem Bereich `2a01:4f8:11a:900::1:0` bis `2a01:4f8:11a:900::1:ffff`. Das Gateway ist `2a01:4f8:11a:900::1`. Verwenden Sie ausschließlich die Ihrem Server zugewiesene Adresse.

Cloud-init richtet IPv6 beim Booten statisch ein; SLAAC und DHCPv6 werden nicht verwendet. Als DNS-Server dienen in diesem Netzwerk Quad9 `2620:fe::fe` und `2620:fe::9`.

IPv6-Adressen sind direkt öffentlich erreichbar; es gibt kein NAT. Öffentliches IPv6 benötigt keine Floating IP. Floating IPs stammen ausschließlich aus dem öffentlichen IPv4-Pool und sind nur für IPv4 verfügbar.
