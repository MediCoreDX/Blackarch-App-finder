# BlackArch Tool Manager

Eine grafische Benutzeroberfläche (GUI) zur Suche, Anzeige, Installation und Deinstallation von Paketen aus dem lokal eingerichteten **BlackArch-pacman-Repository**.

Das Programm wurde mit **Python 3 und PySide6** entwickelt und richtet sich an Benutzer von Arch Linux und Manjaro KDE.

## Funktionen

- Suche nach BlackArch-Tools anhand von Namen und Beschreibungen
- Anzeige von Paketversionen und Beschreibungen
- Anzeige der BlackArch-Kategorien
- Filterung nach Kategorie
- Filterung nach Installationsstatus
- Anzeige installierter und verfügbarer Pakete
- Installation einzelner Pakete über Pacman
- Deinstallation ausgewählter Pakete
- Synchronisierung der Paketdatenbanken
- Hintergrundverarbeitung mit QThread
- Grafische Oberfläche im dunklen Grün-Blau-Design
- KDE-kompatible Bedienung

## Voraussetzungen

Folgende Komponenten werden benötigt:

- Python 3
- PySide6
- Pacman
- pacman-conf
- PolicyKit (pkexec)
- Aktives BlackArch-Repository

PySide6 unter Manjaro installieren:

```bash
sudo pacman -S pyside6
```

## BlackArch-Repository

Das BlackArch-Repository muss bereits in `/etc/pacman.conf` eingerichtet sein.

Aktivierte Repositories anzeigen:

```bash
pacman-conf --repo-list
```

Die Ausgabe sollte unter anderem enthalten:

```text
core
extra
multilib
blackarch
```

Paketdatenbanken synchronisieren:

```bash
sudo pacman -Syy
```

Anzahl der verfügbaren BlackArch-Pakete anzeigen:

```bash
pacman -Sl blackarch | wc -l
```

## Installation

Repository beziehungsweise Projektverzeichnis öffnen:

```bash
cd ~/Downloads/blackarch-tool-manager
```

Installationsskript ausführbar machen:

```bash
chmod +x install.sh
```

Anwendung installieren:

```bash
./install.sh
```

Alternativ kann das Programm direkt gestartet werden:

```bash
python3 blackarch_tool_manager.py
```

## Bedienung

1. BlackArch Tool Manager starten.
2. Warten, bis die lokale Paketdatenbank eingelesen wurde.
3. Über das Suchfeld nach einem Tool suchen.
4. Gewünschte Kategorie auswählen.
5. Installationsstatus filtern.
6. Ein Paket auswählen, um weitere Informationen anzuzeigen.
7. Über die Schaltfläche **Installieren** oder **Deinstallieren** die Paketverwaltung starten.

## Paketverwaltung

### Installation

Die Anwendung verwendet:

```bash
pkexec pacman -S --needed --noconfirm PAKETNAME
```

Die erforderliche Authentifizierung erfolgt über PolicyKit.

### Deinstallation

Die Anwendung verwendet:

```bash
pkexec pacman -R --noconfirm PAKETNAME
```

Dabei wird ausschließlich das ausgewählte Paket entfernt. Gemeinsam genutzte Abhängigkeiten werden nicht automatisch gelöscht.

### Datenbanken synchronisieren

Die Synchronisierung verwendet:

```bash
pkexec pacman -Syy
```

Dieser Vorgang lädt ausschließlich die Paketdatenbanken neu.

**Wichtig:** Es wird kein vollständiges Systemupgrade durchgeführt. Die Anwendung aktualisiert weder Manjaro noch installierte Pakete über diese Schaltfläche.

## Technischer Aufbau

| Komponente | Beschreibung |
|---|---|
| `Package` | Datenmodell für Paketinformationen |
| `run_command()` | Führt Systembefehle ohne Shell aus |
| `Worker` | Führt zeitintensive Aufgaben im Hintergrund aus |
| `MainWindow` | Hauptfenster und grafische Oberfläche |
| `pacman -Ss .` | Liest verfügbare Pakete |
| `pacman -Sgg` | Liest Paketgruppen und Kategorien |
| `pacman -Qq` | Liest installierte Pakete |

## Sicherheitshinweise

**Achtung bei Manjaro:**

BlackArch ist primär für Arch Linux entwickelt. Die Kombination mit Manjaro wird nicht offiziell unterstützt und kann zu Abhängigkeits- und Paketkonflikten führen.

Daher gilt:

- Bevorzugt einzelne Pakete installieren.
- Vor jeder Installation die Pacman-Transaktion kontrollieren.
- Keine unbekannten Abhängigkeiten ungeprüft akzeptieren.
- Das Metapaket `blackarch-officials` nicht unüberlegt installieren.
- Kein unüberlegtes vollständiges Systemupgrade mit aktiviertem BlackArch-Repository durchführen.
- Vor größeren Änderungen ein Backup erstellen.

## Fehlerbehebung

### Repository wird nicht erkannt

```bash
pacman-conf --repo-list
```

Prüfen, ob `blackarch` aufgeführt wird.

### Paketliste bleibt leer

```bash
sudo pacman -Syy
pacman -Sl blackarch | head -20
```

### PySide6 fehlt

```bash
sudo pacman -S pyside6
```

### Installationsdialog erscheint nicht

Prüfen, ob `pkexec` installiert ist und die Desktop-Umgebung eine PolicyKit-Abfrage bereitstellt.

## Lizenz

MIT License

---

**BlackArch Tool Manager**  
Python 3 · PySide6 · Pacman · BlackArch Repository
