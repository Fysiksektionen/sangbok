# Fysiksektionens sångbok
[![Build LaTeX documents](https://github.com/oskarr/Sangbok/actions/workflows/compile.yml/badge.svg)](https://github.com/oskarr/Sangbok/actions/workflows/compile.yml) [![Parse into JSON](https://github.com/oskarr/Sangbok/actions/workflows/json-parse.yml/badge.svg)](https://github.com/oskarr/Sangbok/actions/workflows/json-parse.yml)

Du letar förmodligen efter [Fysiksektionen/Sangbok](https://github.com/Fysiksektionen/Sangbok). Detta repo är enbart för privat bruk, då Overleaf enbart kan pulla från main-branchen (vilken jag inte vill röra i originalrepot). Notera att "Parse into JSON" ovan enbart beskriver om alla låtar kunde läsas av, inte om de lästes av korrekt.

## Användning
Skriptet `compile.sh` kompilerar alla .tex-filer i huvudmappens direkta undermappar (dvs. inte de som är i Äldre-mappen). Notera att då detta repo _inte_ innehåller .aux-filer, måste allt kompileras **2 gånger** för att sidspalten ska komma med ordentligt om det görs manuellt (du behöver dock inte tänka på detta om du använder `compile.sh`).

## Att fixa
* Parsing av tabeller
* Inläsning av upphovsperson och melodi.
* Fixa MathMode-ersättningar för ODE till en husvagn.
* Idiotsäkra parsningen av fetstilt och kursiv text, samt gör något liknande för `\mcode{...}`.
* CI-Kompileringen av innehållet är inte helt som originalet.
* Formatet för Système International är ej standardiserat.

## Förändringar
För att underlätta parsing, är det bra om saker är standardiserade. Denna branch har därför ändrat:
* Alla titlar, så att bokstäver efteråt hamnar _efter_ dollartecknena, och att `\Large` är en för-modifier, utan måsvingar.
* Bytt namn Jesus lever till Ny-18 (Ny-17 var dubblett.).
* Textregistrets sida 2, samt namnregistrets sida 2 och 4 har nu samma marginal som resten av sidorna i registret.
* Fler ändringar kommer, och det återstår att testa så att resultatet blir likvärdigt.

## Ändringar per kapitel
### Visuellt oförändrade kapitel
* Gamma
* Epsilon
* Eta
* Kappa
* Omikron

### Mindre layout-ändringar
* Alfa - Auld lang syne har försumbart ändrat marginalavstånd
* Beta - har en radbrytnings-bugg på första sidan, som är fixad på ett rätt fult sätt.
* Delta - Supreglerna på första sidan radbryts på fel ställen (fyllehundar, middagstupplur, ordinarie)
* Zeta - På sista sidan radbryts "sittningar" på fel ställe.
* Iota - ODE till en husvagn har radbrytning i friktion, till skillnad från innan.
* My - Ändrad så att den följer marginalstandarden (när matematikhatarvisan insattes, hamnade marginalerna ur synk).
* Ny - Radavståndet något ändrat på sidan 16 och 18. Mariginalen är justerad på sista sidan.
* Sigma - Ändrad så att den följer marginalstandarden.

### Mindre innehållsändringar
* Alfa - Hugo Alvén ändrad till Hugo Alfvén.
    * Internationalen - 
        * "slå oss bi" -> "stå oss bi"
        * "han" -> "hen"
        * Fixad typo i "lättingen"
* Delta
    * 7b har nu ett mellanslag efter indexet.
    * 14e har en punkt efter indexet.
    * Mera Skåne har fått "beklagd" utbytt mot "bleklagd".
    * Livet är härligt har fått melodin ändrad till "Polyushko-polye" från "Röda kavalleriet".
* Theta - 11b har nu kursiv källreferens.
* Zeta - 3 "fårä" -> "får"
* Lambda - 9 skrevs av A. de la Halle, inte Hale

## Parser-skillnader
Dvs. skillnader mellan den digitala versionen och den fysiska just nu.
* Alfa
    * I den digitala sångboken är brodersbandet ersatt med systrabandet (men inte i den fysiska). Parsern tar ej hänsyn till detta.
    * I övrigt skiljer sig enbart punktuationer och versaliseringar mellan de två versionerna.
    * Alfa har fler verser i den digitala.
* Beta
    * 1, 7 - saknar citationstecken
    * 3 - Den fysiska upplagan har av-grabbifierats.


### Användbar RegEx
(För manuell ändring till digital version.)
* `\\huge\{(\w{0,2}(\$.*?\$)?)\s(.*)\}` => `\chaptertitle{$1}{$3}` - kapiteltitel
* * `\\Large\s([\$]\\[a-z]+?[,\d\.]*?\$[a-z]?)\.\s([^\\]*?)\s?\\{2}` => `\songtitle{$1}{$2}` vanliga låttitlar
* `\\Large\s([\$]\\[a-z]+?[,\d\.]*?\$[a-z]?)\.\s([^\\]*).*` => `\songtitle{$1}{$2}` - vanliga låttitlar (utan nyrad på slutet)
* `\\Large\s[\$]\\[\w\/]+?\\[a-z]+?\$\.` - numrerade med specialsymboler
* `\\Large\so\d+?[a-z]?\. ` - omikron
* `\\begin\{flushright\}\n?\s{0,8}\\textit\{(.*)\}\n?\\end\{flushright\}` => `\auth{$1}` - upphovsperson