### JSON-LD tutvustus

#### Lahendatav ülesanne

Tahame praktilist skeemi, millega organisatsioonid koostaksid oma avalike teenuste masinloetavad kirjeldused ja paneksid need avalikult veebi üles.

Andmekorjerakendus (ingl _Data Harvester_) korjaks kirjeldused kokku ja publitseerib ühtse kataloogina.

Nii pääseksime manuaalsest andmehõivest RIHAsse, Avalike teenuste kataloogi, eesti.ee-sse jms tsentraalsetesse süsteemidesse.

#### Kuidas seda saavutada?

Vaja on tervet rida asju: mõtteviisi muutmist, uute arhitektuursete mustrite sissetoomist, kokkulepeid mis vormingus kirjeldused peaksid olema ja mõningaid tarkvaralisi tööriistu. Esitan alljärgnevas hästi lühikese sissejuhatuse uude tehnoloogiasse, mis võiks meie vajadustele ühtse masinloetava kirjelduskeele osas ehk kõige paremini sobida.

Esialgu on jutt avalike teenuste kirjeldamisest. Olgu kohe öeldud, et sobivuse korral tahame sama skeemi kasutada infosüsteemide jm riigi infosüsteemi komponentide metaandmete hajusaks, masintöödeldavaks kirjeldamiseks.

Kõigepealt tuleb aga välja selgitada ja järgi proovida, kuidas täpselt tehnoloogia meie vajadusi rahuldaks. Seda teeme pilootprojekti vormis.

#### Kirjeldusstandard

Masinaga andmekorjet saab teha siis, kui teenusekirjeldused järgivad mingit ühtset kirjelduse standardit, kirjelduskeelt või vormingut. Kirjelduse standard peaks olema inimesele võimalikult lihtne. Inimene peab suutma teenused ära kirjeldada. Samas on oluline kirjelduste masintöödeldavus. Masin peab mõistma kirjeldused kokku korjata. Oluline kriteerium on ka kirjelduskeele paindlikkus (laiendatavus). Teenused on erinevad, tõenäoliselt saab ainult mingi ühisosa teenuste atribuutidest ühtse skeemi järgi ära kirjeldada. Organisatsioonid peavad saama kirjeldada teenuseid ka oma spetsiifilisest vaatest ja needki kirjeldused peaksid olema masintöödeldavad.

Lihtsus ja paindlikkus (koos masintöödeldavusega) on alati olnud eesmärk. XML (Extensible Markup Language) on laiendatav. SOAP (Simple Object Access Protocol) on lihtne. Vanad, end tõestanud tehnoloogiad. Selgub, et võimalik on teha veel lihtsamalt.

[JSON-LD](http://json-ld.org/) (JSON for Linked Data) on

Teenuse nimi, kirjeldus, kasutajate rahulolu
````
Riigi infosüsteemi kataloog - Usaldusväärne ja kiire teave riigi infosüsteemi kohta.
	Kasutajate rahulolu 2,7 (2014).
````

Seda on raske masinaga töödelda. JSON on praegu ülipopulaarne lightweight, lihtne vormingukeel, millega kirjeldusele saab anda rohkem struktuuri:
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
See on parem, kuid kust kirjeldaja teab, et peab kasutama täpselt selliseid väljanimetusi?

#### Ühtne sõnastik

Kokkuleppest, et teenusekirjeldused avaldatakse JSON-is ainuüksi ei piisa. Masintöödeldavuse saavutamiseks tuleb kokku leppida ka sõnastik — sõnad ja fraasid, mida kirjelduse elementide tähistamiseks kasutame. Masinale tuleb teha arusaadavaks "nimetus", "kirjeldus", "rahulolu", "periood", "väärtus" jne.

````
    SÕNASTIK
		nimetus
		kirjeldus
		rahulolu
		periood
		väärtus
````
Masinale loetavaid sõnastikke tehakse väljakujunenud mustri järgi: igale sõnale antakse URI; URI viib veebilehele, kust on leitav sõna määratlus.

````
Schema.org
	Näiteks termin "Organization" omab URI "https://schema.org/Organization".
	Selle URI alt avaneb lehekülg termini määratlusega ("An organization
	such as a school, NGO, corporation, club, etc.") ja omaduste kirjeldusega.
	
RIHA sõnastikud
	Näiteks termin "Register" omab URI "https://riha.eesti.ee/Register".
	Selle URI alt avaneb RIHAs lehekülg, termini selgitusega.
````

#### Sõnastiku kasutamine
Masinloetava sõnastiku sõnu saab kirjeldamisel kasutada väga lihtsalt. Sõna asemel kirjutame sõna URI:
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
Kirjelduse iga välja nimetamine URI-ga on ebapraktiliselt pikk ja kohmakas. URI-d saab lühemaks teha konteksti näitamisega:
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
Lühend "TSN" tähendab nüüd seda, et aadressil https://meta.eesti.ee/ on teenuste sõnastik.

Erinevaid kirjeldusi kokku korjav masin saab nüüd kindel olla, et "Nimetus" tähendab igal pool üht ja sama.

Samas jätab JSON-LD sõnastike kasutamisel palju vabadust: ei pea kasutama kõiki sõnastiku sõnu (täpsemalt öeldes, termineid); võib kombineerida erinevaid sõnastikke; võib kasutada oma sõnu.

#### Rahvusvahelise sõnastiku kasutamine

Sõnastiku võiksime ka Eestis isekeskis kokku leppida. Omakeelne sõnavara on hea, kuid oleme väike riik ega jõua kõiki vajalikke standardeid ise koostada. Kui keegi on meile vajalikud terminid juba rahvusvaheliselt määratlenud, siis miks mitte kasutada rahvusvahelist standardit?

Euroopa Komisjoni eestvedamisel on koostatud Avalike Teenuste Tüvisõnastik ([Core Public Service Vocabulary](https://joinup.ec.europa.eu/asset/core_vocabularies/asset_release/core-vocabularies-v11)). Sealtki võime valida endale sobivat sõnavara.

#### Lõpetuseks
Ülalesitatu oli vaid kõige minimaalsem tutvusetegemine JSON-LD võimalustega.

JSON for Linked Data paistab mitmete omaduste poolest parimini vastavat meie visioonile masintöödeldavate metaandmete detsentraliseeritud tootmisest:

- tehnoloogia on standardiseeritud - W3C standard (2014);
- lihtsam teistest RDF serialiseeringutest;
- uus, aga kiirelt levinud ja [kasutusel paljude autoriteetsete organisatsioonide](https://github.com/json-ld/json-ld.org/wiki/Users-of-JSON-LD) poolt.

JSON-LD on ainult üks element metaandmete hajusa tootmise skeemis. Vaja on sõnastiku publitseerimise kohta, kirjelduste koostamise tööriistu, andmekorje rakendust, visualiseerimise vahendeid jm.

JSON-LD enda võimalustest vajame tõenäoliselt esialgu ainult väikest alamhulka. Selle alamhulga väljaselgitamine ja praktiline katsetamine ongi pilootprojekti üks eesmärke.
