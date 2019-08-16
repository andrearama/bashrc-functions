# Bashrc-functions
Ten useful functions to add to bashrc. Most of them are not complex at all, simply they are a useful (and understandable thus personalizable) shortcut for tasks which I happen to use quite often and would require either longer or multiple commands.

### Functions:
#### 1 Print the number of files in a certain directory (or in the current one if no arguments are given)
**Description:** Count the number of (visible) file in current or target directory

**Usage example:** 
```
numfiles Pictures
> 2 files in Pictures
```
**Shell function:**
```
function numfiles() { 
    N="$(ls $1 | wc -l)"; 
    if [[ "$1" == " " || -z "$1" ]]; then 
            echo "$N files in the current directory";
        else
	    echo "$N files in $1";
        fi
    }
```
#### 2 Create and change directory
**Description**: create a new directory with the given name and get into it

**Usage example:**
```
mkdcd newfolder
```

**Shell function:**
```
function mkdcd () {
     mkdir "$1" && cd "$1"
     }
```

#### 3 Autosave 
**Description**: Save the current directory or a specified one in a predefined separated place. I find it useful when I want to try to change or tweak something in a project without having to use version control tools

**Usage example:**
```
autosave Pictures
> Starting autosave..
> Autosave completed successfully
```
**Shell function:**
```
function autosave() { 
    #Save directory path
    SAVE_DIR="/tmp/autosave/"

    #Create save directory (if already not present)
    if [ ! -d "$SAVE_DIR" ]; then
      mkdir -p "$SAVE_DIR"
    fi

    echo "Starting autosave..";
    #If no directory is given copy all elements from the current one
    if [[ "$1" == " " || -z "$1" ]]; then 
            cp -r * "$SAVE_DIR"
        else
	    cp -r "$1" "$SAVE_DIR";
        fi
    echo "Autosave completed successfully";
    } 
```

#### 4 Add suffix to file/folder
**Description**: Usually a program I run produces file/folders as result. Running it multiple time would overwrite the existing results, which sometimes I want to keep. Adding a suffix (in this case "_old") solves the problem quite conveniently without further bothers or confusions.

**Usage example:** 
```
ls
> Pictures
old Pictures
ls
> Pictures_old
```

**Shell function:**
```
function old () {
    # If it is a folder:
    if [[ -d $1 ]]; then
        var=$1
        #if the name ends with "/":
        if [[ "${var: -1}" == "/" ]]; then
            mv "$1" "${var::-1}_old"
        else
            mv "$1" "$1_old"
        fi
    #Otherwise if it is a file:
    elif [[ -f $1 ]]; then
        #Extract filename and extension:
        filename=$(basename -- "$1")
        extension="${filename##*.}"
        filename="${filename%.*}"
        mv "$1" "$filename""_old.""$extension"
    else
        echo "$1 is not valid"
    fi
    }
```






Work in progress...
