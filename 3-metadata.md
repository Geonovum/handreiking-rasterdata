# STAC als ontdekkingslaag voor cloud-native assets 

COG en Zarr regelen efficiënte opslag en toegang. Maar ze vertellen je niet welke data beschikbaar is. Gebruikers willen antwoord op vragen als: 

- Welke datasets bestaan er? 
- Welke assets horen bij een dataset? 
- Welke tijdsperioden zijn beschikbaar? 
- Welk rasterbestand dekt mijn interessegebied? 
- Welk bestand moet ik openen? 

Hier komt [[STAC]] om de hoek kijken. 

## Wat doet STAC? 

STAC (SpatioTemporal Asset Catalog) biedt een gestandaardiseerde manier om geo-assets te beschrijven en te ontdekken. Een STAC-catalogus is opgebouwd uit 4 concepten: 

```Catalog → Collection → Item → Asset ```

> Hierbij is Item het metadata record en Asset de daadwerkelijke (raster)data  

In een rastercontext ziet dat er zo uit: 

```Collection → Item → COG Asset ```

of: 

```Collection → Item → Zarr Asset ```

STAC-metadata beschrijft ruimtelijke en temporele dekking, beschikbare assets, banden of variabelen, verwerkingsniveau en platform- en sensorinformatie. De eigenlijke data blijft gewoon in cloud-objectopslag staan. 

Onthoud: STAC is in de eerste plaats een assetcatalogus, geen datatoegangsservice. 

## Het cloud-native toegangspatroon 

Een typische cloud-native workflow ziet er zo uit: 

```Gebruiker → STAC Search → Asset URL → COG of Zarr → Toegang via client ```

Voorbeelden: 

- Een AHN-tegel vinden via een STAC-catalogus en de bijbehorende COG laden. 
- Een klimaatmodelresultaat ontdekken en het bijbehorende Zarr-dataset direct openen in xarray. 

In beide gevallen regelt STAC de ontdekking; het cloud-native formaat verzorgt de efficiënte gegevenstoegang. De client gebruikt HTTP range requests om alleen de relevante byte-ranges op te halen: 


> 100 GB COG → Nodig: 2 km² rondom Amsterdam → Metadata lezen → Alleen relevante byte-ranges opvragen → Downloaden: een paar MB

Zarr past hetzelfde principe toe op multidimensionale datasets. 

## Metadata-API's en metadatamodellen 

Bij DCAT en STAC is het belangrijk om onderscheid te maken tussen metadata-API's en metadatamodellen. Die twee worden vaak door elkaar gehaald. 

### Metadatamodellen 

Een metadatamodel bepaalt welke informatie beschreven kan worden, hoe metadata gestructureerd is en welke relaties uitgedrukt kunnen worden. 

__DCAT__ beschrijft datasets, distributies en services. Typische vragen: 

- Welke dataset is beschikbaar? 
- Wie publiceert de dataset? 
- Welke licentie geldt er? 
- Welke services bieden toegang? 
- Waar kun je de dataset downloaden? 

Nationale data-infrastructuren zoals het Nationaal Georegister gebruiken DCAT-AP-NL of ISO-19115 als primair metadatamodel. 

__STAC__ beschrijft geo-assets en assetcollecties. Typische vragen: 

- Welke rastertegel dekt mijn gebied? 
- Welke satellietscène moet ik gebruiken? 
- Welke asset bevat de gewenste meting? 
- Welke banden of variabelen zijn beschikbaar? 

STAC is geoptimaliseerd voor cloud-native geo-assets zoals COG's, Zarr-datasets, puntenwolken en Earth Observation-producten. 

DCAT richt zich op datasets; STAC richt zich op individuele assets en assetcollecties. 

## Metadata-API's 

Een metadata-API bepaalt hoe metadata doorzocht, bevraagd en opgehaald kan worden. 

OGC API Records is een algemene API voor het zoeken en ophalen van metadatarecords. De API is grotendeels onafhankelijk van het onderliggende metadatamodel en biedt een gemeenschappelijk mechanisme voor zoeken, filteren, paginering en ophalen. 

```__OGC API Records__ → Metadata → DCAT / STAC / ISO 19115 / GeoDCAT ```

```__STAC API__ → STAC Metadata → COG / Zarr Assets ```

> de STAC API specificatie en de OGC API Records specificatie zijn erg vergelijkbaar. Daarom kun je STAC Items ook makkelijk via OGC API Records serveren

### Positionering van de standaarden 

- OGC API Records is niet exclusief voor ISO 19115 en DCAT. 
- STAC is niet alleen een API; het is in de eerste plaats een metadatamodel. 
- STAC-metadata kan ook via OGC API Records worden ontsloten. 
- OGC API Records en STAC API kunnen naast elkaar bestaan. 