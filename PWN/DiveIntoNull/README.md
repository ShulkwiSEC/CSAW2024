# Diveing into null challnge
## Catgory PWN:
## Remote Challnge
## Chllange Discrption: Oops, I rm -rf 'ed my binaries
```
bash
nc null.ctf.csaw.io 9191
```

leets dive into the server
connect to it and you will get shell (is there esay more the it 0_0)

but we dont know

````
nc null.ctf.csaw.io 9191
bash: cannot set terminal process group (1): Inappropriate ioctl for device
bash: no job control in this shell
groot@diving-into-null-6859fcf8d4-gl2bw:/$ id
id
bash: id: command not found
groot@diving-into-null-6859fcf8d4-gl2bw:/$ ls
ls
bash: ls: command not found
groot@diving-into-null-6859fcf8d4-gl2bw:/$ whomau
whomau
bash: whomau: command not found
groot@diving-into-null-6859fcf8d4-gl2bw:/$ ls -alh
ls -alh
bash: ls: command not found
groot@diving-into-null-6859fcf8d4-gl2bw:/$ echo "hi"
echo "hi"
hi
groot@diving-into-null-6859fcf8d4-gl2bw:/$ echo -ni ''
echo -ni ''
-ni
groot@diving-into-null-6859fcf8d4-gl2bw:/$ exit
exit
exit
````

as you see no commant if you read the challnge discrption it say

```` Oops, I rm -rf 'ed my binaries ````


so as you echo is workd so if you have little knowlege with about bash
you know you can use echo to print file and floders

lets try !

````
 for file in *;do echo "$file";done
for file in *;do echo "$file";done
bin
boot
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
`````
i think we do it we get the file now lets search for the flag file i forget to tell you they left `````cd````, ````pwd````, with ````echo````
for use so we are ready to exploit !!!!!!!!!!!! why i think we forget some one in home ???????
```` OhMycAT ```` we forget you we are soooo sorry i will come back to get you 

````
bash
echo "$(</etc/passwd)"
echo "$(</etc/passwd)"
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
groot:x:1000:1000::/home/groot:/home/bin/bash
````
but  we can upgrade our ls and cat to be better like for ls we need to let it able to give as the file perm and hidden files so lets do some functions

````
bash
ls() {
    # Check if a directory argument is provided
    if [ -z "$1" ]; then
        echo "Usage: ls <directory>"
        return 1
    fi

    # Store the directory argument in a variable
    local directory="$1"

    # Function to convert file mode to human-readable form
    get_permissions() {
        local file="$1"
        local perm=""

        [ -r "$file" ] && perm="${perm}r" || perm="${perm}-"
        [ -w "$file" ] && perm="${perm}w" || perm="${perm}-"
        [ -x "$file" ] && perm="${perm}x" || perm="${perm}-"
        echo "$perm"
    }

    # List all files, including hidden ones, and display their permissions
    for f in "$directory"/{.,}*; do
        if [ -e "$f" ]; then
            local perms=$(get_permissions "$f")
            echo "$perms $f"
        fi
    done
}

cat() {
    # Check if a file argument is provided
    if [ -z "$1" ]; then
        echo "Usage: cat <file>"
        return 1
    fi

    # Store the file argument in a variable
    local file="$1"

    # Check if the file exists and is a regular file
    if [ ! -f "$file" ]; then
        echo "Error: File '$file' does not exist or is not a regular file."
        return 1
    fi

    # Read file contents and store it in a variable
    local file_contents=$(< "$file")

    # Print the file contents
    echo "$file_contents"
}
````

now we are ready do it 
````
bash
groot@diving-into-null-6859fcf8d4-gl2bw:/$ ls
ls
Usage: ls <directory>
groot@diving-into-null-6859fcf8d4-gl2bw:/$ ls .
ls .
r-x ./boot
r-x ./dev
r-x ./etc
r-x ./home
r-x ./lib
r-x ./lib64
r-x ./media
r-x ./mnt
r-x ./opt
r-x ./proc
--- ./root
r-x ./run
r-x ./sbin
r-x ./srv
r-x ./sys
r-x ./tmp
r-x ./usr
r-x ./var
groot@diving-into-null-6859fcf8d4-gl2bw:/$ cd home
cd home
groot@diving-into-null-6859fcf8d4-gl2bw:/home$ ls
ls
Usage: ls <directory>
groot@diving-into-null-6859fcf8d4-gl2bw:/home$ ls .
ls .
r-x ./bin
r-x ./groot
groot@diving-into-null-6859fcf8d4-gl2bw:/home$ cd groot
cd groot
groot@diving-into-null-6859fcf8d4-gl2bw:~$ ls .
ls .
r-- ./.bash_history
r-- ./.bashrc
r-- ./.flag
r-- ./.profile
groot@diving-into-null-6859fcf8d4-gl2bw:~$
````

And Finaly
`````
bash
groot@diving-into-null-6859fcf8d4-t4jj8:~$ cat .flag
cat .flag
|||||||||||||||||||||||||||||||||||||||||||||||||||||
||| csawctf{penguins_are_just_birds_with_tuxedos} |||
|||||||||||||||||||||||||||||||||||||||||||||||||||||
groot@diving-into-null-6859fcf8d4-t4jj8:~$
`````
