

## Installer Raspberry Pi OS
Raspberry Pi OS er en Linux distribution baseret på Debian. Tidligere hed det Raspbian. <br>

Raspberry Pi OS udkommer med en ny version med et par års mellemrum. Den seneste hedder <i>Trixie</i>. Før den kom <i>Bookworm</i> og <i>Bullseye</i>. De ældre systemer bruges stadig, især fordi Trixie kom med en masse ændringer i hvordan systemet fx viser et programvindue, hvilket påvirker hvordan apps fungerer på styresystemet.

### Installer Raspberry Pi Imager

Styresystemet skal installeres direkte på MicroSD kortet som sidder i Raspberry Pi'en. Man skal hente <i>Raspberry Pi Imager</i> til både Windows og MacOS, og bruge dette program til at <i>flashe</i> sit MicroSD kort med den rigtige version af Raspberry Pi OS.
Der er mange muligheder for at vælge forskellige systemer til sin Raspberry Pi; fx en uden Desktop (altså uden grafisk brugerflade) eller et styresystem som er specifikt til fx et mediecenter. Raspberry Pi Imager gør det hele ret gnidningsfrit. Vi skal bruge følgende indstillinger: <br>

**Device:** Raspberry Pi 4 <br>
**OS:** Raspberry Pi OS (64-bit) <br>
**Storage:** MicroSD kortet (kan fx hedde SDHC Card) <br>
**Customisation:** Her kan du præ-indstille styresystemet. Alt du ændrer her, kan du også gøre efter installationen, men det er lidt nemmere at gøre det før.
- **Hostname:** Navnet på din Raspberry Pi. Vælg noget kort som fx <i>rpi</i>; du må kun bruge små (engelske) bogstaver, tal og bindestreger.<br>
- **Localisation:** Sæt tiden til København, keyboard layout til *dk*
- **User** Vælg dit brugernavn, fx initialer. Samme regler som for hostname.
- **Password** Vælg kodeord - eller lad være!
- **Choose WI-FI:** Hvis du kan det rigtige navn og kodeord til det netværk din Pi skal logge på, kan du skrive det her. For nu; spring over
- **Remote Access / SSH Configuration** SSH er en protokol til at styre computere via netværk. Spring over.
- **Raspberry Pi Connect** Raspberry Pi's egen protokol for at styre computere via Internet. Sikkert smart; spring over.<br>

**Writing:** Sidste tjek af dine valg, tryk *Write* og godkend.

### Boot Raspberry Pi
Med en slukket Pi: indsæt MicroSD kortet, tilslut strøm.<br>

### Log på netværk
Klik netværksikonet i menuen øverst til højre og log på netværket. OBS: Måske driller dit keyboard layout og du kan ikke skrive de rigtige tegn. Dette kan ændres i *Menu - Preferences - Control Centre - Keyboard - Set Layout*

Åbn *Terminal* og opdater systemet med de seneste ændringer:
<code> sudo apt update </code> opdaterer *apt*'s lister over packages <br>
<code> sudo apt upgrade </code> installerer seneste ændringer i packages <br>

## Processing
 
### Installer snap og snapd (package managers for Processing)
Der er flere måder at installere Processing IDE på. Den hurtige er at bruge Snapcraft, en package manager fra Canonical. Ja, du skal bruge *apt* til at installere *snap* til at installere *Processing*. Velkommen til Linux, hvor alle har en fiks løsning. <br>
<code>sudo apt install snap </code><br>
<code>sudo apt install snapd </code>

### Installer Processing
<code> sudo snap install processing --classic </code><br>

### Kør Processing fra terminal <br>
<code>/snap/bin/processing cli --sketch=/home/USERNAME/sketchbook/circle --run </code>

Udskift USERNAME med dit brugernavn.

### Åbn Processing automatisk ved boot
*cron* er en indbygget service på Linux systemer der kan starte processer når computeren tænder. *cron* bruger et *cron table*, en tekstfil, til at finde ud af hvad brugeren ønsker der skal ske.

<code>crontab -e</code> åbn tekstfilen i Terminalen

Første gang du bruger denne kommando vil Terminalen spørge hvilken *editor* du vil bruge. Vælg **1** og godkend, for at bruge *nano*, en super simpel tekst editor direkte i Terminalen.

tilføj linje:<br>
<code>@reboot sleep 15 \&\& DISPLAY=:0 XDG\_RUNTIME\_DIR=run/user/1000 /snap/bin/processing cli --sketch=/home/mjn/sketchbook/circle --run</code>

*CTRL+S* for at gemme <br>
*CTRL+X* for at lukke *nano*

*Forklaring af linjen*:<br>

<code> @reboot</code> når computeren tænder<br>
<code> sleep 15</code> vent 15 sekunder<br>
<code> &&</code> tilføj endnu en kommando<br>
<code> DISPLAY=:0 </code> åbn program på display 0 <br>
<code> XDG_RUNTIME_DIR=run/user/1000 </code> indstillinger der sørger for at computeren må køre de næste kommandoer<br>
<code> /snap/bin/processing cli</code> brug processing i command line interface mode <br>
<code> --sketch=/home/mjn/sketchbook/circle --run </code> åbn sketchen <br>

mjn	

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