
id: "" 

guid: "" 

dao: "knife" 

title: "Knowledge Contribution" 

author: "Silvia Kuchtová, Rene Bukovina" 

category: "deliverable" 

type: "knowledge-contribution" 

priority: "" 

tags: [] 

slug: "" 

created: "2025-09-21" 

modified: "" 

status: "draft" 

# ⚖️ IP rights 

rights_holder_content: "" 

rights_holder_system: "Roman Kazička (CAA/KNIFE/LetItGrow)" 

license: "CC-BY-NC-SA-4.0" 

disclaimer: "Use at your own risk. Methods provided as-is; participation is voluntary and context-aware."

locale: "sk" 

---


# Knowledge Contribution  
## Výber hardvéru a pochopenie komponentov
Výber vhodného hardvéru predstavoval prvý zásadný krok v rámci projektu meteorologickej stanice s Raspberry Pi Zero 2 W. Hoci sa na začiatku môže zdať, že ide iba o kúpu „malého počítača a displeja“, v skutočnosti práve táto fáza rozhoduje o tom, či bude projekt stabilný, kompatibilný a ľahko rozšíriteľný. V tejto článkovej časti opisujem, ako som si prešla výberom vhodných komponentov, narazila na prvé problémy, pochopila limity hardvéru a postupne si osvojila základné technické súvislosti.

## 🎯 Čo rieši (účel, cieľ)

Cieľom tejto časti bolo:
- porozumieť jednotlivým komponentom (mikropočítač, displej, napájanie)
- zvoliť vhodnú kombináciu, ktorá bude kompatibilná a energeticky nenáročná
- pripraviť si stabilný základ pre neskorší softvérový vývoj
Výber hardvéru je v podobných projektoch často podceňovaný, ale je to jedna z najdôležitejších častí — rozhoduje o tom, či vôbec dokážeme projekt dokončiť.

## 🧩 Ako to riešiť (princíp)

### 1. Raspberry Pi Zero 2 W – vhodný základ pre IoT

Raspberry Pi Zero 2 W ponúka dobrý výkon, nízku cenu a malú spotrebu.  
Má 40-pinový GPIO header a podporu Wi-Fi, čo je vhodné pre projekty ako meteostanica.

### 2. Waveshare 4" RPi LCD (A) – SPI displej

Tento displej používa SPI rozhranie.  
Vyžaduje špeciálne ovládače (overlays) a nefunguje s 64-bit OS.  
Je energeticky úsporný a vhodný na jednoduché grafické rozhrania.

### 3. SD karta a napájanie

Použitá microSD karta: 256 GB, ale stači aj oveľa menšia.  
Stabilné napájanie bolo dôležité najmä pri konfigurácii displeja.

### 4. Pinout a správne zapojenie

Najdôležitejšie bolo zistiť, kde presne je Pin 1 na Raspberry Pi aj na displeji.  
SPI displej používa piny:  
- 5V  
- GND  
- MOSI  
- MISO  
- SCK  
- CS  
- IRQ (dotyková vrstva)

### 5. Prečo 64-bit OS nefunguje

SPI drivery Waveshare nemajú podporu pre 64-bit Raspberry Pi OS.  
Displej sa síce rozsvieti, ale systém nevytvorí `/dev/fb0`, takže sa nedá použiť.  
Riešenie: použiť **Raspberry Pi OS Lite 32-bit**.

## 🧪 Ako to použiť (aplikácia)

Tieto znalosti sú kľúčové pre:
- budovanie vlastných IoT zariadení
- pochopenie low-level komunikácie (GPIO, SPI, framebuffer)
- správny výber komponentov pre projekty
- diagnostiku najčastejších problémov pri práci s Raspberry Pi
- vytváranie malých low-power zariadení fungujúcich bez monitora

Celý projekt stojí na tom, či rozumieme, ako hardvér spolupracuje.

Až po pochopení týchto základov sa dá efektívne pokračovať:

- programovať aplikáciu
- kresliť grafiku do framebuffera
- čítať senzory počasia
- zobrazovať údaje na displeji

---

## ⚡ Rýchly návod (Top)

1. Použi Raspberry Pi OS (32-bit).
2. Zapoj displej na prvých 26 GPIO pinov (piny 1–26).
3. Zapni SPI: sudo raspi-config.
4. Spusti ovládač displeja:
     cd ~/LCD-show
     sudo ./LCD4-show


Reštartuj a skontroluj /dev/fb0.

---

## 📜 Detailný článok

#### 1. Výber hlavného mikropočítača – Raspberry Pi Zero 2 W
Zero 2 W je ideálna voľba pre malé projekty, najmä kvôli nízkej spotrebe energie a kompaktným rozmerom. Pri výbere som si musela uvedomiť:
- má len 512 MB RAM, čo obmedzuje beh niektorých aplikácií
- používa GPIO 40-pinový header, zdieľaný s ostatným hardvérom
- Wi-Fi funguje len na 2.4 GHz, takže je citlivejší na kvalitu signálu
- nemá štandardné USB porty — vyžaduje OTG adaptér
- nemá HDMI, má iba mini HDMI
Tieto limity ovplyvnili aj výber displeja a senzorov, aby ich bolo možné cez GPIO alebo SPI zapojiť.

#### 2. Výber displeja – Waveshare 4inch RPi LCD (A)
Toto bol pravdepodobne najväčší zdroj problémov.
Display:
- komunikuje cez SPI, takže musí využívať konkrétne GPIO piny
- obsahuje vlastný driver (ili9486), ktorý nie je natívnou súčasťou Raspberry OS
- vyžaduje inštaláciu ovládača pomocou skriptov Waveshare (LCD-show)
- potrebuje presné nastavenie v config.txt pomocou dtoverlay
- obsahuje XPT2046 touch controller, ktorý tiež používa SPI
Najväčšou výzvou bolo pochopiť, ako displej presne komunikuje a ako sa aktivuje framebuffer /dev/fb0, ktorý zabezpečuje zobrazovanie grafiky.

Kým HDMI displeje fungujú okamžite, SPI displeje si vyžadujú:
- modifikáciu kernelu
- aktiváciu SPI rozhrania
- vloženie správneho device tree overlay
- premapovanie framebufferu
Toto všetko som musela pochopiť, aby som dokázala displej „oživiť“.

#### 3. Zapojenie displeja cez GPIO
Aj keď má Pi Zero 2 W 40-pinový header, displej využíva iba prvých 26 pinov. Najskôr mi bolo nejasné, kde sa nachádza PIN 1, pretože orientácia závisí od fyzického natočenia.
Pre Waveshare LCD platí:
- PIN 1 je v pravom dolnom rohu, keď je text otočený správne
- Pi Zero 2 W má PIN 1 pri hranatom rohu dosky, vedľa SD slotu
Až po správnom zasunutí celého 26-pinového konektora displeja na prvých 26 GPIO pinov Raspberry Pi sa SPI rozhranie aktivovalo.

#### 4. SD karta a operačný systém
Výber OS bol ďalšou dôležitou súčasťou.
Na Pi Zero 2 W nefunguje 64-bit Raspberry Pi OS, pretože displejové drivery Waveshare nepodporujú ARM64.
Musela som teda použiť Raspberry Pi OS (32-bit).
Do toho bolo potrebné pri bootovaní pridať Wi-Fi údaje, SSH, používateľa, lokalizáciu.
Pri nesprávnom OS displej vôbec nezapne SPI driver, takže projekt nepokročí.

#### 5. Napájanie a stabilita
Pi Zero 2 W je citlivé na nekvalitné napájanie — ak je napätie slabé:
- zasekáva sa SPI komunikácia
- displej nesvieti alebo „blika“
- Wi-Fi občas vypadáva
- systém sa spontánne reštartuje
Po testovaní som zistila, že najspoľahlivejšie je napájanie cez adaptér 5V / 2A.


---

## 💡 Tipy a poznámky

- Pri SPI displejoch vždy kontroluj config.txt.
- Display nepotrebuje HDMI, zobrazovanie funguje cez framebuffer.
- Ak /dev/fb0 neexistuje, driver sa nenačítal.
- Vždy používaj dostatočne silný adaptér (min. 2A).
- Pi Zero 2 W je výkonovo slabší, takže optimalizácia aplikácie je dôležitá.

---

## ✅ Hodnota / Zhrnutie

Výber hardvéru nie je len o kúpe komponentov — je to proces, ktorý si vyžaduje pochopenie elektrickej kompatibility, spôsobu komunikácie a softvérových obmedzení. V tomto projekte som si prešla chybami, slepými uličkami aj viacerými opravami konfigurácie, ale presne tieto skúsenosti vytvorili pevný základ pre ďalšie etapy projektu.

---

# 📚 Knowledge Contribution

## 🔖 Názov a stručný popis

**Výber hardvéru pre IoT meteostanicu** – stručná analýza hardvéru použitá v projekte, dôvody výberu a skúsenosti s kompatibilitou.

## 🗂️ Taxonómia KNIFE

- **Kategória:** IT / IoT / Hardware  
- **Typ:** návod + prípadová štúdia  
- **Tagy:** raspberry pi, spi, waveshare, meteostanica, hardware

## 📜 Obsah

- Výber Raspberry Pi Zero 2 W  
- Výber a zapojenie Waveshare 4" SPI LCD  
- Problémy so 64-bit OS  
- Pochopenie pinoutu a kompatibility

## 🌍 Referencie

- https://www.waveshare.com/wiki/4inch_RPi_LCD_(A)  
- https://www.raspberrypi.com/documentation/  
- vlastné experimenty počas projektu  

