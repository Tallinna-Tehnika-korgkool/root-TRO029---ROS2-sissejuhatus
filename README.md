# TRO029 – ROS2 sissejuhatus (kaugõppe juhend)

See juhend on mõeldud tudengitele, kes **ei saa osaleda auditoorsetes tundides**
ja läbivad TRO029 "Sissejuhatus ROS2" kaugelt, oma arvutis, kasutades
**Docker Desktopi**. Kõik nädalad kasutavad ühte ja sama teed: kohalik
Docker konteiner.

## Kursuse teemad

- Docker-põhise ROS 2 õpikeskkonna seadistamine
- Linux käsurea tööriistade kasutamine
- ROS 2 arhitektuur ja sõnumipõhine suhtlus
- ROS 2 tööruumi (`workspace`) ja pakettide (`package`) struktuur
- Publisher / Subscriber mustrid
- Services
- Launch failide koostamine ja süsteemide ülesehitus

## Õppematerjalide struktuur

Iga teema on jagatud kaheks nädalaplokiks ja hallatud eraldi Git
submodule'ina kaustas `chapters/`. Kuna tegu on submodule'idega, ei näita
GitHub nende sisu selle repo enda kaustavaates — kasuta tabeli linke, mis
viivad otse vastava nädala repositooriumisse.

| Nädalad | Teema (link) | Sisu |
|---------|--------------|------|
| 01–02 | [`week01-02_intro`](https://github.com/Tallinna-Tehnika-korgkool/TRO029-week01-02_intro-template) | Docker + ROS 2 keskkonna seadistamine |
| 03–04 | [`week03-04_linux`](https://github.com/Tallinna-Tehnika-korgkool/TRO029-week03-04_linux-template) | Linux CLI põhitõed, failisüsteem, protsessid |
| 05–06 | [`week05-06_architecture`](https://github.com/Tallinna-Tehnika-korgkool/TRO029-week05-06_architecture-template) | ROS 2 arhitektuur: noded, topicud, teenused |
| 07–08 | [`week07-08_workspace`](https://github.com/Tallinna-Tehnika-korgkool/TRO029-week07-08_workspace-template) | Workspace ja pakettide loomine |
| 09–10 | [`week09-10_pubsub`](https://github.com/Tallinna-Tehnika-korgkool/TRO029-week09-10_pubsub-template) | Publisher / Subscriber mustri rakendamine |
| 11–12 | [`week11-12_services`](https://github.com/Tallinna-Tehnika-korgkool/TRO029-week11-12_services-template) | Services + kohandatud liidesed |
| 13–14 | [`week13-14_launch`](https://github.com/Tallinna-Tehnika-korgkool/TRO029-week13-14_launch-template) | Launch failid ja süsteemide ühendamine |

Kohalikult kausta sisu kättesaamiseks kloonimisel kasuta:

```bash
git clone --recurse-submodules https://github.com/Tallinna-Tehnika-korgkool/root-TRO029---ROS2-sissejuhatus.git
```

Kui repo on juba kloonitud ilma submodule'ideta:

```bash
git submodule update --init --recursive
```

## Keskkonna seadistamine

### 1. Eeldused

- **Docker Desktop** (Windows / macOS / Linux)
- **Git**
- Terminal (PowerShell / bash / zsh)
- Soovituslik: vähemalt 8 GB RAM, SSD

### 2. Docker image ehitamine

Selle repo juurkaustas:

```bash
docker build -t tro029-ros2 -f docker/Dockerfile .
```

Esimene build võib võtta mitu minutit.

### 3. Konteineri käivitamine

```bash
docker run -it \
  -v "$(pwd):/workspace/repo" \
  -v "tro029-ros2-ws:/workspace/ros2_ws" \
  tro029-ros2
```

- `/workspace/repo` sisaldab seda juhendit (chapters/ READMEd on siin loetavad).
- `/workspace/ros2_ws` on **püsiv nimeline Docker volume** — sinu kood
  säilib ka siis, kui konteiner suletakse või kustutatakse ja uus
  käivitatakse. (Ilma selleta kaob kogu töö, kui konteiner eemaldatakse!)
- Konteineri sulgemiseks: `exit` või Ctrl+D. Uuesti käivitamiseks kasuta
  sama `docker run` käsku (volume `tro029-ros2-ws` säilib).

**Kontroll — kas ROS 2 töötab?**

```bash
ros2 -h
ros2 pkg list
```

Kui käsud töötavad, on keskkond korras.

### Valikuline: graafiline liides (rviz2, rqt_graph, turtlesim)

Docker Desktopil pole vaikimisi GUI väljundit. Kui tahad kasutada
graafilisi tööriistu (nt nädal 05–06 rqt_graph boonusülesanne), pead
seadistama X11 forwardingu:

- **Linux:** `xhost +local:docker` enne `docker run`-i, lisa käsule
  `-e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix`.
- **macOS:** paigalda [XQuartz](https://www.xquartz.org/), luba
  "Allow connections from network clients", lisa käsule
  `-e DISPLAY=host.docker.internal:0`.
- **Windows:** paigalda [VcXsrv](https://sourceforge.net/projects/vcxsrv/)
  või sarnane X server, käivita "Disable access control" valikuga, lisa
  käsule `-e DISPLAY=host.docker.internal:0`.

See on **valikuline** — kõik kohustuslikud ülesanded on tehtavad puhtalt
käsurealt (`ros2 node list`, `ros2 topic echo` jms), ilma GUI-ta.

## Esitamine ja hindamine

Selles juhendis **ei kasutata GitHub Classroomi ega automaatteste**.
Iga nädala lõpus:

1. Veendu, et lahendus töötab sinu konteineris vastavalt selle nädala
   README "Kontrollpunktid" jaotisele.
2. Paki oma `ros2_ws/src/<sinu_paketid>` (ja muud nõutud failid, nt
   nädal 03-04 puhul `harjutus/` kaust) ZIP-failiks.
3. Lae ZIP üles Moodle'isse selle nädala ülesande juurde.
4. Õppejõud kontrollib esitust käsitsi, samade kriteeriumide alusel,
   mida kasutatakse ka auditoorsete tudengite (tro029-vm) juures.

## Abi saamine

- **Tehnilised probleemid Dockeriga:** kirjuta õppejõule.
- **Ülesannete sisu küsimused:** vaata esmalt vastava nädala `README.md`,
  seejärel küsi õppejõult.
