<!doctype html>
<html lang="pl">
  <head>
    <meta charset="UTF-8" />
    <title>Impostor – Setup Gry</title>
    <style>
      body {
        font-family: sans-serif;
        background: #111;
        color: #fff;
        padding: 20px;
        max-width: 500px;
        margin: auto;
      }

      h1,
      h2 {
        text-align: center;
      }

      input,
      select,
      button {
        width: 100%;
        padding: 10px;
        margin: 6px 0;
        font-size: 16px;
      }

      button {
        cursor: pointer;
        background: #3b82f6;
        border: none;
        color: white;
        border-radius: 6px;
      }

      button.secondary {
        background: #333;
      }

      ul {
        list-style: none;
        padding: 0;
      }

      li {
        background: #222;
        padding: 8px;
        margin: 4px 0;
        display: flex;
        justify-content: space-between;
        align-items: center;
      }

      .row {
        display: flex;
        gap: 10px;
      }

      .row button {
        flex: 1;
      }

      .checkbox {
        display: flex;
        align-items: center;
        gap: 10px;
        margin: 10px 0;
      }

      .checkbox input {
        width: auto;
      }

      .hidden {
        display: none !important;
      }
    </style>
  </head>
  <body>
    <h1>🕵️ Impostor</h1>

    <!-- DODAWANIE GRACZY -->
    <div id="setupScreen">
      <h2>Gracze</h2>

      <input id="playerName" placeholder="Nazwa gracza" />
      <button id="addPlayerBtn">Dodaj gracza</button>

      <ul id="playersList"></ul>

      <!-- USTAWIENIA -->
      <h2>Ustawienia gry</h2>

      <label>Ilość impostorów</label>
      <select id="impostorsCount">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
      </select>

      <div class="checkbox">
        <input type="checkbox" id="impostorHint" checked />
        <label for="impostorHint">Impostor dostaje podpowiedź</label>
      </div>

      <label>Kategoria słów</label>
      <select id="category">
        <option value="jedzenie">Jedzenie</option>
        <option value="miejsca">Miejsca</option>
        <option value="przedmioty">Przedmioty</option>
        <option value="zawody">Zawody</option>
        <option value="czynnosci">Czynności</option>
        <option value="postacie">Postacie</option>
        <option value="zwierzęta">Zwierzęta</option>
      </select>

      <div class="row">
        <button class="secondary" id="resetBtn">Reset</button>
        <button id="startBtn">Start gry</button>
      </div>
    </div>

    <!-- EKRAN ODKRYWANIA KART -->
    <div id="cardScreen" class="hidden">
      <h2 id="currentPlayerTitle"></h2>
      <button id="revealBtn">Odkryj kartę</button>

      <div id="cardContent" class="hidden">
        <h3 id="cardRole"></h3>
        <p id="cardWord" style="font-size: 20px; margin: 20px 0"></p>
        <button id="nextBtn">Następny gracz</button>
      </div>
    </div>

    <script>
      const WORDS = {
        jedzenie: [
          { word: "pizza", hint: "okrągła struktura" },
          { word: "hamburger", hint: "warstwowość" },
          { word: "pierogi", hint: "zamknięta forma" },
          { word: "spaghetti", hint: "długie pasma" },
          { word: "lasagne", hint: "poziomy" },
          { word: "kebab", hint: "mobilność" },
          { word: "sushi", hint: "precyzja" },
          { word: "ramen", hint: "głębia" },
          { word: "naleśniki", hint: "cienkość" },
          { word: "schabowy", hint: "kontrast tekstur" },
          { word: "stek", hint: "punkt krytyczny" },
          { word: "sałatka", hint: "kontrolowany chaos" },
          { word: "frytki", hint: "powtarzalność" },
          { word: "hotdog", hint: "liniowość" },
          { word: "taco", hint: "otwarta konstrukcja" },
          { word: "burrito", hint: "zamknięcie" },
          { word: "curry", hint: "intensywność" },
          { word: "risotto", hint: "ciągłe mieszanie" },
          { word: "gulasz", hint: "długi proces" },
          { word: "bigos", hint: "wielokrotność" },
          { word: "gołąbki", hint: "owinięcie" },
          { word: "kopytka", hint: "nieregularność" },
          { word: "pyzy", hint: "objętość" },
          { word: "kluski śląskie", hint: "wgłębienie" },
          { word: "placki ziemniaczane", hint: "ścieranie" },
          { word: "tort", hint: "symetria" },
          { word: "ciasto", hint: "struktura" },
          { word: "sernik", hint: "gęstość" },
          { word: "makowiec", hint: "ziarna" },
          { word: "drożdżówka", hint: "wyrastanie" },
          { word: "pączek", hint: "środek" },
          { word: "donut", hint: "pustka" },
          { word: "lody", hint: "stan skupienia" },
          { word: "sorbet", hint: "czystość" },
          { word: "czekolada", hint: "topnienie" },
          { word: "baton", hint: "kompresja" },
          { word: "chipsy", hint: "kruchość" },
          { word: "popcorn", hint: "ekspansja" },
          { word: "kanapka", hint: "przekładanie" },
          { word: "tost", hint: "reakcja" },
          { word: "bagietka", hint: "wydłużenie" },
          { word: "chleb", hint: "podstawa" },
          { word: "omlet", hint: "transformacja" },
          { word: "jajecznica", hint: "niejednorodność" },
          { word: "kiełbasa", hint: "ciągłość" },
          { word: "parówka", hint: "prostota" },
          { word: "boczek", hint: "warstwa tłuszczu" },
          { word: "szynka", hint: "cienkie plastry" },
          { word: "pasztet", hint: "jednolitość" },
          { word: "hummus", hint: "pasta" },
          { word: "falafel", hint: "kule" },
          { word: "tofu", hint: "neutralność" },
          { word: "pizza hawajska", hint: "kontrowersja" },
          { word: "carbonara", hint: "emulsja" },
          { word: "pesto", hint: "rozgniecenie" },
          { word: "ketchup", hint: "lepkość" },
          { word: "majonez", hint: "emulgowanie" },
          { word: "musztarda", hint: "ostrość punktowa" },
          { word: "jogurt", hint: "fermentacja" },
          { word: "owsianka", hint: "pęcznienie" },
          { word: "granola", hint: "chrupiąca mieszanka" },
          { word: "smoothie", hint: "homogenizacja" },
          { word: "shake", hint: "wstrząsanie" },
          { word: "herbata", hint: "ekstrakcja" },
          { word: "kawa", hint: "pobudzenie" },
          { word: "espresso", hint: "koncentrat" },
          { word: "latte", hint: "rozcieńczenie" },
          { word: "cappuccino", hint: "warstwa piany" },
          { word: "cola", hint: "gaz" },
          { word: "lemoniada", hint: "kwasowość" },
          { word: "sok", hint: "wyciśnięcie" },
        ],
        miejsca: [
          { word: "szkoła", hint: "cykliczność" },
          { word: "szpital", hint: "procedura" },
          { word: "kino", hint: "ciemność" },
          { word: "teatr", hint: "obecność" },
          { word: "biblioteka", hint: "porządek" },
          { word: "restauracja", hint: "oczekiwanie" },
          { word: "kawiarnia", hint: "pauza" },
          { word: "bar", hint: "nocny rytm" },
          { word: "hotel", hint: "tymczasowość" },
          { word: "lotnisko", hint: "przejściowość" },
          { word: "dworzec", hint: "rozjazd" },
          { word: "metro", hint: "pod powierzchnią" },
          { word: "przystanek", hint: "zawieszenie" },
          { word: "parking", hint: "bezruch" },
          { word: "garaż", hint: "schronienie" },
          { word: "biuro", hint: "powtarzalność" },
          { word: "fabryka", hint: "ciągłość" },
          { word: "magazyn", hint: "skala" },
          { word: "sklep", hint: "wymiana" },
          { word: "supermarket", hint: "nadmiar" },
          { word: "targ", hint: "negocjacja" },
          { word: "rynek", hint: "centrum" },
          { word: "plac zabaw", hint: "chaos" },
          { word: "park", hint: "przerwa" },
          { word: "las", hint: "gęstość" },
          { word: "plaża", hint: "linia graniczna" },
          { word: "jezioro", hint: "spokój" },
          { word: "rzeka", hint: "kierunek" },
          { word: "góry", hint: "gradient" },
          { word: "dolina", hint: "obniżenie" },
          { word: "jaskinia", hint: "brak światła" },
          { word: "zamek", hint: "obrona" },
          { word: "pałac", hint: "przepych" },
          { word: "muzeum", hint: "kontekst" },
          { word: "galeria", hint: "ekspozycja" },
          { word: "stadion", hint: "zbiorowość" },
          { word: "hala", hint: "echo" },
          { word: "siłownia", hint: "wysiłek" },
          { word: "basen", hint: "wyporność" },
          { word: "aquapark", hint: "zjazdy" },
          { word: "kościół", hint: "cisza wspólna" },
          { word: "katedra", hint: "pionowość" },
          { word: "cmentarz", hint: "trwałość" },
          { word: "urząd", hint: "formularze" },
          { word: "sąd", hint: "ostateczność" },
          { word: "więzienie", hint: "ograniczenie" },
          { word: "posterunek", hint: "kontrola" },
          { word: "most", hint: "połączenie" },
          { word: "tunel", hint: "przejście" },
          { word: "droga", hint: "ciągłość" },
          { word: "autostrada", hint: "prędkość" },
          { word: "ulica", hint: "ruch" },
          { word: "osiedle", hint: "powtarzalność form" },
          { word: "blok", hint: "segmentacja" },
          { word: "dom", hint: "punkt odniesienia" },
          { word: "mieszkanie", hint: "prywatność" },
          { word: "piwnica", hint: "poniżej" },
          { word: "strych", hint: "powyżej" },
          { word: "ogród", hint: "uprawa" },
          { word: "działka", hint: "rekreacja" },
          { word: "farma", hint: "hodowla" },
          { word: "port", hint: "oczekiwanie na ruch" },
          { word: "marina", hint: "jachty" },
          { word: "wyspa", hint: "izolacja" },
          { word: "pustynia", hint: "ekstremum" },
          { word: "oaza", hint: "kontrast" },
          { word: "wulkan", hint: "ciśnienie" },
          { word: "klif", hint: "urwisko" },
          { word: "kanion", hint: "erozja" },
          { word: "lodowiec", hint: "powolność" },
          { word: "dżungla", hint: "wilgoć" },
          { word: "sawanna", hint: "otwartość" },
          { word: "rezerwat", hint: "ochrona" },
          { word: "schronisko", hint: "tymczasowy azyl" },
        ],
        przedmioty: [
          { word: "telefon", hint: "dotykowy" },
          { word: "krzesło", hint: "siedzisko" },
          { word: "zegarek", hint: "czas" },
          { word: "komputer", hint: "sprzęt" },
          { word: "telewizor", hint: "ekran" },
          { word: "klawiatura", hint: "pisanie" },
          { word: "mysz", hint: "kursor" },
          { word: "rower", hint: "pedały" },
          { word: "plecak", hint: "noszenie" },
          { word: "kubek", hint: "napój" },
          { word: "lustro", hint: "odbicie" },
          { word: "nożyczki", hint: "przecięcie" },
          { word: "młotek", hint: "uderzenie" },
          { word: "śrubokręt", hint: "rotacja" },
          { word: "piła", hint: "zęby" },
          { word: "wiertarka", hint: "penetracja" },
          { word: "gwoździe", hint: "przebicie" },
          { word: "śruby", hint: "gwint" },
          { word: "taśma klejąca", hint: "adhezja" },
          { word: "klej", hint: "połączenie" },
          { word: "nóż", hint: "krawędź" },
          { word: "widelec", hint: "zęby metalowe" },
          { word: "łyżka", hint: "wgłębienie" },
          { word: "talerz", hint: "płaskość" },
          { word: "miska", hint: "zagłębienie" },
          { word: "szklanka", hint: "przezroczystość" },
          { word: "butelka", hint: "szyjka" },
          { word: "termos", hint: "izolacja" },
          { word: "czajnik", hint: "gotowanie" },
          { word: "patelnia", hint: "powierzchnia" },
          { word: "garnek", hint: "objętość" },
          { word: "durszlak", hint: "perforacja" },
          { word: "deska do krojenia", hint: "ofiara" },
          { word: "mikser", hint: "wirowanie" },
          { word: "blender", hint: "rozdrabnianie" },
          { word: "toster", hint: "ciepło radiacyjne" },
          { word: "lodówka", hint: "konserwacja" },
          { word: "zamrażarka", hint: "kriostaza" },
          { word: "zmywarka", hint: "automatyzacja" },
          { word: "pralka", hint: "cykl" },
          { word: "suszarka", hint: "odparowanie" },
          { word: "żelazko", hint: "temperatura i ciężar" },
          { word: "odkurzacz", hint: "podciśnienie" },
          { word: "miotła", hint: "zamiatanie" },
          { word: "szczotka", hint: "włosie" },
          { word: "gąbka", hint: "porowatość" },
          { word: "ścierka", hint: "absorpcja" },
          { word: "wiadro", hint: "pojemnik cylindryczny" },
          { word: "szufelka", hint: "zagarnięcie" },
          { word: "kosz na śmieci", hint: "finał" },
          { word: "lampka", hint: "punktowe światło" },
          { word: "żarówka", hint: "żarzenie" },
          { word: "świeca", hint: "płomień" },
          { word: "zapałki", hint: "tarcie" },
          { word: "zapalniczka", hint: "iskra" },
          { word: "aparat fotograficzny", hint: "migawka" },
          { word: "obiektyw", hint: "optyka" },
          { word: "statyw", hint: "stabilizacja" },
          { word: "powerbank", hint: "rezerwuar energii" },
          { word: "ładowarka", hint: "transmisja prądu" },
          { word: "kabel", hint: "przewodność" },
          { word: "głośnik", hint: "membrana" },
          { word: "słuchawki", hint: "izolacja dźwięku" },
          { word: "mikrofon", hint: "amplifikacja głosu" },
          { word: "pilot", hint: "zdalne sterowanie" },
          { word: "konsola do gier", hint: "interaktywność" },
          { word: "joystick", hint: "manipulator" },
          { word: "pendrive", hint: "pamięć przenośna" },
          { word: "dysk twardy", hint: "archiwizacja" },
          { word: "router", hint: "dystrybucja sygnału" },
          { word: "drukarka", hint: "materializacja" },
          { word: "skaner", hint: "digitalizacja" },
          { word: "koperta", hint: "zamknięcie" },
          { word: "znaczek pocztowy", hint: "opłata" },
          { word: "długopis", hint: "atrament" },
          { word: "ołówek", hint: "grafit" },
          { word: "gumka", hint: "anulowanie" },
          { word: "temperówka", hint: "ostrość" },
          { word: "linijka", hint: "miara" },
          { word: "kalkulator", hint: "obliczenia" },
          { word: "zeszyt", hint: "kartki zszyte" },
          { word: "notes", hint: "notatki" },
          { word: "kalendarz", hint: "dni ułożone" },
          { word: "segregator", hint: "organizacja" },
          { word: "spinacz", hint: "łączenie kart" },
          { word: "zszywacz", hint: "perforacja łącząca" },
          { word: "dziurkacz", hint: "otwory" },
          { word: "nożyce", hint: "przecięcie" },
          { word: "biurko", hint: "powierzchnia pracy" },
          { word: "regał", hint: "półki" },
          { word: "szafa", hint: "przechowywanie pionowe" },
          { word: "komoda", hint: "szuflady" },
          { word: "łóżko", hint: "wypoczynek poziomy" },
          { word: "materac", hint: "elastyczność" },
          { word: "poduszka", hint: "miękkie wsparcie" },
          { word: "kołdra", hint: "ciepło" },
          { word: "prześcieradło", hint: "warstwa ochronna" },
          { word: "zasłony", hint: "regulacja światła" },
          { word: "dywan", hint: "pokrycie podłogi" },
          { word: "obraz", hint: "dekoracja płaska" },
        ],
        zawody: [
          { word: "lekarz", hint: "diagnoza" },
          { word: "programista", hint: "kod" },
          { word: "nauczyciel", hint: "lekcja" },
          { word: "policjant", hint: "patrol" },
          { word: "strażak", hint: "pożar" },
          { word: "kucharz", hint: "kuchnia" },
          { word: "mechanik", hint: "naprawa" },
          { word: "architekt", hint: "projekt" },
          { word: "prawnik", hint: "prawo" },
          { word: "dziennikarz", hint: "artykuł" },
          { word: "fryzjer", hint: "cięcie włosów" },
          { word: "kosmetyczka", hint: "pielęgnacja" },
          { word: "dentysta", hint: "zęby" },
          { word: "okulista", hint: "wzrok" },
          { word: "psycholog", hint: "umysł" },
          { word: "psychiatra", hint: "medykacja psychiczna" },
          { word: "weterynarz", hint: "zwierzęta" },
          { word: "pielęgniarka", hint: "opieka" },
          { word: "ratownik medyczny", hint: "pilność" },
          { word: "chirurg", hint: "skalpel" },
          { word: "anestezjolog", hint: "sen medyczny" },
          { word: "farmaceuta", hint: "recepta" },
          { word: "fizjoterapeuta", hint: "rehabilitacja" },
          { word: "dietetyk", hint: "żywienie" },
          { word: "trener personalny", hint: "forma fizyczna" },
          { word: "masażysta", hint: "ucisk" },
          { word: "nauczyciel przedszkolny", hint: "zabawa edukacyjna" },
          { word: "profesor", hint: "wykład" },
          { word: "bibliotekarz", hint: "katalogowanie" },
          { word: "księgowy", hint: "bilans" },
          { word: "audytor", hint: "weryfikacja" },
          { word: "analityk finansowy", hint: "prognoza" },
          { word: "makler", hint: "transakcja" },
          { word: "agent ubezpieczeniowy", hint: "ryzyko" },
          { word: "kasjer", hint: "wymiana pieniędzy" },
          { word: "sprzedawca", hint: "oferta" },
          { word: "kelner", hint: "serwis" },
          { word: "barista", hint: "kawa artystyczna" },
          { word: "barman", hint: "mikstura" },
          { word: "sommelier", hint: "wino" },
          { word: "piekarż", hint: "wypieki" },
          { word: "cukiernik", hint: "słodycze" },
          { word: "rzeźnik", hint: "mięso" },
          { word: "rybak", hint: "połów" },
          { word: "rolnik", hint: "uprawa" },
          { word: "ogrodnik", hint: "rośliny" },
          { word: "leśniczy", hint: "las" },
          { word: "górnik", hint: "wydobycie" },
          { word: "elektryk", hint: "prąd" },
          { word: "hydraulik", hint: "rury" },
          { word: "malarz", hint: "pokrycie farbą" },
          { word: "stolarz", hint: "drewno" },
          { word: "ślusarz", hint: "metal" },
          { word: "spawacz", hint: "łączenie wysoką temperaturą" },
          { word: "murarz", hint: "cegły" },
          { word: "dekarz", hint: "pokrycie dachu" },
          { word: "glazurnik", hint: "płytki" },
          { word: "tynkarz", hint: "warstwa ścienna" },
          { word: "kierowca autobusu", hint: "transport publiczny" },
          { word: "kierowca tira", hint: "długodystansowość" },
          { word: "taksówkarz", hint: "kurs" },
          { word: "pilot samolotu", hint: "wysokość" },
          { word: "stewardesa", hint: "serwis w powietrzu" },
          { word: "kapitan statku", hint: "morze" },
          { word: "maszynista", hint: "pociąg" },
          { word: "mechanik lotniczy", hint: "skrzydła" },
          { word: "programista gier", hint: "kod rozrywkowy" },
          { word: "administrator systemów", hint: "serwery" },
          { word: "specjalista IT", hint: "wsparcie techniczne" },
          { word: "grafik komputerowy", hint: "obraz cyfrowy" },
          { word: "designer", hint: "estetyka funkcjonalna" },
          { word: "fotograf", hint: "kadr" },
          { word: "kamerzysta", hint: "ruch obrazu" },
          { word: "reżyser", hint: "wizja" },
          { word: "aktor", hint: "rola" },
          { word: "wokalista", hint: "głos" },
          { word: "muzyk", hint: "instrument" },
          { word: "DJ", hint: "miksy" },
          { word: "producent muzyczny", hint: "ścieżki" },
          { word: "tancerz", hint: "choreografia" },
          { word: "poeta", hint: "wers" },
          { word: "pisarz", hint: "narracja" },
          { word: "redaktor", hint: "korekta" },
          { word: "tłumacz", hint: "transfer językowy" },
          { word: "archeolog", hint: "wykopaliska" },
          { word: "historyk", hint: "przeszłość" },
          { word: "geolog", hint: "skały" },
          { word: "biolog", hint: "życie" },
          { word: "chemik", hint: "reakcje" },
          { word: "fizyk", hint: "prawa natury" },
          { word: "astronom", hint: "kosmos" },
          { word: "matematyk", hint: "liczby" },
          { word: "socjolog", hint: "społeczeństwo" },
          { word: "politolog", hint: "władza" },
          { word: "ekonomista", hint: "gospodarka" },
          { word: "sędzia", hint: "wyrok" },
          { word: "prokurator", hint: "oskarżenie" },
          { word: "adwokat", hint: "obrona" },
        ],
        czynnosci: [
          { word: "bieganie", hint: "tempo" },
          { word: "czytanie", hint: "tekst" },
          { word: "gotowanie", hint: "przepis" },
          { word: "pisanie", hint: "litery" },
          { word: "rysowanie", hint: "ołówek" },
          { word: "pływanie", hint: "basen" },
          { word: "sprzątanie", hint: "porządek" },
          { word: "granie", hint: "rozrywka" },
          { word: "uczenie się", hint: "wiedza" },
          { word: "spanie", hint: "sen" },
          { word: "chodzenie", hint: "krok" },
          { word: "skakanie", hint: "oderwanie od ziemi" },
          { word: "tańczenie", hint: "rytm" },
          { word: "śpiewanie", hint: "melodia" },
          { word: "gwizdanie", hint: "powietrze przez usta" },
          { word: "krzyczenie", hint: "głośność" },
          { word: "szeptanie", hint: "cichość" },
          { word: "śmianie się", hint: "wesołość" },
          { word: "płakanie", hint: "łzy" },
          { word: "mycie", hint: "czystość" },
          { word: "suszenie", hint: "odparowanie wody" },
          { word: "prasowanie", hint: "gładkość" },
          { word: "składanie", hint: "minimalizacja objętości" },
          { word: "rozwieszanie", hint: "suspensja" },
          { word: "zamiatanie", hint: "gromadzenie" },
          { word: "wycieranie", hint: "tarcie" },
          { word: "szorowanie", hint: "intensywne tarcie" },
          { word: "odkurzanie", hint: "ssanie" },
          { word: "pranie", hint: "czyszczenie w wodzie" },
          { word: "krojenie", hint: "podział" },
          { word: "siekanie", hint: "rozdrabnianie" },
          { word: "obieranie", hint: "usuwanie skórki" },
          { word: "mieszanie", hint: "homogenizacja" },
          { word: "smażenie", hint: "tłuszcz i ciepło" },
          { word: "pieczenie", hint: "suchość wysokiej temperatury" },
          { word: "gotowanie", hint: "wrzenie" },
          { word: "duszenie", hint: "para" },
          { word: "grillowanie", hint: "bezpośrednie ciepło" },
          { word: "marynowanie", hint: "namaczanie" },
          { word: "pakowanie", hint: "umieszczanie w opakowaniu" },
          { word: "rozpakowywanie", hint: "odsłanianie" },
          { word: "zawijanie", hint: "otulanie" },
          { word: "wiązanie", hint: "węzeł" },
          { word: "rozwiązywanie", hint: "uwolnienie" },
          { word: "otwieranie", hint: "dostęp" },
          { word: "zamykanie", hint: "blokada" },
          { word: "pchanie", hint: "siła od siebie" },
          { word: "ciągnięcie", hint: "siła do siebie" },
          { word: "podnoszenie", hint: "przeciw grawitacji" },
          { word: "opuszczanie", hint: "z grawitacją" },
          { word: "rzucanie", hint: "impuls" },
          { word: "łapanie", hint: "przechwycenie" },
          { word: "kopanie", hint: "uderzenie nogą" },
          { word: "uderzanie", hint: "kontakt siłowy" },
          { word: "głaskanie", hint: "delikatny dotyk" },
          { word: "ściskanie", hint: "kompresja" },
          { word: "rozciąganie", hint: "elongacja" },
          { word: "zginanie", hint: "kąt" },
          { word: "prostowanie", hint: "linia" },
          { word: "kręcenie", hint: "rotacja wokół osi" },
          { word: "wykręcanie", hint: "torsja" },
          { word: "machanie", hint: "ruch wahadłowy" },
          { word: "kiwanie", hint: "potwierdzenie głową" },
          { word: "kręcenie głową", hint: "zaprzeczenie" },
          { word: "wskazywanie", hint: "palec kierunkowy" },
          { word: "klaskanie", hint: "dłonie razem" },
          { word: "tupanie", hint: "stopy o podłogę" },
          { word: "patrze nie", hint: "wzrok" },
          { word: "wąchanie", hint: "nos" },
          { word: "smakowanie", hint: "język" },
          { word: "słuchanie", hint: "uszy" },
          { word: "dotykanie", hint: "kontakt skórny" },
          { word: "myślenie", hint: "proces umysłowy" },
          { word: "pamiętanie", hint: "przypominanie" },
          { word: "zapominanie", hint: "utrata informacji" },
          { word: "wyobrażanie sobie", hint: "wizualizacja" },
          { word: "marzenie", hint: "pragnienie" },
          { word: "planowanie", hint: "przyszłość" },
          { word: "decydowanie", hint: "wybór" },
          { word: "rozwiązywanie", hint: "problem" },
          { word: "liczyć", hint: "matematyka" },
          { word: "mierzyć", hint: "kwantyfikacja" },
          { word: "ważyć", hint: "masa" },
          { word: "budować", hint: "konstrukcja" },
          { word: "burzyć", hint: "destrukcja" },
          { word: "naprawiać", hint: "przywracanie funkcji" },
          { word: "psuć", hint: "uszkadzanie" },
          { word: "tworzenie", hint: "powstanie" },
          { word: "niszczenie", hint: "anihilacja" },
          { word: "malowanie", hint: "nakładanie farby" },
          { word: "rzeźbienie", hint: "odejmowanie materiału" },
          { word: "klejenie", hint: "łączenie substancją" },
          { word: "szywanie", hint: "igła i nitka" },
          { word: "dzierganie", hint: "pętlowość" },
          { word: "haftowanie", hint: "ozdobne ściegi" },
          { word: "fotografowanie", hint: "uchwycenie momentu" },
          { word: "filmowanie", hint: "sekwencja klatek" },
        ],
        postacie: [
          { word: "Sherlock Holmes", hint: "dedukcja" },
          { word: "Batman", hint: "noc" },
          { word: "Harry Potter", hint: "czarodziej" },
          { word: "Spiderman", hint: "pajęczyna" },
          { word: "Darth Vader", hint: "ciemna strona" },
          { word: "Frodo", hint: "pierścień" },
          { word: "Homer Simpson", hint: "kanapa" },
          { word: "James Bond", hint: "agent" },
          { word: "Indiana Jones", hint: "artefakt" },
          { word: "Król Lew", hint: "sawanna" },
          { word: "Superman", hint: "peleryna czerwona" },
          { word: "Wonder Woman", hint: "lasso prawdy" },
          { word: "Iron Man", hint: "zbroja technologiczna" },
          { word: "Captain America", hint: "tarcza" },
          { word: "Thor", hint: "młot" },
          { word: "Hulk", hint: "transformacja zielona" },
          { word: "Black Widow", hint: "szpieg" },
          { word: "Wolverine", hint: "pazury" },
          { word: "Deadpool", hint: "regeneracja i gadatliwość" },
          { word: "Joker", hint: "chaos" },
          { word: "Harley Quinn", hint: "szaleństwo kolorowe" },
          { word: "Catwoman", hint: "elegancka złodziejka" },
          { word: "Pingwin", hint: "parasol" },
          { word: "Bane", hint: "maska oddechowa" },
          { word: "Lex Luthor", hint: "intelekt zły" },
          { word: "Thanos", hint: "rękawica" },
          { word: "Loki", hint: "podstęp" },
          { word: "Gandalf", hint: "szary wędrowiec" },
          { word: "Aragorn", hint: "następca tronu" },
          { word: "Legolas", hint: "łucznik elf" },
          { word: "Gimli", hint: "krasnolud" },
          { word: "Gollum", hint: "mój skarb" },
          { word: "Sauron", hint: "oko" },
          { word: "Bilbo", hint: "tam i z powrotem" },
          { word: "Luke Skywalker", hint: "moc" },
          { word: "Leia Organa", hint: "księżniczka rebelii" },
          { word: "Han Solo", hint: "przemytnik" },
          { word: "Yoda", hint: "odwrócona składnia" },
          { word: "Obi-Wan Kenobi", hint: "wysoka pozycja" },
          { word: "Chewbacca", hint: "futro i ryk" },
          { word: "R2-D2", hint: "gwizdy" },
          { word: "C-3PO", hint: "protokół" },
          { word: "Kylo Ren", hint: "rozdwojenie" },
          { word: "Rey", hint: "przebudzenie" },
          { word: "Jon Snow", hint: "zima nadchodzi" },
          { word: "Daenerys Targaryen", hint: "matka smoków" },
          { word: "Tyrion Lannister", hint: "niski wzrost, wysoki intelekt" },
          { word: "Arya Stark", hint: "lista" },
          { word: "Cersei Lannister", hint: "królowa intryg" },
          { word: "Hermiona Granger", hint: "wiedza" },
          { word: "Ron Weasley", hint: "rudy przyjaciel" },
          { word: "Albus Dumbledore", hint: "mądrość brodą" },
          { word: "Severus Snape", hint: "podwójny agent" },
          { word: "Voldemort", hint: "ten, którego..." },
          { word: "Draco Malfoy", hint: "blond wróg" },
          { word: "Hagrid", hint: "olbrzym delikatny" },
          { word: "Dobby", hint: "skarpetka" },
          { word: "Katniss Everdeen", hint: "kos" },
          { word: "Peeta Mellark", hint: "piekarnik" },
          { word: "Neo", hint: "matrix" },
          { word: "Trinity", hint: "trójca" },
          { word: "Morpheus", hint: "pigułka" },
          { word: "Forrest Gump", hint: "bieganie prostoduszne" },
          { word: "Jack Sparrow", hint: "kapitan?" },
          { word: "Simba", hint: "koło życia" },
          { word: "Mufasa", hint: "chmury" },
          { word: "Elsa", hint: "lód" },
          { word: "Anna", hint: "ciepło serca" },
          { word: "Olaf", hint: "bałwan marzy" },
          { word: "Buzz Lightyear", hint: "nieskończoność" },
          { word: "Woody", hint: "kowboj zabawka" },
          { word: "Shrek", hint: "ogr warstwowy" },
          { word: "Fiona", hint: "transformacja zmierzchowa" },
          { word: "Osioł", hint: "gadatliwy towarzysz" },
          { word: "Gru", hint: "despicable" },
          { word: "Minionki", hint: "żółte nieporadne" },
          { word: "Nemo", hint: "zagubiona płetwa" },
          { word: "Dory", hint: "pamięć krótka" },
          { word: "Marlin", hint: "ojciec troskliwy" },
          { word: "Wall-E", hint: "śmieci i miłość" },
          { word: "SpongeBob", hint: "gąbka kwadratowa" },
          { word: "Patrick", hint: "gwiazda głupia" },
          { word: "Squidward", hint: "kałamarnica zrzędliwa" },
          { word: "Mickey Mouse", hint: "okrągłe uszy" },
          { word: "Donald Duck", hint: "kaczor wybuchowy" },
          { word: "Goofy", hint: "niezgrabny pies" },
          { word: "Scooby-Doo", hint: "pies tchórz detektyw" },
          { word: "Shaggy", hint: "głodny hipis" },
          { word: "Tom", hint: "kot przegrywający" },
          { word: "Jerry", hint: "mysz wygrywająca" },
          { word: "Garfield", hint: "lasagne i lenistwo" },
          { word: "Sonic", hint: "szybkość niebieska" },
          { word: "Mario", hint: "hydraulik skaczący" },
          { word: "Luigi", hint: "zielony brat" },
          { word: "Link", hint: "bohater milczący" },
          { word: "Pikachu", hint: "żółty elektryczny" },
        ],
        zwierzęta: [
          { word: "pies", hint: "lojalność" },
          { word: "kot", hint: "niezależność" },
          { word: "koń", hint: "grzywa" },
          { word: "krowa", hint: "mleko" },
          { word: "świnia", hint: "błoto" },
          { word: "owca", hint: "wełna" },
          { word: "koza", hint: "wspinaczka" },
          { word: "kura", hint: "jaja" },
          { word: "kogut", hint: "pianie" },
          { word: "kaczka", hint: "pływanie" },
          { word: "gęś", hint: "szyja długa" },
          { word: "indyk", hint: "balonik" },
          { word: "królik", hint: "uszy długie" },
          { word: "chomik", hint: "policzki" },
          { word: "świnka morska", hint: "piszczenie" },
          { word: "papuga", hint: "powtarzanie" },
          { word: "kanark", hint: "śpiew melodyjny" },
          { word: "rybka akwariowa", hint: "szkło" },
          { word: "żółw lądowy", hint: "powolność" },
          { word: "żółw wodny", hint: "pancerz i pływanie" },
          { word: "jaszczurka", hint: "słońce" },
          { word: "kameleon", hint: "mimikra" },
          { word: "wąż", hint: "łuski bez nóg" },
          { word: "pyton", hint: "duszenie" },
          { word: "kobra", hint: "kaptur" },
          { word: "krokodyl", hint: "szczęki" },
          { word: "aligator", hint: "bagno" },
          { word: "iguana", hint: "grzbiet kolczasty" },
          { word: "salamandra", hint: "regeneracja" },
          { word: "żaba", hint: "skoki" },
          { word: "ropucha", hint: "brodawki" },
          { word: "traszka", hint: "metamorfoza" },
          { word: "pingwin", hint: "fraki" },
          { word: "struś", hint: "nielot" },
          { word: "emu", hint: "australijski nielot" },
          { word: "orzeł", hint: "szybowanie" },
          { word: "sokół", hint: "nurkowanie" },
          { word: "jastrząb", hint: "polowanie lotne" },
          { word: "sowa", hint: "noc" },
          { word: "bocian", hint: "komin" },
          { word: "żuraw", hint: "taniec" },
          { word: "czapla", hint: "czekanie w wodzie" },
          { word: "pelikan", hint: "worek" },
          { word: "kormoran", hint: "nurkowanie głębokie" },
          { word: "mewa", hint: "wybrzeże" },
          { word: "albatros", hint: "rozpiętość" },
          { word: "kolibr", hint: "zawieszenie" },
          { word: "dzięcioł", hint: "dziobanie" },
          { word: "wrona", hint: "inteligencja ptasia" },
          { word: "sroka", hint: "połysk" },
          { word: "gołąb", hint: "gruchanie" },
          { word: "jaskółka", hint: "wiosna" },
          { word: "wróbel", hint: "powszechność" },
          { word: "słowik", hint: "śpiew nocny" },
          { word: "skowronek", hint: "lot pionowy" },
          { word: "zięba", hint: "śpiew wiosenny" },
          { word: "gil", hint: "czerwona pierś" },
          { word: "sikorka", hint: "akrobacja" },
          { word: "kukułka", hint: "podrzucanie" },
          { word: "paw", hint: "wachlarz" },
          { word: "bażant", hint: "upierzenie metaliczne" },
          { word: "flaming", hint: "balansowanie" },
          { word: "tukan", hint: "dziób kolorowy" },
          { word: "ara", hint: "tropikalna papuga" },
          { word: "kakadu", hint: "czubek" },
          { word: "niedźwiedź", hint: "hibernacja" },
          { word: "niedźwiedź polarny", hint: "biel i lód" },
          { word: "panda", hint: "bambus" },
          { word: "wilk", hint: "wataha" },
          { word: "lis", hint: "chytrość" },
          { word: "jenot", hint: "maska" },
          { word: "szop pracz", hint: "mycie" },
          { word: "wydra", hint: "śliskie futro" },
          { word: "borsuk", hint: "nora" },
          { word: "kret", hint: "ślepota i kopanie" },
          { word: "jeż", hint: "kolce" },
          { word: "wiewiórka", hint: "zapasy" },
          { word: "mysz", hint: "malutki gryzoń" },
          { word: "szczur", hint: "inteligencja gryzonia" },
          { word: "łasica", hint: "smukłość" },
          { word: "gronostaj", hint: "biała zima" },
          { word: "kuna", hint: "poddasze" },
          { word: "ryś", hint: "pędzelki" },
          { word: "gepard", hint: "prędkość" },
          { word: "lampart", hint: "plamy" },
          { word: "jaguar", hint: "różyczki" },
          { word: "puma", hint: "samotnik" },
          { word: "lew", hint: "grzywa królewska" },
          { word: "tygrys", hint: "paski" },
          { word: "zebra", hint: "kod kreskowy" },
          { word: "żyrafa", hint: "szyja najdłuższa" },
          { word: "słoń", hint: "trąba" },
          { word: "nosorożec", hint: "róg" },
          { word: "hipopotam", hint: "zanurzenie" },
          { word: "wielbłąd", hint: "garby" },
          { word: "kangur", hint: "torba" },
          { word: "koala", hint: "eukaliptus" },
        ],
      };

      const players = [];
      let game = null;

      // Czekamy aż DOM się załaduje
      document.addEventListener("DOMContentLoaded", () => {
        // Dodawanie event listenerów
        document
          .getElementById("addPlayerBtn")
          .addEventListener("click", addPlayer);
        document
          .getElementById("resetBtn")
          .addEventListener("click", resetGame);
        document
          .getElementById("startBtn")
          .addEventListener("click", startGame);
        document
          .getElementById("revealBtn")
          .addEventListener("click", revealCard);
        document
          .getElementById("nextBtn")
          .addEventListener("click", nextPlayer);

        // Enter w inpucie dodaje gracza
        document
          .getElementById("playerName")
          .addEventListener("keypress", (e) => {
            if (e.key === "Enter") addPlayer();
          });
      });

      function addPlayer() {
        const input = document.getElementById("playerName");
        const name = input.value.trim();

        if (!name) return;

        players.push(name);
        input.value = "";
        renderPlayers();
      }

      function removePlayer(index) {
        players.splice(index, 1);
        renderPlayers();
      }

      function renderPlayers() {
        const list = document.getElementById("playersList");
        list.innerHTML = "";

        players.forEach((player, index) => {
          const li = document.createElement("li");

          const nameSpan = document.createElement("span");
          nameSpan.textContent = player;

          const removeBtn = document.createElement("button");
          removeBtn.textContent = "❌";
          removeBtn.style.width = "auto";
          removeBtn.style.padding = "5px 10px";
          removeBtn.addEventListener("click", () => removePlayer(index));

          li.appendChild(nameSpan);
          li.appendChild(removeBtn);
          list.appendChild(li);
        });
      }

      function resetGame() {
        players.length = 0;
        renderPlayers();
      }

      function startGame() {
        const impostors = Number(
          document.getElementById("impostorsCount").value,
        );
        const impostorGetsHint =
          document.getElementById("impostorHint").checked;
        const category = document.getElementById("category").value;

        if (players.length < 3) {
          alert("Minimum 3 graczy");
          return;
        }

        if (impostors >= players.length) {
          alert("Za dużo impostorów");
          return;
        }

        const wordObj =
          WORDS[category][Math.floor(Math.random() * WORDS[category].length)];

        const roles = Array(players.length).fill("player");
        for (let i = 0; i < impostors; i++) roles[i] = "impostor";
        shuffleArray(roles);

        game = {
          players: [...players],
          roles,
          word: wordObj.word,
          hint: wordObj.hint,
          impostorGetsHint,
          currentPlayer: 0,
        };

        document.getElementById("setupScreen").classList.add("hidden");
        document.getElementById("cardScreen").classList.remove("hidden");

        updateCardScreen();
      }

      function shuffleArray(array) {
        for (let i = array.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [array[i], array[j]] = [array[j], array[i]];
        }
      }

      function updateCardScreen() {
        document.getElementById("currentPlayerTitle").innerText =
          `Gracz: ${game.players[game.currentPlayer]}`;

        document.getElementById("cardRole").innerText = "";
        document.getElementById("cardWord").innerText = "";

        document.getElementById("revealBtn").classList.remove("hidden");
        document.getElementById("cardContent").classList.add("hidden");
      }

      function revealCard() {
        const role = game.roles[game.currentPlayer];
        const roleEl = document.getElementById("cardRole");
        const wordEl = document.getElementById("cardWord");

        if (role === "impostor") {
          roleEl.innerText = "Jesteś IMPOSTOREM 😈";
          roleEl.style.color = "#ef4444";
          wordEl.innerText = game.impostorGetsHint
            ? `Podpowiedź: ${game.hint}`
            : "Brak podpowiedzi";
        } else {
          roleEl.innerText = "Jesteś graczem 🙂";
          roleEl.style.color = "#10b981";
          wordEl.innerText = `Twoje słowo: ${game.word}`;
        }

        document.getElementById("revealBtn").classList.add("hidden");
        document.getElementById("cardContent").classList.remove("hidden");
      }

      function nextPlayer() {
        game.currentPlayer++;

        if (game.currentPlayer >= game.players.length) {
          const random = Math.floor(Math.random() * players.length);
          alert(
            `Wszyscy znają swoje role. Start gry! Zaczyna: ${players[random]}`,
          );
          location.reload();
          return;
        }

        updateCardScreen();
      }
    </script>
  </body>
</html>
