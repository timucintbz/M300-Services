# LB1 Dokumentation

# Einleitung
In diesem Abschnitt erkläre ich mein Vorgehen der LB1. Die Traktanden hier sind folgende:
* Kubernetes Aufsetzung

# Inhaltsverzeichnis 
1. Vorraussetzungen
2. Deklarativer Aufbau
   1. Kubernetes
3. Funktionen
4. Testing
5. Reflektion
6. Quellen

# Voraussetzungen
- GitHub
- Git/Bash
- Vagrant (Vagrantfile)
- Virtualbox
- Browser

# Deklarativer Aufbau
## Kubernetes
### Funktion 
Kubernetes ist eine portable, erweiterbare Open-Source-Plattform zur Verwaltung von containerisierten Arbeitslasten und Services, die sowohl die deklarative Konfiguration als auch die Automatisierung erleichtert. Es hat einen großes, schnell wachsendes Ökosystem.
### Konfiguration 
Als erstes erstellen wir mit Vagrant eine neue Ubuntu 64 VM. Dies mit folgendem Befehl:
``` 
$ vagrant init ubuntu/focal64
```

Dies dauert einen moment. Nun haben wir eine VM erstellt und können unser Vagrantfile anpassen, so dass wir über den Browser Kubernetes starten können. Wir öffnen das Vagrantfile mit Notepad um es da dann zu bearbeiten.

Befehl zum Vagrantfile mit Notepad zu öffnen:

```
$ notepad Vagrantfile
```

- Was ich hier geändert habe, sind die forwarded Ports, mit welchen ich dann auf Kubernetes über den Browser zugreifen kann. (guest:443, host:8444)
- Dann zusätzlich noch die Ordner, auf welchen die Daten der VM synchronisiert werden. 
- Danach noch der VM Provider (Virtualbox), die Grösse des Memory (4096)
- Zu guter Letzt noch die Installation von Kubernetes selbst, inklusive aktivieren von DNS
```
# Kubernetes config
  config.vm.provision "shell", inline: <<-SHELL
	snap install microk8s --classic
  	microk8s enable dns
  	microk8s kubectl apply -f https://raw.githubusercontent.com/mc-b/duk/master/addons/dashboard-skip-login-no-ingress.yaml
  SHELL
  ```
  Man muss nun nur noch die VM starten und kann sich über den Browser verbinden.
  ```
  vagrant up 
  ```
  # Testing
  Nun sollte Kubernetes über den Browser erreichbar sein. Diesen ruft man auf mit https://localhost:8444 (Port, welcher man im Vagrantfile unter *host* angegeben hat)

  Es wird angezeigt, dass dies keine sichere Verbindung sei, da muss man einfach auf *Erweitert -> Weiter zu localhost*. Danach erscheint das Kubernetes Interface.

# Reflektion
    Ich habe der Kubernetes Aufbau recht einfach gefunden. Ich werde in Zukunft dies auch Privat aufsetzen und mit Kubernetes weiter herumexperimentieren. 
# Quellen
    World Wide Web