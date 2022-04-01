# Validation

Ce document résume les règles de validation qui sont vérifiées par l'application LotsOfBIM.

- [Règles de validation IFC](#ifc)
  - [Schéma IFC](#ifc.schema)
  - [Maquette numérique géoréférencée](#ifc.geo.luref)
  - [Parcelles cadastrales](#ifc.geo.parcel)
  - [IfcBuilding.ElevationOfRefHeight](#ifc.building.elevation)
  - [IfcBuildingStorey](#ifc.IfcBuildingStorey)
  - [IfcSpace](#ifc.IfcSpace)
  - [IfcZone](#ifc.IfcZone)
  - [Separation walls](#ifc.cloisons)
  - [Géometries simplifiées](#ifc.geometries)
  - [Fill the building volume without overlaps nor intersections](#ifc.topologie)

- [Règles de délimitation](#vertical)
  - [Intersection de lots privatifs](#vertical.intersection)
  - [Plan de mensuration officielle](#vertical.mensuration)
  - [Conformité à l'autorisation de construire](#vertical.autorisation)
  - [Conformité des natures](#vertical.nature)
  - [The LotParts of each Lot must be on the same BuildingStorey and must be adjacent](#vertical.mensuration)
  - [Lots privatifs sans accès direct aux communs](#vertical.acces)
  - [Délimitation suivant le même schéma](#vertical.congruence)
  - [Status des éléments de construction](#vertical.statut_element)
  - [Statut des ouvertures](#vertical.statut_ouverture)
  - [Statut des éléments de protection](#vertical.statut_protection)
  - [Statuts des gaines techniques et cheminées](#vertical.statut_gaine)
  - [Statut des cours anglaises](#vertical.statut_cours_anglaise)
  - [Escalier trémie à exclure](#vertical.escalier)
  - [Trappe ou un vide décoratif](#vertical.vide)
  - [Ascenseur ou monte-charge privatif](#vertical.ascenseur)
  - [External one-level staircase is an independent lot ](#vertical.escalier_exterieur)
  - [External footbridge - Passerelle extérieure](#vertical.paserelle.exterieur)
  - [Emplacements à plates-formes superposées ou coulissantes](#vertical.emplacements_superposés)
  - [Au moins un local commun](#vertical.local_commun)    

## Règles de validation IFC <a id="ifc">

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


### Maquette numérique géoréférencée <a id="ifc.geo.luref">

**Besoin**: Le positionnement et l’orientation du point zéro de la maquette numérique doivent être spécifiés dans le système de référence Luxembourgeois LUREF (EPSG: 2169).


### Parcelles cadastrales <a id="ifc.geo.parcel">

### IfcBuilding.ElevationOfRefHeight <a id="ifc.building.elevation">

**Besoin**: La valeur de l'attribut IfcBuilding.ElevationOfRefHeight doit être égale à 0.0.

### IfcBuildingStorey <a id="ifc.IfcBuildingStorey">

**Besoins:** 
 - Chaque étage (IfcBuildingStorey) doit avoir un property ACT_Etage.Nom dont la valeur est conforme aux recommandations de délimitation du cadastre vertical. 
 - L'attribut IfcBuildingStorey.Elevation doit avoir une valeur.

### IfcSpace <a id="ifc.IfcSpace">

**Besoins:** 
 - 

### IfcZone <a id="ifc.IfcZone">

**Besoins:**
 - Chaque attribut IfcSpace.Name doit avoir une valeur.
 - Chaque objet de type IfcSpace doit avoir une géometrie en 3D (IfcShapeRepresentation.RepresentationIdentifier = 'Body') de  type (IfcShapeRepresentation.RepresentationType) ‘Brep’ ‘SweptSolid’, or ‘Clipping’.
 - Chaque IfcSpace doit avoir un property set ACT_PartieDeLot, that both ACT_PartieDeLot.Nature and ACT_PartieDeLot.Lot properties have a value.
 - ACT_PartieDeLot.Nature doit avoir une valeur conforme.
 - La valeur de property ACT_PartieDeLot.Lot contient une valeur structurée: numéro,bloc,escalier,niveau (p.e. 001,A,B,81). 


### Separation walls <a id="ifc.cloisons">

### Géometries simplifiées <a id="ifc.geometries">

### Fill the building volume without overlaps nor intersections <a id="ifc.topologie">



## Règles de délimitation des lots privatifs pour le Cadastre Vertical <a id="vertical">

[Cadastre vertical – Recommandations - 2e partie : La délimitation des lots privatifs, Octobre 2012.](https://act.public.lu/fr/publications/telechargement-documents/technique/Recommandations_Delimitation.html)

### Intersection de lots privatifs <a id="vertical.intersection">
**Besoin**: Aucun des lots privatifs n’empiète sur la parcelle voisine ou la voirie publique. [Délimitation, 2.2]

### Plan de mensuration officielle <a id="vertical.mensuration">
**Besoin**: La délimitation de la parcelle de support fixe parmi d’autres facteurs, la géométrie de l’immeuble qui détermine en premier la délimitation des lots privatifs. Un plan de mensuration officielle qui l’arrête et la documente, doit en conséquence être intégré
à la plupart des états descriptifs de division et scrupuleusement reproduit par tous les plans d’implantation et d’étage. [Délimitation, 2.2]

### Conformité à l'autorisation de construire <a id="vertical.autorisation">
**Besoin**: La délimitation est donc fonction de la partie littérale et graphique de l’autorisation de construire et les plans descriptifs de division doivent s’accorder avec les plans autorisés. Cette concordance assure aussi que la division de l’immeuble est conforme aux réglementations communales relatives à l’aménagement et à la construction, et que tous les lots privatifs sont effectivement réalisables.

### Conformité des natures <a id="vertical.nature">
**Besoin**: chaque lot privatif rassemble le corps d’une même nature et les parties accessoires qui le servent, de manière à constituer une unité transférable de propriété exclusive.

### The LotParts of each Lot must be on the same BuildingStorey and must be adjacent <a id="vertical.mensuration">
**Besoin**: Aucun lot privatif ne peut rassembler des éléments situés sur différents niveaux ou situés sur le même niveau, mais séparés par un autre lot privatif ou par des parties communes (autres que les murs de refend) ; ainsi les duplex (triplex) courants sont formés par deux (trois) lots privatifs sis sur deux (trois) niveaux consécutifs et reliés entre eux. [Délimitation, 2.4].

### Lots privatifs sans accès direct aux communs <a id="vertical.acces">
**Besoin**: une délimitation qui engendre, soit des lots privatifs sans accès direct aux communs, soit des parties communes exiguës ou enclavées, soit des servitudes en conséquence, doit être évitée. (Lots sans accès au communs / parties communes enclaves). [Délimitation, 2.4]

### Délimitation suivant le même schéma <a id="vertical.congruence">
**Besoin**: 2.5. Des lots privatifs semblables, quant à leur nature, à leur situation, à leur géométrie et à leur réalisation, doivent tous être délimités suivant le même schéma (exemple : une série d’emplacements souterrains adjacents). [Délimitation, 2.5]

### Status des éléments de construction <a id="vertical.statut_element">
**Besoin**: Le statut des éléments de construction qui matérialisent la quasi-totalité des limites des lots privatifs intérieurs, est fixé comme suit:
  - Commun : 
     - les dalles
     - les murs de façade et de pignon
     - les murs et les piliers de refend
     - les cloisons délimitant les parties communes communs
  - Mitoyennes : les cloisons délimitant deux lots privatifs contigus mitoyennes
  - Privative : les cloisons (amovibles ou non) à l’intérieur du même lot privatif privatives [Délimitation, 2.6]



### Statut des ouvertures <a id="vertical.statut_ouverture">
**Besoin**: Chaque ouverture (fenêtre, porte, porte-fenêtre, porte de garage, baie vitrée …) a le même statut que l’élément de construction dans lequel elle a été pratiquée. Le repère de la délimitation est alors celle des parties alignées de son encadrement (retombée, allège ou pan latéral aligné) qui avance le plus dans la pièce. [Délimitation, 2.8]

### Statut des éléments de protection <a id="vertical.statut_protection">
**Besoin**: Les éléments de construction et d’équipement, destinés à la protection des personnes et des meubles (escaliers de secours, murs pare-feu …), doivent appartenir aux parties communes. [Délimitation, 2.9]]

### Statuts des gaines techniques et cheminées <a id="vertical.statut_gaine">
**Besoin**: Les gaines techniques et les cheminées, ensemble avec leurs coffrages, appartiennent toutes aux parties communes. Elles peuvent être privatives, si leur affectation à ces parties de l’immeuble, ne porte aucun préjudice à la copropriété et si elles ne servent qu’un seul lot privatif ou qu’un ensemble de lots privatifs qui, de par leur destination et leur disposition, sont détenus par le même copropriétaire. Cette exception ne vaut pas pour les conduits qui passent par une unité d’habitation, assimilable à une maison unifamiliale, mais intégrée dans un immeuble soumis au statut de la copropriété. [Délimitation, 2.10]

### Statut des cours anglaises <a id="vertical.statut_cours_anglaise">
**Besoin**: Les cours anglaises et leurs projections verticales, au moins jusqu’au premier niveau hors sol, appartiennent généralement aux parties communes. [Délimitation, 2.11]

### Escalier trémie à exclure <a id="vertical.escalier">
**Besoin**: La cage d’un escalier reliant deux parties privatives qui sont directement superposées et, de par leur destination et leur disposition, détenues par le même copropriétaire, est intégrée dans le lot privatif en bas de l’escalier. La partie de la trémie en haut de
l’escalier, qui n’est pas surplombée par un deuxième escalier (donc l’entière trémie au niveau supérieur), n’est attribuée à aucun lot privatif. [Délimitation, 2.12]


### Trappe ou un vide décoratif <a id="vertical.vide">
**Besoin**: une trappe ou un vide décoratif ne fait pas partie du lot privatif qui l’englobe [Délimitation, 2.13]

### Ascenseur ou monte-charge privatif <a id="vertical.ascenseur">
**Besoin**: La cage d’un ascenseur ou d’un monte-charge reliant exclusivement plusieurs parties privatives qui, de par leur destination et leur disposition, sont détenues par le même copropriétaire, est intégrée dans le lot privatif qui est desservi par l’ascenseur ou par le monte-charge au niveau correspondant. Un lot privatif indépendant n’est constitué qu’aux niveaux où l’ascenseur ou le monte-charge donne seulement accès aux parties communes. [Délimitation, 2.14].

### External one-level staircase is an independent lot  <a id="vertical.escalier_exterieur">
**Besoin**: L’escalier extérieur qui relie des parties du même niveau (donc l’escalier de quelques marches seulement et par conséquent d’une moindre hauteur) appartient au balcon ou à la terrasse attenante. En revanche, l’escalier extérieur qui relie des parties de deux niveaux consécutifs (donc l’escalier d’une hauteur de l’ordre d’un étage) forme un lot privatif indépendant au niveau inférieur. [Délimitation, 2.15]

### External footbridge - Passerelle extérieure <a id="vertical.paserelle.exterieur">
**Besoin**: La passerelle extérieure, accolée à un balcon ou à une terrasse, fait partie intégrante de ce balcon ou de cette terrasse. Dans le cas contraire, elle constitue un lot privatif indépendant (de nature Accès). [Délimitation, 2.16]

### Installations de parking avec plateformes superposées ou coulissantes <a id="vertical.emplacements_superposés">
**Besoin**: Les emplacements encastrés dans un système de parcage à plates-formes superposées ou coulissantes, sont délimités par ces plates-formes mêmes. En cas de rangées d’emplacements disposées les unes directement derrière les autres, une aire de manoeuvre commune de la taille minimale d’un emplacement, est prévue dans chacune des rangées de devant où les véhicules sont garés sur des plates-formes coulissantes. [Délimitation, 2.17]

### Au moins un local commun <a id="vertical.local_commun">
**Besoin**: Condition pour un établissement d’un cadastre vertical : il faut au moins un local commun, les murs extérieurs ne sont pas suffisants comme parties communes.

