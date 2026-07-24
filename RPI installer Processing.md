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