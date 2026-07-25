### Processing, MidiBus problemer
Processing 4.4.4 portable + MidiBus micycle1 jar fix virker

https://github.com/micycle1/themidibus/releases/tag/p4
https://github.com/processing/processing4/releases/tag/processing-1304-4.4.4

Installér processing 4.4.4 portable (flyt mappe til home, kør executable)
Træk .jar fil ind i IDE

#### skal testes:
skal .jar filen i <i>sketchbook/libraries/themidibus/library</i> ?<br>
virker det med processing 4.5.5?<br>
virker dette fix? https://discourse.processing.org/t/does-themidibus-library-work-in-processing-4/31851


### GPIO problemer
https://www.raspberrypi.com/documentation/computers/config_txt.html#gpio-control

https://discourse.processing.org/t/rpi-4-b-gpio-input-pullup-does-not-work/24877

https://discourse.processing.org/t/issue-with-gpio-access-on-raspberry-pi-5-using-processing-4/43337/2


## Skjul cursoren
Selv i fuldskærm vil din sketch stadig vise musens ikon. Det er lidt uhensigtsmæssigt ved en fast installation.

Problemet er at Raspberry Pi OS Trixie bruger *Wayland* som *window manager*, servicen der tegner vinduer på skærmen. Men der er endnu ikke udviklet en let måde at skjule musen på i Wayland. Derfor skal vi skifte til at bruge den ældre window manager *X11* istedet, før vi kan bruge programmet unclutter til at skjule musen.

### Skift fra Wayland til X11

Terminal: <code>sudo raspi-config</code>
*Advanced Options > Wayland > W1 X11*

Reboot.

### Installér unclutter

<code>sudo apt update && sudo apt install unclutter</code>

### Kør unclutter ved boot

<code>sudo nano /etc/xdg/lxsession/rpd-x/autostart</code><br>
tilføj: <code>@unclutter -idle 0.1 -root</code>


## Hård læring

### Brug mindst 32GB kort.
Raspberry Pi OS kan godt installeres på 16GB, men så er der næsten ingen plads til at installere andet eller lægge filer på Pi'en. Det gør den stort set ubrugelig. 16GB installationer er typisk for folk der kører *headless*, altså uden en skærm sat til.

### Brug en original Raspberry Pi USB-C oplader
Raspberry Pi 4 bruger en meget specifik strømforsyning. Du kan godt bruge en anden strømforsyning, men det kan få computeren til at yde mindre. Eller også virker det slet ikke!

### GPIO problemer i Processing
Processing har et library *Hardware IO* som burde kunne få adgang til GPIO, så man kan tilslutte sensorer og andet. Men; bibliotektet er uddateret og virker ikke i Trixie fordi der er sket en ændring i hvordan Linux-kernen tillader adgang til GPIO.
Der er ikke fundet en løsning endnu, selvom flere efterspørger dette.


### Processing-py
https://pypi.org/project/processing-py/#description

### Pi4J
https://www.pi4j.com

### cron
*cron* er en indbygget service på Linux systemer der kan starte processer når computeren tænder. *cron* bruger et *cron table*, en tekstfil, til at finde ud af hvad brugeren ønsker der skal ske.
Åbn tekstfilen i Terminalen:

```
crontab -e
```

Første gang du bruger denne kommando vil Terminalen spørge hvilken *editor* du vil bruge. Vælg **1** og godkend, for at bruge ```nano```, en indbygget super simpel tekst editor direkte i Terminalen.

tilføj linje:<br>
```
@reboot sleep 15 && DISPLAY=:0 XDG/RUNTIME/DIR=run/user/1000 /snap/bin/processing cli --sketch=/home/mjn/sketchbook/circle --run
```

<kbd>CTRL+S</kbd> for at gemme <br>
<kbd>CTRL+X</kbd> for at lukke ```nano```

Forklaring af linjerne:

```@reboot``` kør de følgende kommandoer når computeren tænder<br>
```sleep 15``` vent 15 sekunder<br>
```&&``` tilføj endnu en kommando<br>
```DISPLAY=:0 ``` åbn program på display 0 <br>
```XDG_RUNTIME_DIR=run/user/1000 ``` indstillinger der sørger for at computeren må køre de næste kommandoer<br>
```/snap/bin/processing cli``` brug Processing i command line interface mode <br>
```--sketch=/home/mjn/sketchbook/circle --run``` åbn sketchen <br>