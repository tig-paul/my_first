# Challenge #1
# Over the Wire Bandit
## <mark>__Level 0__ 

- First of all, we have to login into bandit0 using SSH. The hostname is `bandit.labs.overthewire.org` on port 2220. The password of this level is bandit0.  
So, I used the command `ssh -p 2220 bandit0@bandit.labs.overthewire.org` and the password bandit0 to login.

## <mark>__Level 0 -> Level 1__
- Now in the home directory I found a file named readme.

- `cat readme` gave the password for the next level.

## <mark>__Level 1 -> Level 2__
- Now by using `ssh -p 2220 bandit1@bandit.labs.overthewire.org` and the password from the previous level, logged into bandit1.

- In the home directory I got the file named `-`.But simply running `cat -` didn't work.
- After going through the link [Google Search for “dashed filename”](https://www.google.com/search?q=dashed+filename), I used the command `cat ./-` to get the password.

## <mark>__Level 2 -> Level 3__
- Logged into bandit2.
 
- Easily got the password from the file `spaces in this filename` in the home directory.

## <mark>__Level 3 -> Level 4__
- Logged into bandit3. 

- In the home directory there was another `inhere` directory inside which I found the `.hidden` file using `ls -al` and the password was inside it.

## <mark>__Level 4 -> Level 5__
- Logged into bandit4.

- In the home directory there is an `inhere` directory, inside which there are 10 files among which only one is human-readable. 

- We could check one by one but instead I used the command `file /home/bandit4/inhere/*`. It showed that only the file `-file07` had ASCII text. 

- Using `cat` I got the password.

## <mark>__Level 5 -> Level 6__
- Logged into bandit5.

- In the inhere directory there are multiple `maybehere` directories which also contain multiple files among which only one file has the properties:
  - human-readable
  - 1033 bytes in size
  - not executable

  That file contains the password. 
- Now we can use the `find` command to find the files which has size 1033 bytes i.e. `find -size 1033c`. In this case only one file has size 1033 bytes which is located in `./maybehere07/.file2` that gives us the required password.

## <mark>__Level 6 -> Level 7__
- Logged into bandit6.

- Now the password was stored somewhere in the server and was:
  - owned by user bandit7
  - owned by group bandit6
  - 33 bytes in size

- First I didn't know how to use user and group credentials but after a while searching through chatGPT, I got it. 
- After running the command `find / -user bandit7 -group bandit6 -size 33c`  we get a list of multiple files amongst which most have permission denied. After scrolling a little bit we find a file `bandit7.password`.
- Pasting the full path after cat gives us the password.

## <mark>__Level 7 -> Level 8__
- Logged into bandit7.

- In the home directory there is a file `data.txt`. Simply using the command `grep "millionth" data.txt` gives us the password.

## <mark>__Level 8 -> Level 9__
- Logged into bandit8.

- The password is in the file `data.txt` and is the only line that occurs once.
- Firstly I had no idea how to solve it. After searching in google & chatGPT, I got that firstly we have to sort the entire file and then use the `uniq -u` command to find the unique line.
 
  Then by simply running the command `sort data.txt | uniq -u` gave the password.

## <mark>__Level 9 -> Level 10__
- Logged into bandit9.

- Now the password stored in the file  `data.txt` in one of the few human-readable strings, preceded by several '=' characters. That means we have to use the strings and grep commands.
- If we run `strings data.txt | grep "===="`, we get the password.

## <mark>__Level 10 -> Level 11__
- Logged into bandit10.
  
- Now the `data.txt` file contains base64 encoded data which after decoding gives the password.

## <mark>__Level 11 -> Level 12__
- Logged into bandit11.

- The `data.txt` file has ROT13 encoded data which after decoding gives the password.

## <mark>__Level 12 -> Level 13__
- Logged into bandit12.

- The file `data.txt` is a hexdump of a file that has been repeatedly compressed.
- `hexedit` wasn't working. Hence as told, I went to the /tmp directory by `cd ../../tmp`. There I made a directory `help` and inside it copied the `data.txt` file by `cp /home/bandit12/data.txt /tmp/help/`. 
- Now we have to reverse the hexdump data. Hence I used `xxd -r data.txt > pass`. Now the file `pass` contains the reversed data of `data.txt`.
- `file pass` showed that it was gzip compressed. So, I used `mv pass pass.gz | gunzip pass.gz` to extract it.
- Now it was bzip2 compressed. So, I did `mv pass pass.bz2 | bunzip2 pass.bz2`.
- It was again gzip compressed. So, again I used `mv pass pass.gz | gunzip pass.gz`.
- Now, it was `POSIX tar archive` data. Using `man tar`, I got how to extract data. Next, `tar --extract -f pass`.
- A new file `data5.bin` of type `POSIX tar archive` was created. Again ->`tar --extract -f data5.bin`. Another new file `data6.bin` of type bzip2 compressed data was created. Then `bunzip2 data6.bin` created a new file `data6.bin.out`. Again `tar --extract -f data6.bin.out`, created another gzip compressed file `data8.bin`.
- Now finally `mv data8.bin data.gz | gunzip data.gz` gave a file named `data` that contained the password.

## <mark>__Level 13 -> Level 14__
- In the home directory there is a file `sshkey.private` that contains the private SSH key using which we can login into bandit14 using localhost.
- Easily `ssh -i sshkey.private bandit14@localhost -p 2220` helped me to login into bandit14.

## <mark>__Level 14 -> Level 15__
- As told in the previous level `cat /etc/bandit_pass/bandit14` showed the password of the current level.
- Simply running `nc localhost 30000` and submitting the current password gives the password for the next level.

## <mark>__Level 15 -> Level 16__
- Again we have to use the current password to get the next password but now using openssl encryption.
- After running many commands which didn't work, finally found the command `ncat --ssl localhost 30001` which after submitting the current password gave the next password.

## <mark>__Level 16 -> Level 17__
- Now we have multiple servers among which we have to find the correct one.
- First we can find open servers. For this I used `nmap localhost -p 31000-32000`.
- Then one of the open servers gave the private ssh key after running `ncat --ssl localhost 31790`.
- Now we can save the private key using vim or nano. Giving the command `vim private_key` opens vim and we paste the content of the private key and hit `:qa`. Now `chmod 400 private_key` gives the appropriate permission. 
- And finally we can use `ssh -i private_key bandit17@bandit.labs.overthewire.org -p 2220` and we are logged in.

## <mark>__Level 17 -> Level 18__
- We can use `diff passwords.new passwords.old`. It gives two passwords and we can try one by one to find the correct one.

## <mark>__Level 18 -> Level 19__
- When I try to login using ssh, I immediately get logged out as someone has modified `.bashrc`. The password is stored in the `readme` file in the home directory. So what I have to do is to somehow read the file before I gets logged out.
- The first hint I got from the man page of ssh. There I found that after ssh command we can run another command immediately after that. This could help us to somehow read the file before logging out.
- Hence I used `ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme` and it gave the password immediately.

## <mark>__Level 19 -> Level 20__
- It was totally new stuff. I read the man page but could not understand actually what command to run.
- Then after running `./bandit20-do` got some hint about the format.
- Then finally the command `./bandit20-do cat /etc/bandit_pass/bandit20` gave the password.

## <mark>__Level 20 -> Level 21__
- Just learnt how to listen using netcat and connect between different servers.
- First we split the terminal and login into bandit20 in both of them. In one we run `nc -lvp 12345` and it starts listening. When we try to connect to this from the other device using `./suconnect 12345`, the former one shows connection received. Then we paste the previous password in the first server. The second one reads it and as it matches, sends the password for the next level.

## <mark>__Level 21 -> Level 22__
- First, `cd  /etc/cron.d/` takes us to the required directory.
- Now we are searching for the password for the level 22 so `cat cronjob_bandit22` could be beneficial.
- It gives the location of the `shell-script` for bandit23.
- Now `cat /usr/bin/cronjob_bandit22.sh` shows the shell-script of user bandit22.
- Here the last line `cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` implies that the password of bandit22 is redirected to a file in the tmp directory.
- Simply we can use `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` that will give us the password.

## <mark>__Level 22 -> Level 23__
- Again we run `cd /etc/cron.d` and this time it is `cat cronjob_bandit23`. Now `cat /usr/bin/cronjob_bandit23.sh` shows the shell-script of user bandit23.
- It is written in bash. Here `whoami` refers to bandit23, as this is bandit23's shell-script.
   
  Hence, `bandit23` is stored inside the variable `myname`.

- Now as we know that `$myname = bandit23`, we can execute the code inside `mytarget` and get the output file.
- Now we can read the file using cat.

## <mark>__Level 23 -> Level 24__
- `cd /etc/cron.d` -> `cat cronjob_bandit24` -> `cat /usr/bin/cronjob_bandit24.sh` showed the shell-script for bandit24.

  ### Understnding the shell-script
  - Here `myname` refers to bandit24.
  - The next things were done in the directory `/var/spool/bandit24/foo`.
  - Now starts the for loop which iterates through all the files in that directory. If there is any file in that directory owned by bandit23 then its going to execute it and then remove it.
- Now, we want the password for bandit24 which is stored inside `/etc/bandit_pass/bandit24`. But we can't access it. So, we have to somehow use that shell-script to get the password. 
- So, as directed in bandit, we have to create our own script in some manner that it fetches the password as done in level 21.
- As done in previous levels we can create the script in the /tmp directory. So, we use `mkdir /tmp/script` -> `cd /tmp/script`.
- Now for creating the script we can use `nano script.sh`.
- It opens the text editor and we start writing:
   
  ```#!/bin/bash
  cat /etc/bandit_pass/bandit24 > /tmp/script/passwd
  ```
  Same as in the level 21, if the file `script.sh` gets executed in `/var/spool/bandit24/foo`, then it will read the password and redirect it to `/tmp/script/passwd` and we could access it.
- Now we have to do three things:

  1. Make `script.sh` executable. For that we run `chmod +x script.sh`.
  2. Also bandit24 should have the right permissions to redirect the password into `/tmp/script/passwd`. This can be done by `chmod 777 .`. It gives anyone the permissions to read, write and execute in this directory.
  3. Now we can mv `script.sh` to `/var/spool/bandit24/foo`. But there is a problem. After this file gets executed, it will be removed immediately. And if due to any reason we don't get the password, we have to again create it.
    
      So, instead of moving we can copy it. Hence we run `cp script.sh /var/spool/bandit24/foo`.

- After a few moments later, when the file gets executed in `/var/spool/bandit24/foo`, we get the `passwd` file.
    
## <mark>__Level 24 -> Level 25__
- In this level we have to brute force all 10000 combinations for the pincode. First I tried to make a script using for loop for the pincode and run it by piping after `nc localhost 30002` but it didn't work after 10-12 wrong trials.
- After searching a while found this method of creating a list and piping it with the nc command.
- First as usual created a directory using `mkdir /tmp/script` -> cd `/tmp/script`.
- Then using `nano scritp.sh` made this shell-script:
  
  ```#!/bin/bash
  for i in {0..9}{0..9}{0..9}{0..9}
  do
        echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $i" >> list.txt;
  done

  ```
  Then made it executable by `chmod +x script.sh`.
- Then executed it using `./script.sh` which created a new file `list.txt`. Now this list.txt contained all the possible combinations of the password and the pincode.
- Finally using `cat list.txt | nc localhost 30002` cracked the password.

## <mark>__Level 25 -> Level 26__
- Tried to login to bandit26 using its private key but it kicked me out to bandit25.
- Bandit says that the shell for bandit26 is not /bin/bash but something else. So, I checked the `/etc/passwd` file that showed that actually it was `/usr/bin/showtext`. Now `cat /usr/bin/showtext` showed the following script(in /bin/sh):

  ```#!/bin/sh
  export TERM=linux
  exec more ~/text.txt
  exit 0
  ```
  Now as `/usr/bin/showtext` is the shell for bandit26, when we try to login into bandit26 the above script gets executed and the last line `exit 0` lead us to log out. So, somehow we have to modify the content of the shell or change the shell itself because in general the shells are `/bin/bash` except this one.
  
  Now the thing to bother in the script is the `more` command. And also before login out after the line `Enjoy your stay!` we see a big `bandit26` which didn't seem in the previous levels. So, the `more` command might have a relation with that.
- Now after a long I found a crazy stuff. If I make my terminal too tiny such that the `bandit26` isn't shown entirely then I it's not logging me out. And `man more` showed many options that could be used while we were stuck in the `bandit26` like `!command`,`v`,`e` etc among which `v` opened a text editor, cool!
- Searching about `vi` shells revealed many things like `:set shell?` showed the current shell for bandit26.
- Now searching for a command that could change the shell revealed more. The command `:set shell=new_shell` can be used to change the shell.
- In the editor `:set shell?` showed `/usr/bin/showtext` that was causing us to logout. So, we have to change the shell to the normal shell i.e. `/bin/bash`. So, using `:set shell=/bin/bash` did the job. Now `:shell` opened the bash shell for bandit26.

## <mark>__Level 26 -> Level 27__
- `ls` showed that there was a setuid file `bandit27-do`.
- As in level 19, simply running `./bandit27-do cat /etc/bandit_pass/bandit27` gave the password for next level.

## <mark>__Level 27 -> Level 28__
- I created a directory-`mkdir /tmp/gitt` to clone the repository as the home directory didn't have the permission to do that. Then `cd /tmp/gitt`.
- Now `git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo` created a `repo` directory. By the way, the password for bandit27-git was same as for bandit27.
- In `repo` there was a  `README` file that contained the password.

## <mark>__Level 28 -> Level 29__
- First steps are same. `mkdir /tmp/clone` -> `cd /tmp/clone` -> `git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo` -> `cd repo`.
- Now `cat README.md` showed the follwing note:
  
  ```
  # Bandit Notes
  Some notes for level29 of bandit.

  ## credentials

  - username: bandit29
  - password: xxxxxxxxxx
  ```
  It seemed that the password has been erased.

- Now there is a command `git log` that shows the commit history. It showed that there were 3 commits and each one had a hash alongwith it.
- The command `git checkout` shows the changes made during any commit and takes the hash data as an argument.
- After running `git checkout hash1`, the notes in the README.md had changed. `cat README.md` showed:
  ```
  # Bandit Notes
  Some notes for level29 of bandit.

  ## credentials

  - username: bandit29
  - password: <TBD>
  ```
  The password had been changed.
- In the second commit there was something written like `add missing data` that seemed interesting. So, `git checkout hash2` -> `cat README.md` did this:
  ```
  # Bandit Notes
  Some notes for level29 of bandit.

  ## credentials

  - username: bandit29
  - password: tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S
  ```
- And we got the password.

## <mark>__Level 29 -> Level 30__
- Again we can clone and get `repo`. Inside repo there is a `README.md` file that contains:
  
  ```
  # Bandit Notes
  Some notes for bandit30 of bandit.

  ## credentials

  - username: bandit30
  - password: <no passwords in production!>
  ```
- Now this one has two commits. But now `git checkout hash` doesn't gives the password. So, there is something new.
- There are different versions of a repo. We can see them by simply running `git branch` and it shows our present version to be `HEAD`. There can be multiple versions of a repo which we can seen by `git branch -a`. In this case there are 4 versions `HEAD, dev, master, sploits-dev`. master is the most up-to-date one. So, we can change version from HEAD to master by `git checkout master` and we are now in master. But `cat README.md` gives nothing important.
- Again we can change branch to `dev` and this time `cat README.md` shows the password.

## <mark>__Level 30 -> Level 31__
- Again we proceed similarly. Creating /tmp/clone and git clone to get the repo. Inside the repo there is a `README.md` file that contained a text of no work.
- Checking the commits and nevigating over different versions didn't work.
- For this level we have to check the tags which is another feature of git. We can do `git tag` and it shows a tag named `secret`. Using `git show secret` shows the password.

## <mark>__Level 31 -> Level 32__
- After cloning we get the repo. And the `README.md` now shows:
  
  ```
  This time your task is to push a file to the remote repository.

  Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
  ```
  So, at first we do `git branch` and it shows that our branch is `HEAD` now. So we run `git checkout master`. Then by `nano key.txt`, the text editor opens and we write the content.
- Now for pushing key.txt to the remote repository involves numerous steps as explained below:
  1. First we have to add the file into git. So we run `git add key.txt` but it shows that the `.gitignore` file prevents us to do that, so we remove it by `rm .gitignore`.
  2. Now `git add key.txt` gets operated. We can also check our current status by `git status`.
  3. Next we have to commit our changes. So we run the command `git commit -m "ok"`.
  4. Now we have to push key.txt. So, we simply run `git push` and after giving the password for bandit31-git we get the password for next level.

## <mark>__Level 32 -> Level 33__
- Once logged in, it took me into an uppercase shell. Whatever command I gave, it just converted  it into uppercase and then run it. So, I wasn't able to do anything inside it. I also gave numeric digits and special characters as input but all in vain.
- When I tried to run `$0`, I was out of the uppercase shell and now I could run normal commands.
- Now `ls` showed that there is a `uppercase` file in the home directory. `file uppercase` showed that it was of setuid type. So, as done in precious levels, I tried `./uppershell cat /etc/bandit_pass/bandit33` but unfortunately it again took me in the uppercase shell.
- After coming out I tried to run `cat /etc/bandit_pass/bandit32`, but to be very weird it showed `permission denied`. As I have logged in as bandit32, why should I not be able to read the file `/etc/bandit_pass/bandit32`. 
- Then I tried `whoami` and shockingly it returned `bandit33`. So, actually we were bandit33 thats why we could not access `/etc/bandit_pass/bandit32`. And when I did `cat /etc/bandit_pass/bandit33` it gave me the password.

## <mark>__Level 33 -> Level 34__
- Logged in into bandit33. `cat README.txt` showed that we have completed the last level : )

- [x] ALL DONE !!

---
---
---

# Challenge #2

## 1. <mark>Russian Roulette
  - [x] PYTHON
  ```py
  import random

    def russian_roulette():
        bullet_position = random.randint(1, 6)
        trigger_position = random.randint(1, 6)

        print(f"Bullet is in the chamber {bullet_position}.")
        print(f"Trigger is in the chamber {trigger_position}.")

        if bullet_position == trigger_position:
            print("Oops! You lose.")
        else:
            print("Great! You are safe for now.")


    print("Welcome to Russian Roulette!")
    print("Spinning the cylinder...")
    russian_roulette()

  ```
- [x] C
```c

#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void russian_roulette() {
    srand(time(NULL));

    int bullet_position = rand() % 6 + 1;
    int trigger_position = rand() % 6 + 1;

    printf("Bullet is in chamber %d.\n", bullet_position);
    printf("Trigger is at chamber %d.\n", trigger_position);

    if (bullet_position == trigger_position) {
        printf("Oops! You lose.\n");
    } else {
        printf("Great! You are safe for now.\n");
    }
}

int main(){
    printf("Welcome to Russian Roulette!\n");
    printf("Spinning the cylinder...\n");
    russian_roulette();
    return 0;
}

```

## 2. <mark>Sorting
- [x] PYTHON
```py
def bubble_sort(arr):
    n = len(arr)

    for i in range(n-1):

        for j in range(n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]


arr = [64, 23, 25, 12, 22, 11, 97]
print("Original array:", arr)
bubble_sort(arr)
print("Sorted array:", arr)
```
Output for the previous example will be:
```
Original array: [64, 23, 25, 12, 22, 11, 97]
Sorted array: [11, 12, 22, 23, 25, 64, 97]
```

- [x] C
```c
#include<stdio.h>

void sort(int a[],int n){
    for(int i=1;i<n;i++){
        int count=0;
        for(int j=0;j<n-i;j++){
            if(a[j]>a[j+1]){
                int temp = a[j];
                a[j] = a[j+1];
                a[j+1] = temp;
            }
            else count++;
        }
        if(count==n-i) return;//Optimization
    }
    return;
}
int main(){
    int array[] = {23,45,25,11,9,56};
    int n = sizeof(array)/sizeof(array[0]);
    printf("Original array: ");
    for(int i=0;i<n;i++) printf("%d,",array[i]);
    sort(array,n);
    printf("\nSorted array: ");
    for(int i=0;i<n;i++) printf("%d,",array[i]);
    return 0;
}
```
Output:
```
Original array: 23,45,25,11,9,56
Sorted array: 9,11,23,25,45,56
```

## 3. <mark>List Program
  - ### A. Single user.
    - [x] PYTHON
```py
import os

FILE = "user_list.txt"

def load_list():
    """Load the list from the file."""
    if os.path.exists(FILE):
        with open(FILE, 'r') as file:
            return [line.strip() for line in file.readlines()]
    return []


def display_list(user_list):
    """Display the list to the user."""
    if not user_list:
        print("The list is empty.")
    else:
        print("Your list:")
        for index, item in enumerate(user_list, start=1):
            print(f"{index}. {item}")

def add_item(user_list):
    """Add an item to the list."""
    item = input("Enter the item to add: ")
    user_list.append(item)
    print(f"'{item}' has been added to your list.")

def save_list(user_list):
    """Save the list to the file."""
    with open(FILE, 'w') as file:
        for item in user_list:
            file.write(f"{item}\n")

def main():
    user_list = load_list()
    
    while True:
        print("\nList Menu:")
        print("1. View list")
        print("2. Add item")
        print("3. Save list")
        print("4. Exit")
        
        choice = input("Choose an option (1-4): ")
        
        if choice == '1':
            display_list(user_list)
        elif choice == '2':
            add_item(user_list)
        elif choice == '3':
            save_list(user_list)
            print("List saved.")
        elif choice == '4':
            save_list(user_list)
            print("List saved. Exiting the program.")
            break
        else:
            print("Please enter a valid option.")
```


  - [x] C 
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_ITEMS 100
#define MAX_ITEM_LENGTH 100
#define FILENAME "user_list.txt"

void load_list(char list[MAX_ITEMS][MAX_ITEM_LENGTH], int *count) {
    FILE *file = fopen(FILENAME, "r");
    if (file == NULL) {
        *count = 0;
        return;
    }
    *count = 0;
    while (fgets(list[*count], MAX_ITEM_LENGTH, file) != NULL) {
        list[*count][strcspn(list[*count], "\n")] = '\0'; // Remove newline
        (*count)++;
    }
    fclose(file);
}

void display_list(char list[MAX_ITEMS][MAX_ITEM_LENGTH], int count) {
    if (count == 0) {
        printf("The list is empty.\n");
    } else {
        printf("Your list:\n");
        for (int i = 0; i < count; i++) {
            printf("%d. %s\n", i + 1, list[i]);
        }
    }
}

void add_item(char list[MAX_ITEMS][MAX_ITEM_LENGTH], int *count) {
    if (*count >= MAX_ITEMS) {
        printf("The list is full.\n");
        return;
    }
    printf("Enter the item to add: ");
    fgets(list[*count], MAX_ITEM_LENGTH, stdin);
    list[*count][strcspn(list[*count], "\n")] = '\0'; // Remove newline
    (*count)++;
    printf("'%s' has been added to your list.\n", list[*count - 1]);
}

void save_list(char list[MAX_ITEMS][MAX_ITEM_LENGTH], int count) {
    FILE *file = fopen(FILENAME, "w");
    if (file == NULL) {
        perror("Failed to open file");
        return;
    }
    for (int i = 0; i < count; i++) {
        fprintf(file, "%s\n", list[i]);
    }
    fclose(file);
}

int main() {
    char list[MAX_ITEMS][MAX_ITEM_LENGTH];
    int count;

    load_list(list, &count);

    while (1) {
        printf("\nList Menu:\n");
        printf("1. View list\n");
        printf("2. Add item\n");
        printf("3. Save list\n");
        printf("4. Exit\n");

        printf("Choose an option (1-4): ");
        char choice;
        scanf(" %c", &choice);
        getchar(); // Consume newline left by scanf

        if (choice == '1') {
            display_list(list, count);
        } else if (choice == '2') {
            add_item(list, &count);
        } else if (choice == '3') {
            save_list(list, count);
            printf("List saved.\n");
        } else if (choice == '4') {
            save_list(list, count);
            printf("List saved. Exiting the program.\n");
            break;
        } else {
            printf("Please enter a valid option.\n");
        }
    }
    return 0;
} 
```  

*******
*******

### I am still trying the questions 2.3)B and 2.4 and will try to submit them next time.