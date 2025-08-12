# Analyse travaux préventifs 

### Analyse préliminaire des préventifs au niveau des marchés (et documents pertinents)

##### Éxplication des fichiers

Données préventives 2025: "Tunnels_Cout préventif_13.06.2025 (5).xlsx"  

Données préventives 2023/24: "prodQuantAnLieuUniTyp2324.csv"  


Notebook Python principal: "analyse_preventifs_23_25.ipynb"  

Tables de correspondance (marchés/tunnels): "tableCorrespondanceMarches23_24_25.csv"  

Tableau des préventives aggregés 2023-2025: "preventifs_23_24_25.csv"  


Fichier .pdf des analyses final (rapport): "analyse_preventifs_2023_25.pdf"  


### Analyse des travaux préventifs au niveau des prix 

'prdp_prod_code_montants_designation_2324.csv' : tableau des produits préventifs groupés par marché, prod_code, prdp_code (code prix permanent), montants HT des années 2023/24 et la designation produit (le détail de l'operation) --> ce tableau a été produit à la main  


Exemple:  


| mar_diminutif | prod_code | prdp_code  | montant_ht_2023 | montant_ht_2024 | prod_designation                                                             |
|---------------|-----------|------------|------------------|------------------|--------------------------------------------------------------------------------|
| Bat           | B2P001    | BATMAI001  | 48033.05         | 95676.42         | Prestation de maintenance préventive d'une issue de secours                   |



'prdp_groupe détail.csv' : tableau de correspondance entre les groupes de produits du tableau de Fanny (pour montants normatives 2025) et groupes (6 lettres) des code prix permanents (prdp_code) et leur explication, qui correspond à la logique de chaque marché (groupement par PCTT, par tube, etc...) --> ce tableau a été produit à la main

'coutTunnelsPreventifs_parPrix.csv' : tableau des produits préventifs du tableau de Fanny, groupés par tunnel et marché
- chaque référence prix (ref_prix) est extraite du onglet 'Préventifs_tunnels' du fichier 'Tunnels_Cout préventif_05.05.2025 (5).xlsx';
- le coéfficient est toujours 1, sauf quand le type de formule est une division (prix par tube/équipement);
- frq_totale est la fréquence annuelle des préstations + l'ajustement fréquence supplémentaire;
- nb_equipe est le nombre équipé dans le cas où le prix correspond à un nombre d'équipements;
- 'form_typ' est le type de la formule, qui est soit une référence directe à un seul prix, soit une somme de plusieurs prix, soit une division par 2 (dans le cas où c'est le prix par tube dans un tunnel)
- 'prdp_code' est le code permanent de chaque prix
- 'cout_ht' est le coût HT de chaque produit
- 'cout_total' est le coût HT total (frq_totale * nb_equipe * coeff * cout_ht) par catégorie des produits du tableau de Fanny (~70 par marché)


'coutTunnelsPreventifs_parPrix.ipynb' est le Notebook Python principal qui crée le document 'coutTunnelsPreventifs_parPrix.csv'; des commentaires sont inclus pour expliquer le travail et les lignes de code  

'test_préventifs.ipynb' est le Notebook Python avec les tests d'éxhaustivité de ces fichiers  

'prod_erreurs_montants.csv' est un fichier CSV avec tous les prix qui sont probablement une erreur ou qui ne sont pas clairement décrits et donc pas possible à catégoriser. Il faudra les catégoriser à la main pour chaque prix individuel  


### Tableau préliminaire remplaçant la version de Fanny

###### Fichiers:

- 'equipements_tunnel.csv': tableau contenant toutes les équipements dans Coswin et leur quantité par tunnel
- 'produits_tunnel.csv': tableau contenant tous les travaux préventifs en 2023-2025 par marché, tunnel, code produit permanent (prdp_code), groupe de produits (prod_groupe), fréquence annuelle, fréquence supplémentaire, fréquence totale, nombre d'équipements, coût HT, coût total HT et les équipements utilisées pour le travail

|mar_diminutif|tunnel     |prdp_code|prod_groupe                           |ref_prix  |freq_annuelle|ajust_freq_suppl|frq_totale|nb_equipe|cout_ht|cout_total|equipements                                      |
|-------------|-----------|---------|--------------------------------------|----------|-------------|----------------|----------|---------|-------|----------|-------------------------------------------------|
|ContReg      |Saint-Cloud|CRGDLA101|Vérification des dispositifs de levage|CRC 101,00|1.0          |0.0             |1.0       |0.0      |648.6  |0.0       |Dispositif de levage inférieur ou égal à 3 tonnes|


- 'createExcel.ipynb': code source qui importe 'equipements_tunnel' et 'produits_tunnel' et crée un fichier Excel avec plusieurs feuilles dans lesquelles le budget dépensé en préventifs peut être vu par tunnel, avec les calculs et coefficients correspondants
- 'rapport_tunnels.xlsx': le fichier Excel crée par le code source

###### Remarques:

- Le fichier 'produits_tunnels' contient une colonne 'lookup_key', une pour chaque combinaison unique de 'prod_groupe' et 'tunnel'. J'ai ajouté cette "helper_column" pour rendre les formules Excel plus sécurisées. En utilisant un "string" unique pour chaque combinaison unique, les formules INDEX/MATCH des feuilles tunnels peuvent associer les valeurs plus efficacement et sans distinction de casse. Sans cela, Excel ne peut pas gérer la correspondance dynamique des chaînes avec conversion en minuscules dans les formules, ce qui pourrait conduire à des erreurs ou des références rompues, et empêcher l'ouverture du fichier corrompu.
- Le tableau ne se concentre que sur quelques marchés (puisqu'il s'agit d'une version prototype) ; SignaDyn, MEC, Climatisation. sont absents. Pour les ajouter, un 'prdp_code' devra être généré puis ajouté à la table 'produits_tunnels'.
- Étant donné le nombre de formules et de feuilles, Excel affiche des messages d'erreur (formules et cellules corrompues) qu'il faut ignorer.
- Lorsque les valeurs sont modifiées dans les fichiers 'equipements_tunnel' et 'produits_tunnels', le fichier doit être enregistré et rouvert pour que les formules recalculent les nouvelles valeurs des cellules de chaque feuille tunnel.

