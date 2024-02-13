# OTW-Bandit-Game
This report is to document the thought process and completion of all 10 Bandit levels. 

## Bandit 0
The most frequent and necessary terminal function to use was SSH, which is used to contact the Over-the-wire servers directly through the terminal.

The goal of Level 0 is to use ssh to log into the servers with the givern username and password, both being bandit0, on the 2220 port.

Using the given linux manual for ssh, I found the -p modifier to access the port 2220, and using the username infront of the address, like so: ssh bandit0@bandit.labs.overthewire.org -p 2220

This prompts the servers to ask for the password, in which I type bandit0 and given access to the first level.

## Bandit 0-1

Now that the real games begin, the next task is to open the file in Bandit0 labled "readme"

The site gives multiple different commands: ls, cd, cat, file, du, and find.

After looking up each manual given, I make a small mental approximation of what each command does.

ls - list files in directory
cd - enter directory
cat - view contents of file
file - list file type
du - list file size
find - find a file based on speicifcations

These may not be completely accurate, but they are the basis of how I used them going forward

To access the next password, I typed ls to list all the files and only saw readme, then used cat readme to open the file.

The contents appear to be a randomly generated password, so I copy the password and move on.

Password: NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL

After some trial and error, I found logout works in exiting the bandit servers, since you cannot access the next level inside a past level.

So i head back to my main terminal, type our ssh code again, replacing bandit0 with bandit1 as the username, and pasting the password given and amd given access to Bandit1.

## Bandit1-2

Again for this one, its best to start with the ls commmand to view what I have, and this time the filename I need is labeled -

Now, after attempting to use the cat command, it breaks, and upon further review, its because when typing "cat -", it assumes I'm missing a modifier for the cat command.

To solve this issue by simply googling it, I find that to open a file with a hyphen at the start, you need to include ./ before the filename, as that prefix is another way to tell what file exactly to open. 

After learning this, typing "cat ./-" spits out the next password, and were on the next level.

Password: rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

## Bandit 2-3

Starting with ls again, I find the file named "spaces in the file name"

As expected, using cat directly doesnt work again, but also using ./ doesn't work either, as the spaces seperate the name.

I actually had an assumption about how this could work, so I guessed that something would need to encompass the entire mame, and the most likely option would be surrounding it in quotation marks. And lo and behold, it gives me the next password.

Password: aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

## Bandit 3-4

The ls start this time provides us with the blue text labeled "inhere" which the website refers to as a directory. 

Using our past knowledge, I find using the cd command in place of the cat command is used to open directories instead of files, so we use cd inhere to open the directory

After entering the command, I find myself inside the directory, and again use the ls command to find anything, but return empty. 

Through trial and error, I use the find command by istelf to find 2 files, labeled . and ./.hidden

Using cat, I open the ./.hidden file and get its password and get on outta there.

Password: 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

## Bandit 4-5

Our ls start gives  us another inhere directory, and after cd-ing into the directory and ls-ing again, were presented with 10 different files to find our password in.

I really wanted to optimize this one and find it without brute forcing it, but after strugglingn for a while to find a better alternative, I ended up just going through each file with the cat commmand and finding our only readable file. 

That file ended up being -file07, and we copy and paste our way into bandit 5.

## Bandit 5-6

Ls starting into the inhere directory and ls-ing again reveals twice as many directories, each with multiple files inside. Brute force is not going to work this time. 

Thankfully, the site specifies not only it being human readable, but also being exactly 1033bytes in size and not an executable.

Viewing the find manual lets us search through every single directory and file with those specifications if we add modifiers together. So in combination, we have find (our command) -size 1033c (detecting size, using c to specify how many bytes) ! -executable (! meaning not -executable) | find -size 1033c ! -executable.

This code spits out ./maybehere07/.file2, which tells me that its in the 07 directory and 2nd file. 

I copy that line of code and use cat for that line to open that specific file, which thankfully fives me the password to the next level.

Password: P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

## Bandit 6-7

Our ls start gives nothing, so instead we use a find command by istelf to see all the hidden files, of which we have 4

The site tells us we need a flie owned by the user bandit7 and part of a group bandit6, and that the file is 33 bytes large

With all these in mind, using specific modifier on the find command helps us tremendously. using -user for the specific user, -group for the group, and our past size command we have the following instructions:
find -user bandti7 -group bandit6 -size 33c

To my surprise, this code doesn't work, which is because were working too low in the directory. We need to start from the very lowest level, being root. To access root is actually surprisingly simple, as we just need to add a / after find. Doing this, we get: find / -user bandti7 -group bandit6 -size 33c

Now putting this in gets a long list of outputs, most of which say permission denied or does not exist. Keyword: most.

One line is different than every other line in this output being exactly what we need: var/lib/dpkg/info/bandit7.password

I then cat this line of code to find our password and move on to the next level.

Password: z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

## Bandit 7-8

Starting with ls we can see that we only have data.txt in our directory, and after opening it we can see that it is a huge file that is unrealistic to sort by force.

The site wants us to search for the line with the word millionth on it, and gives us a few more commands as examples. 

The one we need right now is the grep command, which searches for a pattern in a specified file. we use this command along with the word milliontth to search for the line with that word and prints it out. 

grep millionth data.txt

This command spits out the line millonth with our requested password, so copy paste away.

Password: TESKZC0XvTetK0S9xNwm25STk5iWrBvP

## Bandit 8-9

This one presents us witha similar challenge as before, but instead of hacing a specific signifier to search for, we cna only look for the one that is not repeated. Luckily for us, we have a specific code given to us for that exact purpose, being uniq. 

uniq searches for any uniq lines and using the modifier -u prints only unique lines.

What we do is by first sorting all the lines alphabetically, that way its mich easier to check for repeat lines since they would all be next to themselves. We do this by using the sort command.

The final code line we use looks like this: sort data.txt | uniq -u

This gives us our unique line and our password for our final level for this segment.

Password: EN632PlfYiZbn3PhVK3XOGSlNInNE00t

## Bandit 9-10

This final level acts very similar to our last two, contating and single very large text file and we need to search for a specific line, in this case ones that start with a few '='. 

We can use our grep command along side the pattern, being ^=, which will search for every line that starts with =

We have to open the line first and sort by every single one that contants readable text first, which we do by using the cat command for the file, and using strings to saerch for only string types. Our final code line looks like this:

cat data.txt | strings | grep ^=

This gives us a few lines to work with, but the pattern of long strings of random characters being our passwords leads us to believe that our password is the only one that looks like the past ones from all the other levels before.

Password: G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s


## Conclusion
Overall, this experience was actually eye-opening and suprisingly fun to work around in. seeing direct results of my actions and being able to work around and a pseudo arg to go along with it was very comforting than most other comp sci classes that feel like a layer of hell deeper in figureing out what broke. using commands and knowing exactly how, when, and why to use them is helpful for me to want to learn more. I would say this was a genuine success in helping me want to go futher in this career path. Of course i got stuck a few times, in which google brought me to a third party walkthrough many times. I only caved i think twice for level 5 and level 6. 

Source for walkthrough: https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/index.html
