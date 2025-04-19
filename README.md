
# Crafty Controller auf Raspberry Pi (ARM64)

Ein Leitfaden zur Installation von **Crafty Controller**, einem webbasierten Verwaltungstool für Minecraft-Server, auf einem Raspberry Pi 4 mit ARM64-Prozessor. Inklusive Playit.gg für externen Zugriff. Getestet auf **Raspberry Pi 4** mit Ubuntu 64-Bit oder Raspberry Pi OS 64-Bit.

## Voraussetzungen
- Raspberry Pi 4/5 (ARM64-Betriebssystem, z. B. Ubuntu 64-Bit oder Raspberry Pi OS 64-Bit)
- `sudo`-Rechte
- Internetverbindung
- Grundkenntnisse im Terminal

## Installation

### 1. System aktualisieren und Git installieren
```bash
sudo apt update && sudo apt upgrade && sudo apt install git
```

### 2. Java 21 installieren
Crafty Controller benötigt Java 21. Installiert Adoptium Temurin JDK für ARM64.
```bash
wget https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.2+13/OpenJDK21U-jdk_aarch64_linux_hotspot_21.0.2_13.tar.gz && \
tar -xzf OpenJDK21U-jdk_aarch64_linux_hotspot_21.0.2_13.tar.gz && \
sudo mkdir -p /opt/java-21 && \
sudo mv jdk-21.0.2+13 /opt/java-21/ && \
echo -e 'export JAVA_HOME=/opt/java-21/jdk-21.0.2+13\nexport PATH=$JAVA_HOME/bin:$PATH' | sudo tee /etc/profile.d/jdk21.sh > /dev/null && \
source /etc/profile.d/jdk21.sh && \
sudo update-alternatives --install /usr/bin/java java /opt/java-21/jdk-21.0.2+13/bin/java 2121
```

### 3. Crafty Controller installieren
Klonen und installieren Sie Crafty Controller.
```bash
git clone https://gitlab.com/crafty-controller/crafty-installer-4.0.git && \
cd crafty-installer-4.0 && \
sudo ./install_crafty.sh
```

### 4. Crafty Controller starten
Starten Sie Crafty Controller manuell, um die Funktionalität zu prüfen.
```bash
sudo su - crafty -c "cd /var/opt/minecraft/crafty && ./run_crafty.sh"
```

### 5. Standard-Anmeldeinformationen anzeigen
Rufen Sie die Standard-Login-Daten ab.
```bash
sudo cat /var/opt/minecraft/crafty/crafty-4/app/config/default-creds.txt
```
**Hinweis**: Speichern Sie diese Daten sicher.

### 6. Crafty als Dienst einrichten
Aktivieren und starten Sie Crafty Controller als systemd-Dienst.
```bash
sudo systemctl enable crafty.service && sudo systemctl start crafty.service
```

### 7. Playit.gg für Tunneling installieren
Installieren Sie Playit.gg, um den Server extern zugänglich zu machen.
```bash
curl -SsL https://playit-cloud.github.io/ppa/key.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/playit.gpg >/dev/null && \
echo "deb [signed-by=/etc/apt/trusted.gpg.d/playit.gpg] https://playit-cloud.github.io/ppa/data ./" | sudo tee /etc/apt/sources.list.d/playit-cloud.list && \
sudo apt update && \
sudo apt install playit
```

### 8. Playit.gg einrichten
Starten und konfigurieren Sie Playit.gg.
```bash
sudo systemctl start playit && sudo systemctl enable playit && playit setup
```

## Zugriff
- **Crafty Controller Weboberfläche**: `https://<Ihre-Raspberry-Pi-IP>:8443` (HTTPS, Port kann variieren).
- **Playit.gg**: Folgen Sie der `playit setup`-Anleitung, um Tunnel für den externen Zugriff einzurichten.

## Tipps
- Führen Sie die Schritte in der angegebenen Reihenfolge aus.
- Prüfen Sie die Befehlsausgaben auf Fehler.
- Stellen Sie sicher, dass Ihr Raspberry Pi ausreichend Ressourcen (RAM, CPU) hat.

## Fehlerbehebung
- **Java-Probleme**: Überprüfen Sie mit `echo $JAVA_HOME` und `java -version`.
- **Crafty-Probleme**: Logs in `/var/opt/minecraft/crafty/crafty-4/logs/`.
- **Playit.gg-Probleme**: Siehe [playit.gg Dokumentation](https://playit.gg).

## Lizenz
Dieses Projekt steht unter der MIT-Lizenz, sofern nicht anders angegeben. Crafty Controller und Playit.gg haben eigene Lizenzbedingungen.

