# PRODUCT_SPEC.md

## Product Vision
Een mobiele web-app voor scoutingleiding om team- en bonuspunten snel, transparant en auditbaar bij te houden over meerdere kampen.

## Doelen
- Snel punten toekennen op telefoon.
- Eén waarheid voor leaderboard en dagtotalen.
- Volledige audit trail per wijziging.
- Simpel beheer van kampen, teams en deelnemers.

## Rollen
- **Superadmin**: beheert alle kampen en configuratie.
- **Leiding (kamp-sessie via inlogcode)**: kent punten toe en bekijkt leaderboard.
- **Display gebruiker (alleen lezen)**: toont leaderboard op scherm.

## User Stories (35)

### Kampbeheer
1. Als superadmin wil ik een kamp kunnen aanmaken zodat ik een nieuwe competitie kan starten.  
   **Acceptatiecriteria:** naam, begin/einddatum en unieke inlogcode verplicht; kamp is direct zichtbaar in overzicht.
2. Als superadmin wil ik een kamp kunnen wijzigen zodat datums en naam actueel blijven.  
   **Acceptatiecriteria:** wijzigingen worden opgeslagen; wijziging krijgt timestamp in audit log.
3. Als superadmin wil ik een kamp kunnen archiveren zodat oude kampen niet in actieve lijsten staan.  
   **Acceptatiecriteria:** gearchiveerde kampen zijn read-only voor leiding.
4. Als superadmin wil ik alleen unieke inlogcodes per kamp zodat toegang eenduidig is.  
   **Acceptatiecriteria:** duplicate code wordt geweigerd met foutmelding.
5. Als superadmin wil ik kampstatus zien (gepland/actief/afgelopen) zodat ik snel context heb.  
   **Acceptatiecriteria:** status op basis van begin/einddatum.

### Teams en deelnemers
6. Als superadmin wil ik teams aan een kamp kunnen toevoegen zodat punten per patrouille worden bijgehouden.  
   **Acceptatiecriteria:** teamnaam verplicht, gekoppeld aan exact één kamp.
7. Als superadmin wil ik teamnamen kunnen wijzigen zodat deze overeenkomen met kampindeling.  
   **Acceptatiecriteria:** historische transacties blijven aan hetzelfde team-ID gekoppeld.
8. Als superadmin wil ik deelnemers per team kunnen registreren zodat bonuspunten op naam mogelijk zijn.  
   **Acceptatiecriteria:** deelnemernaam gekoppeld aan team, verwijderbaar en bewerkbaar.
9. Als superadmin wil ik tussen 1 en 6 teams ondersteunen zodat zowel weekend- als weekkampen passen.  
   **Acceptatiecriteria:** validatie waarschuwt buiten bereik maar blokkeert niet op DB-niveau.
10. Als superadmin wil ik 2–12 deelnemers per team kunnen beheren zodat schaal past bij praktijk.  
   **Acceptatiecriteria:** UI toont waarschuwing bij afwijking.

### Authenticatie en sessies
11. Als leiding wil ik met kamp-inlogcode kunnen inloggen zodat ik snel toegang krijg zonder aparte accounts.  
   **Acceptatiecriteria:** geldige code opent kamp-dashboard; ongeldige code geeft fout.
12. Als leiding wil ik ingelogd blijven gedurende een dienst zodat ik niet telkens opnieuw hoef in te loggen.  
   **Acceptatiecriteria:** sessie-cookie met time-out (bijv. 12 uur).
13. Als leiding wil ik kunnen uitloggen zodat volgende gebruiker niet onder mijn sessie werkt.  
   **Acceptatiecriteria:** sessie direct ongeldig.
14. Als superadmin wil ik apart kunnen inloggen zodat kampbeheer afgeschermd is.  
   **Acceptatiecriteria:** admin-endpoints vereisen admin-auth.

### Activiteiten
15. Als superadmin wil ik standaardactiviteiten kunnen beheren zodat veelgebruikte acties snel selecteerbaar zijn.  
   **Acceptatiecriteria:** activiteiten hebben naam, categorie en standaardpunten.
16. Als leiding wil ik een standaardactiviteit kunnen kiezen bij puntentoekenning zodat invoer sneller gaat.  
   **Acceptatiecriteria:** dropdown met kamp + globale activiteiten.
17. Als leiding wil ik ad-hoc activiteit kunnen toevoegen zodat onverwachte situaties meteen te registreren zijn.  
   **Acceptatiecriteria:** nieuwe activiteit direct beschikbaar in huidige kamp.
18. Als superadmin wil ik categorieën (spel/activiteit/gedrag) zodat filtering in dashboard mogelijk is.  
   **Acceptatiecriteria:** categorie verplicht veld.
19. Als leiding wil ik alleen punten in stappen van 5 kunnen invoeren zodat systeemconsistentie behouden blijft.  
   **Acceptatiecriteria:** alleen -15,-10,-5,+5..+50 toegestaan.
20. Als leiding wil ik negatieve punten beperkt houden zodat strafpunten consistent zijn.  
   **Acceptatiecriteria:** minpunten alleen -5, -10, -15.

### Puntentransacties
21. Als leiding wil ik punten aan een team kunnen toekennen zodat voortgang direct zichtbaar is.  
   **Acceptatiecriteria:** transactie opgeslagen met tijd, team, punten, activiteit en invoerder.
22. Als leiding wil ik optioneel een persoonsnaam bij bonuspunten kunnen invullen zodat erkenning zichtbaar is.  
   **Acceptatiecriteria:** veld optioneel; bij vullen wordt naam opgeslagen in transactie.
23. Als leiding wil ik een korte notitie kunnen toevoegen zodat context van ad-hoc acties duidelijk blijft.  
   **Acceptatiecriteria:** max lengte en zichtbaar in audit.
24. Als leiding wil ik foutieve transactie kunnen corrigeren zodat score betrouwbaar blijft.  
   **Acceptatiecriteria:** correctie via tegenboeking; originele transactie blijft auditbaar.
25. Als leiding wil ik direct bevestiging zien na opslaan zodat ik weet dat invoer gelukt is.  
   **Acceptatiecriteria:** succesmelding en vernieuwde scorekaart.

### Leaderboard en dashboard
26. Als leiding wil ik totaalscore per team zien zodat ranking duidelijk is.  
   **Acceptatiecriteria:** sortering aflopend op totaalscore.
27. Als leiding wil ik dagtotaal per team zien zodat dagelijkse prestaties inzichtelijk zijn.  
   **Acceptatiecriteria:** aggregatie op lokale kampdatum.
28. Als leiding wil ik zien welke punten vandaag zijn verdiend zodat oorzaak van ranking zichtbaar is.  
   **Acceptatiecriteria:** lijst met transacties laatste 24 uur/‘vandaag’.
29. Als display-gebruiker wil ik fullscreen leaderboard kunnen tonen zodat kampdeelnemers score kunnen volgen.  
   **Acceptatiecriteria:** read-only view, groot lettertype.
30. Als gebruiker wil ik na refresh de nieuwste gegevens zien zodat handmatige update voldoende is.  
   **Acceptatiecriteria:** elke page load haalt actuele data op.
31. Als leiding wil ik op categorie kunnen filteren zodat ik spel/gedrag effect kan vergelijken.  
   **Acceptatiecriteria:** filter past rankingberekeningen aan of toont breakdown.

### Audit en rapportage
32. Als superadmin wil ik audit logs inzien zodat ik kan herleiden wie wat en wanneer toevoegde.  
   **Acceptatiecriteria:** log bevat actor, timestamp, actie en oude/nieuwe waarden.
33. Als superadmin wil ik kampresultaten kunnen exporteren zodat ik een eindrapport kan maken/printen.  
   **Acceptatiecriteria:** CSV-export met teamscores en transacties.
34. Als superadmin wil ik op datum kunnen filteren in audit zodat incidentonderzoek sneller is.  
   **Acceptatiecriteria:** van/tot filter op timestamps.
35. Als leiding wil ik alleen toegang tot mijn kampdata zodat kampen gescheiden blijven.  
   **Acceptatiecriteria:** elke query scoped op kamp_id uit sessie.

## Niet-functionele eisen
- Mobile-first UI, responsive voor telefoon en desktop.
- p95 responstijd < 500ms op lokaal netwerk.
- Alle mutaties auditbaar.
- Timezone vast op Europe/Amsterdam.
- Dataretentie: onbeperkt tot handmatige archivering/verwijdering.
