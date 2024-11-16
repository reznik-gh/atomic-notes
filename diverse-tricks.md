# Diverse Tipps & Tricks
Hier sammele ich verschiedene Tipps & Tricks, über die ich stolpere und die keine eigene Seite brauchen.

## Flatpak 

### Es hakt bei laden der Updates
Das sollte den Cache von Flatpak resetten und dann sollte es flüssig laufen.
```bash
rm -fr /var/cache/PackageKit/*
pkcon refresh force
flatpak update --appstream
```
