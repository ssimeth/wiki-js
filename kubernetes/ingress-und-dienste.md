---
title: Ingress und Dienste im Kubernetes-Cluster
description: Mit **Ingress (Traefik, Gateway API)** veröffentlichen Sie HTTP- und HTTPS-Anwendungen über die Cluster-IP. Sie können die Option beim Erstellen des Clusters aktivieren oder auf der Cluster-Seite unter **Ingress** über **Aktivieren** einschalten.
published: false
date: 2026-11-19T11:51:24.334Z
tags: kundenhilfe
editor: markdown
dateCreated: 2026-11-19T11:51:24.334Z
---

Mit **Ingress (Traefik, Gateway API)** veröffentlichen Sie HTTP- und HTTPS-Anwendungen über die Cluster-IP. Sie können die Option beim Erstellen des Clusters aktivieren oder auf der Cluster-Seite unter **Ingress** über **Aktivieren** einschalten.

Die Plattform stellt das Gateway `traefik-gateway` im Namespace `honeycomb-ingress` bereit und verwaltet es. Legen Sie dafür kein eigenes `Gateway`-Objekt an und ändern Sie das Plattform-Gateway nicht. Lesen Sie dessen Status bei Bedarf mit:

```sh
kubectl get gateway traefik-gateway -n honeycomb-ingress
```

## HTTPRoute für eine Webanwendung

Erstellen Sie im selben Kubernetes-Namespace wie Ihre Anwendung eine `HTTPRoute`. Der Service `meine-app` muss existieren und Port `80` bereitstellen. Ersetzen Sie `app.example.com` durch Ihren Hostnamen und richten Sie dessen DNS-Eintrag auf die Cluster-IP.

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: meine-app
  namespace: default
spec:
  parentRefs:
    - name: traefik-gateway
      namespace: honeycomb-ingress
  hostnames:
    - app.example.com
  rules:
    - backendRefs:
        - name: meine-app
          port: 80
```

`traefik-gateway` ist der Gateway-Name. Die Route muss auf einen Service-Namen und Service-Port zeigen, die tatsächlich im Route-Namespace vorhanden sind.

Speichern Sie das Manifest zum Beispiel als `meine-app-route.yaml` und wenden Sie es an:

```sh
kubectl apply -f meine-app-route.yaml
```

## TLS mit Let’s Encrypt

Unter **Ingress** geben Sie bei **E-Mail für Let’s Encrypt** eine Adresse ein und wählen **TLS aktivieren**. Bei aktivem TLS erstellt `cert-manager` Zertifikate für Ihre Domains. Eine `Certificate`-Ressource im Namespace `honeycomb-ingress` verweist auf den bereitgestellten `letsencrypt`-Issuer und muss den Gateway-Schlüsselnamen `traefik-gateway-tls` verwenden:

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: traefik-gateway-tls
  namespace: honeycomb-ingress
spec:
  secretName: traefik-gateway-tls
  issuerRef:
    name: letsencrypt
    kind: ClusterIssuer
  dnsNames:
    - app.example.com
```

Die DNS-Auflösung und Erreichbarkeit über HTTP müssen für die Zertifikatsprüfung funktionieren.

Speichern Sie das Zertifikatsmanifest zum Beispiel als `app-certificate.yaml` und wenden Sie es an:

```sh
kubectl apply -f app-certificate.yaml
```

## Weitere TCP-Ports

Für einen Dienst außerhalb von HTTP/HTTPS legen Sie einen `LoadBalancer`-Service mit dem Label `honeycomb.cloud/expose: "true"` an. Der Dienst wird über die Cluster-IP erreichbar. Ein Beispiel für Port `5432`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: meine-datenbank
  namespace: default
  labels:
    honeycomb.cloud/expose: "true"
spec:
  type: LoadBalancer
  selector:
    app: meine-datenbank
  ports:
    - name: postgres
      port: 5432
      targetPort: 5432
      protocol: TCP
```

Der Selektor muss zu den Labels der Backend-Pods passen. Über die Cluster-IP können Sie maximal zehn zusätzliche Dienste beziehungsweise Ports veröffentlichen. Die Plattform setzt diese Grenze durch. Die Ports `6443` (Kubernetes-API) sowie `80` und `443` (Traefik) sind reserviert.

Ein `LoadBalancer`-Service ohne dieses Label erhält eine eigene öffentliche IP, sofern das Projektkontingent dies zulässt.
