# Installation von Lubuntu auf dem HP Pro Tablet 408 G1

Diese Anleitung beschreibt die Installation von Lubuntu 64-Bit auf dem **HP Pro Tablet 408 G1**. 

Die größte Hürde bei diesem Tablet ist die Hardware-Architektur: Es nutzt einen **64-Bit Intel Atom Z3736F Prozessor**, verwendet aber ein **32-Bit UEFI (IA32)**. Standardmäßige 64-Bit Linux-Installer starten daher nicht ohne eine spezielle Anpassung.

---

## 1. Benötigte Hardware & Equipment

Da das Tablet nur über einen einzigen Micro-USB-Port verfügt und der Touchscreen im Installer **nicht funktioniert**, wird folgendes Zubehör zwingend benötigt:

* **HP Pro Tablet 408 G1** (vollständig geladen!).
* **USB-Stick** (mindestens 4 GB).
* **Powered Micro-USB-OTG-Hub** (Y-Kabel oder aktiver USB-Hub mit Micro-USB-Stecker).  
  * *Wichtig:* Ein Hub mit eigener Stromversorgung wird dringend empfohlen, da Tastatur + USB-Stick sonst den Akku des Tablets während der Installation schnell leeren.
* **USB-Tastatur** (kabelgebunden).
* *(Optional)* **USB-Maus** (erleichtert die Bedienung des Installers enorm).

---

## 2. Vorbereitung des USB-Sticks

### Schritt 2.1: Lubuntu-ISO herunterladen
Lade ein passendes Lubuntu 64-Bit ISO-Image herunter (empfohlen: **Lubuntu 22.04 LTS** oder **24.04 LTS**).

### Schritt 2.2: USB-Stick flashen
Flasche das ISO-Image mit einem Tool deiner Wahl auf den USB-Stick:
* **BalenaEtcher** oder **Rufus** (in Rufus: *DD-Abbild-Modus* wählen).

---

## 3. Der "32-Bit UEFI Trick" (Kritischer Schritt!)

Damit das 32-Bit UEFI des Tablets den 64-Bit Lubuntu-Bootloader starten kann, muss eine spezielle Datei manuell auf den USB-Stick kopiert werden.

1. Lade die Datei **`bootia32.efi`** herunter:
   * Direkter Download-Link (GitHub):  
     `https://github.com/hirofymy/grub-efi-bootia32/raw/master/bootia32.efi`
2. Öffne den geflashten USB-Stick im Dateimanager.
3. Navigiere in den Ordner:
   ```text
   /EFI/BOOT/
   ```

4. Kopiere die Datei `bootia32.efi` direkt in diesen `/EFI/BOOT/`-Ordner hinein.

> **Ergebnis:** Der Ordner `/EFI/BOOT/` auf dem Stick muss nun unter anderem folgende Dateien enthalten:
> * `bootx64.efi`
> * `bootia32.efi` *(deine neu hinzugefügte Datei)*
> 
> 

---

## 4. BIOS-Einstellungen am Tablet

1. Schalte das Tablet vollständig aus.
2. Schließe den **USB-Hub** mit angeschlossenem **USB-Stick** und **USB-Tastatur** an das Tablet an.
3. Halte die Tasten **Power + Lautstärke Leiser (Vol -)** gleichzeitig gedrückt, bis das Startup-Menü erscheint.
4. Drücke `F10` (auf der Tastatur), um ins **BIOS Setup** zu gelangen.
5. Nimm folgende Einstellungen vor:
* **Security:** `Secure Boot` ➔ **Disabled**
* **Advanced / Boot Options:** `Fast Boot` ➔ **Disabled**


6. Speichere die Änderungen (`F10`) und beende das BIOS.

---

## 5. Lubuntu-Installation starten

1. Starte das Tablet erneut mit gedrückter Kombination **Power + Lautstärke Leiser (Vol -)**.
2. Drücke `F9` (auf der Tastatur), um das **Boot-Auswahlmenü** zu öffnen.
3. Wähle deinen USB-Stick aus der Liste aus (wird meist als *EFI USB Device* angezeigt).
4. Das GRUB-Bootmenü von Lubuntu sollte nun erscheinen.
5. Wähle **"Try or Install Lubuntu"**.

---

## 6. Durchführen der Installation

1. Folge dem grafischen Installer auf dem Bildschirm (Bedienung über USB-Tastatur und USB-Maus).
2. **WLAN-Verbindung:** Verbinde dich während der Installation mit dem Internet, damit Grub und Treiber korrekt eingerichtet werden.
3. **Partitionierung:** Wähle *"Festplatte löschen"* (Erase Disk), um das gesamte eMMC-Laufwerk für Lubuntu zu nutzen.
4. Führe die Installation bis zum Ende durch und ziehe nach dem Herunterfahren den USB-Stick ab.

---

## 7. Bekannte Nacharbeiten (Troubleshooting)

Nach dem ersten Neustart sind typischerweise folgende Dinge auf dem HP Pro Tablet 408 zu beachten:

* **WLAN (5 GHz):** Falls 5 GHz Netze fehlen, setze die Wi-Fi Reg-Domain im Terminal:
```bash
sudo iw reg set DE
```

* **Display-Rotation:** Das Display steht nach dem Booten oft auf dem Kopf oder im Hochformat. Dies lässt sich in den Lubuntu-Sitzungseinstellungen fest auf Querformat einstellen.
* **Touchscreen / Audio:** Je nach Kernel-Version kann der Baytrail-Touchscreen-Treiber Aussetzer haben. Für einen **Kiosk-Betrieb** wird das Display ohnehin rein als Monitor genutzt.

---
