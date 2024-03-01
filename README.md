# Arbeitsagentur NewSkills API 
Die Bundesagentur für Arbeit hat kein Angebot für Personen erstellt, die sich beruflich umorientieren möchten: "New Plan - Kenne dein Können" für die Suche nach "Inspiration".

Konket heißt es auf der Website von [New Plan](https://www.arbeitsagentur.de/k/newplan): 

"Im Beruf vorankommen, die eigenen Stärken erkennen, eine neue Perspektive finden – New Plan unterstützt Sie auf Ihrem beruflichen Weg!"


Eine Authentifizierung erfolgt über die client_id, die als Parameter

**cliend_id:** 7679c728-db18-43ce-beb8-52a9d571d419

Die Client-ID muss bei allen GET-requests als Header-Feld 'X-API-Key' übergeben werden.

## Berufsbezogene Informationen

Die Abfrage berufsbezogener Informationen erfolgt bei NewPlan über eine Serie von GET-requests:

**URL:** https://rest.arbeitsagentur.de/infosysbub/dkz-rest/pc/v1/berufe/{BerufenetID}/

Mithilfe der BerufenetID (z.B. 129987, vgl. [BERUFENET-API](https://github.com/AndreasFischer1985/berufenet-api)) lassen sich Basisinformationen zu Berufen abfragen, insb. der zugehörige KldbCode (z.B. B%2043104). Mit dem kldB-Code kann in einem zweiten Schritt die DkzId angefragt werden, welche für die finale Abfrage benötigt wird.

**URL:** https://rest.arbeitsagentur.de/infosysbub/dkz-rest/pc/v1//kldb2010?codenr={KldbCode}/

Mithilfe des KldbCode (z.B. B%2043104) lassen sich weitere Informationen zum angefragten Beruf abfragen, insb. die DkzId (z.B. 91527), welche für die finale Abfrage benötigt wird.

**URL:** https://rest.arbeitsagentur.de/sete/suggest/pc/v1/inspiration/gattungen/{DkzId}?ausgangsberufDkzId={BerufenetId}

Mithilfe von DkzId und BerufenetId lassen sich schließlich die Kernergebnisse von NewPlan abfragen, namentlich nahe und ferne Berufe zur angegebenen BerufenetId, jeweils mit dem Anteil der diesbezüglichen Jobwechsel.

Darüber hinauslassen sich über folgende Entpunkte weitere Daten über DkzId und BerufenetId abfragen (mehrere kommaseparierte Angaben möglich):
* Berufsvorschläge unter https://rest.arbeitsagentur.de/sete/suggest/pc/v1/inspiration/berufe?berufsgattungDkzIds={DkzId}
* Mediangehalt unter https://rest.arbeitsagentur.de/infosysbub/entgeltatlas/pc/v1/mediandaten?dkzIds={BerufenetId}


### Beispiel:

```bash
info1=$(curl -m 60 \
-H "X-API-Key:7679c728-db18-43ce-beb8-52a9d571d419" \
'https://rest.arbeitsagentur.de/infosysbub/dkz-rest/pc/v1/berufe/129987/')

info2=$(curl -m 60 \
-H "X-API-Key:7679c728-db18-43ce-beb8-52a9d571d419" \
'https://rest.arbeitsagentur.de/infosysbub/dkz-rest/pc/v1//kldb2010?codenr=B%2043104')

info3=$(curl -m 60 \
-H "X-API-Key:7679c728-db18-43ce-beb8-52a9d571d419" \
'https://rest.arbeitsagentur.de/sete/suggest/pc/v1/inspiration/gattungen/91527?ausgangsberufDkzId=129987')
```

