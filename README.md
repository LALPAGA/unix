# 🐧 LALPAGA'S UNIX PERSONAL TOOLBOX

If you dislike *very long explanations* that goes too deeply into details and waste you time, `you're at the right place !`

> To sum up :
>
> &mdash; _**"The UNIX man if i were to do it."**, LALPAGA_

It may seem presumptuous or provocative at first, but just take it as _**"Unix if i were to explain it to my grandma"**_.

# 📚 Table of Contents

- [⏲ PROGRAMMED TASKS](#-programmed-tasks)
- [👾 PROCESSES](#-processes)

# ⏲ PROGRAMMED TASKS

## `at` command

To schedule a task.

### Quick overview

```js
 at now // -> executes now
 at 15:00 // -> at entered time
	ls -l > trace // -> output in trace file
	<EOT> // (ctrl+d) -> EOF to quit 
 atq // -> programmed jobs (<=> at -l)
 atrm <jobid> // -> to remove a job (<=> at -d <jobid>)
```

### To write the output in `stdin`

```js
tty // -> /dev/tty/0
ls -l > /dev/tty // in a batch or at command
```

### Quirky exemple

```js
echo "ps 2>&1 > ps.result" | at now + 1 minute
```

## `batch` command

Same as `at` but waits for the load average to be below 0.8. See [`top`](#top-command) command for load average.
It launches the task anyway, even if the processor is overloaded.

## `batch` and `at` properties

### `at.allow` and `at.deny` 

To determine who can submit jobs via `at` or `batch`.
The format of these files is a list of usernames, one on each line. Whitespace is forbidden. 

#### Location :

```js
/etc/at.deny
/etc/at.allow
```
#### Also

`at.allow` doesn't necessarily exist.
`at.allow` has priority over `at.deny` (it reads it first).

> ### Queues
>
> A queue is referenced by a _single letter_ in `[a-z][A-Z]`. The _c_ queue is the default `at` queue, _E_ for `batch`.
>
> Greater letter = greater niceness level.
>
> `-q` \<queue\> to use the specified queue.

# 👾 PROCESSES

## `top` command 

### Load average 

"charge processeur moyenne" in french. Respecively for the last minute, last 5, and last 15. 
> - `<1` : not enough processes to totally occupy the machine.
>
> - `1` : a single process at every moment, nobody is waiting their turn.
>
> - `>1` : process are in concurrence.  Caution : processes that are waiting for I/O count, even though their don't get processed by the processor.

## `nice` command

To run a program with a modified scheduling priority.

### Quick overview

#### Exemple

```js
nice -n <level> [COMMAND]
```

> **Niceness** ranges from -20 (most favorable scheduling) to 19 (least favorable). _10 by default_.



# 👷‍ [work in progress] 👷‍ LANCEMENT CYCLIQUE : TÂCHE CRON

cron est le deamon qui va lancer des tâches aux intervalles qu'on lui indique

man cron

man 5 crontab


Il faut rajouter la commande dans le fichier /etc/crontab de la sorte : 

mm hh jj MMM JJJ user commande

mm : minutes de 0 à 59

hh : représente l'heure de 0 à 23

jj : représente le numéro du jour du mois (de 1 à 31)

MMM : représente le numéro du mois de 1 à 12

JJJ : représente l'abréviation du nom du jour, 0 : dimanche, 1 : lundi

7 représente aussi dimanche

user : la tache sera exécutée à son nom

\* : à chaque unité de temps

2-5 : unités 2,3,4,5

*/3 : toutes les 3 unités de temps (0,3,6...)

5,8 : unités de temps 5 et 8

# 👹 DEAMONS
