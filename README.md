# wozwaardeloket
Scraper for WOZ data from wozwaardeloket: Efficient property valuation insights.

Scrape/extract wozwaardeloket.nl 

2 different API's

https://api.kadaster.nl/lvwoz/wozwaardeloket-api/v1/wozwaarde/nummeraanduiding/NUMBER(=NUMMERAANDUIDINGID)
https://api.kadaster.nl/lvwoz/wozwaardeloket-api/v1/wozwaarde/wozobjectnummer/WOZOBJECTNUMMER

API 1
https://api.kadaster.nl/lvwoz/wozwaardeloket-api/v1/wozwaarde/nummeraanduiding/NUMBER(=NUMMERAANDUIDINGID)
You need the NUMMERAANDUIDINGID
To find them:
1 
find all addresses in The Netherlands (streetname, housenumber, housenumbersuffix, postalcode, city)

1.1 
Find PostalCodes
q=1000 - q=9999 = 9.000 combinations = 9.000 GET requests.
https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=1012&fq=type:postcode&rows=3000
Done in about 1 minute.

1.2
With PostalCode (PC6 = 4 digits and 2 letters)
= 466.321 unique combinations
https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=1012AB&fq=type:adres&rows=3000
With PostalCode (PC5 = 4 digits and 1 letter)
= 33.649 + 1 unique combinations = 33.650 GET requests.
https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=1012A&fq=type:adres&rows=3000
AND only this one:
https://api.pdok.nl/bzk/locatieserver/search/v3_1/suggest?q=3071A&fq=type:adres&rows=3000&start=3000
Done in about 12 minutes (could be done faster).

1.3
Use the ID number to find NUMMERAANDUIDINGID
adr-xxxxxxxxxxxxxxxxxxxxxxxxxxxx
= 9.653.173 unique ID numbers (adr-xxxxxx)
https://api.pdok.nl/bzk/locatieserver/search/v3_1/lookup?id=adr-xxxxxxxxxxxxxxxxxxxxxxxxxxxx
https://api.kadaster.nl/lvwoz/wozwaardeloket-api/v1/wozwaarde/wozobjectnummer/NUMBER(=WOZOBJECTNUMMER)

1.4
Use the found NUMMERAANDUIDINGID TO FIND THE WOZWAARDE
= 9.653.173  GET requests.
https://api.kadaster.nl/lvwoz/wozwaardeloket-api/v1/wozwaarde/nummeraanduiding/NUMBER(=NUMMERAANDUIDINGID)

