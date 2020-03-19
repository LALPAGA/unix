# 🐧 LALPAGA'S UNIX PERSONAL TOOLBOX

If you dislike *very long explanations* that goes too deeply into details and waste you time, `you're at the right place !`

> To sum up :
>
> &mdash; _**"The UNIX man if i were to do it."**, LALPAGA_

It may seem presumptuous or provocative at first, but just take it as _**"Unix if i were to explain it to my grandma"**_.

# 📚 Table of Contents

- [⏲ PROGRAMMED TASKS](#-programmed-tasks)
- [👾 PROCESSES](#-processes)
- [🕘 CRON](#-cron)
- [👹 DEAMONS](#-deamons)
- [🗃 USEFUL FOLDERS AND FILES](#-useful-folders-and-files)

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

### Quirky example

```js
echo "ps 2>&1 > ps.result" | at now + 1 minute
```

## `batch` command

Same as `at` but waits for the load average to be below 0.8. See [`top`](#top-command) command for load average.

`at` launches the task anyway, even if the processor is overloaded.

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

### Queues

A queue is referenced by a _single letter_ in `[a-z][A-Z]`. The _c_ queue is the default `at` queue, _E_ for `batch`.

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

# 🕘 CRON

`cron` is the deamon that will launch tasks every _period of time_ we give to it.

## QUICK OVERVIEW

### Useful `man` entries

```js
man cron
man 5 crontab
```

### How to use it

#### You have to add a new line in `/etc/crontab` this way : 

```
mm hh jj MMM JJJ user commande
```

##### The fields

> `mm` : minutes from 0 to 59
>
> `hh` : hours from 0 to 23
>
> `jj` : number of the day of the month from 1 to 31
>
> `MMM` : number of the month from 1 to 12
>
> `JJJ` : name of the day,  0 = Sunday, 1 : Monday, _7 is also Sunday_
>
> `user` : the task will be exectued of behalf of this user

##### Notations for values

> `\*` : at every time unit
>
> `2-5` : unités 2,3,4,5
>
> `*/3` : toutes les 3 unités de temps (0,3,6...)
>
> `5,8` : unités de temps 5 et 8

##### Exemple

```js
*/2 7-23 * * 1-5 romain ps -aux | grep romain | wc -l >> /tmp/nb-procs
```

# 👹 DEAMONS

# 🗃 USEFUL FOLDERS AND FILES

## 📁 Files
- `/etc/init.d` : tasks that launch at the start
