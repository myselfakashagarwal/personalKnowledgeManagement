## Preface
A shell script is just a file with commands (GNU). This script is executable in  nature as its contents are executables. 
Just like commands when the script is passed to the shell, a new non interactive chile shell session spawns in which the script commands are executed. 
## Syntax / Tokens 
### Variables 
Shell scripting not do have "only declaration" neither types, Types are defined by the value except  when using declare command to declare a variable explicatively. A variable is  just an identifier holding a value extracted using $ prefix 
#### `Variable expansion and use` 
Variable can be can used with simple `$` before variable name but this have some issues this include variable and message cannot be used together ex. `$VARIABLEmessege`  to separate this curly braces are used to provide distinction ex. `${VARIABLE}messege`, by default the values in the variable get split if they are separated by a delimiter such as tab when passed; to prevent this behaviour variable can be quoted `"${VARIABLE}"messege`. 
#### `All in one`
```bash
VARIABLE="DESIRED-VALUE"             # syntax
PLANET="Uranus"                      # example
echo $PLANET                         # use
```
#### `Strings`
```bash
declare VARIABLE="DESIRED-VALUE"     # syntax
declare GALAXY="Milkyway Galaxy"     # example
echo $GALAXY                         # use
```
#### `Integers`
```bash
declare -i VARIABLE="DESIRED-VALUE"  # syntax 
declare -i GRAVITY="9"               # example
echo $GRAVITY                        # use
```
#### `Boolean`
```bash
# booleans are just comprised of true or fasle binaries not true or false strings 
IS_HABITABLE=false                  # example of false 
IS_BEAUTIFUL=true                   # example of true
echo $HABITABLE                     # use
```
#### `Array`
```bash
declare -a VARIABLE_NAME=("ZEROth Index" , "ONE", "TWO" )   # syntax
declare -a PLANETS=("EARTH", "URANUS")                      # example

# Syntax                             #  Operations 
${ARRAY-NAME[*]}                     # All elements
${#ARRAY-NAME[@]}                    # size of the array
${ARRAY-NAME[INDEX]}                 # indexed operation

# Example                            # Task
echo ${PLANETS[*]}                   # Prints all elements of the array
echo ${#PLANETS[@]}                  # Prints size of the array
echo ${PLANETS[0]}                   # Prints elements at 0th index 
```
#### Shell Variables 
| Variable         | Description                       |
| ---------------- | --------------------------------- |
| `$$`             | Process ID of current shell       |
| `$PPID`          | Parent process ID                 |
| `$!`             | Process ID of last background job |
| `$-`             | Current shell options (flags)     |
| `$0`             | Name of shell or script           |
| `$BASHPID`       | Process ID in subshells           |
| `$BASH_SUBSHELL` | Subshell nesting level            |
| `$SHLVL`         | Shell level (how deeply nested)   |
| `$RANDOM`        | Random integer (0-32767)          |
| `$SECONDS`       | Seconds since shell started       |
#### Last Command information
| Variable | Description                       |
| -------- | --------------------------------- |
| `$?`     | Exit status of last command       |
| `$_`     | Last argument of previous command |
| `$!`     | PID of last background process    |
#### Environment and System Variables
| Variable    | Description                |
| ----------- | -------------------------- |
| `$USER`     | Current username           |
| `$HOME`     | User's home directory      |
| `$PWD`      | Current working directory  |
| `$OLDPWD`   | Previous working directory |
| `$PATH`     | Executable search path     |
| `$SHELL`    | User's login shell         |
| `$TERM`     | Terminal type              |
| `$HOSTNAME` | System hostname            |
| `$HOSTTYPE` | Machine type               |
| `$OSTYPE`   | Operating system type      |
| `$IFS`      | Internal Field Separator   |
### Operator and operations for expressions
The shell expression can be formed with variable commands and operators, they are evaluated individually, however they can be evaluated using `eval` for executing stringified expression. or subshell to process expression prior with returned value at the runtime. $(expression), or just normal expression with operators using $(( expression ))
#### `Comparison (numeric)`
| Operator | Description              |
| -------- | ------------------------ |
| -eq      | Equal to                 |
| -ne      | Not equal to             |
| -gt      | Greater than             |
| -ge      | Greater than or equal to |
| -lt      | Less than                |
| -le      | Less than or equal to    |
#### `String Comparison Operators`
| Operator | Description                     |
| -------- | ------------------------------- |
| <        | Less than (strings)             |
| <=       | Less than or equal (strings)    |
| >        | Greater than (strings)          |
| >=       | Greater than or equal (strings) |
| ==       | Equal to                        |
| !=       | Not equal to                    |
#### `Assignment Operators`
| Operator | Description         |
| -------- | ------------------- |
| =        | Simple assignment   |
| +=       | Add and assign      |
| -=       | Subtract and assign |
| *=       | Multiply and assign |
| /=       | Divide and assign   |
| %=       | Modulus and assign  |
#### `Arithmetic Operators`
| Operator | Description         |
| -------- | ------------------- |
| +        | Addition            |
| -        | Subtraction         |
| *        | Multiplication      |
| /        | Division            |
| %        | Modulus (remainder) |
#### `Logical Operators`
| Operator | Description |
| -------- | ----------- |
| !        | Negation    |
| &&       | AND         |
| \|       | OR          |
### Flow Control

#### Conditional Statements 
```bash
# Executes block of codee if the condition satisfies 

if [[ condition ]]
then
   commands
fi
```

```bash
# If the condition satisfies the if block will be executed otherwise the commands present in the else block will be executed

if [[ condition ]]
then
   commands
else
   commands
fi
```

```bash
# The respective blocks are executed on the basics of the condition they satisfies but if now condition satisfies the else block will be executed

if [[ condition ]]
then
   commands
elif [[ commands ]]
then
   commands
else
   commands
fi

```

```bash
# In case the constants are evaluated, if the constant matches the choice provided that respective block is executed. If any of the condition fails to satisfy the defult case denoted by * is executed.

case "${choice}" in
    constant1)
        commands
        ;;
    constant2)
        commands
        ;;
     *)
        commands
        ;;
esac
```

```bash
# The select is quite the iterative one yet its conditional on user side. Init out of multiple options one is selected by user according to which the respective block executes, User can select with nth option number 

select option in "Option 1" "Option 2" "Option 3" "Quit"
do
    if [[ "$option" == "Option 1" ]]; then
        echo "You chose Option 1"
    elif [[ "$option" == "Option 2" ]]; then
        echo "You chose Option 2"
    elif [[ "$option" == "Option 3" ]]; then
        echo "You chose Option 3"
    elif [[ "$option" == "Quit" ]]; then
        echo "Quitting..."
        break
    else
        echo "Invalid selection"
    fi
done
```
#### Iterative statements 
```bash
# The loop will run will the range of input until condition fails
for element in ${elements}
do
  commands
done
```

```bash
# The loop will run till the control variable fails to satisfy the condition
for((i=0;i<5;i++))
do
  commands
done
```

```bash
# The loop will run continiously if condition satisfies
while [[ condition ]]
do
  commands
done
```

```bash
# The loop will run until the condition satisfies i.e it will run if the condition fails 
until [[ condition ]]
do
  commands
done
```
#### Jump Statements 
```bash

# Break can be used to move the control out of the block is currently in
#!/bin/bash
for i in {1..5}
do
    echo $i
    if [[ $i == 3 ]]
    then
        break
    fi
done
```

```bash
# continue can be used to skip a set of iterations 
#!/bin/bash
for i in {1..5}
do
    if [[ $i == 3 ]]
    then
        continue
    fi
    echo $i
done
```
#### Functions 
```bash
# Functions in shell scripting can be called with arguments
function_name() {
    commands
}

# Call the function
function_name argument1 argument2
```

## Script invocation & command line 
### Positional Parameters
When a shell script is invoked data (parameters) can be passed onto it (String) with no limit. from 1 to 9 they can be addressed as variables after 10 >= use brackets ${10}. They can be 
#### Positional Argument Aggregators
| Variable | Description                         | Behavior                        |
| -------- | ----------------------------------- | ------------------------------- |
| `$#`     | Number of positional parameters     | `echo "Argument count: $#"`     |
| `$*`     | All arguments as single string      | Joins with IFS (default: space) |
| `$@`     | All arguments as separate strings   | Preserves individual arguments  |
| `"$*"`   | All args joined with first IFS char | Single string: `"$1 $2 $3"`     |
| `"$@"`   | Each arg as separate quoted strings | `"$1" "$2" "$3"`                |
