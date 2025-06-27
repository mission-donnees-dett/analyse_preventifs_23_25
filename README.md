### Éxplication des fichiers

##### Analyse préliminaire des préventifs au niveau des marchés (et documents pertinents)

Données préventives 2025: "Tunnels_Cout préventif_05.05.2025.xlsx"
Données préventives 2023/24: "prodQuantAnLieuUniTyp2324.csv"

Notebook Python principal: "analyse_preventifs_23_25.ipynb"
Tables de correspondance (marchés/tunnels): "tableCorrespondanceMarches23_24_25.csv"
Tableau des préventives aggregés 2023-2025: "preventifs_23_24_25.csv"

Fichier .pdf des analyses final (rapport): "analyse_preventifs_2023_25.pdf"

##### Analyse des préventifs au niveau des prix 

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

'prod_erreurs_montants.csv' est un fichier CSV avec tous les prix qui sont probablement une erreur ou qui ne sont pas clairement décrits et donc pas possible à catégoriser. Il faudra les catégoriser à la main pour chaque prix individuel.
