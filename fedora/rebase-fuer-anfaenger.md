## Rebase-ing für Anfänger

Da die *offizielle* Fedora Dokumentations nur das aller nötigste beschreibt, sind hier mal ein paar *common parctices*, die für sich für mich als hilfreich erwiesen haben und eben nicht in der Dokumentation stehen.
> [!IMPORTANT]
> Ich gehe davon, dass das System up-to-date ist. Wenn nicht, noch schnell updaten...

### Schritt 1: Deployment anpinnen
Da man aller Wahrscheinlichkeit nach auf etwas rebased, dass man nicht kennt, sollte man das aktuelle Deployment absichern. Ansonsten wird es irgendwann gelöscht, denn rpm-ostree behält nur 2 Deployments: Das aktuelle und das letzte funktionierende (funktierend heisst hier: es bootet). Und um zu verhindern, dass ein Deployment gelöscht wird, kann man es anpinnen:
```
sudo ostree admin pin 0
```
Das basiert auf der Ausgabe von `rpm-ostree status`, gezählt wird von oben und gestartet von der 1. ganzen nicht negativen Zahl, also 0.
Um das angepinntes Deployment wieder zu lösen:
```
sudo ostree admin pin -u 0
```

### Schritt 2: Aufräumen
Da ein Rebase in den allermeisten scheitert, wenn man zusätzliche Pakete gelayert hat, sollte man den Rotz vor dem eigentlichen Rebase entfernen. Das kann man per Hand machen oder man nutzt einfach
```
rpm-ostree reset -l
```
Das ist schneller. Und einfacher. Profis rebooten jetzt nicht, weil rpm-ostree mehrere Veränderungen stapeln und in einem ausführen kann. Und damit kommen wir zum eigentlichen Ding:

### Schritt 3: Rebase
Jetzt wird's spannend! Das eigentliche Rebase macht man mit
```
rpm-ostree rebase $TARGET
```
Die Frage ist, was denn das Ziel ist. Wenn man jetzt *nur* ein Upgrade auf die nächste Fedora Version machen möchte, kann man mit
```
ostree remote refs fedora | grep $(uname -m) | grep kinoite
```
rausfinden ob eins verfügbar ist. Wenn man z.B. auf Fedora 41 (mit Plasma Desktop, versteht sich) upgraden möchte, dann wäre der Befehl
```
rpm-ostree rebase fedora:fedora/41/x86_64/kinoite
```
Die Benennung erfolgt dabei nach dem Schema `fedora:fedora/$RELEASE/$ARCH/$DESKTOP`. Der Desktop behält hier den ursprünglichen Codenamen. Soll heissen:
 * Gnome heisst Silverblue
 * Plasma heisst Kinoite
 * Budgie heisst Onyx
 * Sway heisst Sericea
 
Die logische Schlussfolgerung daraus ist, dass man mit dem Befehl `rpm-ostree rebase fedora:fedora/41/x86_64/onyx` von KDE Plasma nach Budgie wechselt. Wenn's es einem nicht gefällt, kann man ja mit `rpm-ostree rollback` und einem Reboot wieder zurück wechseln.

#### Universal Blue
Richtig spannend wird es aber erst, wenn man [Universal Blue](https://universal-blue.org/) kennt. Das ist ein Projekt, dass custom Images der Atomic Desktops anbietet. Das bekannteste Image davon dürfte das für Gaming optimierte [Bazzite](https://bazzite.gg/) sein. Und ja, man kann mit `rpm-ostree rebase` von einem 08/15 Atomic Desktop ein rebase auf Bazzite machen. Und natürlich wieder zurück.
Da das Thema ab hier ein wenig komplexer wird, werde das in ein eigenes Dokument packen. Wenn ich Zeit habe. Und Lust. Also nie ;)
