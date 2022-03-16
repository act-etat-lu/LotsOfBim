 # rac_basic_sample_project.rvt

Cet exemple est basé sur le fichier IFC 
'[rac_basic_sample_project.rvt](https://knowledge.autodesk.com/fr/support/revit/getting-started/caas/CloudHelp/cloudhelp/2021/FRA/Revit-GetStarted/files/GUID-61EF2F22-3A1F-4317-B925-1E85F138BE88-htm.html)' 
de AutoDesk. Ce fichier fait partie des [exemples de fichiers de projet Revit](https://knowledge.autodesk.com/fr/support/revit/getting-started/caas/CloudHelp/cloudhelp/2021/FRA/Revit-GetStarted/files/GUID-61EF2F22-3A1F-4317-B925-1E85F138BE88-htm.html) 
qui sont disponibles sur Autodesk Knowledge Network sous une 
[licence 3.0 non transposée "Creative Commons Attribution - Pas d'utilisation commerciale - Partage dans les mêmes conditions"](https://creativecommons.org/licenses/by-nc-sa/3.0/deed.fr).

Le fichier Revit a été modifié selon le [guide de modélisation pour le cadastre vertical](../../LotsOfBIM-cadastre-vertical-modelisation-BIM.pdf).
Le résultat a été exporté en format IFC avec l'outil [revit-ifc](https://github.com/Autodesk/revit-ifc), 
moyennent ces fichiers de configuration Revit: 
* [ACT_pset_config.txt](ACT_pset_config.txt),
* [IFC_Configuration_-_IFC2x3_Coordination_View_-_Cadastre_Vertical.json](IFC_Configuration_-_IFC2x3_Coordination_View_-_Cadastre_Vertical.json), et
* [IFC_Configuration_-_IFC4_Reference_View_-_Cadastre_Vertical.json](IFC_Configuration_-_IFC4_Reference_View_-_Cadastre_Vertical.json). 


## Liste de changements:
* 2021-11-16: première version
* 2022-01-10: ajouter fichier CSV avec un résumé des lots
* 2022-03-06: compléter pset ACT_Propriete pour des éléments IfcCurtainWall, ajouter pset ACT_Acces.AccessPrincipal
* 2022-03-16: ajouter pset ACT_Etage.Nom
