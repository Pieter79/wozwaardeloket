# wozwaardeloket

Scraper for WOZ data from wozwaardeloket: Efficient property valuation insights.

Scrape/extract wozwaardeloket.nl



API 1
https://api.kadaster.nl/lvwoz/wozwaardeloket-api/v1/wozwaarde/nummeraanduiding/NUMBER(=NUMMERAANDUIDINGID)
You need the NUMMERAANDUIDINGID
To find them:

Find all addresses in The Netherlands (streetname, housenumber, housenumbersuffix, postalcode, city)

A. Find PostalCodes
Search all possible PostalCodes (only the 4 numbers).
q=1000 - q=9999 = 9.000 combinations = 9.000 GET requests.
https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=1000&fq=type:postcode&rows=3000
to
https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=9999&fq=type:postcode&rows=3000

Return should be something like this:
{"type":"postcode","weergavenaam":"'Streetname, PostalCode City","id":"pcd-abcdefg123456789","score":5.0248566}

Extract the found PostalCodes
PC6 (4 numbers and 2 letters) 
RegEx: [1-9][0-9]{3}[A-Z]{2}
It should return 466.321 unique combinations

PC5 (4 numbers and 1 letter) 
RegEx: [1-9][0-9]{3}[A-Z]
It should return 33.649 unique combinations

Use PC5 or PC6 to find all addresses.
PC6
https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=PC6&fq=type:adres&rows=3000

B. PC5
https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=PC5&fq=type:adres&rows=3000
For PostalCode 3071A you can use:
https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=3071A&fq=type:adres&rows=3000

https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=3071&fq=type:adres&rows=3000&start=3000
Total of 33.650 GET requests.

Extract the found adr-ID.
adr-xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
RegEx: adr-[a-z0-9]+
It should return 9.653.170 unique ID's.

Use that adr-ID
https://api.pdok.nl/bzk/locatieserver/search/v3_1/lookup?id=adr-abcdefgh12345678
Total of 9.653.170 GET requests.
Amongst the found information is also a string with "nummeraanduiding_id":"xxxxxxxxxx"

Use the nummeraanduiding_id
https://api.kadaster.nl/lvwoz/wozwaardeloket-api/v1/wozwaarde/nummeraanduiding/NUMBER(=NUMMERAANDUIDINGID)
Total of 9.653.170 GET requests.

Total GET requests:
A + B + C + D = 19.348.990 GET requests.
