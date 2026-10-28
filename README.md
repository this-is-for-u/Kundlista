# Pågående kunder – installationsguide

Appen delar en live-lista över kunder som kollegorna sköter, så ingen snor en kund. Den körs på GitHub Pages och använder Firebase (gratis Spark-plan) för inloggning och databas.

## 1. Skapa Firebase-projekt
1. Gå till https://console.firebase.google.com och välj **Add project**. Google Analytics behövs inte.
2. **Build → Authentication → Get started → Sign-in method → Email/Password → Enable.**
3. **Build → Firestore Database → Create database.** Välj en EU-region (t.ex. `europe-north1` eller `eur3`) och **production mode**.
4. Öppna fliken **Rules** i Firestore, ersätt allt med innehållet i `firestore.rules` och tryck **Publish**.

## 2. Koppla appen till projektet
1. **Project settings (kugghjulet) → Your apps → Web (</>)** och registrera en webbapp.
2. Kopiera värdena `apiKey`, `authDomain`, `projectId` och `appId`.
3. Klistra in dem överst i `<script type="module">` i `index.html` (där det står `FYLL_I`).
   `apiKey` är inte hemlig. Skyddet ligger i inloggningen och reglerna i `firestore.rules`.

## 3. Publicera på GitHub Pages
1. Skapa ett repo på GitHub och ladda upp alla filer i den här mappen (rotnivå).
2. **Settings → Pages → Deploy from a branch → main / (root) → Save.**
3. Adressen blir `https://DITTANVANDARNAMN.github.io/REPONAMN/`.
4. **Viktigt:** Firebase → **Authentication → Settings → Authorized domains → Add domain** och lägg till `DITTANVANDARNAMN.github.io`. Annars fungerar inte inloggningen.

## 4. Gör dig själv till chef
1. Öppna appen och skapa ett konto (du får skärmen "Väntar på godkännande").
2. I Firebase: **Authentication → Users**, kopiera din **User UID**.
3. **Firestore → Start collection** med namnet `members`. Dokument-ID = din UID. Lägg till fältet `admin` (boolean) = `true`.
4. Ladda om appen. Nu ser du listan, och kan ta bort andras poster.

## 5. Godkänn kollegor
Kollegan skapar eget konto i appen. Du hittar hen under **Authentication → Users** (sök på e-post), kopierar UID och skapar dokumentet `members/<UID>` med fältet `admin` = `false`. Därefter ser kollegan listan (hen trycker **Kolla igen**). För att ta bort någons åtkomst raderar du dokumentet.

## 6. Lägg på telefonens hemskärm
- **iPhone (Safari):** Dela → Lägg till på hemskärmen.
- **Android (Chrome):** Meny → Installera app / Lägg till på startskärmen.

## Så fungerar behörigheten
- Bara godkända medarbetare kan läsa listan.
- Var och en kan bara ändra och avsluta sina egna kunder. Det avgörs av databasreglerna, inte bara av knapparna.
- Chefer (`admin: true`) kan även ta bort andras poster.
- Sidan kräver internet.

## Personuppgifter
Kundnamn och telefonens sista 4 siffror är personuppgifter. Bestäm med den som ansvarar för GDPR hur länge de får finnas kvar. Avslutade kunder raderas direkt när någon trycker Klar, men glömda poster ligger kvar tills någon tar bort dem.
