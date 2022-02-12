---
layout: post
title: Intro to Bash Scripting
subtitle: Notes from Datacamp Course
categories: Bash
tags: [Bash, Intro]
---
*The content is based on DataCamp's [Intro to Bash Scripting](https://www.datacamp.com/courses/introduction-to-bash-scripting).*

## Intro

> Bash stands for **B**ourne **A**gain **Sh**ell

Bash was developed in the 80's. It is often the default in Unix systems and Macs. Unix is the backbone of the internet, which is why all mayor cloud providers have commandline interfaces to their products.

### Prerequisites: 

- Understand what the command line is (terminal, shell)
- Understand basic commands such as `cat`, `grep`, `sed` etc.
- check out the [bash shell cheat sheet](https://www.educative.io/blog/bash-shell-command-cheat-sheet) and this [explanation of sed](https://www.grymoire.com/Unix/Sed.html#uh-0)

#### Basic commands

- `(e)grep` filters input based on regex pattern matching
- `cat` concatenates file contents line-by-line
- `tail` \ `head` give only the last -n lines
- `wc` does a word or line count (flags: -w -l)
- `sed` pattern-matched string replacement

#### REGEX

Regular expression are a vital skill for Bash scripting. Great [site to test your expressions](https://regex101.com/).


- `[]` create a set, for example: `[afct]`
- `^` inverse a set, for example: `^[afct]`

#### Piping

- `|` used for piping, for example: `sort | uniq -c`

#### Examples

fruits.txt
```bash
banana
apple
carrot
```

##### 1
```Bash
grep 'a' fruits.txt
```
```Output
banana
apple
carrot
```
##### 2
```Bash
grep 'p' fruits.txt
```
```Output
apple
```
##### 3
```Bash
grep '[pc]' fruits.txt
```
```Output
apple
carrot
```
##### 4
```Bash
cat fruits.txt | sort | uniq -c | head -n 3
```
```Output
1 apple
1 banana
1 carrot
```

## Bash Scripts

### General structure

A bash script usually begins with

- `#!/usr/bash` is called ***shebang*** and tells the interpreter that this is a bash script
- `which bash` can be used to *check where bash is*
- *Middle* of script contains the code
- `.sh` is conventionally used as file extension
- `bash script_name.sh`, run the script
- `./script_name.sh`, run the script if shebang is the first line 

#### Example

```bash
#!/usr/bash
echo "hello world"
echo "Goodbye world
```
```Bash
./eg.sh
```

or
```Bash
bash eg.sh
```

```Output
Hello world
Goodbye world
```

Each line in your Bash script can be a shell command. Thus, you can also include pipes in your Bash script.

### Three streams for programs

- `STDIN` (standard input) - stream of data into the program
- `STDOUT` (standard output) - stream of data out of the program
- `STDERR` (standard error) - errors in the program

The streams come from and write out to the terminal.

`2> /dev/null` in script calls redirects STDERR to be deleted (`1> /dev/null` would be STDOUT)

![STDOUT_STDIN](/assets/images/post_images/intro_to_bash_scripting/STDOUT_STDIN.png)

#### Example

sports.txt
```txt
football
basketball
swimming
```

```Bash
cat sports.txt 1> new_sports.txt
cat new_sports.txt
```
```Output
football
basketball
swimming
```

### Arguments (ARGV)

Pass arguments by adding a space after the script execution call

- `ARGV` is the array of all the arguments given to the program
-  Each argument can be accessed via the `$` notation
    - the first argument as `$1`, the second as `$2` etc.
- `$@` and `$*` give all the arguments in ARGV
- `$#` give the length (number) of arguments

#### Example

```bash
#!/usr/bash
echo $1
echo $2
echo $@
echo "There are " $# "arguments"
```

> bash args.sh one two three four five

```Output
one
two
one two three four five
There are 5 arguments
```

## Basic variables in Bash

### Quotes and backticks
```Bash
firstname='Cynthia'
lastname='Liu'
echo "Hi there" $firstname $lastname
```
```Output
Hi there Cynthia Liu
```

- the `$` is crucial for bash to treat something as a variable and not a string
- Do not add spaces around the `=` sign
- Single quotes (`'sometext'`) = Shell interprets what is netween the quotes literally
- Double quotes (`"sometext"`) = Shell interprets literally **except** using `$`and backticks
- Backticks (**\`sometext\`**) = creates a *shell-within-a-shell*; Shell runs the command and captures STDOUT back into a variable

### Examples

#### Single quotes
```Bash
now_var='NOW'
now_var_singlequote='$now_var'
echo $now_var_singlequote
```
```Output
$now_var
```
#### Double quotes
```Bash
now_var='NOW'
now_var_doublequote="$now_var"
echo $now_var_doublequote
```
```
NOW
```

Typing the following command into the terminal returns the current date
```Bash
date
```

```Output
Tue Jan 18 10:51:50 CET 2022
```
#### Backticks

By using backticks, you can use `date` to invoke a *shell-within-a-shell*

```Bash
rightnow_doublequote="The date is `date`."
echo $rightnow_doublequote
```
```Output
The date is Tue Jan 18 10:51:50 CET 2022
```
A *shell-within-a-shell* can also be achieved by using `$(date)`
```Bash
rightnow_doublequote="The date is $(date)."
echo $rightnow_doublequote
```
```Output
The date is Tue Jan 18 10:51:50 CET 2022
```

## Numeric variables in Bash

Numbers are not natively supported in Bash.

### expr

- `expr` is a useful utility program (like `cat` or `grep`)

```Bash
expr 1 + 4
```

```Output
5
```

- `expr` cannot handle decimal places

### bc

- `bc` (basic calculator) opens calculator which can handle decimal places
- `bc` can be used in piping by sending a string

```Bash
echo "5 + 7.5" | bc
```

```Output
12.5
```

- `bc` has a `scale` argument for defining the number of decimal places

```Bash
echo "10 / 3" | bc
```

```Output
3
```

```Bash
echo "scale=3; 10 / 3" | bc
```

```Output
3.333
```

```Bash
dog_name='Roger'
dog_age=6
echo "My dog's name is $dog_name and he is $dog_age years old"
```

### Double bracket notation (not for decimals)

```Bash
expr 5 + 7
```

```Output
12
```

```Bash
echo $((5 + 7))
```

```Output
12
```

```Bash
model1=87.65
model2=89.20
echo "The total score is $(echo "$model1 + $model2" | bc)"
echo "The average score is $(echo "($model1 + $model2) / 2" | bc)"
```

## Arrays in Bash

### 1) *Normal* numerical-indexed structure

- equivalent to a list in Python

#### Option 1 (declare)

```Bash
declare -a my_first_array
```

#### Option 2 (brackets and spaces)

```Bash
my_array=(1 3 5 2)
```


##### Return full array
```Bash
echo ${my_array[@]}
```

```Output
1 3 5 2
```

##### Return length of an array

```Bash
echo ${#my_array[@]}
```

##### Accessing array elements
```Bash
echo ${my_array[2]}
```

```Output
5
```
##### Manipulating array elements

```Bash
my_array[1]=999
echo ${my_array[@]}
```

```Output
999 3 5 2
```
##### Slicing arrays

- Use `array[@]:N:M` to slice out a subset of the array
    - `N` is the starting index
    - `M` is the number of elements to be returned

```Bash
my_array=(15 20 300 42 23 2 4 33 54 67 66)
echo ${my_array[@]:3:2}
```

```Output
42 23
```

##### Appending to arrays

- Use array+=(elements)

```Bash
my_array=(300 42 23 2 4 33 54 67 66)
my_array+=(10)
echo ${my_array[@]}
```
```Output
300 42 23 2 4 33 54 67 66 10
```

### 2) *Associative* arrays

- similar to normal array, but with key-value pairs instead of numerical indexes (similar to a dictionary in Python)
- only available in Bash 4 onwards


#### Multiple line approach
```Bash
declare -A city_details # Declare first
city_details=([city_name]="New York" [population]=14000000) # Add elements
echo ${city_details[city_name]} # Index using key to return a value
```
```Output
New York
```

#### One line approach

```Bash
declare -A city_details=([city_name]="New York" [population]=14000000)
echo ${city_details[city_name]}
```

```Output
New York
```

#### Access the keys

- `!` to access the *keys*

```Bash
echo ${!city_details[@]}
```
```
city_name population
```

## IF statements

### Basic structure

```Bash
if [ CONDITION ]; then
    # some code
else
    # some other code
fi
```

- spaces between square brackets and conditional elements
- Semi-colon after close brakcet `];`

### Strings

- `==` for *equal to*
- `!=` for *not equal to*

```Bash
x="Queen"
if [ $x == "King" ]; then
    echo "$x is a King!"
else
    echo "$x is not a King"
fi
```
```
Queen is not a King!
```

### Arithmetic IF statements
#### 1) Double-paranthesis structure
```Bash
x=10
if (($x > 5)); then
    echo "$x is more than 5!"
fi
```
```Output
10 is more than 5!
```
#### 2) Square brackets with flags
##### Arithmetic bash conditional flags
- `-eq` for *equal to* (`==`)
- `-ne` for *not equal to* (`!=`)
- `-lt` for *less than* (`<`)
- `-le` for *less than or equal to* (`<=`)
- `-gt` for *greater than* (`>`)
- `-ge` for *greater than or equal to* (`>=`)

```Bash
x=10
if [ $x -gt 5 ]; then
    echo "$x is more than 5!"
fi
```
```Output
10 is more than 5!
```

##### Other bash conditional flags
- `-e` if the file exists
- `-s` if the file exists and has a size greater than zero
- `-r` if the file exists and is readable
- `-w` if the file exists and is writeable

More bash conditional expressions can be found [here](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html).


### AND and OR

- `&&` for AND
- `||` for OR

#### Chaining conditionals (1)

```
x=10
if [ $x -gt 5 ] && [ $x -lt 11 ]; then
    echo "$x is more than 5 and less than 11!"
fi
```

#### Chaining conditionals (2)

```
x=10
if [[ $x -gt 5 && $x -lt 11 ]]; then
    echo "$x is more than 5 and less than 11!"
fi
```

### IF and command-line programs
#### Option 1
words.txt
```txt
echo "Hello world!"
```
```Bash
if grep -q Hello words.txt; then
    echo "Hello is inside!"
fi
```
```Output
Hello is inside!
```
#### Option 2

```Bash
if $(grep -q Hello words.txt); then
    echo "Hello is inside!"
fi
```
- `-q` stand for *quiet* so it doesn't return the matched lines like grep normally does. It just returns true if any lines match.
- when using command-line arguments like grep in IF statements, there is no need for square brackets

## FOR loops
### Basic structure
```Bash
for x in 1 2 3
do
    echo $x
done
```
```Output
1
2
3
```
### Number ranges
#### 1) Brace expansion
- {START..STOP..INCREMENT}

```Bash
for x in {1..5..2}
do
    echo $x
done
```
```Output
1
3
5
```
#### 2) Three expression
- double parenthesis
```Bash
for ((x=2;x<=4;x+=2))
do
    echo $x
done
```
```Output
2
4
```

### Glob expansions
- `*` for pattern-matching expansions in for loops

```Bash
for book in books/*
do
    echo $book
done
```
```Output
books/book1.txt
books/book2.txt 
```
### Shell-within-a-shell
Let's assume the following folder structure:
```
books/
|+--AirportBook.txt
|+--CattleBook.txt
|+--FairMarketBook.txt
|+--LOTR.txt
|+--file.csv
```
Loop through the results of a call to shell-within-a-shell using `$()`:
```Bash
for book in $(ls books/ | grep -i 'air')
do
    echo $book
done
```
```
AirportBook.txt
FairMarketBook.txt 
```

## WHILE statements
### Syntax
```Bash
x = 1
while [ $x -le 3 ];
do
    echo $x
    ((x+=1))
done
```

```Output
1
2
3
```
<p style="color:red">Make sure not to create an infinite loop!</p>

```Bash
x = 1
while [ $x -le 3 ];
do
    echo $x
done
```
This will print out 1 forever!

## CASE statements

### Why CASE statements?

More optimal once IF statements get complex

### A complex IF statement

```Bash
if grep -q 'sydney' $1; then
    mv $1 sydney/
fi
if grep -q 'melbourne|brisbane' $1; then
    rm $1
fi
if grep -q 'canberra' $1; then
    mv $1 "IMPORTANT_$1"
fi
```


### CASE statement structure

```Bash
case 'STRINGVAR' in
    PATTERN1)
    COMMAND1;;
    PATTERN2)
    COMMAND2;;
    *)
    DEFAULT COMMAND;;
esac
```

### From IF to CASE

```Bash
case $(cat $1) in
    *sydney*)
    mv $1 sydney/ ;;
    *melbourne*|*brisbane*)
    rm $1 ;;
    *canberra*)
    mv $1 "IMPORTANT_$1" ;;
    *)
    echo "No cities found" ;;
esac
```

## Basic Functions

### Syntax
#### Option 1
```Bash
function_name () {
    #function_code
    return #something
}
```

#### Option 2
```Bash
function function_name {
    #function_code
    return #something
}
```
### Calling a function
```Bash
function print_hello () {
    echo "Hello world!"
}
print_hello
```
```Output
Hello world!
```
### Example
```Bash
temp_f=30
function convert_temp () {
    temp_c=$(echo "scale=2; ($temp_f - 32) * 5 / 9" | bc)
    echo $temp_c
}
convert_temp
```
```Output
-1.11
```

### Arguments, return values, and scope

- `$1` notation to access arguments
- `$@` and `$*` to give all the arguments in `ARGV`
- `$#` gives the length (number) of arguments

```Bash
function print_filename {
    echo "The first file was $1"
    for file in $@
    do
        echo "This file has name $file"
    done
}
print_filename "LOTR.txt" "mod.txt" "A.py"
```
```Output
The first file was LOTR.txt
This file has name LOTR.txt
This file has name mod.txt
This file has name A.py
```

### Scope
#### Global by default

> *Scope* in programming refers to how accessible a variable is.

- **Global**: something is accessible anaywhere in the program, including inside FOR loops, IF statements, functions etc.
- **Local**: something is only accessible in a certain part of the program

<p style="color:red">In Bash, all variables are global by default.</p>

```Bash
function print_filename {
    first_filename=$1
}
print_filename "LOTR.txt" "model.txt"
echo $first_filename
```

```Output
LOTR.txt
```

#### Restricting scope

- `local` to restrict variable scope

```Bash
function print_filename {
    local first_filename=$1
}
print_filename "LOTR.txt" "model.txt"
echo $first_filename
```
```Output

```

#### Return values

The `return` option in Bash is only meant to determine if the funciton was a success (0) or failure (other values 1-255). It is captured in the global variable `$?`

```Bash
function function_2 {
    echlo
}
function_2
echo $?
```

```Output
function_2: command not found: echlo
127
```

#### Returning correctly

```Bash
function convert_temp {
    echo $(echo "sclae=2; ($1 - 32) * 5 / 9" | bc)
}
converted=$(convert_temp 30)
echo "30F in Celsius is $converted C"
```

```Output
30F in Celsius is -1 C
```

## Cron to schedule scripts

Cron (from *chronos*) has been part of unix-like systems since the 70's. 

A **crontab** is a file that contains **cronjobs**, which each tell **cron** what code to run and when.

### Crontab - the driver of cronjobs

```Bash
crontab -l
```
```Output
crontab: no crontab for user
```
### Crontab and cronjob structure

There are 5 stars to set, one for each time unit.

```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                  7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * * <command to execute>
```
The default, `*` means *every*

### Example

```Bash
5 1 * * * bash myscript.sh
```
- Minutes star is 5 (5 minutes past the hour). Hours star is 1 (after 1am). The last three are `*`, so every day and month
    - Overall: **run every day at 1:05am**

```Bash
15 14 * * 7 bash myscript.sh
```
- Minutes star is 15 (15 minutes past the hour). Hours star is 14 (after 2pm). Next two are `*` (every day of month, every month of year). Last star is day 7 (on Sundays).
    - Overall: **run every day at 2:15pm every Sunday**

### Advanced cronjob structure

Run something multiple times per day or every *X* time increments.

- use a comma for specific time intervals
    - `15,30,45 * * * *` will run at 15, 30 and 45 minutes mark for whatever hours are specified by the second star
- use a slash for *every X increment*
    - `*/15 * * * *` runs every 15 minutes

### Create your first cronjob

Good [tutorial for Mac users](https://ole.michelsen.dk/blog/schedule-jobs-with-crontab-on-mac-osx/).

Let's schedule a script called `extract_data.sh` to run every morning at 1:30am:

1. In terminal type `crontab -e` to edit your list of conrjobs
    - It may ask what editor to use. `nano` is an easy option and a less-steep learning curve than vi (vim)
2. Create the cronjob:
    - 30 1 * * * extract_data.sh
3. Exit the editor to save it
    - `nano` (on Mac) you would use `ctrl` + `o` then `enter` then `ctrl` + `x` to exit
    - You will see a message `crontab: installing new crontab`
4. Check it is there by running `crontab -l`

```Output
30 1 * * * extract_data.sh
```



