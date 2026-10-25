# Dech barev – podklady pro aplikaci bez internetu

Složka je **kompletní web, který běží bez připojení**. Nic se nestahuje z internetu.

## Co je uvnitř
- `index.html` – celá aplikace (14 barev CZ + EN, dotazník Moje barevné nastavení)
- `pdf/` – 28 celých knih (`Dech_barev_*.pdf`, `Breath_of_Colours_*.pdf`) a `pdf/k/` – 280 PDF kapitol
- `fonts/` – písma Lora a Mrs Saint Delafield (dřív se brala z Google Fonts)
- `lib/html2canvas.min.js` – knihovna pro uložení výsledku dotazníku jako obrázek/PDF (dřív z CDN)
- `manifest.webmanifest`, `icon-192.png`, `icon-512.png` – aby šla stránka nainstalovat jako aplikace
- `sw.js` – služební skript, který si po prvním otevření uloží vše do zařízení (pak funguje i v letadle)

Celkem asi 170 MB, z toho PDF skoro všechno.

## Co jsem změnila oproti webu
- Google Fonts a CDN knihovna nahrazeny lokálními soubory.
- Tlačítko „Stáhnout PDF“ používá obyčejné stažení souboru (na webu Claude se používá jeho vlastní uložení, které mimo něj nefunguje).
- Přidán manifest a offline cache. Obsah stránek ani texty jsem neměnila.

## Jak to vyzkoušet
Soubory musí běžet přes `http://` nebo `https://` (offline cache se z `file://` nespustí):
1. ve složce spusť `python3 -m http.server 8000` a otevři `http://localhost:8000`
2. načti stránku jednou, počkej pár vteřin, pak vypni internet a obnov stránku – musí fungovat.

## Tři cesty k aplikaci
1. **Instalovatelná webová aplikace (PWA)** – nejjednodušší. Složku nahraj na jakýkoli hosting s HTTPS. Na telefonu: Chrome → „Přidat na plochu“, Safari (iPhone) → Sdílet → „Přidat na plochu“. Po prvním otevření funguje offline. Bez obchodů, bez poplatků.
2. **Balená aplikace pro obchody** – složku obalí nástroj Capacitor (nebo Cordova) do Android/iOS aplikace; soubory jsou přímo v aplikaci, takže offline je zaručené od instalace. Potřebuje vývojáře nebo službu na sestavení a účty vývojáře v obchodech (poplatky zjisti aktuálně u Google a Apple, nevím je jistě).
3. **Jen složka na USB/disk** – otevřít `index.html` přímo ze souboru jde, ale offline cache a instalace nefungují a některé prohlížeče omezují ukládání. Na rychlé předvedení stačí.

## Na co si dát pozor
- **Velikost:** 170 MB je pro PWA hodně. Úspora: do cache dát jen `index.html`, písma a 28 celých knih a kapitoly stahovat až na požádání (v `sw.js` vyházet `pdf/k/` ze seznamu `FILES`).
- **Odpovědi z dotazníku** se ukládají jen v zařízení (localStorage), nesynchronizují se mezi zařízeními.
- **Sdílení odkazu** (tlačítko Sdílet) vytváří odkaz na adresu, kde aplikace běží; v balené aplikaci je potřeba nastavit, co se má sdílet.
- **Aktualizace obsahu:** po změně souborů zvedni číslo `V` v `sw.js` (`dech-barev-v1` → `v2`), jinak si zařízení nechá starou verzi.
- Ikony jsou z loga 180 px zvětšené na 192/512 px; pro obchody je vhodné dodat ostré logo aspoň 1024 px.
- Test jsem dělala v Chromiu (offline načtení, písma, PDF z cache). Na iPhonu a v obchodech jsem to nezkoušela.
