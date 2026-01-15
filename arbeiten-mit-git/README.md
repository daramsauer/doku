---
icon: git-branch
order: 9000
---
# Arbeit mit Git
## Konfiguration
 
Die Konfiguration wird bei der ersten verwendung durchgeführt. Anschließend gelten die Werte für alle neuen Projekte.
 
In der Eingabeaufforderung werden folgende Befehle ausgeführt:
 
```
git config --global user.git config --global user.name "david ramsauer"
email "dramsauer@schueler.hakzell.at"
git config --global init.defaultBranch "main"
```
 
Da Der Parameter `--global` verwendet wird können diese Befehle in einem beliebigen Verzeichnis ausgeführt werden.
 
**Hinweis:** Falls Git neu installiert wurde, müssen sämtliche Eingabeaufforderungen (Fenster) vor der Befehlsausführung geschlossen werden.
 
Nach der Ausführung dieser Befehle befindet sich im Benutzerprofil (`%userprofile%`)
eine Datei namens `.gitconfig` mit folgendem Inhalt:
```
[user]
    name = david ramsauer
    email = dramsauer@schueler.hakzell.at
[init]
    defaultBranch = main
```
 
![Benutzerprofil](assets/userprofile.png)
 
 
![config](assets/config-file.png)
 
 
## Allgemeines
 
Ein Git-Repository unterscheidet grundsätzlich zwischen drei
Bereichen:
 
-`working copy`
-`staging area`
-`local repository`
 
![Git](assets/remote-repository.png)
 
im **working copy** befinden sich alle Dateien eines Verzeichnisses
(z.B. README.md, Bilder, Word-Dateien, etc.) Möchte man den Inhalt eines Ordners versionieren ("unter Git stellen"), so müssen die Dateien zur **staging area** hinzugefügt werden. Nach einem Commit werden die verwalteten Dateien in einem **repository** gespeichert.

## Laufende Arbeit mit Git

### Repository erstellen

Ein Repository wird **ein einziges Mal** erstellt. Hierzu wechselt man in den Ordner und führt dort in der Kommandozeile bzw. Powershell

```
git init
```

aus.

**Hinweis:** Durch `git init`wird im Ordner ein *.git*-Verzeichnis erstellt.
Eventuell ist das Verzeichnis nicht sichtbar (ausgeblendete Elemente in Windows zuvor einblenden.)

### Elemente hinzufügen

Datein und (nicht leere) Ordner werden folgendermaßen hinzugefügt:

```
git add .
git commit -m "add file"
```

Mit `git add .` werden alle Veränderungen (zB neue Datei, veränderte Datei, gelöschte Datei, etc.) in den Staging-Bereich gegeben. Mit `git commit` wird der Staging-Bereich ins Repository geschrieben (gesichert). Um Commits mit logischen (zusammengehörenden) Einheiten zu erstellen, können gezielt Datein mittels `git add filename` zum Staging-Bereich hinzugefügt werden. Dadurch können "bessere" Commits erstellt werden. In den Anführungszeichen wird eine kurze Nachricht, die den Inhalt des Commits beschreibt festgelegt.

### Remote-Repository hinzufügen

Ein lokales Repository kann auf zB GitHub remote gespeichert werden. Hierzu ist eine Anmeldung (Authentifizierung) bei GitHub notwendig.

Die Anmeldung erfolgt mittels *SSH-Schlüssel*, welche folgendermaßen in der Eingabeaufforderung generiert werden:

```
ssh-keygen - t ed 25519
```

Hierdurch wird ein **Schlüsselpaar** erstellt. Der private Schlüssel bleibt immer beim Ersteller - der öffentliche Schlüssel darf beliebig weitergegeben werden. Je nach Sicherheitsbedarf wird für den privaten Schlüssel ein Passwort vergeben.

Die Keys werden automatisch im Verzeichnis `C:\Users\%userprofile%\.ssh` gespeichert.

![SSH-Ordner](assets/ssh-ordner.png)

Der **öffentliche Schlüssel** wird in GitHub im Bereich Settings hinzugefügt:

![SSH-Schlüssel](assets/ssh-github.png)

Anschließend kann ein "Repository" in GitHub mittels "New" erstellt werden. da **lokal bereits ein Git-Repository erstellt wurde** muss das **Remote-Repository bei GitHub leer** sein.
![Leeres Repository auf GitHub erstellen](assets/github-neues-leeres-repository.png)

**Achtung:** Ein Repository darf NIEMALS lokal und remote (bei GitHub) erstellt werden!

Das Remote-Repository wird folgendermaßen hinzugefügt:

```
git remote add origin git@github.com:daramsauer/demo.git
git push -u origin main
```

Mittels `git push` werden stets die Commit zu GitHub übertragen.
Da der SSh-Schlüssel zu Anmeldung verwendet wird, muss auf die Auswahl SSh geachtet werden:

![SSH-Auswahl](assets/github-ssh-auswahl.png)

Während der Programmierung werden fortlaufend Commits erstellt.
Diese Commits werden mittels `git push` hinaufgeladen. Falls online Änderungen in der Zwischenzeit erfolt sind, so werden diese mittels `git pull` heruntergeladen.


### Remote-Repositorys klonen

Um auf ein Remote-Repository zugreifen zu können, muss das Repository **öffentlich** sein bzw. muss man in den Einstellungen als Mitarbeitender hinzugefügt worden sein.

Nach einem Klick auf dem "Code-Button" wird die SSH-URL kopiert und in der Eingabeaufforderung wird

```
git clone git@github.com:perasser/apr-2b.git
```
eingefügt. Dadurch wird das Remote-Repository heruntergeladen und in einem Ordner namens *apr-2b* gespeichert. Möchte man einen anderen Ordnernamen wählen, so muss dieser hinter dem Repositroynamen angegeben werden:

```
git clone git@github.com:perasser/apr-2b.git apr2-ordner
```

### Repository löschen

In GitHub können Repositorys im Settings-Bereich im Abschnitt "General" gelöscht werden.

**Hinweis:** Fehlende Berechtigungen können dazu füheren, dass kein Settings-Bereich verfügbar ist (zB Classroom-Repositorys).

### Mehrere Remote-Repositorys

Bei einem Repository können mehrere Remote-Repositorys hinzugefügt werdn. Standardmäßig erhält ein Remote-Repository den Namen *origin*.

Das Remote-Repository wird mittels:

```
git remote add origin git@github.com:daramsauer/xyz.git
```

hinzugefügt. Ein Name (zB origin) darf nur ein einziges Mal verwendet werden. Möchte man ein zweites Remote-Repository hinzufügen, so könnte man folgendermaßen schreiben:

```
git remote add classroom git@github.com:daramsauer/xyz.git
```


Bei `git push` muss ausgewählt werden, wohin die lokalen Commits übertragen werden.


```
git push origin main
```

bzw.

```
git push classroom main
```

Ist nur ein Remote-Repository verbunden, so reicht die Angabe von `git push`(ohne Details).

**Hinweis:** Die verbundenen Remote-Repositorys können mittels `git remote -v` angezeigt werden.

### Dateien/Ordner ausschließen

Um Dateien bzw. Ordner vom Repository auszuschließen, wird eine Datei namens `.gitignore`im Repository gespeichert. In dieser Datei werden die einzelnen Einträge (Dateiname bzw. Ordnername) zeilenweise aufgelistet.

Um zum Beispiel einen Ordner names `bin` vom Repository auszuschließen, wird eine `.gitignore` -Datei mit folgendem Inhalt erstellt:

```
bin/
```

Um zusätzlich auch noch eine Datei namens `geheim.txt` auszuschließen wird folgender Inhalt in die Datei geschrieben:

```
bin/
gheim.txt
```


### Achtung: Fehler

Im folgenden abschnitt wird gezeigt, welche Fehlermeldung erscheint, wenn ein Repository lokal und remote mit Inhalt erstellt wird.

![README.md wird online hinzugefügt](assets/github-onlin-mit-inhalt.png)

![README.md wird online hinzugefügt](assets/github-online-mit-inhalt-2.png)

Anschließend wird in einem Ordner ebenfalls eine Datei hinzugefügt, ein commit erstellt und mit dem zuvor erstellten GitHub-Repository verbunden:

![Fehler - GitHub und lokal erstellt](assets/github-und-lokal-fehler.png)






#### Exkurs: Public-Key-Verfahren

Bei den SSH-Schlüsseln handelt es sich um eine asymmetrische Verschlüsselung. Dies bedeutet, dass keine Schlüsselübertragung zu Verschlüsselung erfolgen muss (kein Passwort)

1. Der öffentliche Schlüssel dient zum Verschlüsseln - der private Schlüssel zur Entschlüsselung.
2. Mit dem privaten Schlüssel kann signiert wreden - mit dem öffentlichen Schlüssel kann die Signatur überprüft werden.
3. Wird der Public-Key auf einem Server hinterlegt, so kann eine Anmeldung auf diesem Server mit dem privaten Schlüssel erfolgen (ohne Passwort).