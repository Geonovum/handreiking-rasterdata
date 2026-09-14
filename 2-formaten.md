# Cloud-native rasterdata: COG en Zarr 

De geo-wereld omarmt steeds vaker cloud-native benaderingen voor rasterdata. In plaats van alles via een gespecialiseerde service als WCS te laten lopen, sla je data direct op in cloud-objectopslag. Clients halen dan alleen de stukken data op die ze echt nodig hebben. 

De twee dominante cloud-native rasterformaten zijn: 

- Cloud Optimized GeoTIFF (COG) 
- Zarr 

## Cloud Optimized GeoTIFF (COG) 

Een COG is een GeoTIFF-bestand dat slim georganiseerd is voor efficiënte toegang via HTTP en cloud-objectopslag. De belangrijkste kenmerken: 

- Interne tiling 
- Meerdere resolutieniveaus (overviews) 
- Ondersteuning voor HTTP range requests 
- Compatibel met bestaande GeoTIFF-tooling 

Clients zoals GDAL, Rasterio, QGIS en ArcGIS halen zo alleen de stukken op die nodig zijn voor een specifieke weergave of analyse. 

Client → HTTP Range Requests → Cloud Optimized GeoTIFF 

Voor veel datasets – zoals AHN-hoogtemodellen, orthofoto's, satellietbeelden en landbedekkingskaarten – maakt dit aparte raster-extractieservices overbodig. 

Omdat COG volledig compatibel is met bestaande GeoTIFF-workflows, kunnen organisaties overstappen op cloud-native architecturen zonder hun productieomgevingen te hoeven verbouwen. 

## Zarr 

COG is ideaal voor traditionele rasterproducten. Zarr is juist ontworpen voor multidimensionale wetenschappelijke data. Denk aan dimensies als: 

x, y, z, tijd, scenario, ensemble 

Zarr slaat data op als onafhankelijk opvraagbare chunks, direct uit cloud-objectopslag. Daarmee is Zarr bij uitstek geschikt voor: 

- Klimaatprojecties 
- Weersvoorspellingen 
- Hydrologische simulaties 
- Omgevingsmodellen 
- Digital Twin-simulaties 
- Aardsysteemmodellen
- Statistiek

De wetenschappelijke wereld combineert Zarr steeds vaker met tools als xarray en Dask om grote multidimensionale datasets te analyseren – zonder speciale middleware. 

## De cloud-native opslaglaag 

Beide formaten vormen samen de cloud-native opslaglaag: 

Objectopslag 
   - COG 
   - Zarr 

In deze architectuur: 

- Is opslag direct toegankelijk. 
- Blijft data in objectopslag staan. 
- Halen clients alleen de benodigde chunks of tegels op. 
- Zijn aparte rasterservices optioneel, niet verplicht. 

Dit is een flinke breuk met traditionele, service-georiënteerde architecturen. 