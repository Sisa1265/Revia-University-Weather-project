--- 
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

## 🎯 Čo rieši (účel, cieľ)

Táto časť opisuje proces výberu vhodného hardvéru pre IoT meteostanicu.  
Cieľom je pochopiť, prečo bol zvolený konkrétny hardvér, aké má vlastnosti a aké problémy môže používateľ očakávať.

## 🧩 Ako to rieši (princíp)

Riešenie spočíva v porovnaní dostupných možností a výbere komponentov, ktoré spolu spoľahlivo fungujú.  
Dôležité faktory: kompatibilita, výkon, spotreba, rozšíriteľnosť a podpora ovládačov.

## 🧪 Ako to použiť (aplikácia)

Poznatky z tejto časti umožnia rýchlo a správne vybrať hardvér pre malé IoT projekty.  
Pomáhajú predísť problémom ako nefunkčný displej, nekompatibilné ovládače alebo slabý zdroj.

---

## ⚡ Rýchly návod (Top)

1. Použi Raspberry Pi Zero 2 W pre malé IoT projekty.  
2. Vyber SPI displej, ktorý má funkčné ovládače pre Raspberry Pi OS 32-bit.  
3. Pri Waveshare LCD zvoliť OS: Raspberry Pi OS Lite 32-bit (nie 64-bit).  
4. overiť pinout a umiestnenie Pin 1.  
5. Pripraviť napájanie, SD kartu a základné príslušenstvo.

---

## 📜 Detailný článok

### 1. Raspberry Pi Zero 2 W – vhodný základ pre IoT

Raspberry Pi Zero 2 W ponúka dobrý výkon, nízku cenu a malú spotrebu.  
Má 40-pinový GPIO header a podporu Wi-Fi, čo je vhodné pre projekty ako meteostanica.

### 2. Waveshare 4" RPi LCD (A) – SPI displej

Tento displej používa SPI rozhranie.  
Vyžaduje špeciálne ovládače (overlays) a nefunguje s 64-bit OS.  
Je energeticky úsporný a vhodný na jednoduché grafické rozhrania.

### 3. SD karta a napájanie

Použitá microSD karta: 32 GB, Class 10.  
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

---

## 💡 Tipy a poznámky

- Pri výbere displeja vždy over kompatibilitu s verziou OS.  
- SPI displeje sú pomalšie než HDMI – vhodné na statické UI, nie animácie.  
- Zero 2 W má obmedzený výkon → preto je najvhodnejší OS Lite (bez grafického desktopu).  
- Najčastejšia chyba pri zapojení je zlé zarovnanie Pin 1.

---

## ✅ Hodnota / Zhrnutie

Táto KC sumarizuje najdôležitejšie poznatky o výbere hardvéru pre IoT projekt.  
Obsahuje praktické skúsenosti a riešenia problémov, ktoré vznikli pri používaní SPI displeja s Raspberry Pi Zero 2 W.  
Tieto informácie sú hodnotné pre každého, kto chce pracovať s GPIO, displejmi alebo IoT platformami.

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

