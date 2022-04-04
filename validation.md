# Validation

Ce document résume les règles de validation qui sont vérifiées par l'application [LotsOfBIM](https://lotsofbim.geoportail.lu/) lors d'une demande de désignation cadastrale des lots privatifs d'un immeuble soumis au statut de la copropriétés.

- [1 Règles de validation IFC](#ifc)
  - [1.1 Schéma IFC](#ifc.schema)
  - [1.2 Maquette numérique géoréférencée](#ifc.geo.luref)
  - [1.3 Parcelles cadastrales](#ifc.geo.parcel)
  - [1.4 IfcBuilding.ElevationOfRefHeight](#ifc.building.elevation)
  - [1.5 IfcBuildingStorey](#ifc.IfcBuildingStorey)
  - [1.6 IfcSpace](#ifc.IfcSpace)
  - [1.7 IfcZone](#ifc.IfcZone)
  - [1.8 Murs de séparation](#ifc.cloisons)
  - [1.9 Géometries simplifiées](#ifc.geometries)
  - [1.10 Remplissage sans intersections](#ifc.topologie)

- [2 Règles de délimitation](#délimitation)
  - [2.1 Intersection de lots privatifs](#délimitation.intersection)
  - [2.2 Plan de mensuration officielle](#délimitation.mensuration)
  - [2.3 Conformité à l'autorisation de construire](#délimitation.autorisation)
  - [2.4 Conformité des natures](#délimitation.nature)
  - [2.5 Adjacence parties de lot](#délimitation.adjacence)
  - [2.6 Lots privatifs sans accès direct aux communs](#délimitation.acces)
  - [2.7 Délimitation suivant le même schéma](#délimitation.congruence)
  - [2.8 Status des éléments de construction](#délimitation.statut_element)
  - [2.9 Statut des ouvertures](#délimitation.statut_ouverture)
  - [2.10 Statut des éléments de protection](#délimitation.statut_protection)
  - [2.11 Statuts des gaines techniques et cheminées](#délimitation.statut_gaine)
  - [2.12 Statut des cours anglaises](#délimitation.cours_anglaise)
  - [2.13 Escalier trémie à exclure](#délimitation.escalier)
  - [2.14 Trappe ou un vide décoratif](#délimitation.vide)
  - [2.15 Ascenseur ou monte-charge privatif](#délimitation.ascenseur)
  - [2.16 Escalier extérieur](#délimitation.escalier_extérieur)
  - [2.17 Passerelle extérieure](#délimitation.paserelle_extérieure)
  - [2.18 Emplacements à plates-formes superposées ou coulissantes](#délimitation.emplacements_superposés)
  - [2.19 Au moins un local commun](#délimitation.local_commun)    

## 1 Règles de validation IFC <a id="ifc">

Les règles de validation IFC se basent sur les critères du guide de modélisation ([LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf)).

### 1.1 Schéma IFC <a id="ifc.schema">

**Besoin:** Le fichier IFC doit respecter les versions de schéma [IFC2x3 TC1](https://standards.buildingsmart.org/IFC/RELEASE/IFC2x3/TC1/HTML/) ou [IFC4 ADD2 TC1](https://standards.buildingsmart.org/IFC/RELEASE/IFC4/ADD2_TC1/HTML/) de BuildingSMART International , plus spécifiquement la 'coordination view MVD' de IFC2x3 TC1 et la 'reference view MVD' de IFC4 ADD2 TC1. Le fichier IFC doit être en format STEP Physical Format (IFC-SPF). Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf).

**Méthode de test**:
* Le fichier IFC doit commencer par la valeur 'ISO-10303-21' (le format STEP);
* L'en-tête du fichier IFC doit indiquer le schéma IFC:  `FILE_SCHEMA(('IFC4'));` ou `FILE_SCHEMA(('IFC2x3'));`.
* L'en-tête du fichier IFC doit indiquer le MVD appliqué: `'ViewDefinition [ReferenceView_V1.2]'` (IFC4) or `'ViewDefinition [CoordinationView_V2.0]'` (IFC2X3).
  
**Résultat**: erreur, en cas de non-conformité.

**Type** test automatisé.

**Exemple:**
```
ISO-10303-21;
HEADER;
FILE_DESCRIPTION(('ViewDefinition [ReferenceView_V1.2]','ExchangeRequirement [Architecture]'),'2;1');
FILE_NAME('001-00','2021-05-31T22:40:38',(''),(''),'The EXPRESS Data Manager Version 5.02.0100.07 : 28 Aug 2013','22.0.2.392 - Exporter 22.1.0.0 - Alternate UI 22.1.0.0','');
FILE_SCHEMA(('IFC4'));
ENDSEC;
```


### 1.2 Maquette numérique géoréférencée <a id="ifc.geo.luref">

**Besoin** : Le positionnement et l’orientation du point zéro de la maquette numérique doivent être spécifiés dans le système de référence Luxembourgeois LUREF ([EPSG: 2169](https://act.public.lu/fr/gps-reseaux/spslux1/spsluxgeodeticdatum.html)). Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf).

**Méthode de test** :
  1. Vérifier que le fichier IFC contient le paramètres MapConversion et ProjectedCRS:
      - Si le schéma IFC est IFC4, l'objet `IfcProject' doit être associé avec (`IfcProject.RepresentationContexts`) un objet  `IfcGeometricRepresentationContext` that describes a CoordinateOperation (IfcMapConversion) for a TargetCRS (IfcProjectedCRS).
      - Si le schéma IFC est IFC2X3, l'objet `IfcSite` doit être associé avec un property set `ePset_MapConversion` et  `ePset_ProjectedCRS`.
  2. Vérifier si toute information est correcte:
      - **Eastings**
      - **Northings** 
      - **OrthonalHeight**
      - **XAxisAbscissa**
      - **XAxisOrdinate**
      - **Scale** (optionnel)
  3. Test whether the following values are provided for the ProjectedCRS object:
      - **Name**: la valeur doit être `EPSG: 2169`. 

**Type**: test automatisé.

**Résultat**: erreur, en cas de non-conformité.

**Exemple:**
```
#126= IFCPROJECTEDCRS('EPSG:2169','Luxembourg 1930 / Gauss','Luxembourg30b',$,$,$,#125);
#128= IFCMAPCONVERSION(#116,#126,76670.,77179.,293.7,0.945518575599319,-0.32556815445715,$);
#130= IFCPROJECT('3ZGD7y6S5209$mGLi_sPlj',#42,'001-00',$,$,'Sample House','Project Status',(#116),#109);
```

### 1.3 Parcelles cadastrales <a id="ifc.geo.parcel">

**Besoin:** la géolocalisation de la maquette doit correspondre aux parcelles cadastrales sélectionnées dans l'application LotsOfBIM. Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf).

**Méthode de test:**
  1. Déterminez l'empreinte 2D de tous les objets IfcSpace dans le modèle BIM et convertissez-la en polygone 2D.
  2. Extrayez les paramètres IfcMapConversion du modèle BIM. Convertissez l'empreinte 2D en LUREF.
  3. Récuperez les parcelles cadastrales selectionnées dans l'applicaiton LotsOfBIM par l'utilisateur lors de la création du dossier. Vérifiez si au moins un des parcelles cadastrales identifiées intersectent avec l'empreinte 2D des objets IfcSpace. 

**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.


### 1.4 IfcBuilding.ElevationOfRefHeight <a id="ifc.building.elevation">

**Besoin**: La valeur de l'attribut `IfcBuilding.ElevationOfRefHeight` doit être égale à 0.0. Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf).

**Méthode de test:**
  1. Testez si `IfcBuilding.ElevationOfRefHeight` est à égale à 0.0, nul ou absent.


**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.

### 1.5 IfcBuildingStorey <a id="ifc.IfcBuildingStorey">

**Besoin:** Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf). 
 - Chaque étage (IfcBuildingStorey) doit avoir un property `ACT_Etage.Nom` dont la valeur est conforme aux recommandations de délimitation du cadastre vertical. 
 - L'attribut `IfcBuildingStorey.Elevation` doit avoir une valeur.

**Méthode de test:**
 Pour chaque objet de type `IfcBuildingStorey`:
   - Vérifiez s'il a une propriété `ACT_Etage.Nom`;
   - vérifiez que l'attribut `IfcBuildingStorey.Elevation` a une valeur.

**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.

**Exemple:** 
```
#172= IFCBUILDINGSTOREY('3ZGD7y6S5209$mGLl19cKg',#42,'81','premier sous-sol','Level:8mm Head',#171,$,'81-my-suffix',.ELEMENT.,0.);
#330302= IFCPROPERTYSINGLEVALUE('Nom',$,IFCTEXT('81'),$);
#330303= IFCPROPERTYSET('1uIsFIJBqOVaiR97zp9Ma2',#42,'ACT_Etage','',(#330302));
#330310= IFCRELDEFINESBYPROPERTIES('2rGHcrIw9EiO58rLVprMOV',#42,$,$,(#172),#330303);
```


### 1.6 IfcSpace <a id="ifc.IfcSpace">

**Besoin:** Chaque espace fonctionnel doit être modélisé comme `IfcSpace`. Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf). 
  - Chaque `IfcSpace` doit avoir un nom indiqué dans l'attribut `IfcSpace.Name`.
  - La géométrie de chaque IfcSpace doit être modélisée correctement en 3D de préférence avec une géométrie de type Body Brep Geometry (les géométries de type ‘Body SweptSolid Geometry’ et ‘Body Clipping Geometry’ seront également supporté par l’ACT), afin de connaître l’empreinte de la partie de lot et la hauteur des plafonds. 
  - `ACT_PartieDeLot.Nature` doit avoir une valeur conforme.
  - La valeur de property `ACT_PartieDeLot.Lot` contient une valeur structurée: numéro,bloc,escalier,niveau (p.e. 001,A,B,81).

**Méthode de test:**
Pour chaque objet IfcSpace :
  - Vérifiez que l'attribut `IfcSpace.Name` porte une valeur
  - Vérifiez que l'objet a une géométrie en 3D: l'objet doit avoir au moins une représentation 'body' (`IfcShapeRepresentation.RepresentationIdentifier = 'Body'`) de type (`IfcShapeRepresentation.RepresentationType`) `'Brep'` `'SweptSolid'`, ou `'Clipping'`.
  - Vérifiez que si l'objet a un propertyset `ACT_PartieDeLot` associé, que les properties `ACT_PartieDeLot.Nature` et `ACT_PartieDeLot.Lot` ont une valeur.
  - Vérifiez si ACT_PartieDeLot.Nature a une valeur de la liste de codes correspondante.
  - Vérifiez que si l'objet a un propertyset `ACT_PartieDeLot` associé, la valeur du property ACT_PartieDeLot.Lot respecte les exigences de format requises: *numéro,bloc,escalier,niveau* (p.e. 001,A,B,81).

**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.


### 1.7 IfcZone <a id="ifc.IfcZone">

**Besoin:** Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf).
Chaque objet IfcZone associé à un ou plusieurs parties de lots, doit avoir les attributs suivants:
  - `IfcZone.Name`: le nom doit être composé du numéro,bloc,escalier,niveau (p.e. 001,A,B,81). Ceci permet de générer le tableau descriptif de division.
  - `IfcZone.ObjectType`: l’attribut ObjectType doit être rempli avec une des valeurs qui représentent la nature des lots (voir Annex II).

**Méthode de test:**
Pour chaque objet IfcZone qui est associé (`IfcZone.IsGroupedBy`) avec des objet IfcSpace qui ont un property set `ACT_PartieDeLot`: 
  - Vérifiez si le nom `IfcZone.Name` a une valeur et respecte les exigences de format (numéro,bloc,escalier,niveau (p.e. 001,A,B,81)).
  - Vérifiez si `IfcZone.ObjectType` a une valeur de la liste de codes correspondante.

**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.

### 1.8 Murs de séparation <a id="ifc.cloisons">

**Besoin:** chaque élément de séparation doit avoir un propertyset (`Pset_WallCommon`, `Pset_ColumnCommon`, `Pset_WindowCommon`, `Pset_CurtainWallCommon`) dont les caractéristiques suivants doivent être fournies : `LoadBearing` et `IsExternal`. Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf).

**Méthode de test:**
Pour chaque élément de séparation (`IfcWall`, `IfcCurtainWall`, `IfcWindow` ou `IfcColumn`) :
  - vérifiez que l'élément a un jeu de propriétés associé pour lequel les propriétés `LoadBearing` et `IsExternal` sont fournies.
  - Vérifiez que les éléments porteurs ou externes (`LoadBearing = true` ou `IsExternal = true`) ne sont pas classés comme étant privés (`ACT_Propriete.Nature = 'privatif'`) ou mutualisés (`ACT_Propriete.Nature = 'mutuel'`).

**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.

### 1.9 Géometries simplifiées <a id="ifc.geometries">

**Besoin:** Evitez les types de géométries compliqués: le format IFC permet de types de géométrie paramétriques compliqués qui sont à présent peu utilisés et donc peu supportés par les outils.  Pour cette raison, le fichier IFC ne peut que contenir géométries selon les contraintes incluses dans la coordination view MVD de IFC2x3 TC1 et reference view MVD de IFC4 ADD2 TC1. Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf).

**Méthode de test:**
Pour chaque élément spatial, vérifiez que l'élément n'a pas de représentation 'Body' (`IfcShapeRepresentation.RepresentationIdentifier = 'Body'`) de type (`IfcShapeRepresentation.RepresentationType`) `CSG` `AdvancedSweptSolid` ou `AdvancedBrep`.

**Type:** test automatisé.

**Résultat:** avertissement, en cas de non-conformité.

### 1.10 Remplissage sans intersections <a id="ifc.topologie">

**Besoin:** Afin que le tableau descriptif de division puisse être correctement généré, les intersections (doublons, géométries contenues par une autre géométries) suivantes doivent impérativement être évitées :
  - IfcSpace - IfcSpace: deux parties de lot ne peuvent pas s’intersecter. Ceci vaut aussi pour les IfcSpace déterminant le volume pris par des escaliers ou ascenseurs.
  - IfcFlowSegment - IfcWall: les gaines techniques (p.e. IfcFlowSegment) sont incluses dans des ouvertures des objets mur.
  - IfcSpace - IfcWall: les murs extérieurs mais aussi les murs intérieurs ne peuvent pas être inclus dans la géometries d’un IfcSpace. Source: [LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf](./LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf).


**Méthode de test:**

**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.


## 2. Règles de délimitation des lots privatifs pour le Cadastre Vertical <a id="délimitation">

Les règles de délimitation sont dérivés du document '[Cadastre vertical – Recommandations - 2e partie : La délimitation des lots privatifs, Octobre 2012.](https://act.public.lu/fr/publications/telechargement-documents/technique/Recommandations_Delimitation.html)'.

### 2.1 Intersection de lots privatifs <a id="délimitation.intersection">
**Besoin**: Aucun des lots privatifs n’empiète sur la parcelle voisine ou la voirie publique [Délimitation, 2.2](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=2).

**Méthode de test:**
  -Déterminez l'empreinte en 2D de chaque élément IfcSpace et de chaque élément de séparation (IfcWall (et sous-classes), IfcCurtainWall, IfcWindow). Le résultat sont des polygones.
  -Appliquez une transformation de coordonnées Helmert (échelle, rotation, translation) pour convertir les géométries en LUREF (EPSG:2169) en utilisant les paramètres IfcMapConversion.
  -Vérifiez que les polygones obtenus ne se croisent pas ou ne sont pas situés à l'extérieur de la géométrie parcellaire cadastrale dans le BD-MO. Appliquer une erreur de tolérance de 2 dm. En cas de chevauchement, déclenchez un avertissement.

**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.

### 2.2 Plan de mensuration officielle <a id="délimitation.mensuration">
**Besoin**: La délimitation de la parcelle de support fixe parmi d’autres facteurs, la géométrie de l’immeuble qui détermine en premier la délimitation des lots privatifs. Un plan de mensuration officielle qui l’arrête et la documente, doit en conséquence être intégré
à la plupart des états descriptifs de division et scrupuleusement reproduit par tous les plans d’implantation et d’étage [Délimitation, 2.2](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=2).

**Type:** test manuel.

### 2.3 Conformité à l'autorisation de construire <a id="délimitation.autorisation">
**Besoin**: La délimitation est donc fonction de la partie littérale et graphique de l’autorisation de construire et les plans descriptifs de division doivent s’accorder avec les plans autorisés. Cette concordance assure aussi que la division de l’immeuble est conforme aux réglementations communales relatives à l’aménagement et à la construction, et que tous les lots privatifs sont effectivement réalisables [Délimitation, 2.3](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=2)..

**Type:** test manuel.

### 2.4 Conformité des natures <a id="délimitation.nature">
**Besoin**: chaque lot privatif rassemble le corps d’une même nature et les parties accessoires qui le servent, de manière à constituer une unité transférable de propriété exclusive [Délimitation, 2.4](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=2).

**Méthode de test:** Dans le fichier IFC, la nature des lots est encodée sous la forme `IfcZone.ObjectType`. La nature des LotParts (parties de lot) est incluse dans `ACT_PartieDeLot.Nature`.
  - Vérifiez pour chaque ACT_PartieDeLot.Nature si elle est autorisée en combinaison avec la valeur correspondante pour IfcZone.ObjectType.
  - Générez une erreur si les natures des parties de lot sont incompatibles avec la nature du lot auquel elles appartiennent.

**Type:** test automatisé.


### 2.5 Adjacence parties de lot <a id="délimitation.adjacence">
**Besoin**: Aucun lot privatif ne peut rassembler des éléments situés sur différents niveaux ou situés sur le même niveau, mais séparés par un autre lot privatif ou par des parties communes (autres que les murs de refend) ; ainsi les duplex (triplex) courants sont formés par deux (trois) lots privatifs sis sur deux (trois) niveaux consécutifs et reliés entre eux. [Délimitation, 2.4](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=3).

**Méthode de test:** 
 1. Pour chaque objet IfcZone (Lot), vérifiez que:
    - Tous les objets IfcSpace (parties de lot) associés à ce lot sont liés au même objet IfcBuildingStorey ; or
    - Tous les objets IfcSpace associés à ce lot ne sont liés à aucun objet IfcBuildingStorey (dans le cas d'espaces extérieurs ; jardins).
Génère une erreur si un lot contient des parties de lot qui se trouvent sur différents magasins de construction.
 2. Pour chaque objet IfcZone (Lot), vérifiez que :
    - Le lot ne comprend qu'un seul objet IfcSpace ; or
    - l'empreinte 2D de chaque objet IfcSpace est au moins à moins de 0,5 m d'un autre objet IfcSpace. Émettre un avertissement si un lot se compose de parties de lot séparées.

**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.

### 2.6 Lots privatifs sans accès direct aux communs <a id="délimitation.acces">
**Besoin**: une délimitation qui engendre, soit des lots privatifs sans accès direct aux communs, soit des parties communes exiguës ou enclavées, soit des servitudes en conséquence, doit être évitée. (Lots sans accès au communs / parties communes enclaves) [Délimitation, 2.4](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=3).

**Type:** test manuel.

### 2.7 Délimitation suivant le même schéma <a id="délimitation.congruence">
**Besoin**: Des lots privatifs semblables, quant à leur nature, à leur situation, à leur géométrie et à leur réalisation, doivent tous être délimités suivant le même schéma (exemple : une série d’emplacements souterrains adjacents) [Délimitation, 2.5](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=3).

**Type:** test manuel.

### 2.8 Status des éléments de construction <a id="délimitation.statut_élément">
**Besoin**: Le statut des éléments de construction qui matérialisent la quasi-totalité des limites des lots privatifs intérieurs, est fixé comme suit:
  - Commun : 
     - les dalles
     - les murs de façade et de pignon
     - les murs et les piliers de refend
     - les cloisons délimitant les parties communes communs
  - Mitoyennes : les cloisons délimitant deux lots privatifs contigus mitoyennes
  - Privative : les cloisons (amovibles ou non) à l’intérieur du même lot privatif privatives [Délimitation, 2.6](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=4).

**Type:** test manuel.


### 2.9 Statut des ouvertures <a id="délimitation.statut_ouverture">
**Besoin**: Chaque ouverture (fenêtre, porte, porte-fenêtre, porte de garage, baie vitrée …) a le même statut que l’élément de construction dans lequel elle a été pratiquée. Le repère de la délimitation est alors celle des parties alignées de son encadrement (retombée, allège ou pan latéral aligné) qui avance le plus dans la pièce [Délimitation, 2.8](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=4).

**Type:** test manuel.

### 2.10 Statut des éléments de protection <a id="délimitation.statut_protection">
**Besoin**: Les éléments de construction et d’équipement, destinés à la protection des personnes et des meubles (escaliers de secours, murs pare-feu …), doivent appartenir aux parties communes [Délimitation, 2.9](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=5).

**Type:** test manuel.

### 2.11 Statut des gaines techniques et cheminées <a id="délimitation.statut_gaine">
**Besoin**: Les gaines techniques et les cheminées, ensemble avec leurs coffrages, appartiennent toutes aux parties communes. Elles peuvent être privatives, si leur affectation à ces parties de l’immeuble, ne porte aucun préjudice à la copropriété et si elles ne servent qu’un seul lot privatif ou qu’un ensemble de lots privatifs qui, de par leur destination et leur disposition, sont détenus par le même copropriétaire. Cette exception ne vaut pas pour les conduits qui passent par une unité d’habitation, assimilable à une maison unifamiliale, mais intégrée dans un immeuble soumis au statut de la copropriété [Délimitation, 2.10].

**Type:** test manuel.

### 2.12 Statut des cours anglaises <a id="délimitation.cours_anglaise">
**Besoin**: Les cours anglaises et leurs projections verticales, au moins jusqu’au premier niveau hors sol, appartiennent généralement aux parties communes [Délimitation, 2.11](https://act.public.lu/fr/publications/telechargement-documents/technique/Recommandations_Delimitation.html).

**Type:** test manuel.

### 2.13 Escalier trémie à exclure <a id="délimitation.escalier">
**Besoin**: La cage d’un escalier reliant deux parties privatives qui sont directement superposées et, de par leur destination et leur disposition, détenues par le même copropriétaire, est intégrée dans le lot privatif en bas de l’escalier. La partie de la trémie en haut de
l’escalier, qui n’est pas surplombée par un deuxième escalier (donc l’entière trémie au niveau supérieur), n’est attribuée à aucun lot privatif [Délimitation, 2.12](https://act.public.lu/fr/publications/telechargement-documents/technique/Recommandations_Delimitation.html).

**Méthode de test:** 
Pour chaque géométrie d'escalier à exclure (donct objets IfcSpace avec `ACT_PartieDeLot.Nature` égal à `'ESCALIER INTERIEUR - TREMIE A EXCLURE'`)
  - Vérifiez qu'il existe au moins un autre objet IfcSpace avec `ACT_PartieDeLot.Nature` dans ('ESCALIER INTERIEUR') qui a la même valeur que `ACT_PartieDeLot.Unite` comme géométrie d'escalier exclue et dont la géométrie d'empreinte 2D touche ou est adjacente à la géométrie de la géométrie de l'escalier exclue.
  - Émettre un avertissement si ce n'est pas le cas.

### 2.14 Trappe ou un vide décoratif <a id="délimitation.vide">
**Besoin**: une trappe ou un vide décoratif ne fait pas partie du lot privatif qui l’englobe [Délimitation, 2.13](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=5.

**Type:** test manuel.

### 2.15 Ascenseur ou monte-charge privatif <a id="délimitation.ascenseur">
**Besoin**: La cage d’un ascenseur ou d’un monte-charge reliant exclusivement plusieurs parties privatives qui, de par leur destination et leur disposition, sont détenues par le même copropriétaire, est intégrée dans le lot privatif qui est desservi par l’ascenseur ou par le monte-charge au niveau correspondant. Un lot privatif indépendant n’est constitué qu’aux niveaux où l’ascenseur ou le monte-charge donne seulement accès aux parties communes [Délimitation, 2.14](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=6).

**Type:** test manuel.

### 2.16 Escalier extérieur <a id="délimitation.escalier_exterieur">
**Besoin**: L’escalier extérieur qui relie des parties du même niveau (donc l’escalier de quelques marches seulement et par conséquent d’une moindre hauteur) appartient au balcon ou à la terrasse attenante. En revanche, l’escalier extérieur qui relie des parties de deux niveaux consécutifs (donc l’escalier d’une hauteur de l’ordre d’un étage) forme un lot privatif indépendant au niveau inférieur [Délimitation, 2.15](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=6).

**Type:** test manuel.

### 2.17 Passerelle extérieure <a id="délimitation.paserelle_exterieure">
**Besoin**: La passerelle extérieure, accolée à un balcon ou à une terrasse, fait partie intégrante de ce balcon ou de cette terrasse. Dans le cas contraire, elle constitue un lot privatif indépendant (de nature Accès) [Délimitation, 2.16](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=6).

**Type:** test manuel.

### 2.18 Installations de parking avec plateformes superposées ou coulissantes <a id="délimitation.emplacements_superposés">
**Besoin**: Les emplacements encastrés dans un système de parcage à plates-formes superposées ou coulissantes, sont délimités par ces plates-formes mêmes. En cas de rangées d’emplacements disposées les unes directement derrière les autres, une aire de manoeuvre commune de la taille minimale d’un emplacement, est prévue dans chacune des rangées de devant où les véhicules sont garés sur des plates-formes coulissantes [Délimitation, 2.17](https://act.public.lu/dam-assets/fr/publications/documents-techniques/Recommandations_Delimitation.pdf?page=6).

**Type:** test manuel.

### 2.19 Au moins un local commun <a id="délimitation.local_commun">
**Besoin**: Condition pour un établissement d’un cadastre vertical : il faut au moins un local commun, les murs extérieurs ne sont pas suffisants comme parties communes.

**Méthode de test:** 
Vérifiez qu'il existe au moins un objet IfcSpace qui n'a pas de valeur pour ACT_PartieDeLot.Lot, car cet espace peut être considéré comme un espace commun.

**Type:** test automatisé.

**Résultat:** erreur, en cas de non-conformité.
