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

### Gem sketchen
>**NB!** Der er et problem med at tilgå filer via Processings IDE. Vinduet glitcher og bliver ikke tegnet rigtigt. Du kan stadig gemme ved at bruge <kbd>ENTER</kbd> mens det glitchede vindue er åbent, men sketchen vil gemmes med et autogeneret navn, fx *sketch_260725a*, som du kan omdøbe senere.

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
Herunder er der to løsninger på at åbne din sketch når din Raspberry Pi booter. Raspberry Pi OS Trixie bruger en service til at tegne programvinduer, cursoren osv på skærmen. Det kaldes en en *window manager*, og som udgangspunkt bruges *Wayland*. Selvom Wayland skulle være mere effektiv og moderne, har den også skabt problemer som ikke er løst endnu. Derfor skal vi skifte til at bruge den ældre window manager *X11* istedet, så vi kan bruge ældre løsninger på forskellige problemer. 

### Skift fra Wayland til X11

```
sudo raspi-config
```

*Advanced Options > Wayland > W1 X11*

Reboot.

Du kan tjekke om du er skiftet til X11:
```
echo $XDG_SESSION_TYPE
```
Dette bør give resultatet  *X11*

### Rediger autostart
Window manageren har indflydelse på hvordan vi kan automatisere at åbne en proces ved boot. Under Wayland kan man bruge processen ```cron```, men med X11 kan vi i stedet rette tekstfilen *autostart*.

```
sudo nano /etc/xdg/lxsession/rpd-x/autostart
````

Filen har allerede nogle processer skrevet ind, disse skal forblive uændrede:

```
@lxpanel-pi
@pcmanfm-pi
@xscreensaver -no-splash
````

For at tilføje din sketch skal du indsætte linjen

```
@ /snap/bin/processing cli --sketch=/home/BRUGERNAVN/sketchbook/PROCESSINGSKETCH --run
```

Det er den samme linje

## Problem: Skjul cursoren
Selv i fuldskærm vil din sketch stadig vise musens ikon oven på grafik. Det er lidt uhensigtsmæssigt ved en fast installation.

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