
## User Admin Basic
A user is an account that can access the system


>[Important Basic commands]


```
sudo useradd john  - to add user

sudo passwd john - to set password for the user

sudo useradd -uid 1100 smith :- to manually set id for user 


cat /etc/passwd :- it shows the user name,id,groupid,home directory and preffered shell 

ls -l /home/ :- to display the usersl

ls -ln /home :- this shown in a numeric option

id - display id

whoami - display current user name

useradd --help :- it describes the command of the useradd

sudo userdel username

sudo userdel -r john :- delete the files of this user

```

to switch user : su username

>[To change directories ,Username,   LoginShell]



```
usermod means user modify
 
 
sudo usermod --home /home.otherdirectory --move-home john 
				or
sudo usermod -d /home.otherdirectory -m john

sudo usermod --login jane john == usermod -l jane john

sudo usermod --shell /bin/othershell ==
usermod -s /bin/othershell 


```


## To lock and unlock user account

![[Pasted image 20260312101743.png]]


## Password Expiration

>[ --lastday 0  or  -d 0 :- today is last day if next time user login he/she need to change the password then only they able to login ]
> [--lastday -1  or  -d -1 : - to unexpires the password]
> [--maxdays 30  or -M 30 :- to need to change for every 30 days]
> [--maxdays -1  or  -M -1:- password never expires for the user ]
> [--list jane  or - l :- to list the users whose password expired]


ex : sudo chage --lastday 0 jane 
which means if next the jane user login he need to changes his password or else he won't able to login


==chage==
It used to manage  the password 

![[Pasted image 20260312102845.png]]

**sudo userdel -r jane - to del user**
**sudo groupdel john - to del group**




## Local Groups and membership
![[Pasted image 20260312105343.png]]
1. john user is created ==usedadd john
2. developers group is created ==groupadd developers
3. then john is added to the developers group ==gpasswd -add  john developers
4. we can able to see who all are in the developers group using command called ==groups john==
5. then john is removed from the developers group using ==-delete or -d


![[Pasted image 20260312105857.png|697]]


```
main thing is :

for user john - his primary group is john then secondary group is developer
but if we want change means we can use the above command to change the primary group

sudo usermod -g or --gid developers john 

"the change is first group name then only user name comes"
but its quite opposite for adding the user to the group
```

![[Pasted image 20260312110347.png]]

```
change group name 

sudo groudmod -n newname oldname

we cannot delete the group of someone primary group 
if we want ,we need to change that user primary group

then for delete sudo groupdel group_name

   
   


l