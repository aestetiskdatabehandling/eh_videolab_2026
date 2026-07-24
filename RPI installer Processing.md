# Processing
 

## Installation af Processing IDE
### Installer snap og snapd (package managers for Processing)
Der er flere måder at installere Processing IDE på. Den hurtige er at bruge Snapcraft, en package manager fra Canonical. Ja, du skal bruge *apt* til at installere *snap* til at installere *Processing*. Velkommen til Linux, hvor alle har en fiks løsning. <br>
```
sudo apt install snap
sudo apt install snapd
```

### Installer Processing via snap

>NB! Der er problemer med den seneste udgave af Processing (version 4.5.6) og librariet TheMidiBus. En del af løsningen er at installere en ældre udgave af Processing, version 4.4.4

#### Download Processing 4.4.4 fra GitHub
Brug ```wget``` til at downloade den rigtige udgave af Processing til ```/Downloads```

```
wget https://github.com/processing/processing4/releases/download/processing-1304-4.4.4/processing-4.4.4-linux-aarch64.snap
```

Herefter navigerer du til ```/Downloads``` og installerer processing fra den fil du har hentet.

```
cd Downloads
sudo snap install --dangerous processing-1304-4.4.4/processing-4.4.4-linux-aarch64.snap --classic
```

```--dangerous``` = vi vil installere noget der ikke er en del af den seneste officielle *snap package* <br>
```--classic```= vi vil installere et program med alle tilladelser til systemet

#### Hvis fremtiden er lysere
Hvis det skulle ske at problemerne med TheMidibus bliver løst i en fremtidig udgave af Processing, kan du hente den seneste udgave af Processing med ```snap``` i stedet for alt bøvlet ovenfor.

```
sudo snap install processing --classic
```

## Lav en Processing sketch
Åbn Processing IDE og skriv følgende som en test:
```
void settings() {
	fullScreen();
}

void setup() {
	rectMode(CENTER);
}

void draw() {
	background(0);
	fill(255);
	translate(width/2, height/2);
	rotate(millis()*0.01);
	rect(0, 0, 200,200);
}
```

Gem din Processing sketch med et navn du kan huske. Sketchen vil blive gemt i ```/home/BRUGERNAVN/sketchbook/PROCESSINGSKETCH``` hvor BRUGERNAVN er dit brugernavn og PROCESSINGSKETCH er den titel du har givet din sketch.

Processing har en filstruktur hvor hver sketch gemmes i en mappe med samme navn. Filen og mappen den ligger i skal have samme navn, fx:
```
/home/BRUGERNAVN
└── /sketchbook
  └── /test
    └── test.pde
```

Du skal omdøbe både mappen og filen, hvis du ønsker at kalde din sketch noget andet.

### Kør Processing fra terminal <br>
Du kan køre din sketch via interfacet i Processing IDE, men det er nyttigt at vide hvordan man kan køre den via Terminalen.

```
/snap/bin/processing cli --sketch=/home/BRUGERNAVN/sketchbook/PROCESSINGSKETCH --run
```

### Åbn Processing sketch automatisk
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

## Problem: Skjul cursoren
Selv i fuldskærm vil din sketch stadig vise musens ikon oven på grafik. Det er lidt uhensigtsmæssigt ved en fast installation.

Problemet er at Raspberry Pi OS Trixie bruger *Wayland* som *window manager*, servicen der tegner vinduer på skærmen. Men der er endnu ikke udviklet en let måde at skjule musen på i Wayland. Derfor skal vi skifte til at bruge den ældre window manager *X11* istedet, før vi kan bruge programmet unclutter til at skjule musen.

### Skift fra Wayland til X11

```
sudo raspi-config
```

*Advanced Options > Wayland > W1 X11*

Reboot.

### Installér unclutter
```unclutter``` er en proces der kan fjerne uønsket grafik fra interfacet, fx cursoren.

```
sudo apt install unclutter
```

### Kør unclutter ved boot

```
sudo nano /etc/xdg/lxsession/rpd-x/autostart
````

tilføj følgende linje i dokumentet via  ```nano```:<br> 
```@unclutter -idle 0.1 -root```

<kbd>CTRL+S</kbd> for at gemme <br>
<kbd>CTRL+X</kbd> for at lukke ```nano```