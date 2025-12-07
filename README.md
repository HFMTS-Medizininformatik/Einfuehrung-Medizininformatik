# HFMTS-Medizininformatik

Dieses Readme beschreibt...
1) die Vorbereitung einer Windows-Maschine für die lokale Virtualisierung, 
2) das Einrichten eines Open Source PACS, inkl. einer Orthanc Docker Container Demo und
3) das Einrichten eines Open Source KIS, inkl. einer Ozone HIS Docker Container Demo.
4) ein Tutorial zum Entdecken von Ozone HIS - Frontend und Backend.


## 1) Virtualisierung vorbereiten

1. Microsoft Store öffnen: \
Windows-Taste drücken -> `Store` eingeben -> mit Enter bestätigen

2. Im Store nach `Docker Desktop` suchen und installieren.

3. Ein Docker-Images testen.
    - Eine Console (Windows PowerShell) öffnen und folgender Befehl ausführen:
    ```
    docker run hello-world --rm
    ```
    - In der Console wird folgendes ausgegeben: 
    ```
    ...
    Hello from Docker!
    This message shows that your installation appears to be working correctly. 
    ...
    ```


### Wichtige Hinweise

- Meldet euch bei mir, falls eine Fehlermeldung ausgegeben wird!
- Es ist <u>keine</u> Anmeldung/Registrierung bei Docker nötig! Einfach auf die`skip`-Option klicken.

- **Optional** kann eine eigene Linux Distribution eingerichtet werden:
    1. Microsoft Store öffnen: \
    Windows-Taste drücken -> `Store` eingeben -> mit Enter bestätigen
    2. Im Store nach `Ubuntu` suchen und installieren.
    3. In Docker Desktop die Integration mit Ubuntu aktivieren: \
    Docker Desktop starten -> `Settings` öffnen -> Register `Resources` -> und `WSL integration` anwählen -> Option `Ubuntu` aktivieren

## 2) Open Source PACS (DICOM)

Orthanc Docker Container Demo.


### Ressourcen
- Website: https://www.orthanc-server.com/
- Projektseite (Orthanc Docker Images): https://orthanc.uclouvain.be/
- Orthanc auf Docker Hub: https://hub.docker.com/r/jodogne/orthanc


### DICOM-Server automatisch herunterladen und ausführen

1. Dokumentation lesen: https://orthanc.uclouvain.be/book/users/docker.html

2. Eine Console (Windows PowerShell) öffnen.

3. Um das Docker-Image von der Registry (Docker Hub) herunterzuladen, den Container zu erstellen und:
    
    a) Orthanc ohne Plugins auszuführen:
    ```
    docker run --name orthanc -p 4242:4242 -p 8042:8042 -d --rm jodogne/orthanc
    ```
    b) Othanc mit Plugins auszuführen (bevorzugte Methode):
    ```
    docker run --name orthanc -p 4242:4242 -p 8042:8042 -d --rm jodogne/orthanc-plugins
    ```

4. mit dem Web-App verbinden und anmelden: http://localhost:8042/
    ```
    Benutzername: orthanc
    Passwort: orthanc
    ```

5. Have fun ✨

6. Um den Docker-Container zu stoppen:
    ```
    docker stop orthanc
    ```


## 3) Open Source KIS (MRS+LIS+PACS+…)

Ozone HIS Docker Container Demo.


### Ressourcen
- Website: https://www.ozone-his.com/
- Dokumentation - Komponentenübersiche (Ozone HIS Apps): https://docs.ozone-his.com/users/
- Source Code (GitHub): https://github.com/ozone-his


### Gesamtes KIS-Ökosystem lokal aufbauen und testen

> **Achtung:** \
> Das Build kann nur auf einem Linux-System erstellt werden. 
> Folglich ist eine eigene Lunix Distribution einzurichten. 
> Folge hierzu den optionalen Anweisungen unter *1) Virtualisierung einrichten*.

1. Dokumentation lesen: https://docs.ozone-his.com/getting-started/run-locally/

2. Die Ubuntu-Console ausführen: \
Windows-Taste drücken -> `Ubuntu` eingeben -> mit Enter bestätigen.`

3. In der Console, das Ubuntu-System updaten:
    ```
    sudo apt-get update
    ```

4. In der Console das Installations-Script herunterladen und ausführen:
    ```
    curl -s https://raw.githubusercontent.com/ozone-his/ozone/main/scripts/install-stable.sh | bash /dev/stdin
    ```

    > Dies wird den Source-Code kopieren und ein Build (Docker-Image) erstellen.

    
    **Hinweis:**
    Falls die Java SDK Installation fehlt, wird ein entsprechender Fehler ausgegeben. Um den Fehler zu beheben, kann die SDK installiert werden:
    ```
    sudo apt install default-jdk
    ```
    Danach ist Schritt 4 zu wiederholen.


5. Ozone HIS ausführen:
    ```
    cd ozone/run/docker/scripts/

    ./start-demo.sh
    ```

6. Mit den gewünschten Web-App-Komponenten verbinden und anmelden.
    > Die dazu benötigten Angaben werden in der Konsole ausgegeben.

7. Have fun ✨

8. Um den Docker Container zu stoppen:
    ```
    ./destroy-demo.sh
    ```
    Oder alternativ, um alle Einstellungen beizubehalten:
    ```
    ./stop-demo.sh
    ```

## 4) Ozone HIS Tutorial

> Egal ob Orthanc oder einer der Komponenten des HIS-Ökosystems, alle sind Database-driven WebApps.

**Ziel:** Open Source KIS kennenlernen.

Teil 1: Ressourcen monitoren, Datenverarbeitung anschauen (Frontend) \
Teil 2: Auf die Datenbank zugreifen, Struktur nachvollziehen und Daten abrufen (Backend)

### Teil 1 - Frontend

Im MRS:
- Patientin registrieren
- Visite einplanen, durchführen und Labor-Auftrag auslösen

Im LIS:
- Laborauftrag entgegennehmen, ausführen und freigeben

Im MRS:
- Laborergebnisse abrufen und anschauen
- Medikamente ausgeben

Im ERP
- Dienstleistung und Medikamente in Rechnung stellen

### Teil 2 - Backend

Services erkunden: `docker ps`

Senaite einloggen: `docker exec -it <id> sh`

Umgebungvariablen auslesen: `printenv`

Postgres einloggen: `docker exec -it <id> sh`

Umgebungvariablen auslesen: `printenv`

Mit Pgadmin und den Zugangsdaten Datenbank durchsuchen.
