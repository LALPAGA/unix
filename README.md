# 🐧 LALPAGA'S UNIX PERSONAL TOOLBOX

If you dislike verylongexplanations that goes too deeply into details and waste you time, you're at the right place !

aKa _"The UNIX man if i were to do it."_

pour faire une référence : [`[]` is truthy, but not `true`](#-is-truthy-but-not-true)

# 📚 Table of Contents

- [⏲ PROGRAMMED TASKS](#-programmed-tasks)
- [👾 PROCESSES](#-processes)

# ⏲ PROGRAMMED TASKS

## `at` command

To schedule a task.

### Quick overview

```java
 at now // -> executes now
 at 15:00 // -> at entered time
	ls -l > trace // -> output in trace file
	<EOT> // (ctrl+d) -> EOF to quit 
 atq // -> programmed jobs (<=> at -l)
 atrm <jobid> // -> to remove a job (<=> at -d <jobid>)
```

### To write the output in `stdin`:

```
tty // -> /dev/tty/0
ls -l > /dev/tty // in a batch or at command
```

### Quirky exemple

```
echo "ps 2>&1 > ps.result" | at now + 1 minute
```

## `batch` command

Same as `at` but waits for the load average to be below 0.8. See [`top`command](#top-command) command for load average.
At launches the task anyway, even if the processor is overloaded.

## `batch` and `at` properties

### `at.allow` and `at.deny` 

To determine who can submit jobs via `at` or `batch`.
The format of these files is a list of usernames, one on each line. Whitespace is forbidden. 

#### Location :

```
/etc/at.deny
/etc/at.allow
```
#### Also

`at.allow` doesn't necessarily exist.
`at.allow` has priority over `at.deny` (it reads it first).

### Queues

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
