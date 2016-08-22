### JSON-LD tutvustus

````
Riigi infosüsteemi kataloog pakub usaldusväärset ja kiiret teave riigi infosüsteemi kohta. Kasutajate rahulolu kataloogiga 2014. a oli 2,7.
````

Seda inimkeelset teavet on raske masinaga (arvutiga) töödelda. JSON on praegu ülipopulaarne lightweight, lihtne vormingukeel, millega kirjeldusele saab anda rohkem struktuuri:
````
{	
  "nimetus": "Riigi infosüsteemi kataloog",
  "kirjeldus": "Usaldusväärne ja kiire teave riigi infosüsteemi kohta",
	"rahulolu": { 
		"periood": "2014",
		"väärtus": "2.7"
	}
}
````
Seda on juba parem masinaga töödelda. Kuid kust kirjeldaja teab, et peab kasutama täpselt selliseid väljanimetusi?

[JSON-LD](http://json-ld.org/) (JSON for Linked Data) on veel üks samm edasi masintöödeldavate kirjelduste suunas.

#### Masintöödeldav sõnastik

Kokkuleppest, et teenusekirjeldused avaldatakse JSON-is ainuüksi ei piisa. Masintöödeldavuse saavutamiseks tuleb kokku leppida ka sõnastik — sõnad ja fraasid, mida kirjelduse elementide tähistamiseks kasutame. Masinale tuleb teha arusaadavaks "nimetus", "kirjeldus", "rahulolu", "periood", "väärtus" jne.

````
  SÕNASTIK:
  nimetus
  kirjeldus
  rahulolu
  periood
  väärtus
````
Masintöödeldavates sõnastikes tavaliselt igale sõnale antakse URI; URI viib veebilehele, kust on leitav sõna määratlus. Näiteks:
- Schema.org sõnastikud. Näiteks termin "Organization" omab URI [https://schema.org/Organization](https://schema.org/Organization). Selle URI alt avaneb lehekülg termini määratlusega ("An organization 	such as a school, NGO, corporation, club, etc.") ja omaduste kirjeldusega jne.
- RIHA sõnastikud. Näiteks termin "Register" omab URI [https://riha.eesti.ee/Register](https://riha.eesti.ee/Register). Selle URI alt avaneb RIHAs lehekülg, termini selgitusega.

#### Sõnastiku kasutamine
Masinloetava sõnastiku sõnu saab kirjeldamisel kasutada väga lihtsalt. Eeldades, et sõnad on määratletud veebiaadressil `https://meta.eesti.ee`, saab sõna asemel kirjutada sõna URI:
````
{	
  "https://meta.eesti.ee/Nimetus": "Riigi infosüsteemi kataloog",
  "https://meta.eesti.ee/Kirjeldus": "Usaldusväärne ja kiire teave riigi infosüsteemi kohta",
	"https://meta.eesti.ee/Rahulolu": { 
		"https://meta.eesti.ee/Periood": "2014",
		"https://meta.eesti.ee/Väärtus": "2.7"
	}
}
````

#### URI-de lühendamine konteksti abil
Kirjelduse iga välja nimetamine URI-ga on ebapraktiliselt pikk ja kohmakas. URI-d saab lühemaks teha nn konteksti näitamisega. JSON-LD pakub selleks elementi `@context`:
````
{
  "@context": { "TSN": "https://meta.eesti.ee/" },
  "TSN:Nimetus": "Riigi infosüsteemi kataloog",
  "TSN:Kirjeldus": "Usaldusväärne ja kiire teave riigi infosüsteemi kohta",
	"TSN:Rahulolu": { 
		"TSN:Periood": "2014",
		"TSN:Väärtus": "2.7"
	}
}
````
Lühend `TSN` tähendab nüüd seda, et aadressil `https://meta.eesti.ee/` on teenuste sõnastik.

Erinevaid kirjeldusi kokku korjav masin saab nüüd kindel olla, et "Nimetus" tähendab igal pool üht ja sama.

Samas jätab JSON-LD sõnastike kasutamisel palju vabadust: ei pea kasutama kõiki sõnastiku sõnu (täpsemalt öeldes, termineid); võib kombineerida erinevaid sõnastikke; võib kasutada oma sõnu.

#### Rahvusvahelised ja oma sõnastikud

Sõnastiku võiksime ka Eestis isekeskis kokku leppida. Omakeelne sõnavara on hea, kuid oleme väike riik ega jõua kõiki vajalikke standardeid ise koostada. Kui keegi on meile vajalikud terminid juba rahvusvaheliselt määratlenud, siis miks mitte kasutada rahvusvahelist standardit?

Euroopa Komisjoni eestvedamisel on koostatud Avalike Teenuste Tüvisõnastik ([Core Public Service Vocabulary](https://joinup.ec.europa.eu/asset/core_vocabularies/asset_release/core-vocabularies-v11)). Sealtki võib valida endale sobivat sõnavara.

#### Lõpetuseks
Ülalesitatu oli vaid kõige minimaalsem tutvusetegemine JSON-LD võimalustega.

JSON for Linked Data paistab mitmete omaduste poolest parimini vastavat meie visioonile masintöödeldavate metaandmete detsentraliseeritud tootmisest:

- tehnoloogia on standardiseeritud - W3C standard (2014);
- lihtsam teistest RDF serialiseeringutest;
- uus, aga kiirelt levinud ja [kasutusel paljude autoriteetsete organisatsioonide](https://github.com/json-ld/json-ld.org/wiki/Users-of-JSON-LD) poolt.

JSON-LD on ainult üks element metaandmete hajusa tootmise skeemis. Vaja on sõnastiku publitseerimise kohta, kirjelduste koostamise tööriistu, andmekorje rakendust, visualiseerimise vahendeid jm.
