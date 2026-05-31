# 1. Print your username, hostname, and current directory
#    all in one go using a single line
echo "User: $(whoami) | Machine: $(hostname) | Location: $(pwd)"

# 2. Create this exact folder structure in one command
#    day01/
#    ├── notes/
#    ├── scripts/
#    └── practice/
mkdir -p day01/{notes,scripts,practice}

# 3. Create a file called myinfo.txt inside day01/notes/
#    and write your name and today's date into it — without
#    opening any editor
echo "Name: Anand" > day01/notes/myinfo.txt
echo "Date: $(date)" >> day01/notes/myinfo.txt

# 4. Prove the file has content
cat day01/notes/myinfo.txt

# 5. Copy myinfo.txt into the practice folder
#    then rename it to backup.txt
cp day01/notes/myinfo.txt day01/practice/backup.txt

# 6. List all files in day01/ including subfolders
#    in one command
ls -R day01

# 7. Find out how many lines are in backup.txt
 cat day01/notes/myinfo.txt |wc  

# 8. Add a third line to backup.txt that says
#    "This is my backup file"
#    without overwriting what's already there

echo "This is my backup" >> day01/practice/backup.txt 

Key point:

> = overwrite file
>> = append to file (keep existing content) 


# 9. Create a file called secret.txt in day01/
touch day01/secret.txt

# 10. Make it so ONLY you can read and write it
#     nobody else can do anything with it
chmod 600 day01/secret.txt

# 11. Check the permission string looks right
ls -l day01/secret.txt
# should show: -rw-------

# 12. Now create a file called script.sh
#     and give it execute permission for everyone
touch day01/script.sh
# your command here...

# 13. Check it
ls -l day01/script.sh
# should show: -rwxr-xr-x