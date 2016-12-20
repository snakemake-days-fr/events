Comment gérer des analyses de grande taille (plusieurs dizaines voir centaines de flowcells hiseq 4000) ? Comment gérer des centaines de milliers de commandes à exécuter et de fichiers résultats ?
Reprises d’erreur possible ?

Dans le cadre du projet France Génomique (https://www.france-genomique.org/spip/
), les différents partenaires peuvent avoir accès au Centre de Calcul Recherche et Technologie du CEA (http://www-ccrt.cea.fr/).

Ce centre, comme d’autres centres à travers la France ou l’Europe, offre les ressources (de stockage et de calcul) nécessaires aux traitements de données génomiques de plus en plus importantes.

Dans ces centres, il est tout à fait envisageable de mobiliser plusieurs dizaines milliers de coeurs en parallèle afin de réaliser des analyses d’envergure. Comme par exemples lors des études du projet Tara (http://oceans.taraexpeditions.org/) ou lors d'autres études comparatives sur des milliers d’échantillons.

L’architecture de ces centres (HPC) impose un certains nombre de règles afin que l’ensemble des communautés ayant accès aux calculateurs puissent cohabiter et effectuer leurs analyses dans des bonnes conditions. Les principales contraintes sont : 
la durée de job : < 24H,
le nombre de job soumis : < 128 (au CCRT),
...

Pour que snakemake puisse être utilisé sur ces centres afin d’effectuer des analyses mobilisant plusieurs dizaines milliers de coeurs, il est nécessaire de :
regrouper plusieurs règles dans un même job,
distribuer en broadcast certains fichiers (comme des références, ou des BDD) sur les TMP des noeuds.

Nous avons pas pu déterminer si snakemake possède ces fonctionnalitées aujourd’hui.

Snakemake pourrait par exemple regrouper des règles parallélisables et ayant les mêmes caractéristique dans des jobs arrays (drma ( https://www.drmaa.org/ )pourrait éventuellement offrir cette possibilité, et ne nécessiterait pas d'implémenter la gestion de jobs arays pour chaque gestionnaires de ressources).

L’utilisation d’un autre gestionnaire de workflow, comme Pegasus (https://pegasus.isi.edu/)
 peut être également envisagé. 

En état, snakemake peut exécuter uniquement 128 jobs (et donc règles) en parallèle au CCRT en utilisant les directives sbatch (des tests sont prévus prochainement), il s’agit d’une limite si nous avons plusieurs millions de commandes à exécuter et plusieurs dizaines de coeurs de disponibles…
