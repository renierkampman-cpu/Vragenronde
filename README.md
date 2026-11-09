# Anonieme vragenronde (webversie)

Live, anonieme meerkeuzepeiling voor een teamoverleg (± 20 collega's). Je hoeft niets te
installeren: alles werkt in de browser.

- **Groot scherm:** <https://renierkampman-cpu.github.io/vragenronde/>
- **Collega's** scannen de QR-code op het scherm. Eén scan is genoeg voor de hele sessie.

## Zo gebruik je het

1. Open op de presentatiecomputer **<https://renierkampman-cpu.github.io/vragenronde/>**.
   - De eerste keer wordt automatisch een eigen sessie aangemaakt, met een geheime
     beheersleutel in het adres (`#beheer=...`).
   - Deze browser onthoudt de sessie. Open je later het hoofdadres opnieuw, dan ga je verder
     waar je was, met dezelfde QR-code en dezelfde tellingen.
   - Andere computer of browser? Gebruik dan **jouw presentatorlink**. Die staat onder
     *Vragen beheren*: kopieer hem en bewaar hem als bladwijzer. **Deel die link niet.**
2. Collega's scannen de QR-code. Hun link bevat alleen de sessiecode, niet de beheersleutel.
3. Gebruik **◀ Vorige / Volgende ▶** (of de pijltjestoetsen) om van vraag te wisselen. Alle
   telefoons volgen automatisch, binnen een paar seconden, zonder opnieuw te scannen.
4. Iemand tikt een antwoord aan → het staafdiagram (percentage + aantal + totaal) werkt
   meteen bij. De telefoon toont *"Bedankt! … Wacht op de volgende vraag"*.
5. Daarnaast heb je deze knoppen:
   - **Vragen beheren**: vragen toevoegen, bewerken of verwijderen (2 tot 5 antwoordopties per
     vraag) en een vraag aanklikken om hem live te zetten.
   - **Antwoorden deze vraag wissen** / **Alle antwoorden wissen**.
   - **Standaardvragen herstellen**.
   - **Nieuwe sessie**: geeft een nieuwe QR-code, en oude links doen dan niet meer mee.
   - **QR verbergen** en **Volledig scherm**.

### Standaardvragen

1. Wat neem je mee uit dit overleg? Nieuwe informatie / Afstemming met collega's / Beslissingen / Weinig / Anders
2. Wat mis je eraan? Interactie / Duidelijke agenda / Diepgang / Niets / Anders
3. Zou je het missen als het wegviel? Ja, zeker / Een beetje / Neutraal / Nee
4. Haal je iets uit het bijzonderhedenrondje aan het einde van het overleg? A: ja, ik hoor er vaak iets nuttigs / B: soms / C: zelden / D: nee, het is vooral routine
5. Moeten wij doorgaan met het BT-overleg (binnenteamoverleg)? A: ja / B: nee

## Hoe werkt het technisch?

- De pagina is een statische website op GitHub Pages: `index.html` plus de QR-bibliotheek
  `qrcode.js` (qrcode-generator, MIT).
- Voor de live-verbinding gebruikt de pagina de gratis, openbare berichtendienst
  **[ntfy.sh](https://ntfy.sh)**. Er is geen account nodig. Per sessie zijn er twee
  willekeurige, onraadbare "topics":
  - het grote scherm publiceert de **actieve vraag**;
  - telefoons publiceren hun **antwoord**: alleen vraag-ID, optienummer en een willekeurig
    stem-ID per vraag.
- De vraag die het grote scherm stuurt is **digitaal ondertekend** (ECDSA). Telefoons tonen
  alleen vragen met een geldige handtekening van de presentator. Iemand die de collegalink kent,
  kan dus niet de vraag op de telefoons veranderen.
- De tellingen worden op het grote scherm berekend en in die browser bewaard. ntfy.sh bewaart
  berichten 12 uur. Herlaad je het presentatorscherm, dan zijn de tellingen er dus nog.
- Dubbel stemmen: elke telefoon onthoudt lokaal dat een vraag beantwoord is. Opnieuw laden
  geeft dus geen tweede stem. Een tweede tik of netwerkherhaling met hetzelfde stem-ID telt
  één keer.

## Privacy en beperkingen (eerlijk)

- **Anoniem voor jou en collega's:** geen naam, e-mail of inlog. Er wordt geen vast
  toestel-ID meegestuurd, en elk antwoord krijgt een los, willekeurig stem-ID.
- **Externe dienst:** antwoorden gaan via ntfy.sh, een openbare dienst van een derde partij.
  ntfy.sh ziet de (anonieme) antwoordberichten en, zoals elke website, de IP-adressen. Wie de
  sessiecode kent (iedereen met de QR-link), kan de antwoordberichten in principe ook lezen.
  Gebruik dit dus niet voor gevoelige of persoonsgegevens.
- **Stemmen kunnen gemanipuleerd worden** door iemand met de link die moeite doet (bijvoorbeeld
  een tweede browser of zelf berichten sturen). Voor een informele teampeiling is dat
  acceptabel.
- **Limieten van ntfy.sh (gratis, per IP-adres):** 250 berichten per dag, 60 verzoeken direct
  en daarna 1 per 5 seconden, en max. 30 open verbindingen. Zitten alle telefoons op hetzelfde
  kantoor-wifi, dan delen ze één IP-adres. Voor ± 20 collega's en 5 vragen past dat ruim
  (± 100–150 berichten). Verwacht je veel meer deelnemers, laat dan een deel mobiele data
  gebruiken.
- **Netwerk:** sommige bedrijfs- of gastnetwerken blokkeren ntfy.sh. De telefoon toont dan
  *"Geen verbinding"*. Oplossing: mobiele data. Blokkeert de werk-pc het, dan staat er onderin
  het presentatorscherm een melding.
- Vragen en tellingen staan in de browser van het grote scherm (localStorage). Een andere
  computer met jouw presentatorlink begint met de standaardvragen, en de tellingen van de
  standaardvragen worden dan (binnen 12 uur) weer uit ntfy.sh opgehaald.
- Werkt in recente versies van Chrome, Edge, Safari en Firefox.
