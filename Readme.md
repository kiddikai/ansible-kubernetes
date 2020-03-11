# Anwendung

Um mithilfe der Playbooks aus diesem Projekt einen Kubernetes-Cluster
einzurichten, müssen Material und Infrastruktur vorbereitet werden.
Anschließend werden die SD-Karten einzeln mit Raspbian beschrieben und mit dem
Playbook `local-raspbian` für den Headless-Betrieb konfiguriert.
Sobald alle Raspberry Pis online sind, wird Kubernetes mit dem Playbook
`kubernetes` aufgesetzt.

## Vorbereitung

Zum erfolgreichen Ausführen dieser Anleitung müssen folgende
Voraussetzungen erfüllt sein:

  - **Raspberry Pis** in beliebiger Anzahl und ebenso viele
    Speicherkarten und Netzteile stehen bereit.

  - **Ein Linux-Rechner** steht bereit, um die Speicherkarten zu flashen
    und die Ansible-Playbooks auszuführen. Dafür sind Balena Etcher\[1\]
    und Ansible\[2\] installiert, das Git-Repo zu diesem Projekt mit den
    Verzeichnissen `inventory` und `playbook` ist ausgecheckt und ein
    Image von Raspbian Lite\[3\] ist heruntergeladen.

  - **Ein Terminal** mit `playbook` als Current Working Directory ist
    geöffnet.

  - **Ein WiFi-Access Point** mit Internetzugriff ist in Betrieb. Seine
    Einstellungen (IP, SSID, WPA2-Key) entsprechen den Angaben in den
    Dateien `inventory/group_vars/all.yaml` und
    `playbook/local-raspbian.yaml`. Der zuvor erwähnte Rechner ist
    mit dem Access Point verbunden.

  - **Die Inventory-Datei** `inventory/k8s-cluster.yaml` bildet den
    derzeitigen Cluster ab – enthält also keine Einträge unter `hosts`,
    falls ein neuer Cluster eingerichtet werden soll:
    
        nodes:
          hosts:

Die benötigte Zeit für die gesamte Einrichtung beträgt ca. 10
Minuten pro Raspberry Pi plus ca. 30 Minuten für den gesamten
Cluster.

## Raspbian installieren

Die Schritte in diesem Abschnitt müssen für jeden Raspberry Pi einzeln
durchgeführt werden.

1.  Speicherkarte in den Linux-Rechner einlegen.

2.  Raspbian-Image mit Balena Etcher auf die Speicherkarte flashen.

3.  Raspbian-Playbook ausführen:
    
        ansible-playbook -i ../inventory/k8s-cluster.yaml local-raspbian.yaml

4.  Speicherkarte in den Raspberry Pi einsetzen und booten.

## Cluster aufsetzen

Wenn alle Raspberry Pis gestartet sind, wird die Installation mit dem
Kubernetes-Playbook fortgesetzt:

    ansible-playbook -i ../inventory/k8s-cluster.yaml kubernetes.yaml

Der Kubernetes-Cluster ist anschließend einsatzbereit.

## Weitere Nodes hinzufügen

Sollen zu einem fertigen Cluster weitere Nodes hinzugefügt werden, kann auch dafür diese Anleitung verwendet werden. Die Inventory-Datei darf dann nicht leer sein, sondern muss die bereits vorhandenen Nodes enthalten.

1.  <https://www.balena.io/etcher/>

2.  z. B. `apt install ansible`

3.  <https://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2020-02-14/2020-02-13-raspbian-buster-lite.zip>