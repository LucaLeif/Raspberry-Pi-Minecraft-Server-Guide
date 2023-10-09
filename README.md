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

Als nächstes muss eine SD Karte ausgewählt werden.

Im Anschluss müssen noch einige Einstellungen übernommen werden.

![2](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/imager2.png?raw=true)

Der Hostname lautet **minecraft-pi**

SSH sollte auch aktiviert werden. Als Nutzernamen verwenden wir **minecraft** und als Passwort **pi**.

![3](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/imager3.png?raw=true)

Nachdem die wichtigsten Einstellungen erledigt sind müssen noch allgemeine Einstellungen vorgenommen werden.

![4](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/imager4.png?raw=true)
![5](https://github.com/LucaLeif/Raspberry-Pi-Minecraft-Server-Guide/blob/main/Bilder/imager5.png?raw=true)

Zum schluss muss das Image nurnoch geschrieben werden. Dies dauert in der Regel ca. 5 min.

## 2. Ubuntu einrichten

```
sudo -s
apt update
apt upgrade
```

Nachdem das System auf dem neusten Stand ist schauen wir nach welche die aktuellste openjdk version ist.

## 3. Java installieren

mit folgendem Befehl kann man nach der aktuellsten JDK Version suchen

```
apt search openjdk
```

nun muss die neuste Version installiert werden.

```
apt install openjdk-17-jdk-headless

```

nachdem JDK installiert ist muss noch ein Ordner angelegt werden für den Server

```
mkdir /home/ubuntu/minecraft
cd /home/ubuntu/minecraft
```

nachdem wir im Ordner sind laden wir den Server herunter mit folgendem Befehl ([Link](https://www.minecraft.net/en-us/download/server))

```
wget https://piston-data.mojang.com/v1/objects/5b868151bd02b41319f54c8d4061b8cae84e665c/server.jar
```

## 4. Minecraft Server installieren
## 5. Server autostart Konfigurieren

