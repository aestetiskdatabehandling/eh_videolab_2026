# Raspberry Pi OS Terminal Cheat Sheet

## Bruger og kommandoer
I Terminalen er man altid en bruger et sted på computeren. Når man starter en ny terminal, vil det se sådan ud: <br>
```
brugernavn@computernavn:~$
```

```brugernavn``` = *username*, den bruger du er logget ind som<br>
```computernavn``` = *hostname*, den computer du styrer med Terminal <br>
```~``` = forkortelse for ```/home/brugernavn```. Det betyder at Terminalens udgangspunkt er din brugers hjemmemappe<br>
```$``` = dine rettigheder som bruger. <code>$</code> betyder almindelig bruger, ```#``` betyder bruger med administratorrettigheder. <br>

Så ```brugernavn@computernavn:~$``` betyder *"du er logget ind som brugernavn på computernavn, du er i hjemmemappen og har almindelige rettigheder"*.

Når du skal lave store ændringer i systemet, fx rette systemfiler eller installere ny software, skal du gøre det med administratorrettigheder. Det gør man ved at skriver kommandoen ```sudo```, før den kommando man giver. 

>Hvad betyder ```sudo```? Der er forklaringer for enten *substitute user, do*, eller *superuser do*, men i praksis er betydningen den samme; at man tilskriver brugeren ekstra rettigheder for at gennemføre en handling.

### Processer og flags
En kommando kan se ud på mange måder, men der er elementer i de fleste kommandoer der går igen. Eksempel:<br>

```
brugernavn@computernavn:~$ vlc -fullscreen Movies/videofil.mp4
```

```vlc``` åbner processen VLC, en videoafspiller. Man ville typisk omtale VLC som et *program* eller *application*, men for computeren er den bare en af mange processer. Nogle processer gør ikke mere end at skifte mappe, andre er meget komplekse - fx VLC.<br>
```-fullscreen``` er et *flag* der fortæller VLC at videofilen skal åbnes i fuld skærm. Et flag markeres med ```-```, og er en indstilling der skal påvirke processen.<br>
```Movies/videofil.mp4``` stien til den videofil der skal åbnes.<br>

---

## Navigation
Når man navigerer i Terminalen skal man være fortrolig med de udtryk der bruges for mapper og filer.

Et *directory* er en mappe, markeret med skråstreg fx ```/mappe```<br>
En *fil* vil have sin filtype med en *extension* markeret efter et punktum, fx ```fil.txt```<br>
En *path* er en sti, addressen, til en mappe eller fil, fx ```/home/bruger/Documents/filnavn.txt.```<br>

>Terminalen kigger altid på en sti, som udgangspunkt hjemmemappen for brugeren. Det kan nogle gange være nødvendigt at vide præcis hvor en fil eller et program ligger i computerens mappestruktur.

### Kommandoer til navigation

| Kommando 		| Beskrivelse					| 
|-				|-								|
| `pwd` 		| Vis den nuværende sti til mappen |
| `ls` 			| Vis en liste af filerne i mappen |
| `ls -l` 		| Vis en liste af filerne med detaljer	|
| `ls -a` 		| Vis skjulte filer i mappen		|
| `cd FOLDER` 	| Gå til mappen FOLDER 	|
| `cd ..` 		| Gå til mappen over |
| `cd ~` 		| Gå til brugerens hjemmemappe ```/home```|
| `cd /` 		| Gå til computerens rodmappe ```/root```	|

Alt efter dit keyboard-layout er der forskellige måder at skrive de forskellige tegn på. Kig på tastaturets markeringer og prøv dig frem.

### Auto-udfyld
Terminalen kan auto-udfylde tekst med <kbd>TAB</kbd>. Det er meget nyttigt hvis du skal bruge nogle filer med lange navne. Hvis du fx går ind i ```/Documents``` og finder tre filer: <br>
```
brugernavn@computernavn:~$ cd /Documents
brugernavn@computernavn:~$ ls
supermegaherrelangtfilnavn.txt	Kringlet_filnavn.txt	supertræls_filnavn.txt
```

I stedet for at skrive <code>supermegaherrelangtfilnavn.txt</code> kan du nøjes med at skrive ```su``` og så trykke <kbd>TAB</kbd>. Så auto-udfylder Terminalen resten af filnavnet, hvis den kan finde det. <br>
Du skal skrive mindst 2 bogstaver - hvis stien starter med stort, fx *Kringlet_filnavn.txt* skal du starte med  ```Kr``` <br> 
Hvis der er to filer med ens bogstaver i starten, skal du skrive alle de ens bogstaver før Terminalen kan finde den rigtige. Fx skal du skrive ```supert``` før den skelner ```supertræls_filnavn.txt``` fra ```supermegaherrelangtfilnavn.txt`

### Absolute/relative paths
Man skelner mellem relative paths og absolute paths.

En *relative path* er en sti til en fil i den mappe du allerede er i. I eksemplet herunder finder vi vej i denne mappestruktur:
```
/home/brugernavn
└── /Documents
  ├── /nevøer
  │   ├── rip.filnavn
  │   ├── rap.filnavn
  │   └── rup.filnavn
  └── /niecer
      ├── kylle.filnavn
      ├── pylle.filnavn
      └── rylle.filnavn
```

Med ```cd``` og ```ls``` kan man navigere gennem mapper og få en liste over de filer der er i hvert niveau:
```
brugernavn@computernavn:~$ cd Documents
brugernavn@computernavn:~$ ls
nevøer	niecer
brugernavn@computernavn:~$ cd nevøer
brugernavn@computernavn:~$ ls
rip	rap	rup
```

Bemærk det ikke er nødvendigt at skrive ```cd /Documents``` men bare ```cd Documents``` fordi ```Documents``` er en relative path til ```home``` - altså en undermappe til den mappe vi allerede er i.

Hvis man allerede kender stien til den mappe man vil ind i, er det følgende hurtigere at skrive den. Det er en *absolute path*.
```
brugernavn@computernavn:~$ cd /Documents/nevøer/rip
```
Bemærk at man <u>skal</u> bruge ```/``` ved absolute paths. 
Absolute paths bruges typisk hvis man vil skifte fra en mappe til en helt urelateret mappe.

---

## Lav filer og mapper

| Command 			| Description 						| Example 	|
|-					|-									|-			|
| `mkdir DIR` 		| Lav en ny mappe DIR	| `mkdir Projects` |
| `mkdir -p A/B/C` 	| Lav en sti af mapperne A, B og C | `mkdir -p Projects/Python/Test` |
| `touch FILE.EXTENSION` 		| Create an empty file | `touch script.py` |

>Fun fact: ```touch``` bruges i dag mest til at lave nye filer. Navnet stammer fra en trick programmører brugte til at få computeren til at genindlæse en fil, selvom indholdet var uændret. ```touch``` sørger nemlig for at filens datomærkning fornys, fordi den er blevet *rørt*.
---

## Copying Files

| Command | Description | Example |
|---------|-------------|---------|
| `cp FILE1 FILE2` | Kopierer indholdet af FILE1 til FILE2 | `cp notes.txt backup.txt` |
| `cp FILE DIR` | Kopier FILE ind i mappen DIR | `cp notes.txt Documents/` |
| `cp -r DIR1 DIR2` | Kopier mappen DIR1 og dens indhold til mappen DIR2 | `cp -r Project Backup` |

>```cp``` kan som udgangspunkt ikke kopiere en tom mappe, der ville man bruge ```mkdir```. Flagget ```-r``` betyder *recursively* og indikerer at mappens indhold skal påvirkes. 
---

## Flyt og omdøb

| Command 			| Description 					| Example |
|-					|-								|-|
| `mv FILE FOLDER/` | Flyt FILE ind i FOLDER | `mv filnavn.txt Documents`|
| `mv OLD NEW` 		| Omdøb en mappe eller fil fra OLD to NEW | `mv report.txt report_old.txt` |

---

## Slet filer og mapper

| Command | Description | Example |
|- 					|- |- |
| `rm FILE` 		| Slet en fil | `rm notes.txt` |
| `rm *.txt` 		| Slet alle filer med af typen .txt | `rm *.txt` |
| `rmdir FOLDER` 	| Slet en mappe | `rmdir EmptyFolder` |
| `rm -r FOLDER` 	| Slet en mappe og dens filer | `rm -r OldProject` |
| `rm -rf FOLDER` 	| Slet en mappe og dens filer uden bekræftelse | `rm -rf temp` |

Terminalen vil som udgangspunkt ikke slette en mappe hvis der er filer i. Derfor bruger man flagget ```-r``` for at slette *recursively*, altså både mappe og indhold. 

Nogle filer er beskyttede og kræver en bekræftelse fra brugeren. Bruger man ´´´-f´´´ undlader Terminal at spørge om lov. Kombineret som ```-rf``` bliver mapper og deres indhold slettet uden forbehold.

---

## Diverse systemkommandoer

| Kommando 			| Beskrivelse 	|
|- 					|-				|
| `clear` 			| Nulstil terminalen |
| `history` 		| Vis kommando historikken |
| `man COMMAND` 	| Vis manual for proces eller kommando |
| `sudo COMMAND` 	| Kør som administrator |
| `whoami` 			| Vis nuværende bruger |
| `hostname` 		| Vis computerens navn |
| `df -h` 			| Vis hvor meget diskplads der bruges |
| `du -sh DIR` 	| Vis størrelse på mappen FOLDER |
| `free -h` 		| Vis hvor meget hukommelse, RAM, der bruges |
| 'xdg-open' FILE	| Åbn FILE i det program det typisk åbnes i |

Flagget ```-h``` betyder *show in human readable format*, altså outputtet skal gøres lettere læseligt for en almindelig bruger. <br>

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| <kbd>TAB</kbd> | Auto-udfyld stier og filnavne |
| <kbd>↑</kbd> | Vis den forrige kommando |
| <kbd>!!</kbd> | Gentag den forrige kommando |
| <kbd>Ctrl</kbd> + <kbd>C</kbd>| Stop processen |
| <kbd>Ctrl</kbd> + <kbd>D</kbd> | Luk terminalen |
| <kbd>Ctrl</kbd> + <kbd>L</kbd> | Nulstil terminalen |
| <kbd>Ctrl</kbd> + <kbd>SHIFT</kbd> + <kbd>C</kbd>| Kopier markeret tekst |
| <kbd>Ctrl</kbd> + <kbd>SHIFT</kbd> + <kbd>V</kbd> | Indsæt markeret tekst |

---