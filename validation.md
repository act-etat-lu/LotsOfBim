# Validation

Ce document résume les règles de validation appliqués par l'application LotsOfBIM.

## Règles de validation IFC

([LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf)

### Schéma IFC <a id="ifc.schema">

**Besoin**
Le fichier IFC doit respecter les versions de schéma [IFC2x3 TC1](https://standards.buildingsmart.org/IFC/RELEASE/IFC2x3/TC1/HTML/) ou [IFC4 ADD2 TC1](https://standards.buildingsmart.org/IFC/RELEASE/IFC4/ADD2_TC1/HTML/) de BuildingSMART International , plus spécifiquement la 'coordination view MVD' de IFC2x3 TC1 et la 'reference view MVD' de IFC4 ADD2 TC1. Le fichier doit être livré en format STEP Physical Format (IFC-SPF).

**Méthode de test**
* The magic number of the file must read ‘ISO-10303-21’;
* The header of the STEP file contains the schema. The value should be ‘IFC4’ or ‘IFC2X3’.
* The header file description must contain the value ‘ViewDefinition [ReferenceView_V1.2]’ (IFC4) or ‘ViewDefinition [CoordinationView_V2.0]’ (IFC2X3).
  
**Type**: erreur, en cas de non-conformité

**Test type:** automatisé

**Exemple:**
```
ISO-10303-21;
HEADER;
FILE_DESCRIPTION(('ViewDefinition [ReferenceView_V1.2]','ExchangeRequirement [Architecture]'),'2;1');
FILE_NAME('001-00','2021-05-31T22:40:38',(''),(''),'The EXPRESS Data Manager Version 5.02.0100.07 : 28 Aug 2013','22.0.2.392 - Exporter 22.1.0.0 - Alternate UI 22.1.0.0','');
FILE_SCHEMA(('IFC4'));
ENDSEC;
```


### Maquette numérique géoréférencée <a id="ifc.geo">
### IfcBuilding.ElevationOfRefHeight <a id="ifc.building.elevation">
### IfcBuilding.BuildingAddress <a id="ifc.building.address">
### IfcBuildingStorey <a id="ifc.building.address">
### IfcSpace
### IfcZone
### Separation walls
### Géometries simplifiées
### Fill the building volume without overlaps nor intersections

## Règle de délimitation

### There must be an official survey plan
### Conformity with building permit
### The natures attributed to Lot and LotPart must be compatible
### The LotParts of each Lot must be on the same BuildingStorey and must be adjacent
### Each Lot must have access to the common parts. There can be no isolated common parts.
### Each Lot must be delimited according to the same logic
### Delimitation of construction elements
### Delimitation of opening elements
### Construction elements related to fire regulation and emergencies
### Ducts and chimneys
### Windows of type ‘cours anglaise’
### Escalier trémie à exclure
### Hatch and “vide”
### Private elevator and freight elevator - ascenseur ou monte-charge privatif
### External one-level staircase is an independent lot
### External footbridge - Passerelle extérieure
### Parking places
### Minimally one common space
