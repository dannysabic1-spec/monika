# MONIKA BOT Discord Bot — v4.4

Ovo je kompletan Discord bot sa ljubavnim sistemom, virtuelnom ekonomijom,
kućama za parove, ribolovom, rang-listama, welcome panelom i Info/GWS & Events
panelom.

## 1. Instalacija

Potrebno je:

- Python 3.10 ili noviji
- Discord bot token

U folderu gdje se nalazi `bot.py` pokreni:

```bash
python -m pip install -r requirements.txt
```

Na Windowsu možeš koristiti i:

```bash
py -m pip install -r requirements.txt
```

## 2. Postavljanje tokena

Token nemoj upisivati direktno u `bot.py`.

### Windows PowerShell

```powershell
$env:DISCORD_TOKEN="OVDJE_UNESI_DISCORD_BOT_TOKEN"
python bot.py
```

### Windows CMD

```cmd
set DISCORD_TOKEN=OVDJE_UNESI_DISCORD_BOT_TOKEN
python bot.py
```

### Linux/macOS/Replit Shell

```bash
export DISCORD_TOKEN="OVDJE_UNESI_DISCORD_BOT_TOKEN"
python bot.py
```

Na Replitu napravi Secret/Environment Variable pod imenom:

```text
DISCORD_TOKEN
```

## 3. Discord Developer Portal

U bot podešavanjima uključi:

- **Message Content Intent**
- **Server Members Intent**

Botu na serveru daj najmanje ove dozvole:

- View Channel
- Send Messages
- Embed Links
- Read Message History
- Add Reactions

Za automatsku ulogu novim članovima dodatno treba:

- Manage Roles

Botova uloga mora biti iznad uloge koju pokušava dodijeliti.

Prilikom pozivanja bota koristi scopeove:

- `bot`
- `applications.commands`

## 4. Podešavanje Welcome sistema

Na serveru kao administrator pokreni:

```text
/welcome-podesi
```

Odaberi:

- `kanal` — kanal glavne welcome poruke
- `pravila` — kanal sa pravilima
- `chat` — glavni/chat kanal
- `music` — kanal na koji vodi dugme `play music`
- `pozdrav` — kanal gdje se šalje poruka nakon klika na `Poželi dobrodošlicu`
- `uloga` — automatska uloga za novog člana

Primjer:

```text
/welcome-podesi kanal:#welcome pravila:#pravila chat:#general music:#music pozdrav:#dobrodoslice uloga:@Member
```

Zatim provjeri izgled:

```text
/welcome-test
```

`/welcome-podesi` odmah šalje test welcome embed u odabrani kanal. Prave welcome
poruke šalju se pri ulasku novih članova; za to `Server Members Intent` mora biti
uključen i u kodu i u Discord Developer Portalu. Test komanda prijavljuje dozvolu
koja nedostaje ako bot ne može slati poruke ili embede.

Pregled podešavanja:

```text
/welcome-status
```

Isključivanje:

```text
/welcome-iskljuci
```

Welcome dugme radi ovako:

- samo prva osoba može poslati dobrodošlicu za jednu welcome poruku
- nakon prvog klika dugme se deaktivira
- drugi pokušaji dobijaju objašnjenje da je dugme već iskorišteno
- iskorišteni klikovi se čuvaju u `ljubav_data.json`

Ako korisnik ima zatvorene privatne poruke, Discord ne dozvoljava botu da ih
otvori. U tom slučaju `.pomoc` prikazuje pomoć u kanalu, a `.poruka` u završnom
izvještaju odvojeno prikazuje Discord odbijene DM-ove i druge HTTP greške.

## 5. Podešavanje Info/GWS & Events panela

Podesi kanale:

```text
/info-podesi kanal:#info gws:#giveaways events:#events
```

Postavi mali panel:

```text
/info-postavi
```

Panel ima tri dugmeta:

- `Info` — pravila, informacije o serveru i načini zabave
- `GWS & Events` — giveaway i event kanali
- `Opis • sve komande` — privatan pregled svih registrovanih prefix i slash komandi

Nakon ažuriranja ponovo pokreni `/info-postavi` da objaviš panel s novim dugmetom.
Pregled podešavanja:

```text
/info-status
```

## 6. Staff prijave i Ticket sistem

Podesi kanal u koji staff dobija prijave i opcionalnu staff ulogu:

```text
/staff-podesi kanal:#staff-prijave uloga:@Staff
```

Postavi Staff panel u trenutni ili izabrani kanal:

```text
/staff-postavi
/staff-postavi kanal:#prijave
```

Korisnik klikom na `Staff Apply` dobija privatni formular. Nakon slanja,
prijava se šalje samo u podešeni staff kanal.

Podesi kategoriju za privatne tickete i staff ulogu:

```text
/ticket-podesi kategorija:TICKETS uloga:@Staff
```

Postavi Ticket panel:

```text
/ticket-postavi
/ticket-postavi kanal:#podrska
```

Klik na `Otvori Tiket` pravi privatni kanal za korisnika i staff. Jedan
korisnik može imati samo jedan otvoren tiket, a tiket se zatvara dugmetom
`Zatvori tiket`.

### Report sistem

Report je odvojen od običnog ticketa i podešava se jednom komandom:

```text
/setreport kanal:#reports kategorija:REPORTS uloga:@Staff
```

Komanda odmah šalje panel sa dugmetom `Prijavi korisnika`. Klik otvara privatni
kanal koji vide autor reporta i staff. Report se zatvara dugmetom `Zatvori report`.
Komandu vidi bot-vlasnik i server administratori; ostali članovi ne mogu je koristiti.

Svi embed-i koriste sivo-crnu dark boju (`#2B2D31`) i kompaktan prikaz bez `>` oznaka.

## 7. Slash komande

```text
/setkanal
/ukloni-kanal
/setlogin
/setpomoc
/welcome-podesi
/welcome-status
/welcome-test
/welcome-iskljuci
/info-podesi
/info-postavi
/info-status
/staff-podesi
/staff-postavi
/ticket-podesi
/ticket-postavi
/setreport
/setmonika
/iskljuci-moniku
/opis
```

Za ograničavanje komandi i mini-igara na određene kanale koristi:

```text
/setkanal ljubav #ljubav
/setkanal pecanje #pecanje
/setkanal counting #counting
/setkanal toplo_hladno #toplo-hladno
/setkanal kaladont #kaladont
/setkanal casino #casino
```

Za zaključavanje svih prefix i slash komandi na jedan kanal koristi:

```text
/setlogin kanal:#komande
```

Komanda `.pomoc` može imati poseban kanal:

```text
/setpomoc kanal:#pomoc
```

Ako se `.pomoc` upiše u drugom kanalu, bot prikaže embed sa tačnim kanalom
za pomoć.

Podešavanje je dostupno i kao prefix komanda:

```text
.setpomoc #pomoc
```

Ako kanal nije naveden, koristi se kanal u kojem je komanda pokrenuta.

`/setreport`, `/setpomoc` i `/setlogin` su vidljive bot-vlasniku i server
administratorima. Nakon izmjene, pri pokretanju konzola ispisuje broj
sinhronizovanih slash komandi i njihova imena. Ako se log ne poklapa s ovim
paketom, vjerovatno se pokreće stara kopija bota.

Mini-igre se više ne prepoznaju automatski po nazivu kanala. Skraćeni unos
bez prefiksa radi samo u kanalu koji je administrator podesio za tu igru.

Komanda `/opis` je dostupna samo vlasniku bota. Objavljuje uputstvo za članove
u sedam povezanih embed poruka: način rada, članske prefix i slash komande,
ljubavni sistem, ekonomiju, kuću, ribolov, mini-igre i rang-liste.
Svaki dio uputstva dobija malu sliku servera, odnosno sliku bota ako server
nema postavljenu ikonu, a tekst ostaje bez emoji-ja.

## 7. Monika razgovor

Administrator može uključiti Moniku u određenom tekstualnom kanalu:

```text
/setmonika
/setmonika kanal:#zabava
```

Ako kanal nije naveden, koristi se kanal u kojem je komanda pokrenuta.
Monika prati samo taj jedan kanal, ali ne odgovara na svaku poruku i ne
presreće prefix komande. Kada prepozna određeni dio zabavne riječi, najviše
jednom u 3 minute taguje sljedećeg člana po abecedi i postavlja mu pitanje.
Kada neko direktno označi Moniku, odgovara baš toj osobi uz kraći cooldown od
30 sekundi. Red, vrijeme posljednjeg odgovora i aktivni izazovi čuvaju se u
`ljubav_data.json` i nastavljaju se nakon restarta.

U istom kanalu Monika pokreće izazove **Pogodi broj od 1 do 100** i
**Pogodi riječ**. Novi broj se objavljuje svakih 60 minuta, a nova riječ
svakih 70 minuta, kako se izazovi ne bi objavili jedan za drugim. Prvi član
koji pogodi dobija 100 coin i čestitku sa tagom. Svaka runda traje dvije
minute, a svaki član ima tačno jedan pokušaj po rundi. Aktivni broj, riječ,
pobjednik i pokušaji čuvaju se u
`ljubav_data.json`, pa se stanje nastavlja nakon restarta.

Za isključivanje koristi:

```text
/iskljuci-moniku
```

Podešavanje se čuva u `ljubav_data.json` i ostaje aktivno nakon restarta bota.

## 8. Sve prefix komande

Prefix je:

```text
.
```

### Vlasnička pozivna poruka

Vlasnik bota ili servera može poslati pozivnicu u privatne poruke najviše
100 članovima po pokretanju:

```text
.poruka https://discord.gg/tvoj-link 100
```

Komandu može koristiti vlasnik bota ili vlasnik servera. Poruka sadrži pozivni
link i pregled glavnih komandi. Broj je opcionalan (podrazumijevano 100; raspon
1–100). Botovi se preskaču. Završni izvještaj razdvaja poslato, Discord odbijene
DM-ove i druge HTTP greške. Bot ne može zaobići članske postavke privatnosti.
Ako je `/setlogin` podešen, `.poruka` se mora pokrenuti u tom komandnom kanalu.

### Ljubav i veze

```text
.veza @Korisnik
.profil [@Korisnik]
.brak @Partner
.prsten [@Partner]
.svadba
.obnova
.zakletva [tekst]
.razvod
.datum
.godisnjica
.ljubomora
.razina
.statistika
.djeca
.beba
.album
```

### Ljubavni bodovi i XP

```text
.spavanje
.srce
.zagrljaj @Korisnik
.poljubac @Korisnik
.izlet
.vecera
.putovanje
```

### Zabava

```text
.casino [coin|slot] <ulog>
.counting [broj]
.toplo-hladno [broj]
.kaladont [riječ]
.ples @Korisnik
.pijte
.film
.istina
.izazov
.ljubavni-test @Korisnik
.compatibility @Korisnik
```

### Mini-igre i kanali

Prefix svih komandi je tačka (`.`):

- `.ekonomija` — pregled gotovine, banke i ekonomskih komandi
- `.casino` — `coin` ili `slot`, minimalni ulog je 10 novčića
- `.counting 1` — brojanje po kanalu; bot odgovara direktno na poruku s brojem
- `.toplo-hladno` — pogodi broj od 1 do 100
- `.kaladont` — riječ mora početi sa posljednja dva slova prethodne riječi

Stanje svake igre je odvojeno po Discord kanalu. Counting prikazuje prihvaćeni
broj, sljedeći broj i tagovanog člana u tamnom embed-u, kao odgovor na poruku s brojem.
Ne možeš brojati dva broja zaredom; pogrešan broj ili isti član resetuju rundu
na 1. Broj i posljednji član čuvaju se u `ljubav_data.json` i preživljavaju
restart. Ostali odgovori mini-igara koriste kompaktni embed stil.

Kaladont taguje člana, ispisuje riječi velikim slovima i uz odgovore naizmjenično
prikazuje dvije ugrađene slike iz ZIP paketa.

U podešenim kanalima za `counting`, `toplo-hladno`, `kaladont` ili
`casino` moguće je slati i skraćeni unos bez prefixa:

- `counting` — pošalji samo `1`, `2`, `3`...
- `toplo-hladno` — prvo pokreni `.toplo-hladno`, zatim šalji samo `44`, `55`...
- `kaladont` — prvo pokreni `.kaladont`, zatim šalji samo riječi
- `casino` — pošalji `coin 50` ili `slot 50`

Slash komande `/komande` i `/igre` se automatski registruju nakon pokretanja
bota i prikazuju pregled prefix komandi i mini-igara.

Komanda `.pomoc` jednim unosom šalje jednu DM poruku sa tri stranice pomoći:
`Pomoć 1/3`, `Pomoć 2/3` i `Pomoć 3/3`. Stranice se mijenjaju klikom na dugmad
ispod embed poruke, kao mali navigacioni panel. Pojedinačna početna stranica
se može otvoriti i komandama `.pomoc 2` ili `.pomoc 3`. Svaki član može
pokrenuti najviše tri prikaza pomoći u 60 minuta; ograničenje se čuva u
`ljubav_data.json` i nastavlja se nakon restarta. Ako su DM poruke zatvorene,
bot će to prijaviti u kanalu.

Kada se runda pokrene prefix komandom, nastavak se šalje kao obična poruka:

- `.kaladont astma` → zatim drugi član šalje `mama`
- `.toplo-hladno 44` → zatim članovi šalju `55`, `60` itd.
- `.counting 1` → zatim članovi šalju `2`, `3` itd.

Bot odgovara najviše jednom na jednu Discord poruku. Ako se ista poruka
ipak pojavi dva puta, provjeri da nije pokrenut drugi primjerak bota u
drugom Replit procesu.

Counting odgovor se prikazuje kao Discord reply na poruku člana. Za početak
runde šalje se `1`; zatim drugi član šalje `2`, pa naredni `3`. Nagrade u
novčićima i dalje se dodjeljuju za svaki tačan broj.

### Pokloni

```text
.poklon @Korisnik
.cvijece @Korisnik
.cokolada @Korisnik
.ruza @Korisnik
.ogrlica @Korisnik
.sat @Korisnik
.parfem @Korisnik
.auto @Korisnik
.jahte @Korisnik
```

### Kuća i sobe

```text
.stan
.kuca
.soba
.kuhinja
.spavaca
.kupatilo
.vrt
.bazen
.garaza
.namjestaj
```

### Ekonomija

```text
.rad
.plata
.novac [@Korisnik]
.banka
.uplati <iznos>
.podigni <iznos>
.transfer @Korisnik <iznos>
.market
.kupi <predmet>
.pokloni @Korisnik <predmet>
```

### Ribolov

```text
.pecanje
.ribnjak [@Korisnik]
.prodaj
.ribar [@Korisnik]
```

### Rang-liste i pomoć

```text
.topljubav
.topxp
.topbogati
.topparovi
.topstanovi
.top-ribar
.pomoc
.pomoc 2
.pomoc 3
.giveaways
```

`.giveaways` može koristiti samo vlasnik bota. Objavljuje Nitro Boost giveaway
za 1 mjesec, sa uslovima da učesnik dovede 2 osobe i stavi tag `mrak`.
Svaki giveaway traje 24 sata i nakon isteka bot objavljuje pobjednika.

## 9. Podaci

Bot sam pravi fajl pored `bot.py`:

```text
ljubav_data.json
```

U njemu se čuvaju veze, brakovi, djeca, XP, novac, bankovni račun,
inventar, kuće, ribolov, podešeni kanali, Info panel i iskorištene welcome
poruke, kao i stanje counting igara.

Redovno napravi backup tog fajla.

Bot automatski čuva i `ljubav_data.json.bak`. Ako se glavni JSON ošteti zbog
prekida procesa, pri sljedećem pokretanju pokušava vratiti posljednju sigurnu
kopiju.

## 10. Provjera ovog paketa

Provjereno:

- 78 prefix komandi
- 21 slash komanda registrovana u kodu
- nema dupliranih naziva komandi
- Python sintaksa prolazi bez greške
- nema hardkodovanog Discord tokena
- nema stvarnih GIF fajlova ili GIF URL-ova u projektu
- `/setreport`, `/setpomoc` i `/setlogin` vidljive su bot-vlasniku i server administratorima
- pri pokretanju se ispisuje stvarni rezultat slash komandi sinhronizovanih sa Discordom
- dugmad panelâ koriste sigurne Unicode ikone, kako nevažeći custom emoji ne bi blokirao slanje
- counting stanje se čuva trajno, a tačan unos se vraća kao reply embed
- Kaladont mijenja dvije priložene slike naizmjenično; Info panel ima dugme za sve komande

### Evidencija ljubavnih aktivnosti

Svaka uspješna ljubavna aktivnost sada se trajno upisuje u `ljubav_data.json`.
Bodovi ljubavi i XP prikazuju se u profilu, a aktivnosti prema partneru se
knjiže u oba profila. To uključuje vezu, brak, zakletvu, svadbu, obnovu
zavjeta, spavanje, srce, zagrljaj, poljubac, ples, poklone, film, istinu,
izazov i testove kompatibilnosti.

Kaladont sada provjerava redoslijed igrača, pa isti član ne može odigrati dvije
riječi zaredom, a unos mora postojati u B/H/S rječniku. Podaci se čuvaju
atomarno kako prekid procesa ne bi oštetio postojeći profil.

Welcome, Staff i Ticket paneli koriste fiksne Unicode ikone. Ikone se više
ne mijenjaju prema redoslijedu emoji-ja u poruci niti nakon restarta bota.

### Blockquote oznake

Embed poruke koriste početne `>` oznake za željeni blockquote izgled.
Prazni separator redovi koriste nevidljivi znak, pa se neće prikazati
samo usamljeni `>` karakter.

- `description`
- naziva i vrijednosti fieldova
- footer-a
- author teksta

Svi field naslovi bez postojeće ikone automatski dobijaju ikonu prije
naslova, dok se postojeće ikone ne mijenjaju. Nakon izmjene obavezno
restartuj bota.

### Naslovi i footer

Naslovi embedova se automatski prebacuju u prvi blockquote red unutar
teksta embed-a, umjesto posebnog Discord naslovnog reda. Ako naslov nema
ikonu, bot je dodaje ispred naslova. Iz footera se uklanjaju `MONIKA BOT` i
verzija, pa cooldown ostaje samo kao `Cooldown: 24h`.

Važno:

- `.izazov` samo napiše tekstualni izazov koji može tražiti da korisnik pošalje GIF
- GWS & Events panel prikazuje podešene kanale, ali nije automatski giveaway/event sistem
- dugme `play music` vodi na kanal; ne pokreće audio player