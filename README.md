# Raspberry-Pi-Minecraft-Server-Guide
In diesem Repository wird erklärt wie man einen Minecraft Server auf einem Raspberry Pi 4 Installiert

# Über den Server
Der Server ist ein normaler Vanilla Minecraft 1.20.2 Server (zum Zeitpunkt 06.10.2023)

# Setup
Für den Server habe ich ein Raspberry PI 4 mit 4 GB genutzt. Es ist auch ein Raspberry Pi 3 möglich zu empfehlen ist aber eindeutig ein Raspberry Pi 4.
Als Speicherplatz habe ich eine SD Karte mit 64 GB verwendet 8 GB sollten aber auch vollkommen ausreichen.

## 1. Erstellen eines Ubuntu Server Images

Als erstes muss Ubuntu Server 20.04.5 LTS (64 Bit) ausgewählt werden
![1](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/imager1.png?raw=true)

Nun musst du noch eine SD Karte auswählen

Im Anschluss müssen noch einige Einstellungen übernommen werden.

![2](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/imager2.png?raw=true)

Der Hostname lautet **minecraft-pi**

SSH sollte auch aktiviert werden. Als Nutzernamen verwenden wir **minecraft** und als Passwort **pi**.

![3](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/imager3.png?raw=true)

Nachdem die wichtigsten Einstellungen erledigt sind müssen noch allgemeine Einstellungen vorgenommen werden.

![4](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/imager4.png?raw=true)
![5](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/imager5.png?raw=true)

Zum schluss muss das Image nurnoch geschrieben werden. Dies dauert in der Regel ca. 5 min.

Über Putty greifen wir auf den Pi zu.


## 2. Ubuntu einrichten

```
sudo -s
apt update
apt upgrade
```

## 3. Java installieren

Nachdem das System auf dem neusten Stand ist schauen wir nach welche die aktuellste openjdk version ist.

```
apt search openjdk
```

nun muss die neuste Version installiert werden.

![6](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/jdk.png?raw=true)

```
apt install openjdk-17-jdk-headless
```

## 4. Minecraft Server installieren

nachdem JDK installiert ist muss noch ein Ordner angelegt werden für den Server

```
mkdir /home/ubuntu/minecraft
cd /home/ubuntu/minecraft
```

nachdem wir im Ordner sind laden wir den Server herunter mit folgendem Befehl ([Link](https://www.minecraft.net/en-us/download/server))

Einfach Rechtsklick auf den Link und die Adresse in das wget Statement kopieren

![7](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/mc.png?raw=true)

```
wget https://piston-data.mojang.com/v1/objects/5b868151bd02b41319f54c8d4061b8cae84e665c/server.jar
```

## 5. Server autostart Konfigurieren
mit folgendem Befehl startet der Server. 
```
java -Xmx3G -Xms3G -jar server.jar nogui
```

Zuerst muss die eula mit "true" bestätigt werden.

```
nano eula.txt
```

Als nächstes richten wir den Autostart ein

```
nano /home/minecraft/minecraft/screen-create.sh
```

In das Shell Skript sollte jetzt folgender Code eingegeben werden

```
#!/bin/bash
su - root -c "screen -m -d minecraft"
sleep 5
screen -S minecraft -X stuff 'cd /home/minecraft/minecraft\n'
screen -S minecraft -X stuff 'java -Xmx3G -Xms3G -jar server.jar nogui\n'
```

-Xmx und -Xms sind die angaben des Ram. 3G steht für 3GB.

Man sollte immer mindestens 1 GB ram für das OS überlassen.

Die Datei benötigt noch Rechte zum ausführen.

```
chmod +x /home/minecraft/minecraft/screen-create.sh
```

Um den den Server automatisch zu starten sobald er Strom hat, benötigen wir noch eine Einstellung.

```
crontab -e 
```

In der Datei sollte ganz unten folgender Text eingefügt werden

```
@reboot /home/minecraft/minecraft/screen-create.sh
```

Mit diesem Statement wird die screen-create.sh automatisch ausgeführt und startet den Server.
Als letztes sollte der Pi einmal neu gestartet werden, um auf nummer sicher zu gehen. Außerdem solltest du einmal den Autostart testen. 
Sobald der Pi hochgefahren ist kann man mit folgendem Befehl prüfen ob der Prozess läuft
```
screen -r minecraft
```

# Usage

mit  **screen -r minecraft** kann man sich den aktuellen Prozess ansehen und mit Strg + A und Strg + D geht man wieder aus dem Prozess hinaus. Zum Stoppen des Servers gibt man **stop** ein.


Zum Verwenden braucht der Raspberry Pi lediglich Strom und ein Ethernet anschlsus.

