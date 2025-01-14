# Lab 5 - introduction to regular expressions in Linux bash console

## 1. Streams and commands

Command pipelining, like redirecting a standard stream to an external file, for example, allows you to pass the results of a command to another command as an input pipeline. This means you can execute a chain of commands where the result of the previous one is passed to the next one, until the last command. One of the most common cases is the use of the `grep` (Global Regular Expression Print) command, which searches a text string for the indicated values specified by a pattern. To begin, however, an example with the `wc` (word count) command.

```console
$ ls | wc -l
```

This command will pass the result of the `ls` command as input to the `wc -l` command, which will count the number of lines in the passed stream.
We can, of course, redirect such a result to an external resource, such as a file.

```console
$ ls | wc -l > how_many_resources.txt
```

If we need to simultaneously display the value that was redirected to a file we will use the `tee` command.

```console
$ ls | wc -l | tee file.txt
# or with an addition to the file
$ ls | wc -l | tee -a file.txt
```
Note that the output value of the last command, the `wc -l` command, is displayed.

## 2. Regular expressions introduction with grep command

In order to effectively use piping when searching for resources in the system, you need to learn a little more about regular expressions. Lab 03 has already presented the basic components and some examples. As a reminder:

`.` - a period replaces exactly one character  
`[abc]` - denotes the occurrence of any character from among those given - a,b or c - repeatedly and in various combinations  
`[a-z]` - denotes any character from the range a to z  
`[^]` - complement of the set e.g.: `[^aA]` - stands for any character that is not a or A, while.  
`[^a-z]` - means any character different from a to z  
`^` - such a designation will mean 'begins with', e.g.  
`^po` - begins with the letters `po`.  
`$` - means 'ends with', e.g. `ski$` - ends with `ski`.  
`\` - interprets the next character literally, e.g. `\\.` looks for a period instead of any character  

These are not all the possibilities at our disposal, but let's consider some examples using the above elements.

Command 
```console
$ ls | grep '[AOE]'
```
will match files whose name contains A,O or E.
```console
$ ls | grep '^[AOE]'
```
will match all files whose name begins with A,O or E.

```console
$ grep '[AOE]la' imiona.txt
```
finds all lines in the file `imiona.txt` that contain the words Ala, Ola or Ela. Also those of which the above values are part, such as Olaf. To limit the search to full words only, add the `-w` option to the `grep` command.

```console
$ grep -w '[AOE]la' imiona.txt
# we can also sort the results by piping to the sort command
$ grep '[AOE]la' imiona.txt | sort
```

The `-w` option for the `grep` command causes the search to look for words that match the pattern in its entirety rather than just containing it. 

The `sort` command also has useful options (check the manual) and among them:
* `-o` (output) - save the sorting result in a file with the specified name.
* `-r` (reverse) - reverses the result of the comparison.  
* `-u` (unique) - removes duplicate lines.

Now combining the `grep` and `sort` tools, we can find all the unique values in the file that meet the condition, sort them and count them.
```console
$ grep '[AOE]la' imiona.txt | sort | wc -l
```

The `grep` command itself also allows you to count the matches found, but using it with a pipe to the sort command will not work in this case.

The following command will search for the names of all files in the `/var/log` directory whose name contains any digits. Equivalently, we can use the command:
```console
$ ls /var/log | grep '[0-9]'
```

By using negation, we can search for the complements of the set specified by the pattern.
```console
$ ls /var/log | grep '[^0-9]'
```

However, this command may not work as expected for some. Well, let's go back to the previous example `$ ls /var/log | grep [0-9]` - means search for all values that at any position have one of the values - here a digit. And now translating the current command we can say “search for all values that on any position do not contain a digit”. So the command works as expected.
However, to display the values that do not contain digits we can use the `-v` option of the `grep` command:
```console
$ ls /var/log | grep -v '[0-9]'
```

We can also use this notation to search for values that have two digits in adjacent positions:
```console
$ ls /var/log | grep '[0-9][0-9]'
```

However, there are special tags that are used to determine the number of occurrences of a given pattern position. These can also be found in the manual of the `grep` command: https://manpages.ubuntu.com/manpages/noble/en/man1/grep.1posix.html.

It is also worth mentioning here an extended version of regular expressions, a brief description of which can be found at: https://en.wikipedia.org/wiki/Regular_expression#POSIX_extended and in the `grep` and `regex` command manual.

| Marker|Description|
|---|---|
|?| The preceding element is optional and will be matched at most once.
|* | The preceding element will be matched zero or more times. Called Kleene's complement.
|+ | The preceding element will be matched one or more times.
|{n} | The preceding element will match exactly n times.
|{n,} | The preceding element will match n or more times.
|{,m} | The preceding element matches at most m times. This is an extension of GNU.
|{n,m} | The preceding element matches at least n times but no more than m times.

**Examples**.
```console
# we can replace the search for a two-digit string from one of the examples above with the notation
# here using the -E option is necessary
# sometimes there is egrep command available also
# which is the same as grep -E
$ ls | grep -E '[[:digit:]]{2}'
```

The `-x` option, on the other hand, works similarly, but causes the pattern to be applied to the entire line - this means that the notation:
```console
$ ls | grep -x '[a-z]\+'
$ ls | grep -x '[a-z0-9]\+'
```

will only write out lines that consist of at least one lowercase letter. Another example is only lowercase letters and numbers. If we use the `-E` option of the `grep` command then we don't need to use the `\` character before `+`:
```console
$ ls | grep -xE '[a-z]+'
$ ls | grep -xE '[a-z0-9]+'
```

The extended version of expressions include, among others, character classes.

| Class | Matching characters |
|---|---|
[:alnum:] | Alphanumeric characters.
[:alpha:] | Alphabetic characters.
[:blank:] | Space and tab characters
[:cntrl:] | Control characters
[:digit:] | Numeric characters
[:punct:] | Punctuation marks

Be sure to use double square brackets, such as:
```console
$ ls /var/log | grep '[[:digit:]]'
# with explicit use of the -E - extended expression option
$ ls /var/log | grep -E '[[:digit:]]''
```

The `|` operator is used as an `OR` expression so that we can:
```console
$ ls | grep -wE 'Ola|Ala'
```
display all values that contain the word Ola or Ala.

Adding parentheses `()` allows us to group patterns and use them in the following way:
* the pattern `Physic(s|ist)` fits both `Physics` and `Physicist`
* `ca[gk]e` fits both `cage` and `cake` patterns.

Also remember that we can pipe the value returned by `grep` back to the `grep` command.

**Exercises**.

1. Display all resource names from your home folder whose name begins with a capital letter.
2. From your home folder, display all the resource names whose name begins with a '.' (period).
3. Count all the names from task number 2.
4. Display all names from your home folder that contain only uppercase and lowercase letters.
5. Display all the names from your home folder that do not contain uppercase letters.
6. Display all names from your home folder that do not contain a 3 letter extension.
7. In the `/etc` folder, find all names that start with 'c' and end with 'y'.
8. In the `/etc` folder, find all names that have two adjacent 's' letters.
9.  Build an expression that will select names that contain a six-letter string of characters, of which the first, third and fifth character can be any character, the second and fourth must be an uppercase letter, the sixth must be an `e`.
10. From your home folder, display all names that are exactly 4 characters in length. Those names should contain only lowercase and uppercase letters and numbers.
11. From the `/var/log` folder, display all names that have the extension 'log'.
12. Display the lines and their numbers by searching the `/var/log/auth.log.1` file for your username.
13. The same as in task 11, but searching the subfolders as well.
14. From the attached file `imiona.txt`, display all names starting with letters `A` through `K`.
15. From the file `imiona.txt` display only those names whose name is 4 or 5 letters long.
16. From the file `imiona.txt` count how many unique names ending with the letter 'A' there are.