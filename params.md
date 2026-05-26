# Katalog funkčních a nefunkčních požadavků

## Projekt: Informační systém pro architektonické fórum a 3D marketplace

## 1. Katalog funkčních požadavků (Functional Requirements)

### 1.1 Hierarchie uživatelských rolí a přístupových práv (RBAC)
Systém implementuje vertikální dědičnost práv. Každá vyšší role automaticky dědí všechna oprávnění rolí nižších, pokud není explicitně uvedeno jinak.

#### 1.1.1 Nepřihlášený uživatel (Host)
* **Prohlížení obsahu:** Volný přístup k veřejné domovské stránce a katalogu modelů.
* **Vyhledávání a filtrace:** Možnost vyhledávat v modelech pomocí textu a základních filtrů.
* **Prohlížení 3D:** Možnost otevřít model v integrovaném 3D prohlížeči.
* **Nákup a stahování:** Možnost stáhnout modely, které jsou zdarma, nebo zakoupit placené modely jako host (vyžaduje zadání e-mailu nebo telefoního čísla pro doručení).
* **Monetizační omezení:** Uživateli se na všech stránkách zobrazují reklamní banery.

#### 1.1.2 Přihlášený uživatel (Architekt)
* **Správa profilu:** Registrace, přihlášení (e-mail/heslo, OAuth2 přes Google), úprava osobních údajů a správa portfolia.
* **Tvorba a správa modelů:** Přístup k vestavěnému 3D modelátoru. Možnost nahrávat vlastní architektonické modely na cloud.
* **Marketplace funkce:** Možnost publikovat model do katalogu. Nastavení licenčních podmínek a ceny (0 Kč pro free modely, nebo libovolná částka pro placené).
* **Feedback systém:** Možnost udělit modelu hodnocení (Like/Dislike), napsat textový komentář nebo si model uložit do sekce "Oblíbené" (záložky).
* **Finanční přehled:** Přístup k interní peněžence, zobrazení historie prodejů vlastních děl a možnost zažádat o vyplacení prostředků.
* **Monetizační omezení:** Uživateli se stále zobrazují reklamy.

#### 1.1.3 Premium uživatel (Předplatitel)
* **Dědičnost:** Přebírá všechna práva Přihlášeného uživatele.
* **Simulační jádro:** Plný a neomezený přístup k pokročilému Fyzikálnímu simulátoru stability a Geografickému simulátoru lokality.
* **Absence reklam:** Kompletní odstranění všech reklamních prvků z uživatelského rozhraní (Ad-Free prostředí).

#### 1.1.4 Moderátor
* **Dědičnost:** Přebírá všechna práva Premium uživatele.
* **Správa obsahu:** Přístup k moderačnímu panelu. Možnost odstranit (skrýt) nevhodné komentáře, toxicitu nebo modely porušující autorská práva.
* **Reverzibilita akcí:** Možnost obnovit omylem nebo chybně smazané příspěvky a modely zpět do veřejného oběhu.
* **Správa uživatelů:** Možnost udělit dočasný nebo trvalý zákaz činnosti (ban) uživatelům, kteří opakovaně porušují pravidla komunity.

---

### 1.2 Hlavní moduly webové aplikace

#### 1.2.1 Homepage a Vyhledávací algoritmus
* **Domovská stránka:** Dynamicky generovaný obsah zobrazující nejpopulárnější modely týdne, novinky od sledovaných autorů a doporučený obsah na základě uživatelských preferencí.
* **Fulltextové vyhledávání:** Indexované vyhledávání v názvech, popisech a tagech modelů.
* **Pokročilé filtrování:** Filtrování podle ceny (zdarma / placené / cenové rozpětí), hodnocení (poměr like/dislike), autora, použitých materiálů, kompatibility a data nahrání.

#### 1.2.2 Zpětná vazba (Feedback systém)
* **Lajkování:** Binární systém hodnocení (palec nahoru / palec dolů) pro rychlé vyjádření kvality modelu. Každý uživatel může dát pouze jedno hodnocení na jeden model.
* **Komentářová sekce:** Podpora víceúrovňových (vláknových) komentářů pod detaily modelů pro diskuzi o technických detailech stavby.
* **Ukládání obsahu:** Možnost organizovat uložené modely do uživatelských složek (např. "Inspirace pro interiéry", "Konstrukce střech").

#### 1.2.3 AI Asistent moderování (Předmoderování)
* **Automatická analýza textu:** Subproces běžící na pozadí, který pomocí LLM/NLP skenuje nově přidané komentáře a popisky modelů.
* **Detekce rizikového obsahu:** Vyhledávání spamu, vulgarit, hate speech, odkazů na phishingové weby a pokusů o obcházení platební brány.
* **Příznaky (Flagging):** AI systém nemaže obsah automaticky (aby se předešlo false-positive chybám), ale označí rizikový příspěvek "příznakem" a odešle jej do fronty moderátorů ke schválení/smazání.

#### 1.2.4 Bezpečná platební brána a finanční distribuce
* **Integrované metody:** Podpora moderních platebních metod včetně Google Pay, Apple Pay a přímé platby kreditními/debetními kartami.
* **Byznys model (Provize 80/20):** Systém automaticky rozděluje každou transakci:
    * **80 %** z prodejní ceny odchází na interní konto autora modelu.
    * **20 %** z prodejní ceny si ponechává platforma jako servisní poplatek za zprostředkování, provoz serveru a výpočetní výkon simulací.

---

### 1.3 Integrovaný 3D Modelátor

Webový editor integrovaný přímo v prohlížeči slouží pro rychlé skicování, úpravu nebo ověřování architektonických konceptů.

* **Inteligentní napojování prvků (Snapping):** Pokud uživatel přiblíží dva konstrukční bloky ze stejného materiálu k sobě, systém je automaticky magneticky přichytí a vizuálně/strukturálně sloučí do jednoho spojitého celku (např. spojení dvou betonových stěn).
* **Land Transformátor (Terénní editor):** Nástroj pro plynulou deformaci podkladového terénu ve stylu digitálního sochařství (sculpting). Umožňuje uživateli tahem myši/štětce zvedat povrch (tvorba kopců, valů), stlačovat jej dolů (tvorba údolí, příkopů, základových jam) a vyhlazovat ostré hrany.
* **Vizuální subsystém a Rendering:**
    * Podpora knihovny reálných textur (beton, dřevo, cihly, sklo, ocel).
    * Výpočet jednoduchých světelných odrazů (reflexí) na lesklých površích (skleněné fasády).
    * Dynamické osvětlení scény na základě nastavení virtuálního času (poloha slunce na obloze).
* **Orbitální kamera (CAD/Blender styl):** Navigační systém, kde se kamera otáčí kolem uživatelem definovaného bodu zájmu či vybraného objektu, nikoliv kolem své vlastní osy. Podpora zoomování na střed a posunu (pan).

---

### 1.4 Simulační jádro (Premium funkce)

#### 1.4.1 Fyzikální simulátor stability
Simulátor provádí strukturální analýzu modelu v čase a vizuálně (např. barevným spektrem od zelené po červenou) zobrazuje deformace, napětí a případné kritické hroucení budovy. Podmínky simulace plně definuje uživatel:
* **Gravitace:** Nastavitelná konstanta zrychlení. Výchozí hodnota je pozemských $9.81 \text{ m/s}^2$, uživatel však může simulovat chování konstrukce v jiných prostředích.
* **Vítr:** Definice síly (intenzity v m/s) a směru proudění vzduchu pomocí 2D vektoru na horizontální rovině.
* **Zemětřesení:** Simulace seizmické aktivity a otřesů půdy zadávaná v hodnotách Richterovy stupnice (0 až 10).
* **Tornádo:** Destrukční simulace rotačního vzdušného víru lokalizovaného v prostoru, definovaná stupni podle Fujitovy stupnice (F0 až F5).
* **Bouře (Zásah bleskem):** Simulace atmosférického výboje. Uživatel zvolí bod dopadu (nebo nechá systém vybrat nejvyšší bod stavby). Systém simuluje šíření elektrického proudu konstrukcí, detekuje případné absence hromosvodů a počítá elektrické dopady (přetížení sítě, zkraty, destrukce elektronických prvků).

#### 1.4.2 Geografický simulátor lokality
Modul umožňuje zasadit navržený 3D model do reálného světa na základě GIS dat (Geografických informačních systémů).
* **Mapová integrace:** Využití rozhraní globálních mapových podkladů (Google Maps API) a specifických státních topografických a katastrálních map. Vyžaduje geografické pokrytí (coverage) v daném místě.
* **Analýza inženýrských sítí:** Zjištění a vizuální vykreslení tras a bodů napojení pro:
    * Pitnou a užitkovou vodu (vodovodní řady).
    * Elektrickou přenosovou síť.
    * Plynovod.
* **Socio-demografická a infrastrukturní data:** Výpočet vzdálenosti k nejbližší stanici veřejné dopravy (MHD, vlak) a automatická klasifikace habitatu (městská zástavba vs. venkovská oblast).
* **Klimatické a meteorologické podmínky:** Na základě zadaných GPS souřadnic systém načte dlouhodobá statistická data pro dané místo: průměrné roční i sezónní teploty, vlhkost vzduchu, srážkové úhrny a průměrné větrné podmínky.
* **Závěrečná vizualizace:** Výsledkem je zasazení uživatelova 3D modelu přímo do 3D/2D zobrazení vybrané parcely na mapě světa se zobrazením textového a grafického reportu výše zmíněných parametrů.

---

## 2. Katalog nefunkčních požadavků (Non-functional Requirements)

### 2.1 Technologický stack (Architecture & Infrastructure)
* **Databázová vrstva:** **PostgreSQL** – hlavní relační databáze pro správu uživatelských účtů, transakcí, relací feedbacku a metadat modelů. Bude rozšířena o modul **PostGIS** pro pokročilé ukládání a dotazování prostorových geografických dat (využito v simulátoru lokality).
* **Backendová vrstva:** **Node.js** (např. s frameworkem NestJS nebo Express) – asynchronní architektura řízená událostmi, která umožňuje rychlé zpracování I/O operací, komunikaci s AI API a obsluhu platebních transakcí bez blokování hlavního vlákna.
* **Frontendová vrstva:** **Next.js** (React framework) – zajišťuje optimální vykreslování na straně serveru (SSR) a statickou generaci (ISR) pro katalog a domovskou stránku, což zaručuje bleskové načítání a vysoké hodnocení v SEO (vyhledávačích).
* **Simulační a grafické rozhraní:** **WebGPU** – nízkoúrovňové API pro přístup k hardwarové akceleraci grafické karty přímo z prohlížeče. Na rozdíl od staršího WebGL bude WebGPU využito pro své **Compute Shaders**. Ty umožňují přenést masivní matematické a fyzikální výpočty simulací (např. částicové simulace větru, šíření napětí v konstrukci, blesky) přímo na GPU koncového uživatele, čímž se radikálně sníží zátěž na backendové servery platformy.

### 2.2 Bezpečnost a shoda s předpisy (Security & Compliance)
* **Zabezpečení platebních dat:** Systém nesmí na vlastních serverech ani v databázi ukládat citlivé údaje o platebních kartách (číslo karty, CVV kód). Veškeré transakce musí probíhat formou bezpečné tokenizace skrze certifikovaného prostředníka (např. Stripe) splňujícího standard **PCI-DSS Compliance**.
* **Zabezpečení uživatelských dat:** Hesla uživatelů musí být v databázi hashována robustním algoritmem (např. bcrypt nebo Argon2) s unikátní solí. Komunikace mezi klientem a serverem musí být šifrována pomocí protokolu HTTPS (TLS 1.3).

### 2.3 Odolnost proti chybám a spolehlivost (Fault Tolerance)
* **Ošetření výpadků mapových služeb:** Vzhledem k závislosti simulátoru lokality na externích API (Google Maps, státní mapy) musí být systém schopen detekovat nedostupnost těchto služeb nebo absenci pokrytí (no coverage) pro dané GPS souřadnice. V takovém případě nesmí dojít k pádu aplikace, ale k zachycení výjimky a zobrazení validní, uživatelsky srozumitelné chybové hlášky (např. *"Pro vybranou lokalitu nejsou dostupná data o inženýrských sítích"*).
