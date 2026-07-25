

## Installer Raspberry Pi OS
Raspberry Pi OS er en Linux distribution baseret på Debian. Tidligere hed det Raspbian. <br>

Raspberry Pi OS udkommer med en ny version med et par års mellemrum. Den seneste hedder <i>Trixie</i>. Før den kom <i>Bookworm</i> og <i>Bullseye</i>. De ældre systemer bruges stadig, især fordi Trixie kom med en masse ændringer i hvordan systemet fx viser et programvindue, hvilket påvirker hvordan apps fungerer på styresystemet.

### Installer Raspberry Pi Imager

https://www.raspberrypi.com/software/

Styresystemet skal installeres direkte på MicroSD kortet som sidder i Raspberry Pi'en. Man skal hente <i>Raspberry Pi Imager</i> til både Windows og MacOS, og bruge dette program til at <i>flashe</i> sit MicroSD kort med den rigtige version af Raspberry Pi OS.
Der er mange muligheder for at vælge forskellige systemer til sin Raspberry Pi; fx en uden Desktop (altså uden grafisk brugerflade) eller et styresystem som er specifikt til fx et mediecenter. Raspberry Pi Imager gør det hele ret gnidningsfrit. Vi skal bruge følgende indstillinger: <br>

**Device:** Raspberry Pi 4 <br>
**OS:** Raspberry Pi OS (64-bit) <br>
**Storage:** MicroSD kortet (kan fx hedde SDHC Card) <br>
**Customisation:** Her kan du præ-indstille styresystemet. Alt du ændrer her, kan du også gøre efter installationen, men det er lidt nemmere at gøre det før.
- **Hostname:** Navnet på din Raspberry Pi. Vælg noget kort som fx <i>rpi</i>; du må kun bruge små (engelske) bogstaver, tal og bindestreger.<br>
- **Localisation:** Sæt tiden til København, keyboard layout til *dk*
- **User** Vælg dit brugernavn, fx initialer. Samme regler som for hostname.
- **Password** Vælg et kodeord
- **Choose WI-FI:** Hvis du kan det rigtige navn og kodeord til det netværk din Pi skal logge på, kan du skrive det her. For nu; spring over
- **Remote Access / SSH Configuration** SSH er en protokol til at styre computere via netværk. Spring over.
- **Raspberry Pi Connect** Raspberry Pi's egen protokol for at styre computere via Internet. Sikkert smart; spring over.<br>

**Writing:** Sidste tjek af dine valg, tryk *Write* og godkend.

### Boot Raspberry Pi
Med en slukket Pi: indsæt MicroSD kortet, tilslut strøm.<br>

### Log på netværk
Klik netværksikonet i menuen øverst til højre og log på netværket. OBS: Måske driller dit keyboard layout og du kan ikke skrive de rigtige tegn. Dette kan ændres i *Menu - Preferences - Control Centre - Keyboard - Set Layout*

Åbn *Terminal* og opdater systemet med de seneste ændringer:<br>
```
sudo apt update 
sudo apt upgrade
```