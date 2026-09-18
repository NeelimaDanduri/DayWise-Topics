Linux File Permissions and User Management 

1. Groups

sudo groupadd Gryffindor

sudo groupadd Slytherin

sudo groupadd Ravenclaw

sudo groupadd Hufflepuff

sudo groupadd Professors

sudo groupadd DarkLords

2. Users and Group Memberships

sudo useradd -m -G Gryffindor harry

sudo useradd -m -G Gryffindor hermione

sudo useradd -m -G Gryffindor ron

sudo useradd -m -G Slytherin draco

sudo useradd -m -G Slytherin,Professors snape

sudo useradd -m -G Ravenclaw luna

sudo useradd -m -G Hufflepuff cedric

sudo useradd -m -G DarkLords voldemort

sudo useradd -m dumbledore

Verify:

groups harry

groups draco

groups snape

Expected:

harry: harry Gryffindor

draco: draco Slytherin

snape: snape Slytherin Professors

3. Create Hogwarts Directory

mkdir ~/hogwarts

cd ~/Hogwarts

Create files:

touch gryffindor_file.txt slytherin_file.txt ravenclaw_file.txt hufflepuff_file.txt restricted_file.txt

Add some content:

echo "Welcome to Gryffindor" > gryffindor_file.txt

echo "Welcome to Slytherin" > slytherin_file.txt

echo "Welcome to Ravenclaw" > ravenclaw_file.txt

echo "Welcome to Hufflepuff" > hufflepuff_file.txt

echo "Only Professors and Dumbledore" > restricted_file.txt

4. Assign Groups to Files

sudo chgrp Gryffindor gryffindor_file.txt

sudo chgrp Slytherin slytherin_file.txt

sudo chgrp Ravenclaw ravenclaw_file.txt

sudo chgrp Hufflepuff hufflepuff_file.txt

sudo chgrp Professors restricted_file.txt

5. Assign Permissions

chmod 660 gryffindor_file.txt

chmod 660 slytherin_file.txt

chmod 660 ravenclaw_file.txt

chmod 660 hufflepuff_file.txt

chmod 660 restricted_file.txt

660 = rw-rw----

Verify:
ls -l

Expected:

-rw-rw---- gryffindor_file.txt

-rw-rw---- slytherin_file.txt

-rw-rw---- ravenclaw_file.txt

-rw-rw---- hufflepuff_file.txt

-rw-rw---- restricted_file.txt

6. Allow Harry and Draco to Reach the Directory

The Hogwarts directory must be accessible:

chmod 755 /home/neelima_danduri

chmod 755 /home/neelima_danduri/Hogwarts

755 = rwxr-xr-x

Verify:

ls -ld /home/neelima_danduri

ls -ld /home/neelima_danduri/hogwarts

7. Test Harry

su - harry

cd /home/neelima_danduri/hogwarts

cat gryffindor_file.txt # success

cat slytherin_file.txt # permission denied

8. Test Draco
su - draco

cd /home/neelima_danduri/hogwarts

cat slytherin_file.txt # success

cat gryffindor_file.txt # permission denied

Observations

Users can access files that belong to their group.

Users cannot access files belonging to other groups.

File ownership and group membership determine access rights.

Directory permissions are important for users to traverse directories.

Linux file permissions provide secure access control.