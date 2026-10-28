# MONIKA BOT Discord Bot — v4.9

Ovo je kompletan Discord bot sa ljubavnim sistemom, virtuelnom ekonomijom,
kućama za parove, ribolovom, rang-listama, welcome panelom i Info/GWS & Events
panelom. Dodane su komande `.ship`, `.iq`, `.opis` i `.obrisi 100`; profili
se automatski dopunjavaju ako su sačuvani u starijem formatu.

## Brzi početak na Replitu

1. Otpakuj sadržaj ZIP-a u glavni folder projekta; `bot.py`, `main.py`,
   `requirements.txt` i stikeri treba da budu zajedno.
2. U Replit Secrets dodaj tajnu pod imenom `DISCORD_TOKEN`. Ne upisuj token u
   fajl i ne šalji ga u chat.
3. Instaliraj zavisnosti komandom `python -m pip install -r requirements.txt`.
4. Pokreni projekat sa `python main.py` (ili postavi tu komandu kao Run/Workflow).
   `main.py` pokreće `bot.py`.
5. U konzoli provjeri da piše `Registrovano slash komandi: 28`.

Komanda `.opis` pokazuje kako se koriste mini-igre i u kojim kanalima rade.
Nepostojeće komande (npr. `.xo`) ne šalju opomenu.
Komande `.ship` i `.iq` pripadaju ljubavnom sistemu i rade samo u kanalu
ljubavi (`#brak-veza` ili kanalu koji je administrator podesio preko
`/setkanal`).

Komande za podešavanje koriste slash `/`, a obične članske komande prefix `.`.
Vlasnik bota može postaviti početni Counting broj, npr. `.counting 1240`;
sljedeći članovi zatim šalju prikazani broj kao običnu poruku.
ZIP uključuje slike za Counting, Toplo-hladno i Kaladont. Casino radi i bez
dodatnih stikera, ali njegove slike nisu dio ovog paketa.

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
- Manage Channels (za kreiranje i zatvaranje privatnih ticket/report kanala)
- Ban Members (za `/ban` i `/unban`)
- View Audit Log (opcionalno, prikazuje moderatora u ban/unban evidenciji)

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

Welcome embed ima tri ravnomjerno poravnata inline polja za pravila, chat i muziku,
thumbnail novog člana i custom emoji-je na dugmadima.
`/welcome-podesi` odmah šalje test welcome embed u odabrani kanal. Pri ulasku novog
člana bot šalje welcome poruku samo u podešeni server kanal — welcome DM funkcija
je uklonjena. `Server Members Intent` mora biti uključen i u kodu
i u Discord Developer Portalu. Test komanda prijavljuje dozvolu koja nedostaje ako
bot ne može slati poruke ili embede.
Welcome poruke, test i welcome status koriste sedam dostavljenih custom emoji-ja.
Bot ih prvo traži među emoji-jima servera/aplikacije i koristi postojeće MONIKA
emoji-je kao fallback ako neki od njih nije dostupan.

Pregled podešavanja:

```text
/welcome-status
```

Isključivanje welcome poruka u server kanalu:

```text
/welcome-iskljuci
```

Welcome dugme radi ovako:

- samo prva osoba može poslati dobrodošlicu za jednu welcome poruku
- nakon prvog klika dugme se deaktivira
- drugi pokušaji dobijaju objašnjenje da je dugme već iskorišteno
- iskorišteni klikovi se čuvaju u `ljubav_data.json`

Ako korisnik ima zatvorene privatne poruke, Discord ne dozvoljava botu da ih
otvori. U tom slučaju `.pomoc` prikazuje pomoć u kanalu, a `.poruka` šalje
privatnu potvrdu autoru komande. Pozivnica se objavljuje jednom u kanalu u kojem
je komanda pokrenuta; bot ne šalje pozivnice masovno u DM.

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

Ponovno pokretanje `/info-postavi` ažurira postojeći panel u tom kanalu umjesto
da objavi novu kopiju. Bot mora imati dozvole za čitanje historije poruka.
Pregled podešavanja:

```text
/info-status
```

`/info-podesi` i `/ticket-podesi` samo sačuvaju podešavanja i pokažu privatnu
potvrdu. Za javni panel pokreni odgovarajuću komandu `-postavi`.

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
Ponovno pokretanje `/staff-postavi` u istom kanalu ažurira postojeći panel,
umjesto da napravi novu objavu.

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
Ponovno pokretanje `/ticket-postavi` u istom kanalu ažurira postojeći panel.

Za slanje vlastitog teksta kao embed poruke koristi:

```text
/embed tekst:Poruka koju želiš objaviti naslov:Naslov kanal:#kanal
```

Naslov i kanal su opcionalni; bez kanala embed se šalje u kanal u kojem je
komanda pokrenuta. Komanda je dostupna administratorima, a sama embed poruka
je javna. Potvrda o slanju prikazuje se samo tebi.

### Report sistem

Report je odvojen od običnog ticketa i podešava se jednom komandom:

```text
/setreport kanal:#reports kategorija:REPORTS uloga:@Staff
```

Komanda odmah šalje panel sa dugmetom `Prijavi korisnika`. Klik otvara privatni
kanal koji vide autor reporta i staff. Report se zatvara dugmetom `Zatvori report`.
Ponovno pokretanje `/setreport` u istom kanalu ažurira postojeći panel.
Komandu vidi bot-vlasnik i server administratori; ostali članovi ne mogu je koristiti.

Svi embed-i koriste sivo-crnu dark boju (`#2B2D31`) i kompaktan prikaz bez `>` oznaka.

## 7. Slash komande

```text
/setkanal
/ukloni-kanal
/setlogin
/evidencija
/topaktivnost
/ban
/unban
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
/embed
/setreport
/poruka
/sync
/setmonika
/iskljuci-moniku
/opis
/komande
/igre
```

Prefix komande koriste odvojene kanale po kategoriji. Ako je kanal već nazvan
po kategoriji, npr. `#kaladont` ili `#counting`, bot ga prepoznaje automatski.
Ljubavne komande automatski rade i u kanalu `#brak-veza`.
Za kanale drugačijih naziva administrator može eksplicitno podesiti tip:

```text
/setkanal ljubav #ljubav
/setkanal pecanje #pecanje
/setkanal ekonomija #ekonomija
/setkanal kuca #kuca
/setkanal zabava #zabava
/setkanal rangliste #rangliste
/setkanal counting #counting
/setkanal toplo_hladno #toplo-hladno
/setkanal kaladont #kaladont
/setkanal casino #casino
```

Ljubavne, ribolovne, ekonomske, kućne, zabavne, rang-lista i igračke prefix
komande rade samo u svom odgovarajućem kanalu. Administrativne slash komande
ostaju dostupne administratorima i prikazuju privatne potvrde.

U `#kaladont` pokreni `.kaladont` jednom, pa svaki sljedeći igrač šalje samo
riječ koja odgovara posljednja dva slova — bez ponavljanja komande. Za
Toplo-hladno pokreni `.toplo-hladno`, pa upisuj samo brojeve. U Counting kanalu
`.counting` prikazuje sljedeći broj. Samo vlasnik bota može postaviti početak,
npr. `.counting 1240`; svi zatim šalju samo očekivani broj kao poruku.
Tačan potez dobija ✅, pogrešan ❌. Pogrešan potez ne šalje tekst ni DM.
Unosi su ograničeni na jedan potez svakih 0,75 sekundi po članu i kanalu; višak unosa se
preskače bez javne poruke. Aktivnu rundu Toplo-hladno/Kaladonta može resetovati
samo vlasnik servera ili administrator; nakon završetka bilo ko može započeti
novu rundu. Za reakcije botu treba dozvola **Add Reactions**.

`/setlogin` više ne zaključava komande. Postavi kanal za evidencijske embedove
dolasaka članova i ban/unban događaja. Nakon ažuriranja pokreni `/setlogin`
ponovo da odabereš kanal evidencije:

```text
/setlogin kanal:#evidencija
```

Statistika počinje brojati poruke nakon pokretanja ove verzije. `/evidencija`
prikaže statistiku tebe ili odabranog člana, uključujući poruke po tekstualnom
kanalu i voice vrijeme po voice kanalu. Ista evidencija dostupna je i kao
prefix komanda `.evidencija [@član]`. `/topaktivnost` prikazuje prvih 10 članova
po broju poruka ili voice vremenu. Brojači i voice podaci čuvaju se u
`ljubav_data.json`; bot ne broji voice vrijeme dok je ugašen.

Za moderaciju koristi `/ban clan razlog` i `/unban clan_id razlog`. Komande
šalju privatnu potvrdu, a događaji se upisuju i u kanal postavljen preko
`/setlogin`. Ban/unban akcije napravljene direktno u Discordu takođe se
evidentiraju. Bez dozvole **View Audit Log**, moderator može biti prikazan kao
nepoznat, ali sam događaj se i dalje bilježi.

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
sinhronizovanih slash komandi i njihova imena. `/sync` ručno osvježava slash
komande bez restartovanja procesa; koristi ga vlasnik bota ili administrator.
Ako se log ne poklapa s ovim paketom, vjerovatno se pokreće stara kopija bota.

Mini-igre se prepoznaju po nazivu kanala (npr. `#kaladont`, `#counting`) ili
po ID-u kanala podešenom preko `/setkanal`. Skraćeni unos bez prefiksa radi
samo u tako prepoznatom kanalu.

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

Vlasnik bota ili servera može objaviti jednu pozivnicu u kanalu u kojem pokrene
komandu. Radi zaštite od neželjenih masovnih DM-ova, bot ne šalje pozivnice
privatno članovima:

```text
.poruka https://discord.gg/tvoj-link
```

Ista funkcija radi i kao slash komanda:

```text
/poruka server_link:https://discord.gg/tvoj-link
```

Prefix i slash komandu mogu koristiti vlasnik bota ili vlasnik servera. U
objavljenoj poruci su pozivni link i pregled glavnih komandi. Pozivnica može
biti objavljena najviše jednom u pet minuta po serveru; potvrda/greška se šalje
privatno autoru komande.

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
- `.counting` — prikaži sljedeći broj; samo vlasnik bota može postaviti početak, npr. `.counting 1240`
- `.toplo-hladno` — pokreni rundu, zatim šalji brojeve od 1 do 100
- `.kaladont` — pokreni rundu, zatim šalji obične riječi bez prefiksa

Stanje svake igre je odvojeno po Discord kanalu. Counting prikazuje prihvaćeni
broj i sljedeći broj kao reply embed. Svaki član može nastaviti brojanje ako
upiše očekivani broj; samo vlasnik bota može promijeniti početni broj komandom
`.counting <broj>`. Pogrešan broj vraća brojač na 1 i dobija samo ❌.
Broj se čuva u `ljubav_data.json` i preživljava restart.

Kaladont prihvata riječ direktno u svom kanalu, provjerava slova, rječnik,
ponavljanje i traženi nastavak. Prihvata se prvi ispravan potez bez obzira ko
ga je poslao; pogrešna riječ ne šalje javni tekst. Counting, Toplo-hladno i
Kaladont koriste slike iz ovog paketa. Casino radi bez slike ako njegovi
dodatni stikeri nisu dodani.

U kanalima podešenim preko `/setkanal` za `counting`, `toplo-hladno`,
`kaladont` ili `casino` može se slati unos bez prefixa:

- `counting` — provjeri broj sa `.counting`; vlasnik bota može postaviti početak, a svi šalju sljedeći broj kao poruku
- `toplo-hladno` — pokreni `.toplo-hladno`, pa šalji brojeve `44`, `55`...
- `kaladont` — prvo pokreni `.kaladont`, zatim šalji riječi direktno u kanal
- `casino` — pošalji `coin 50` ili `slot 50`

Pogrešan potez dobija samo ❌ na originalnoj poruci — bot ne objavljuje grešku,
ne šalje DM i ne kažnjava člana. Pogrešan kanal dobija privatno objašnjenje.
Tačni potezi i javna stanja igre ostaju u kanalu da igrači mogu pratiti rundu.

Slash komande `/komande` i `/igre` se automatski registruju nakon pokretanja
bota. `/komande` prikazuje prefix i slash komande abecednim redom, uz opise
slash komandi; `/igre` objašnjava upotrebu mini-igara.

Komanda `.opis` objašnjava korake za Casino, Counting, Toplo-hladno i Kaladont
i prikazuje kanal koji je administrator podesio. Bez posebnog podešavanja bot
prepoznaje kanale čiji nazivi sadrže `casino`, `counting`, `toplo-hladno` ili
`kaladont`; administratori mogu izabrati i druge kanale pomoću `/setkanal`.

### Dodatne komande

```text
.ship @Korisnik
.iq [@Korisnik]
.opis
.obrisi 100
```

`.ship` prikazuje zabavni procenat za označeni par; rezultat je stabilan za isti
par. `.iq` daje šaljiv dnevni rezultat i nije pravi IQ test. `.opis` prikazuje
uputstva za igre i njihove kanale. `.ship` i `.iq` rade u bilo kojem kanalu.
`.obrisi 100` briše do 100 poruka i zahtijeva
dozvolu **Manage Messages** za člana i bota. Nepostojeća komanda poput `.xo`
ne izaziva opomenu.

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
- `.counting 1240` (vlasnik bota) → zatim članovi šalju `1240`, `1241` itd.

Bot odgovara najviše jednom na jednu Discord poruku. Ako se ista poruka
ipak pojavi dva puta, provjeri da nije pokrenut drugi primjerak bota u
drugom Replit procesu. Brojanje više ne pokušava drugo slanje ako Discord vrati
grešku na prvom reply-u, da ne bi napravio dupli odgovor.

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
poruke, stanje counting igara, statistika poruka po kanalu i voice vrijeme.

Redovno napravi backup tog fajla.

Bot automatski čuva i `ljubav_data.json.bak`. Ako se glavni JSON ošteti zbog
prekida procesa, pri sljedećem pokretanju pokušava vratiti posljednju sigurnu
kopiju.

## 10. Provjera ovog paketa

Provjereno:

- 79 prefix komandi
- 28 slash komandi registrovanih u kodu, uključujući ručni `/sync`
- nema dupliranih naziva komandi
- Python sintaksa prolazi bez greške
- nema hardkodovanog Discord tokena
- nema stvarnih GIF fajlova ili GIF URL-ova u projektu
- `/setreport`, `/setpomoc` i `/setlogin` vidljive su bot-vlasniku i server administratorima
- pri pokretanju se ispisuje stvarni rezultat slash komandi sinhronizovanih sa Discordom
- interaktivna dugmad koriste Discord custom emoji-je, a panel komande ažuriraju postojeću objavu umjesto da je dupliraju
- counting stanje se čuva trajno; ispravni potezi dobijaju ✅, a pogrešni ❌
- pogrešni potezi dobijaju samo ❌; pogrešan kanal se objašnjava privatno
- direktan unos radi u kanalima prepoznatljivog naziva i bez ručnog `/setkanal`
- limit po članu/kanalu prigušuje spam bez javnog flooda ili automatskog bana
- `/poruka` objavljuje jednu pozivnicu u kanalu uz cooldown; ne šalje masovne DM-ove
- Kaladont mijenja dvije priložene slike naizmjenično; Info panel ima dugme za sve komande
- `/setlogin` postavlja kanal za logove dolazaka, banova i unbanova; više ne ograničava komande
- `.evidencija` i `/evidencija` prikazuju poruke i voice vrijeme po kanalu; `/topaktivnost` rangira članove
- `/ban` i `/unban` šalju privatnu potvrdu i prave moderacijske evidencijske embede

### Evidencija ljubavnih aktivnosti

Svaka uspješna ljubavna aktivnost sada se trajno upisuje u `ljubav_data.json`.
Bodovi ljubavi i XP prikazuju se u profilu, a aktivnosti prema partneru se
knjiže u oba profila. To uključuje vezu, brak, zakletvu, svadbu, obnovu
zavjeta, spavanje, srce, zagrljaj, poljubac, ples, poklone, film, istinu,
izazov i testove kompatibilnosti.

Kaladont prihvata svaki ispravan sljedeći potez, čak i od istog člana, i provjerava
da riječ postoji u B/H/S rječniku. Aktivnu rundu ne može resetovati običan član.
Podaci se čuvaju atomarno kako prekid procesa ne bi oštetio postojeći profil.
Stariji ljubavni profili dobijaju nedostajuća polja pri učitavanju; postojeći
bodovi i datumi se čuvaju. Nepostojeće prefix komande ne šalju poruke članovima.
`.obrisi` ograničava brisanje na 1–100 poruka i provjerava dozvole.

Welcome, Info, Staff, Ticket, Report, Giveaway i pomoć dugmad koriste
Discord custom emoji-je. Ponovno postavljanje panela u istom kanalu ažurira
postojeću botovu poruku; ranije objavljene duplikate bot ne briše automatski.

### Blockquote oznake

Sav vidljivi tekst u embedovima prikazuje se kao Discord blockquote:
naslov je podebljan i odvojen praznim redom, a svaka rečenica, oznaka i vrijednost
dobijaju svoju `>` oznaku. To uključuje opis, nazive i vrijednosti fieldova,
author tekst i footer tekst. Nazivi fieldova se prikazuju kao citirane oznake
unutar njihove vrijednosti i podebljani su; kratke oznake prije dvotačke takođe
se podebljavaju. Postojeći inline raspored fieldova ostaje sačuvan.
Naslov, opis, slike, thumbnail, boja, timestamp, dugmad i funkcionalnost
komandi ostaju sačuvani.

Naslov bez postojeće ikone automatski dobija ikonu prije teksta, a postojeće
ikone ostaju iste. Ticket panel naslov je `OTVORI TIKET` velikim slovima, a
info emoji i podebljan tekst ističu pitanje „Treba ti pomoć?”.
Iz footera se uklanjaju `MONIKA BOT` i verzija, pa cooldown ostaje samo kao
`Cooldown: 24h`. Nakon izmjene obavezno restartuj bota.

Važno:

- `.izazov` samo napiše tekstualni izazov koji može tražiti da korisnik pošalje GIF
- GWS & Events panel prikazuje podešene kanale, ali nije automatski giveaway/event sistem
- dugme `play music` vodi na kanal; ne pokreće audio player