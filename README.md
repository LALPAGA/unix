# LALPAGA'S UNIX PERSONAL TOOLBOX

pour faire une référence : [`[]` is truthy, but not `true`](#-is-truthy-but-not-true)

## `at` command

> at now -> exécute la commande maintenant
> at 15:00 -> exécutera la commande à l'heure indiquée
	> ls -l > trace -> mettra le résultat de ls -l dans le fichier trace
	> <EOT> (ctrl+d) -> pour quitter le at (caractère end of file)
> atq -> affiche les liste des actions programmées en attente (<=> at -l)
> atrm <jobid> -> supprime la tache programmée (<=> at -d <jobid>)

pour écrire le résultat de la commande de at dans le terminal :
> tty -> /dev/tty/0
> (dans at now) ls -l > /dev/tty

executes commands when system load levels permit; in other words, when the load average drops below 0.8, or the value specified in the invocation of atd.
top -> load average : donne la charge processeur moyenne de la dernière minute, des 5 dernières minutes et du dernier quart d’heure
<1 : pas assez de processus pour occuper complètement la machine
1 : à tout moment un seul processus est en état de travail, aucun autre n'attend son tour
>1 : les processus sont en concurrence. attention les processus en attend d'I/O sont comptabilisés dans le load average mais il est "bloqué"
cela ne signifie donc pas toujours que le processeur est surchargé
(le processeur constitue l'architecture matérielle sur lequel les processus d'une système 
d'exploitation s'exécutent)
ainsi, la commande batch permet de lancer les commandes données seulement quand la charge du système 
le permet (pour ne pas dépasser la capacité du processeur)
avec at la commande sera lancée quand même, même s'il y a actuellement une grosse charge

autre exemple :
echo "ps 2>&1 > ps.result" | at now + 1 minute

at.allow, at.deny - determine who can submit jobs via at or batch
The format of the files is a list of usernames, one on each line. 
Whitespace is not permitted.
à noter que at.allow n'existe pas toujours
at.allow a la priorité sur at.deny
exemple : cat /etc/at.deny
syn
sys
www-data

J'ai rajouté mon user "romain" dedans en sautant une ligne dans la liste
maintenant j'ai besoin de faire at avec un sudo sinon ça medit "You do not have permission to use at."

les files d'attente : -q file Utiliser  la  file d'attente mentionnée.  Une file
               est designée par une simple lettre, dans  l'inter­
               valle  a jusqu'à z, et A jusqu'à Z.  La file c est
               la file d'attente par défaut pour at tandis que la
               file  E est celle par défaut pour batch.  Plus les
               files ont une lettre importante, plus les  travaux
               seront  exécutés  avec  une  valeur de gentillesse
               (voir nice(1)) élevée.  Si un travail  est  enreg­
               istré  dans une file avec une lettre majuscule, il
               sera transmis à batch à l'heure indiquée.
nice va de -20 (plus favorable) à 19 (moins favorable)
nice [OPTION] [COMMAND ARG...]
