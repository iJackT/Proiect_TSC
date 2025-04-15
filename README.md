OpenBook E-book Reader

Urmatorul fisier contine detalii despre implmentarea proiectului

Descriere generala
OpenBook este un e-reader open-source construit pe baza unui controller ESP32-C6. Acesta se foloseste de un ecran de 7.5" dar nu numai.

Diagrama Bloc:

![image](https://github.com/user-attachments/assets/37a9a138-f1be-4408-9418-5c64a5490471)


Componente importante:

Microcontroller – ESP32-C6-WROOM-1
CPU: RISC-V pe 32 de biți, frecvență de 160 MHz
Memorie: 512 KB SRAM + 8 MB flash extern (model W25Q512JVEIQ)
Conectivitate: Suportă Wi-Fi 6 (802.11ax), Bluetooth 5 Low Energy, USB 2.0 Full-Speed


Ecran E-Ink – Waveshare WSH-13187, 7.5”
Rezoluție afișaj: 800x480 pixeli, alb-negru
Comunicare: Interfață SPI cu fire dedicate CS, DC, RST, BUSY
Consum de curent: Între 15–25 mA la reîmprospătare, consum nul în stare statică


Senzor ambiental – Bosch BME688
Măsurători: Temperatură, umiditate, presiune, calitate aer (eCO2, TVOC)
Precizie: ±1 °C (temperatură), ±3% (umiditate), ±1 hPa (presiune)
Comunicare: Protocol I²C @ 400 kHz


Alimentare și gestiunea energiei
Baterie: Li-Po 3.7V, capacitate 2500 mAh
Monitorizare baterie: MAX17048 (interfață I²C)
Încărcare: MCP73831, input 5V/1A prin conector USB-C


Interfață butoane – 3x switch tactil
Tip: Panasonic EVQ-Q2A03W
Legătură: GPIO cu debounce hardware (filtru RC)
Utilizare: Navigare în meniu, derulare pagini, confirmare acțiuni


Conector USB-C
Funcții: Încărcare baterie și transfer de date prin USB 2.0
Protecții: ESD și rezistențe de terminare corect configurate
Capabilități adiționale: Suport actualizări firmware prin USB și OTA prin Wi-Fi


Conector rapid I²C – Qwiic / STEMMA QT
Pini: VCC, GND, SDA, SCL
Rol: Permite conectarea ușoară a modulelor și senzorilor compatibili I²C


Slot MicroSD – Attend 112A-TAAR-R03
Scop: Permite stocarea externă de fișiere (cărți, jurnale, firmware)
Interfață: Conectare prin SPI


Ceas de timp real (RTC) – DS3231
Precizie: ±2 ppm
Interfață: I²C
Backup: Alimentare de rezervă prin supercapacitor

Pini folositi

| Pin ESP32-C6 | Componentă / Semnal                | De ce?                                                                 |
|--------------|-------------------------------------|------------------------------------------------------------------------|
| GPIO1        | I²C SDA (BME688, MAX17048, DS3231, Qwiic) | Linie partajată I²C – economisește pini și permite extinderea facilă |
| GPIO2        | I²C SCL (BME688, MAX17048, DS3231, Qwiic) | Clock I²C pentru toți senzorii și extensii Qwiic                     |
| GPIO5        | SPI MISO (E-Paper, Flash extern)    | Linie comună de intrare date în SPI                                   |
| GPIO6        | SPI MOSI (E-Paper, Flash extern)    | Linie de ieșire date către e-paper și flash                           |
| GPIO7        | SPI CLK                             | Semnal de clock pentru magistrala SPI                                 |
| GPIO8        | E-Paper CS                          | Selectează afișajul pe magistrala SPI                                 |
| GPIO9        | E-Paper DC (Data/Command)           | Comută între comandă și date către e-paper                            |
| GPIO10       | E-Paper RST                         | Reset hardware e-paper                                                |
| GPIO11       | E-Paper BUSY                        | Pin de stare (ocupat/liber) de la display                             |
| GPIO12       | Button #1                           | Intrare digitală pentru navigare pagină înainte                      |
| GPIO13       | Button #2                           | Intrare digitală pentru navigare pagină înapoi                       |
| GPIO14       | Button #3                           | Intrare digitală pentru selectare/opțiune                            |
| GPIO15       | MAX17048 ALERT (opțional)           | Pin de alertă nivel scăzut baterie                                    |
| GPIO16       | USB D+ (intern la USB PHY)          | Linie USB 2.0 (fixă pe ESP32-C6)                                      |
| GPIO17       | USB D- (intern la USB PHY)          | Linie USB 2.0 (fixă pe ESP32-C6)                                      |
| GPIO18       | LED Status (opțional)               | Indicator pentru activitate Wi-Fi, charging, notificări               |
| GPIO19       | SD Card CS                          | Selectare chip pentru card microSD (SPI)                              |
| GPIO20       | SD Card MISO                        | Citire date de pe card microSD                                        |
| GPIO21       | SD Card MOSI                        | Scriere date pe card microSD                                          |
| GPIO4        | SD Card CLK                         | Semnal de clock pentru card microSD (SPI)                             |


Comunicatii si Interfete

| Tip       | Componente                                          |
|-----------|-----------------------------------------------------|
| **SPI**   | E-Paper, Flash extern, SD Card (în mod SPI)         |
| **I²C**   | BME688, MAX17048, DS3231, Qwiic/Stemma              |
| **USB**   | Încărcare și transfer date prin USB-C               |
| **GPIO**  | Butoane, semnale control afișaj, charging enable    |
