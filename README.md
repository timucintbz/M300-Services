# M300-Services
# M300 Dokumentation
# Einleitung
Dies ist das Projekt des Moduls 300 - Plattformübergreifende Dienste in ein Netzwerk integrieren.


# Inhaltsverzeichnis
* 10-Toolumgebung
* 20-Infrastruktur
* 25-Sicherheit 1
* 30-Container
* 35-Sicherheit 2
* 40-Container-Orchestrierung
* 50-Add-ons
* 60-Reflexion
* Mein Kommentar

# 10 - Toolumgebung
## Voraussetzung
* PC/Notebook mit min. 8 GB freiem RAM, ca. 20 GB freiem HD-Speicher und einer Ethernet-Netzwerkkarte.
* Einfache Linux und Apache Web Server Kenntnisse sind von Vorteil.
* Ein schneller Netzwerk- (Kabel!) und Internet-Anschluss
## SSH-Keys
Der SSH Key ist in unserem Fall dafür zuständig, dass man bei einem Commit oder Push sich nicht immer manuell mit dem Github-Account muss anmelden. Man kann ihn auch so generieren, dass man einfach ein neues Passphrase erstellen kann.

Der erste schritt den man machen muss ist so ein SSH-Key erstellen. Dies macht man im Git-Bash mit folgendem Befehl:

```
$  ssh-keygen -t rsa -b 4096 -C "timucin.soenmez@edu.tbz.ch"
```

Danach wird der Key erstellt. 

Der Key ist nun erstellt und wir können ihm im Github an unserem Konto anhängen. Der Key wurde Lokal im User Verzeichnis im Ordner .ssh überschrieben. Wir kopieren diesen und fügen ihn im Github unter Settings -> SSH and GPG Keys -> New SSH key ein. Man muss dem Key dann auch einen Namen geben, damit falls ein weiterer Key eingefügt werden muss, man sicher gehen kann, welcher Key welcher ist.


## Git-Setup
### Client konfigurieren
Damit die Arbeiten lokal auf dem eigenen PC erfolgen können, muss der sogenannte "Git Client", auf Windows "Git/Bash" installiert werden. Dieser ermöglicht uns, Cloud-Repositories zu klonen, zu pullen (herunterladen) oder ein lokales Repository zu pushen (hochladen).

Hierzu müssen folgende Schritte durchgeführt werden:

Natürlich muss auf Github mal ein Account erstellt werden. Ich denke, dazu muss ich keine Erklärung schreiben.

Danach lädt man, falls nicht vorhanden, Git Bash herunter und danach geht man auf das Git Bash Terminal und registriert seinen eigenen Github Account. Dies funktioniert folgendermassen:

```
  $ git config --global user.name "<username/timucintbz>"
  $ git config --global user.email "<e-mail/timucin.soenmez@edu.tbz.ch>"
```
Danach ist die Konfiguration abgeschlossen.
## Repo-Setup
### Repository herunterladen & aktualisieren (clone/pull)
Im Git Bash Terminal erstellen wir nun einen Ordner, damit wir unser Repository vom Bash aus konfigurieren können, später machen wir das dann über Visual Studio Code (Sehe hier).

Wir gehen in das Verzeichnis, wo man den Ordner haben möchte. Ich habe es direkt im C: erstellt. Die Befehle lauten folgendermassen:

```
  #Directory wechseln
  $ cd C:
  #Zum erstellen des Ordners:
  $ mkdir M300-Services
```
Nachdem man den Ordner Lokal erstellt hat verbindet man das Repository, welches man auf Github erstellt hat, mit dem Ordner, damit man Lokal arbeiten kann. 

Zuerst das Repo in Github erstellen.

Unter Profile -> Your repositories -> New erstellt man das Repo. Man sollte darauf schauen, dass die Namen des Repo's und des Lokalen Ordners einheitlich sind, damit man die Übersicht beibehalten kann.


Ich habe es auf Private gesetzt, damit nicht jeder den Code lesen und die Infos klauen kann. Man kann es jedoch bestimmten und gewollten Personen freigeben. Dies macht man im Repo unter Settings -> Manage Access und dann kann man Github users dies freigeben:



Nun klonen wir das Repo Lokal in unseren Ordner. Dazu kann man im Github den URL des vorhin erstellten Repo kopieren und folgenden Befehl eingeben:

```
  $ git clone git@github.com/timucintbz/M300-Services
```
Wir haben jetzt erfolgreich unser Github Repo lokal mit unserem Ordner synchronisiert. Testen kann man dies mit einem git pull Befehl, es sollte dann die Message "Already up to date." erscheinen.
## Virtualbox
### VM erstellen
Das erstellen von einer VM haben wir schon dutzende von Male gemacht und deswegen werde ich dies in diesem Schritt nur kurz erläutern, da es ein einfacher Schritt ist. Das Vorgehen ist folgendes:

1. VirtualBox starten
2. Links oben, innerhalb der Anwendung, auf Neu klicken
3. Im neuen Fenster folgende Informationen eintragen:
  - Name: M300_Ubuntu_XX.04_Desktop
- Typ: Linux
- Version: Ubuntu (64-bit)
- Speichergrösse: 2048 MB
- Platte: [X] Festplatte erzeugen
4. Auf Erzeugen klicken
5. Weiteres Fenster öffnet sich, folgende Informationen eintragen:
- Dateipfad: standard
- Dateigrösse: 10.00 GB
- Dateityp der Festplatte: VMDK (Virtual Maschine Disk)
- Storage on physical hard disk: dynamisch alloziert
6. Ebenfalls auf Erzeugen klicken, dann im Hauptmenü die VM anwählen (blau markiert) und den Punkt Ändern aufrufen
7. Im Abschnitt Massenspeicher den SATA-Controller anwählen und auf das CD+Symbol klicken
8. Unter Medium auswählen das zuvor heruntergeladene Systemabbild (ISO-Datei) anwählen
9. Alle Änderungen speichern und die VM starten
10. Den Installationsanweisungen der OS-Installation folgen und anschliessend zu Abschnitt "VM einrichten" gehen

### VM einrichten
Nun kann man die VM starten und einrichten wie man es möchte, also User, Sprache, Land etc. 
## Vagrant
Ein weiterer Schritt ist der erste Kontakt mit Vagrant zu erreichen. Für dies installieren wir als erstes Mal die Software. Für dies gehen wir auf die [Website](https://www.vagrantup.com/) selbst und installieren die Software, danach kann mit dem erstellen der ersten Vagrant VM gestartet werden. 

### Virtuelle Maschine erstellen
Starten wir mit dem erstellen der VM mit Vagrant. Wir beginnen damit, dass wir auf den Pfad wechseln wo wir die VM haben möchten und als erstes mal einen Ordner erstellen. Danach wechseln wir auf diesen Ordner und erstellen dann die VM. Die Befehle für die ersten Schritte sind folgende:

```
  $ cd Wohin/auch/immer
  $ mkdir MeineVagrantVM
  $ cd MeineVagrantVM
```
Zum erstellen von der VM müssen wir angeben, welche OS mit wievielen Bits wir sie haben wollen, und daanch starten wir diese gleich. Die Befehle sind:

```
  $ vagrant init ubuntu/xenial64        #Vagrantfile erzeugen
  $ vagrant up --provider virtualbox    #Virtuelle Maschine erstellen & starten
```
Die VM sollte nun in Betrieb sein und auch in der Virtualbox Übersicht erscheinen:

![GIT](/images/MeineVagrantVM.png)

Wir können sie nun über SSH-Zugriff bedienen. Dies funktioniert folgendermassen:

```
  $ cd Pfad/zu/meiner/Vagrant-VM      #Zum Verzeichnis der VM wechseln
  $ vagrant ssh                       #SSH-Verbindung zur VM aufbauen

  #Anschliessend können ganz normale Bash-Befehle abgesetzt werden:

  $ ls -l /bin  #Bin-Verzeichnis anzeigen
  $ df -h       #Freier Festplattenspeicher
  $ free -m     #Freier Arbeitsspeicher
```
Am Ende kann dann die VM über das Virtualbox-GUI ausgeschalten werden.
## Virtual Studio Code
### Programm installieren
Als Editor habe ich mich für Visual Studio Code entschieden, da es sehr einfach zu bedienen ist und ich schon Erfahrungen mit dem Programm gesammelt habe. 

Wie bei Vagrant muss auch dies zuerst installiert werden. Dies kann man unter diesem [Link](https://code.visualstudio.com/)

### Extensions
Nun laden wir noch ein paar Extensions herunter um das Arbeiten noch einfacher zu machen:

- Markdown All in One (von Yu Zhang)
- Vagrant Extension (von Marco Stanzi)
- vscode-pdf Extension (von tomiko1207)
### Einstellungen
Damit keine Dateien der virtuellen Maschinen dem Cloud-Repository hinzugefügt werden (da Dateien zu gross), müssen diese in den Einstellungen "exkludiert" werden:
1. Visual Studio Code öffnen
2. Unter File > Preferences > Settings (Ctrl + ,) auf Open setting.json klicken
3. Zu diesem Abschnitt gehen:
```
 // Configure glob patterns for excluding files and folders. For example, the files 
 explorer decides which files and folders to show or hide based on this setting. 
 Read more about glob patterns here. (...)
 ```
4. Nachstehenden Code einfügen:
```
// Konfiguriert die Globmuster zum Ausschließen von Dateien und Ordnern.
 "files.exclude": {
   "**/.git": true,
   "**/.svn": true,
   "**/.hg": true,
   "**/.vagrant": true,
   "**/.DS_Store": true
 },
 ```
 5. Änderungen speichern und die Einstellungen schliessen

Nun sollten keine Dateien mit den Endungen .git / .svn / .hg / .vagrant / .DS_store hochgeladen werden. Wie man die Änderungen innerhalb von Visual Studio Code richtig pusht, wird im nachfolgenden Abschnitt erklärt.
### Repo hinzufügen & pushen
Wir können jetzt unser Repository hinzufügen und von dort aus arbeiten. Es ist sehr einfach, man kann in VSCode unter File -> Open Folder -> Ordner auswählen -> Ordner auswählen. Nun kann man das File von VSCode aus bearbeiten. 


Das pushen ist von hier aus sehr viel einfacher als beim Git Bash. Bei Änderungen muss man den Fortschritt speichern (ctrl + s), danach folgende Schritte ausführen:
1. In der linken Leiste das Symbol mit einer "1" aufrufen
2. Unter dem Abschnitt Changes die betroffenen Files bezüglich ihres Changes "stagen" (Stage Changes)
3. Nachricht hinterlegen (Message) und Haken (Commit) setzen
4. Bei den 3 Punkten (...) die Funktion Push aufrufen
5. Warten, bis Dateien vollständig gepusht wurden


# 20 - Infrastruktur
# 25 - Sicherheit 1
# 30 - Container
# 35 - Sicherheit 2
# 40 - Container-Orchestrierung
# 50 - Add-ons
# 60 - Reflexion
# Mein Kommentar
# Quellen
https://github.com/adam-p/markdown-here/wiki/Markdown-Here-Cheatsheet
