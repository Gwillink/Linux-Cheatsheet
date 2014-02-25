Linux-Cheatsheet - simple descriptive language to easily refind commands

Awk, Sed, Tr, Cut, sort, Egrep, Fgrep, grep, find, 


================
______________Explore directories [begin end last days minutes size greater]
find . -type d          ####! select all subdirectory names 
find . -type d -ls | fgrep -i 'mydir'      ####! select all directories & subdirectories with 'mydir' somewhere (in between ) in its name , include the times last saved and access, ignore case
find . -type d -name "mydir*" -ls ####! select all directories & subdirectories beginning with 'mydir' , include the saved times and access 
find . -type d -name "*mydir" -ls   ####! select all directories & subdirectories ending in 'mydir' , include the saved times and access 
find -type d -name ".*"   ####! select all hidden subdirectory names
du . | sort -n     ####!+ sort sizes of each directory and subdirectory to see which is biggest in size to identify which files may need deleting (try doing that in Windows..) 
du . | awk '{if (($1+0) > 2) print $0}' ####! directory and subdirectory sizes greater than 2
du . | awk '{if (($1+0) < 2) print $0}' ####! directory and subdirectory sizes less than 2
find . -type d -mtime -2     ####! select directory and subdirectory with files saved in last 2 days (2 months = 62 days, 2 years = 730 days)
find . -type d -mtime -2 -ls    ####! select directory and subdirectory with files saved in last 2 days, include size, saved dates
find . -type d -mmin -2 -ls    ####! select directory and subdirectory with files saved in last 2 (second) minutes
______________Explore Filenames   [begin end filename hidden myextention last days minutes size greater 2K 2M]
find . -type f -print     ####! selects all files names from all subdirectories
find . -name "*" -ls        ####! select all filenames (include size/date information) in all subdirectories. TIP: to then just show the filesnames only, also use:  | cut -d'.' -f2-
find . -name "*myfile*" -ls ####!+ select filenames with 'myfile' in between/somewhere in the filename (include size/date & subdirectory/path information) in all sub directories
find . -name \*.pdf -print ####! filenames,  myextention is pdf, in all subdirectories, include just the filesnames & subdirectory name/path
find . -name "*.myextention" -ls ####! filenames with myextention in all subdirectories, include size, date, path 
find . -name '*myfile' -ls   ####! select only filenames ending in 'myfile' 
find . -name 'myfile*' -ls   ####! select only filenames beginning with 'myfile' 
find . -exec ls -al {} \;   ####! select all files and subdirectories , include socalled hidden files
find . -type f -printf '%T@ %s %t %p\n' | sort -rk 1 -n | awk '{$1="";print $0}' ####! select all files in subdirectories, sizes and saved dates. sorted by date 
find . -type f -printf '%s %t %p\n' | sort -rk 1 -n  ####! select all files in subdirectories, sizes and saveddates, sorted by size
find . -type f -exec ls -s {} \; | sort -n  ####!+ select all files in subdirectories, sorted by size
ls -lgo --sort=extension    ####! Sorting files by extension/version, only this directory
find . -exec ls -al {} \;  | rev | sed 's/\./#/1' | rev | sort -t"#" -k2 | tr '#' '.' ####! Sorting files by extension , sundirectories
find . -type f -print | egrep '(.jpg|.gif|.png)'   ####!+ select jpg or gif or png files in any subdirectory 
find . -type f -print | fgrep -i myfile   ####! select files with the name 'myfile' somewhere/between in it (ignoring case) in all directories . show full path 
find . -size +2 -print    ####! files greater 2K in size 
find  . -size -2 -ls      ####! files less 2K in size 
find . -size +2k -size -2M -ls   ####!+ select all files in all subdirectories , size between 2K & 2Mb. Each k is 1024 bytes, include sizes, saved date, directory path 
find . -size +2k -size -2M -print    ####! select files in all subdirectories between 2,000 & 2Megs 
find . -mtime -2 -name "*" -ls   ####!+ files in subdirectories saved in last 2 days
find . -mtime +2 -print          ####! files saved in last 2 days or older
find . -mtime 0  -ls   ####! select files and directories saved  between now and 1 days ago (last 24 hours) 
find . -mmin +2 -mmin -10        ####!+ files in subdirectories saved between last 2 minutes and 10 minutes ago 
find . -mtime +2 -size +8 -print    ####! files saved in last 2 days or more aswell as greater 8K in size)
find . -mtime +2 -size +8 -ls    ####! files saved last 2 days or more aswell as greater 8K in size) include date, size
find . -newer myfile.txt -ls  ####! select all files saved after the time that myfile.txt was saved
______________Capture lines with 'mytext' in files [filename begin end ignore case number aswell mysecondtext]
TIP: goto the top level of the data using 'cd', use the command below and save it to a temporary file in a directory where you can save it. ie: fgrep -rai 'mytext' * >/cygdrive/c/mytest/myfile.txt ####!
fgrep -rai 'mytext' *   ####!+ select lines with 'mytext' in all files in all subdirectories (entire hard drive / network), ignore case, include filename TIP: cant use wildcard, like *.txt !! 
egrep -rai 'mytext|mysecondtext|mythirdtext' *  ####! select lines with 'mytext' or 'mysecondtext' or 'mythirdtext', ignore case, in all directories
fgrep -raic mytext m | fgrep -v ':0' ####! how many times is 'mytext is found in files in all subsirectories, ignore case
find . -type f -print0 | xargs -0 grep -ai "mytext"  ####!+ select 'mytext' (not case sensitive) from all subdirectories & select the directories, filenames & the lines if 'mytext' appears (most comprehensive but can take a while to process )
find . -name '*myfile[ABC]*' -print | xargs grep -aHE 'mytext'  ####! select only files with 'myfileA myfileB or myfileC somewhere in the filename in subdirectories, and only those lines with 'mytext' ie: using the Regex '^Mytext'
find . -name '*myfile' -print0 | xargs -0 grep -aHE 'mytext' ####! select only filenames ending in 'myfilename' (myextention) and select 'mytext'
find . -name '*myfile*' -print0 | xargs -0 grep -aHE '^mytext'  ####! select 'mytext' at beginning of the line, but only in files with "myfile" between/in its filename.
find . -name "*.myextention" | xargs fgrep -a 'mytext' ####! select 'mytext' in files with myextention 
find . -name 'myfile*mytext' -print0 | xargs -0 grep -a [alnum] ####!+ complete text of all filenames beginning with 'myfile' and also contain 'mytext' in the filename in all subdirs
find . -name "*.txt" -exec awk '{if ($1 ~/[A-Z][A-Z]/) print $0 }' {} \;   ####! in any subdirectory for files with 'txt' extention, select lines if there are atleast 2 consecutive uppercase characters somewhere on the line
find . -name "*.txt" -exec egrep -H "\b[0-9]{2,}\b" {} \;   ####! select lines in any subdirectory if there is an integer of atleast 2 consecutive numbers/digits ie 09 or 10 or 9999 (but not 2.5) 
find . -type f -print0 | xargs -0 egrep '(mytext|mysecondtext|mythirdtext)' ####! select lines in subdirectories with 'mytext' or 'mysecondtext' or 'mythirdtext'
find . -type f -print0 | xargs -0 grep -o "mytext.*mysecondtext"  ####! select lines with 'mytext' aswell as 'mysecondtext'
find . -print | xargs grep -aC2 'mytext'   ####! select mytext in all files & subdirectories , and also select 2 (second) lines above and below 'mytext' Address Pattern
find . -print | xargs grep -aA2 'mytext'   ####! select mytext in all files & subdirectories , and also select 2 (second) lines below 'mytext' Address Pattern
find . -print | xargs grep -aB2 'mytext'   ####! select mytext in all files & subdirectories , and also select 2 (second) lines above 'mytext' Address Pattern
find . -mtime -2 -size -2k -name 'myfile*' -print | xargs grep -ias 'mytext'  ####!+ select 'mytext (ignore case) in files saved in last 2 days, size less than 2K (not greater than 2K) , but only for filenames beginning with 'myfile' 
find . -mtime +2 -size +2k -name '*myfile' -print | xargs grep -ias 'mytext'  ####! select 'mytext (ignore case) in files saved in last 2 days or more, size greater than 2K  , but only for filenames ending with 'myfile' 
find . -mmin -2 -print | xargs grep -ias 'mytext'  ####!+ all files saved in last 2 minutes, containing mytext (ignoring case)
find . -mmin -2 -size -10k -print | xargs grep -ias 'mytext' ####! files saved todays date, in last 2 minutes , size less than 10K (not greater than 10K)
fgrep -rif /cygdrive/c/mytest/mylist.txt *   ####! select lines from a list of text/words in the file mylist.txt, (mytext or mysecondtext or ThirdText etc) all subdirectories from here on, ignore case  TIP:on a Windows PC, ensure you run d2u dos2unix on mylist.txt!!!!
find . -exec grep -if /cygdrive/c/mytest/mylist.txt {} \;  ####! select lines from a list of text/words in the file c:/mytext/mylist.txt, (mytext or mysecondtext or ThirdText etc) all subdirectories from here on, ignore case  TIP:on a Windows PC, ensure you run d2u dos2unix on mylist.txt!!!!
find . -name "*.txt" -exec grep -iHf /cygdrive/c/mytest/mylist.txt {} \;  ####! select lines from a list of text/words in the file c:/mytest/mylist.txt, (mytext or mysecondtext or ThirdText etc) all subdirectories from here on, only for files with "txt" as myextention, ignore case , show filenames. TIP:on a Windows PC, ensure you run d2u dos2unix on mylist.txt!!!!
fgrep -H 'mytext' */*/*     ####! select lines with 'mytext' in all files but only the third level subdirectories below this one, include filenames
find . -name "*.myextention" -exec cat {} \;   ####! select all lines in all the files with myextention. ie glue together all '*.txt' files into one single file
find . -name "*.myextention" -exec grep -H '' {} \; ####! select lines with myextention in all subdirs & include filename. ie glue together all '*.txt' files into one single file
______________Capture lines containing mytext  [begin end before after aswell or mysecondtext mythirdtext word ignore case]
TIP: now you have your temporary file such as 'myfile.txt', type 'cat myfile.txt | ' followed by a command shown below. IE: cat myfile.txt | fgrep  -i 'mytext' | wc ####!
fgrep  -i 'mytext'  ####! select lines with 'mytext' ignore case. could match MytEXT mytext MYTEXT etc
fgrep  -iw 'myword'  ####! select lines with 'myword', ignore case.  so will select 'MYWord' or 'myWORD', but wont select 'mywords' or 'allmyword' 
egrep -i 'mytext|mysecondtext|mythirdtext'  ####!+ select lines with either 'mytext' or 'mysecondtext' or 'mythirdtext', ignore case
fgrep 'mytext' | fgrep 'mysecondtext'   ####!+ select lines with both 'mytext' aswell as 'mysecondtext' in any order on the line 
fgrep -if mylist  ####!+ select any of the texts in the file 'mylist'. 'mytext' or 'mysecondtext' or 'mythirdtext' etc TIP: in Windows ensure you run d2u on mylist, so its in Unix format!!! 
awk '$0 ~/mytext.*mysecondtext/'   ####!+ select lines with if 'mytext' is before 'mysecondtext', 'mysecondtext' after 'mytext' 
awk '$0 ~/mytext.*mysecondtext.*mythirdtext/' ####! lines with 'mytext' before 'mysecondtext' aswell as followed after by 'mythirdtext'. ie lines with many occurrence of mychars/delimiters such as '|'  
egrep '\bmytext\w*\b'   ####!+ select lines if there is a word that begins with the text 'mytext' ie '(mytextualisation)' 
egrep '\b\w*mytext\b'   ####!+ select lines with words ending in 'mytext'. ie words ending in 'phobia' 'ing'
awk '{if ($2 ~/^[A-Z]{2,}/) print $0 }' ####! select lines where second column begins with 2 consecutive uppercase characters (range) after / before
grep '^\b[[:upper:]]\{2\}' ####! select lines that begin with a word of atleast 2 uppercase characters (range)
awk '{if ($0 ~/[A-Z][A-Z][0-9][0-9]/) print $0}' ####! select lines with 2 consecutive uppercase characters and 2 numbers anywhere on the line (range)
egrep 'mytext\>'   ####! select lines with words that end with 'mytext' ie: egrep 'ing\>'  will find 'beginning' '(starting)' '--+[going]' 
egrep '\b(\w+)\s+\1\b'   ####! select lines with words that are duplicated, any word immediately followed/after by same second word  
egrep 'mytext(.*?)\mytext'  ####! select lines if 'mytext' appears atleast twice duplicated - also for a second time on each line 
sed -n '/\([a-zA-Z][a-zA-Z]*\) \1/p'  ####! select lines with text that is duplicated, any text after/before the same second text
egrep 'mytext(.*?)mysecondtext'  ####! only select lines if the text 'mytext' is then followed/after somewhere on the same line with 'mysecondtext' ie: egrep '@(.*?)\.' ## select email address must have '@' aswell as '.' later on the line 
______________Capture a section of lines [lines above below mytext blankline between mysecondtext] 
awk '{if (match($0,"mytext")) exit; print $0}'   ####!+ select begin lines above 'mytext' ( but not including mytext) delete 'mytext' and all lines below/end
awk '{print $0; if (match($0,"mytext")) exit}'   ####! select begin lines above and including 'mytext', delete all lines below
awk '{print $0; if (match($0,"^mytext")) exit}'   ####! select begin lines above and including 'mytext', if 'mytext' is at beginning of line.  delete all lines below
awk '{print $0; if (length($0)==0) exit}'   ####! select the beginning lines (above) upto the beginning blankline, delete lines below beginning blankline TIP:may need to use command to delete/trim all ending/trailing spaces of each line 
awk '/mytext/ {p=1;next}; p==1 {print $0}'  ####!+ select lines below 'mytext' to the end of file ( but not including 'mytext') . delete beginning lines above 'mytext' 
sed -n '/mytext/,$p'   ####! select lines below 'mytext' to end of file, including 'mytext'. delete beginning lines above 'mytext' 
sed -n '1!G;h;$p' | awk '{print $0;if (length($0)==0) exit}' | sed -n '1!G;h;$p' ####! select all lines below the final ending blankline , delete all lines up to the final ending blankline 
awk '/^$/ {p=1;next}; p==1 {print $0}'    ####! delete all lines up to the beginning blankline, select all lines below beginning blankline TIP:may need to use command to delete all ending spaces of each line 
awk '/^$/ {p++;next}; p>1 {print $0}'  ####! select all lines below the second blankline. (delete all lines above the second blankline) 
awk '/mytext/{p++;next}; p>1 {print $0}'  ####! select all lines below the second 'mytext'. (delete all lines above the second 'mytext') 
awk '{print $0; if (match($2,"mytext")) exit}'   ####! select begin lines above and including 'mytext', if 'mytext' is in second column.  delete all lines below
awk '$2 == "mytext" , $2 == "mysecondtext" ' ####!+ select section of lines between the beginning line with 'mytext' in second column to 'mysecondtext' in second column 
head -20 |  sed '1,1d' ####! select section lines between second (fixed) line to line 20 (fixed) ie: cat -n | head -20 |  sed '1,1d'
sed '/mytext/,/mysecondtext/d'   ####!+ delete the section of lines between/after the beginning occurrence of 'mytext' & above/end occurrence of 'mysecondtext', if these words are anywhere on the line 
head -2   ####!+ select the beginning (fixed) begin and second lines, delete lines below second line
tail -2   ####!+ select (fixed) end line and second from end line , delete begining/above lines. ie: tail -100 , end 100 lines
tail -1000f myfile.txt ####! select the ending 1000 lines of myfile.txt and continue showing any new updates to the file
fgrep -B2 'mytext'    ####!+ select the line with mytext, aswell as the beginning and second lines above each mytext - near Address Pattern 
fgrep -A2 'mytext'    ####!+ select the line with mytext, aswell as the beginning and second lines below each mytext - near Address Pattern ie 1st 2 lines after wegsite has "Buy" :   sed 's/Buy/Buy\n#/g' | grep -A2 "#"    
fgrep -C2 'mytext'    ####!+ select 'mytext', aswell as the beginning and second fixed lines above & below 'mytext' - near Address Pattern 
_______________Capture 'mytext' on each line [mytext ignore case range begin end punctuation length words]
fgrep -i 'mytext'        ####! select lines with 'mytext' somewhere on the line (ignore case) 
fgrep '^mytext'          ####!+ select lines that begin with 'mytext' 
fgrep '^mytext[2345]'    ####! select lines that begin with (range) 'mytext2' or 'mytext3' or 'mytext4' or 'mytext5'
fgrep 'mytext$'          ####!+ select lines ending with 'mytext'
egrep '/[,'\.\?]'      ####! select the punctuation character '.' and '?' These are special characters.  To use the character as literal character, it has be preceded by backslash. 
egrep '^[ABCD]'     ####! select lines that begin with character 'A' or 'B' or 'C' or 'D' (range)
egrep '^[^ABCD]'     ####! select lines that dont begin with character 'A' or 'B' or 'C' or 'D' (range)
fgrep 'mytext '    ####!+ select lines with words/columns ending in 'mytext'
fgrep ' mytext'    ####!+ select lines with words/columns beginning with 'mytext'
grep '([A-Za-z][A-Za-z])' ####! select lines with 2 character US state names, surrounded by round braces. mychar '(' , range A-Za-z , range A-Za-z, mychar ')'
egrep '[A-F][g-k][0-9][0-9]'  ####! select words/text anywhere on line beginning with a range of characters A or B,C,D,E,F followed/after by a second character of g or h,i,j,k aswell as followed by a number and a second number  
fgrep -w 'myword'  ####! select lines with the exact word 'myword' on the line, so wont select lines with 'mywords' or 'allmyword' for example 
awk 'length($0)>2'    ####! select those lines if the length is greater than 2 characters, delete lines smaller than 1 2 beginning and second length ( < less)
awk 'length > 2'    ####!+ select lines greater than (fixed) 2 characters length (second) , delete lines smaller than 1 2 ( < less than)
awk 'length > max {max=length;lin=$0} END {print lin;}' ####!  select the longest line 
egrep '\<\w{2}\>'  ####! select lines with words of length of 2 characters (second)
egrep '\<\w{2,}\>'  ####!+ select lines with words of length of 2 characters or more (second)
egrep '\<\w{2,8}\>'   ####!+ select lines with words of length of 2 to 8 characters   (second)
______________numbers or values    [greater smaller equals number end begin second column delete]
egrep '[0-9]{2}'   ####! select lines with 2 consecutive numbers
egrep '\b(100|[1-9]?[0-9]\b)'   ####! select lines if there's a number between 0-100, greater than 0  WARNING: wont find number '2.0' TIP: use awk example 
awk '{if (($2 + 0) > 2.1) print $0 }' ####!+ if second column has a number/value : is greater than 2.1, select whole line. WARNING: '> 0' is the same as selecting the whole line)  (flip '>' sign to make smaller) ie greater than 2013. TIP:check for any interfering punctuation 
awk '{if (($1 + 0) < 2.1) print $2,$3,$1 }' ####! begin column has a number/value : is smaller than 2.1, select second,third and third column
egrep '\b[0-9]{2,}\b' ####! lines have a numbers with length of 2 or consecutive more/greater numbers (second), somewhere on the line
awk '{for (i=1;i<=NF;i++) if(($i+0)> 2.1) {print $0;i=NF}}' ####!+ if value of any number on the line is greater/less than 2.1 ,select whole line. range 
awk '{if (($2+0)> 2.1) {$2="mytext" $2;print $0} else print $0 }' ####! insert 'mytext' before second column, if 2nd column is greater than number/value 2.1
tr -d '[:digit:]'      ####! delete all numbers (range of characters 0-9)
sed "s/^/      /; s/ *\(.\{7,\}\)/\1/"  ####! right align numbers / format
grep "[0-9]\{2,\}"   ####! lines with 2 consecutive numbers/digits, or more (length)
egrep '[^0-9]'        ####! delete all lines that have just numbers (lines beginning with just single integer amount) (can select the range/character set other than numeric characters)  
egrep '[0-9]{5}'    ####! select US zip codes (5 fixed numbers ) anywhere on the line 
awk '($2 + 0) != 2.1'   ####! if second column has a number/value : is NOT equals to 2.1 ie column could show 10.000, 10 or 010, delete that whole line 
awk '($2 + 0) == 2.1'   ####! if second column has a number/value : is exactly equals to 2.1  ie column could select 10.000, 10 or 010, select whole line 
______________replace or convert text  [mysecondtext beginning ignore case mythirdtext begin end line mychar duplicate space list]
sed 's/mytext/mysecondtext/g'   ####!+ replace every 'mytext' with 'mysecondtext' 
sed 's/mytext/mysecondtext/1'    ####!+ replace only the beginning occurrence of 'mytext' on each line with 'mysecondtext' 
sed 's/mytext/mysecondtext/2'    ####!+ replace only the second occurrence of 'mytext' on each line with 'mysecondtext' 
sed 's/mytext/mysecondtext/gi'   ####!+ replace every 'mytext' with 'mysecondtext', ignoring case of 'mytext' 
awk '/mythirdtext/{gsub(/mytext/,"mysecondtext")};1'  ####! replace 'mytext' with 'mysecondtext' only on lines containing 'mythirdtext'
awk '!/mythirdtext/{gsub(/mytext/,"mysecondtext")};1'  ####! replace 'mytext' with 'mysecondtext' only on those lines NOT containing 'mythirdtext'
sed 's/mytext\(.*\)mysecondtext/mytext\1mythirdtext/' ####! select 'mytext' on the line, then for the remainder of the line replace (the end occurrence of ) 'mysecondtext' with 'mythirdtext'
sed '2 c\mytext'    ####! replace second (fixed) line with 'mytext'
sed '$ c\mytext'    ####! replace end line with 'mytext'
sed -e 's/mytext.*/mysecondtext/' ####!+ replace everything after 'mytext' with 'mysecondtext'. replacing mytext and everything after mytext
rev | sed 's/mychar/mysecondchar/1' | rev  ####!+ replace end occurrence of 'mychar' with mysecondchar
sed 's/mytext/£/g' | rev | sed 's/£/#/1' | rev | sed 's/£/mytext/g' | sed 's/#/mysecondtext/1' ####! replace end occurrence of 'mytext' with mysecondtext TIP:ensure chars '£' & '#' are not in the file
sed 's/^$/mytext/g'   ####!+ replace blanklines with 'mytext'. insert 'mytext' TIP:may need to ensure is truly blankline
awk '/mytext$/ {sub(/mytext$/,""); getline t; print $0 t; next}; 1'  ####! if 'mytext' at end of the line, glue the line below after this line
awk -v OFS=" " '$1=$1'  ####! replace all multiple/duplicate/consecutive spaces with single space, compress text 
awk '{gsub("mytext","mysecondtext",$2);print $0}' ####!+ if 'mytext' in second column, replace with 'mysecondtext' ($NF if mytext is in end column) 
awk '{gsub("\\<[a-zA-Z0-9]*[a|A]\\>", "mysecondtext" );print}' ####!+ replace words/columns ending in character 'a' or 'A' with 'mysecondtext'
sed 's/\(.*\)mytext/\1mysecondtext/g'   ####!+ if 'mytext' is at the end on the line , replace with 'mysecondtext'  
awk '$0 ~/mytext/{n+=1}{if (n==2){sub("mytext","mysecondtext",$0)};print }' ####!+ replace only the second instance of 'mytext in the whole file with 'mysecondtext' 
sed -f /cygdrive/c/muppix/myreplacelist.txt  ####!+ replace or delete or insert mytext or mysecondtext (many texts) using a list of multiple/duplicate texts in a file 'myreplacelist.txt'. ie convert/normalise/clean text 
sed '/mytext/c\mysecondtext'  ####!+ if 'mytext is found anywhere on line, replace whole line with 'mysecondtext'
________________Select / delete Columns [ mytext begin end second or delete mychar mydelimiter split]
awk '{print $1}'    ####!+ select beginning column only 
awk '{print $2,$NF}'  ####!+ select second column and end column 
awk '{print $NF}'      ####!+ select only the end column delete columns before the end column
awk '{if ($2 ~/mytext$/) print }'    ####!+ select line if second column ends with 'mytext'
awk '{if ($2 ~/^mytext/) print }'    ####!+ select line if second column begins with 'mytext'
awk '{print $2}'  FS=","    ####! select second column, but using ',' comma as mydelimiter
awk '{print $(NF-1)}'  ####!+ select only second from end column , delete
awk '{$2=$3=$5=""}{print $0}'   ####! delete second aswell as third fifth (2 3 5) columns , regardless how many columns there are 
awk 'NF > 2'  ####! select only those line that have more than 2 columns (delete lines with begin and second columns) length
awk '{if ($2 == "mytext" ) print $1,$2,$3,$4 }'   ####! select column 1,2,3,4 if second column is 'mytext' 
awk '{if ($2 == "mytext" ) print $0 }'   ####! select line if second column is 'mytext' 
awk '{if (substr($0,2,6)=="mytext") print $0 }'  ####! select line if (fixed) character columns 2-7 is "mytext" (from second character, for 6 characters , as length of "mytext" is 6 )
awk '{if ($2 ~/mytext|mysecondtext|mythirdtext/) print $0}'   ####! select whole line if 'mytext' or 'mysecondtext' or 'mythirdtext' is somewhere in the second column 
awk '{if ($2 !~/mytext/) print }'  ####! delete line if second column is 'mytext'   
sed -e 's/\<[[:alnum:]]*[A|a]\>/&/g' ####! delete words/columns ending in 'A' or 'a' (range)
cut -d ',' -f2-   ####! select second column (using mydelimiter ',') & all columns after 2, (split lines)
______________insert lines / append text [begin end between before after mysecondtext blankline file] 
sed '/mytext/{x;p;x;}'    ####!+ insert a blankline above those lines if 'mytext' is somewhere on the line 
sed '/mytext/G'       ####!+ insert a blankline below lines with 'mytext' on the line  
sed '1i\mytext'  ####!+ insert 'mytext' above all the lines/above beginning of lines 
awk '$0 ~/mytext/{print "mysecondtext " $0 }'     ####!+ select lines with 'mytext', insert word/column 'mysecondtext ' at beginning of line 
sed '$ a\mytext'    ####!+ insert 'mytext' below end of all lines
sed '1i\\n'  ####!+ insert blankline above beginning of all the lines
sed '$ a\'   ####!+ insert blankline at end of all the lines (below end line)
awk '{if (""==$2) {print "mytext" $0} else {if (pre==""){print "mytext" $0} else {print $0}} ; pre=$2}'  ####! insert 'mytext' at the beginning of all paragraphs
echo "mytext" | cat - myfile.txt ####! take the text results of some command, insert/glue/join below the file 'myfile.txt', and then continue with other commands. useful in processing in a loop
sed 's/mytext/mytext\n/g' ####!+ insert newline after 'mytext'. split the line after mytext
sed 's/mytext/\nmytext/g' ####!+ insert newline before 'mytext'. split the line before mytext so every mytext is at the beginning of the line
sed '/mytext/i\mysecondtext' ####! if 'mytext' is found annywhere on line, insert 'mysecondtext' on line above
sed '/mytext/a\mysecondtext' ####! if 'mytext' is found annywhere on line, insert 'mysecondtext' on line below
awk '{if ($0 ~/mytext/){printf("%s\n%s\n", "mysecondtext", $0);printf("%s\n%s\n",$0,"mythirdtext")} else {print $0}}' ####! if 'mytext' is found, insert 'mysecondtext' on line above aswell as 'mythirdtext' below
awk '{if ($2 ~/^mytext|^mysecondtext|^myothertext/){print $0 "mythirdtext"} else print $0}' ####! if 'mytext' or 'mysecondtext' or 'myothertext' is found in beginning of second column, insert 'mythirdtext' at end of the line 
awk '{if ($2 ~/mytext$|mysecondtext$|myothertext$/){print "mythirdtext" $0} else print $0}' ####! if 'mytext' or 'mysecondtext' or 'myothertext' is found in end of second column, insert 'mythirdtext' at beginning of the line 
sed  -e '/mytext/r myfile.txt' -e 'x;$G' ####! insert file 'myfile.txt' above a line with 'mytext' on it
sed '/mytext/r myfile.txt' ####! insert file 'myfile.txt' below a line with 'mytext' on it
________________insert text on the line   [mytext before after column blankline]
sed 's/^/mytext /'    ####!+ insert 'mytext ' / column  before beginning of each line ie: sed 's/^/ /' #indent lines
sed 's/.*/&mytext/'    ####!+ insert 'mytext' or column after at the end of each line 
sed 's/mytext/mysecondtextmytext/g' ####!+ insert 'mysecondtext' before 'mytext'
sed 's/mytext/mytextmysecondtext/g' ####!+ insert 'mysecondtext' after 'mytext'
awk '{if(match($0,"mytext")){print "mysecondtext" $0} else {print $0}}'  ####! insert mysecondtext/column at beginning of line if line has 'mytext'
awk '{if(match($0,"mytext")){print $0 "mysecondtext"} else {print $0}}' ####! insert mysecondtext/column at end of line if line has 'mytext'
sed 's/mytext[AB]/mysecondtext&/g' ####! insert 'mysecondtext' before 'mytextA' or 'mytextB (range)'
awk '{if ($2 ~/mytext/){$2="mysecondtext" $2;print $0}else print $0}'  ####! if 'mytext' is in second column, insert 'mysecondtext' before the second column 
awk '{if ($2 ~/mytext/){$2=$2 "mysecondtext";print $0}else print $0}'  ####! if 'mytext' is in second column, insert 'mysecondtext' after the second column 
awk '{getline addf <"myfile.txt"}{$2=$2 addf;print $0}' ####! insert file 'myfile.txt' after second column TIP: if myfile has less lines, it will repeat the last line
sed -e 's/\<[[:alnum:]]*[mytext|mysecondtext]\>/mythirdtext&/g' ####! insert 'mythirdtext' before words/columns ending in 'mytext' or 'mysecondtext'
nl -ba   ####!+ insert linenumbers at the beginning of each line ie find out linenumbers with 'mytext' : cat myfile.txt| nl -ba |fgrep 'mytext' 
______________delete lines    [begin end above below duplicate blanklines]
fgrep -v 'mytext'      ####!+ delete if 'mytext' is somewhere on the line TIP: dont ignore case & also first check which lines will be deleted by running: fgrep 'mytext' 
grep -v '^mytext'       ####!+ delete all lines that begin with 'mytext' 
grep -v 'mytext$'       ####!+ delete all lines that end with 'mytext'
egrep -v 'mytext|mysecondtext'  ####!+ delete lines with 'mytext' or 'mysecondtext'
egrep -v 'mytext(.*?)mysecondtext' ####!+ delete lines with 'mytext' before 'mysecondtext' ('mysecondtext' after 'mytext'
awk NF        ####!+ truly delete all blanklines which may have some spaces or tabs or no spaces at all.  
sort -u       ####! sort, then delete duplicate lines, ( dont maintain  the original order (its a lot faster))
sort | uniq -d  ####! select only the duplicate lines, ie those lines that occur twice or more 
awk '!x[$0]++'  ####! delete duplicate lines, but maintain the original order ( without sorting) select begin occurrence of each line. useful for ensuring table has all the column names
sed '/mytext/,/mysecondtext/d'  ####! delete lines that begin with 'mytext' aswell as end 'mysecondtext' ie: xml tags:  sed '/<myxml>/,/<\/myxml>/d'
awk '!($2 in a){a[$2];print $0}' ####! delete duplicate lines, based on duplicates in second column only, select begin occurence of 2nd column,preserve order of lines
egrep -v 'mytext(.*?)\mytext'    ####! delete lines if 'mytext' appears twice , for a second time on each line 
sed '/mytext/,/<\/p>/d'     ####!+ delete all lines below 'mytext' , select beginning lines above 'mytext' 
sed '/^mytext/,/<\/p>/d'     ####! delete all lines below 'mytext' is in the beginning of the line, select beginning lines above 'mytext'
sed '/./,/^$/!d'     ####! delete all multiple/duplicate/consecutive blanklines from the lines except the beginning line; also deletes all blanklines from beginning and end of the lines 
sed '/^$/N;/\n$/N;//D'   ####! delete all multiple/duplicate/consecutive blanklines except the beginning and second: 
______________delete text within lines [before after mytext between mysecondtext mychar begin end mydelimiter]
sed -n -e 's/.*mytext\(.*\)mysecondtext.*/\1/p'   ####! select between 'mytext' to end occurrence of 'mysecondtext' on each line .  will find 'anymytext to the somemysecondtext'  
sed -n -e 's/.*\bmytext\b\(.*\)\bmysecondtext\b.*/\1/p'  ####! select text between the exact words 'mytext' and 'mysecondtext' on the same line, delete before 'mytext' and after 'mysecondtext'
grep -o "mytext.*mysecondtext"  ####!+ select text between 'mytext' and 'mysecondtext' on the same line. delete before 'mytext' aswell as after 'mysecondtext', include mytxt & mysecondtext
sed 's/mytext.*mysecondtext//g'   ####! deletes text between 'mytext' to 'mysecondtext' if found on a line 
tr -d 'a'         ####! deletes 'a' characters ('a' is mychar/mytext) 
tr -d 'and'       ####! deletes all occurrence of any of these 3 single character 'a' or 'n' or 'd' (ie also delete 'and' 'dan' etc) delete multiple/duplicate characters 
tr -d '"'         ####! delete double quote character (mychar/mytext)
tr -d "'"         ####! delete single quote character (mychar/mytext)
sed 's/^.*mytext//'   ####! delete everything on the line before the end occurrence of 'mytext' including 'mytext' . select everythings after end occurrence of 'mytext' on line
rev | cut -d '/' -f2- | rev   ####! delete everything on the line after the end occurrence of '/' (mychar) , select everything before end mydelimiter ('/') ie select path of the full filename /tmp/mydir/mysubdir/myfile.txt 
rev | sed 's/mychar/@/g' | cut -@ '/' -f2- | rev   ####! delete everything on the line after the end occurrence of 'mychar' , select everything before end 'mychar'  
______________delete text within a line  [before after begin end word column occurrence second character number]
sed 's/mytext//g'  ####!+ delete 'mytext' on the line if found 
sed 's/mytext//2'  ####! delete only the second occurrence of 'mytext' on each line
awk '{$1="";print $0}'  ####!+ delete beginning word / column 
awk '{$NF="";print $0}'  ####!+ delete end word / end column
awk '{$2="";print $0}'  ####!+ delete second word / column 
awk '{$1=$2=$3=$NF=""; print $0 }' ####! delete begin , second , third, end column
sed 's/mytext.*//g'     ####!+ select everything before 'mytext' on the line, ( delete 'mytext' & everything after)
sed 's/.*mytext//g'     ####!+ delete everything before 'mytext' on the line, ( select all text after 'mytext')
sed 's/mytext/#+#/2'|sed 's/#+#.*//g'  ####!+ delete everything after second occurrence of 'mytext', select everything before 2nd occurrence of 'mytext'
sed 's/mytext/#+#/2'|sed 's/.*#+#//g' ####!+ delete everything before second occurrence of 'mytext', select everything after 2nd occurrence of 'mytext'
sed 's/[^` ]*mytext*[^ ]*//g'  ####!+ delete words/columns anywhere on the line, with 'mytext' somewhere inside/between the word ie will delete words such as 'allmytext' or 'mytexting' or 'mytext'
sed 's/.$//'   ####! delete end character on each line
sed 's/,$//'   ####! delete end character if its a comma is mychar
sed 's/[0-9]//g'    ####!+ delete all numbers 
cut -c 3-      ####! delete the (fixed) beginning and second character of each line (select third character onwards) 
awk -v OFS=" " '$1=$1'   ####!  delete/replace all multiple/duplicate/consecutive spaces with single space/blank 
tr -dc '\11\12\40-\176'   ####! delete all non-printable punctuation characters TIP: initially use this when you get "Binary file (standard input) matches"
tr '[:punct:]' ' '     ####! replace punctuation with space (delete all punctuation)
______________Sort & rearrange order   [sort second column delimiter split]
sort -n   ####! sort by numbers ie look at beginning column as numeric values and sort TIP: if there are punctuation characters, sort may not work & delete them 
sort -k2          ####! sort whole text by second column 
sort -t":" -k2   ####! sort text by second column, if ":" is mydelimiter
sort -rk2         ####! sort second column in reverse order
rev              ####! reverse/rotate each character on the line, end char becomes begin characer
_______________paragraphs
cut -d ' ' -f2   ####!select second column only using each space character ' ' as a column mydelimiter. split TIP: shld delete multiple spaces
cut -c 2-    ####! select fixed text after the second character onwards, delete beginning 2 characters 
cut -d '#' -f2 | cut -d '.' -f2-   ####! select all text after the 1st '#' mydelimiter  character on the line, and then all text after the next '.' character split 
awk 1 ORS=' ' ####! convert whole text into 1 single line ## ??
tr -c [:alnum:] ' '| tr ' ' '\n'  ####!+ replace punctuation characters with spaces, then replaces spaces with newlines , converting text to a long list of words/products
awk '{temp = $1;$1 = $2;$2 = temp;print}'  ####! select second column and then beginning column , and then all the other columns (swap columns 1 & 2 around )  (structure) 
sed 's/mytext/\n/g'    ####! split up line everytime it finds 'mytext'.ie insert newline when it finds 'mytext' (structure) 
pr -T2   ####! convert single list (one column) into 2 columns (filling the 1st column going down, then second column etc)
tr '[:punct:]' ' ' | tr ' ' '\n' ####! convert text into single list of words
sed ':a;s/\B[0-9]\{3\}\>/,&/;ta'     ####! format numbers : insert commas to all numbers, changing '1234567' to '1,234,567'  (GNU sed) 
diff -w myfile mysecondfile   ####! select differences in 2 files, but ignore differences of extra spaces or tabs (white space)  TIP "<" in the output means diff in 'myfile', ">" differences in 'mysecondfile'
touch myfile >myfile.txt ####! delete/empty-out all contents/lines in myfile.txt
________________loop:
mylist ; do mycommand ; mysecondcommand ; done  ####! loop trough list ( mylist)  do some Unix commands and repeat ie:
find . -name \*.txt -print | while read f ; do echo "$f" ; u2d "$f" ;  done   ####! loop to convert all txt files (myextention), display the filename, convert, to be readable in windows 
while sleep 2 ; do  date  ; done     ####! select current time every 2 seconds  
pdftotext -layout myfile.pdf    ####! generates a new file myfile.txt in the current directory. TIP for cygwin need to include package during installation . pdf to text converted on unix
_______________reading in websites as text ie twitter feed
wget http://www.mywebsite.com/  ####! download html of mywebsite, saved as a file called index.html,& also craetes a directory 'www.mywebsite.com' , can be loaded in to a browser
______________save / join / append files [directory extention database insert join] 
TIP: dont ever cat a file or search a file and then save it with the same name again. ie dont : cat myfile.txt| mycommand >myfile.txt !!  ####!
>myfile.txt    ####! save results to myfile.txt in this directory (TIP: pls note there is no "|" ) ie: ls -al >myfile 
>myfile.dat  ####! save as text file for viewing in notepad *.dat 
>mydatabase.csv     ####! save to database or Excell/spreadsheet or Access . cat myfile|grep 'mytext' >/cygdrive/c:/mytest/mydatabase.csv 
cat myfile >>mysecondfile    ####! insert myfile at beginning/before mysecondfile (even if mysecondfile doesnt exist yet) Insert/save mysecondfile below end of myfile 
paste myfile mysecondfile | sed 's/   .//'   ####! join 2 files, insert some spaces in between
pr -tm myfile mysecondfile  ####! join/insert 2 files (after/begin/end) side by side as 2 columns
u2d  ####!  TIP may need to run unix2dos or u2d , before looking at the file in Windows say notepad
_______________end of webversion of Muppix Toolkit 

_______________Linux basic navigation commands in the terminal window
TIP: the Muppix command is everything before the '##' in this Toolkit document. This is what you cut/paste & fill in ####!
pwd       ####! the full name of this directory you're in. use file manager/Explorer to find directories
cd c:     ####!+ Cygwin way of to goto the C: harddrive
cd mydir ####!+ cd goto directory called mydir ie: cd mydir  TIP: can type beginning few cjaracters & TAB and it will find the fill directory name
cd ..  ####!+ change directory, goto, up 1 directory level
ls -al    ####!+  all filesnames (including socalled hidden files) and time stamps in this directory only filenames & details, this directory only
ls -altr  ####! filenames , sorted by date - what is the most recent file in this directory only
find . -name "*" -ls    ####!+ select all filenames (& all its size/date information) in this dir & all sub directories, also select each subdirectory name with each file 
cal    ####! calendar , to see when was the directory or file was last saved, amount of  days to another date
date      ####! today's date & system time
date -d "-2 months -2 days" ####! what calendar date was (last) 2 months and 2 days ago
cat myfile.txt  ####!+ select whole file ie: cat muppix.txt
cat myfile.txt | head    ####! begin 10 lines of file TIP: try all your commands on just these 10 lines ie: cat muppix.txt | head
cat myfile.txt | tail    ####! end 10 lines of file  TIP: use this subset of the file to try all your commands  ie: cat muppix.txt | tail
find . -exec cat {} \;  ####! select every line of every file in every subdirectory
more    ####! view the results  ie: cat muppix.txt | more
less -I   ####! view the results (see below for less Ctrol keys)  ie: cat muppix.txt | less
cut -c-2-88  ####!+ delete character  before than 2, select between character 2 ( second) and 88. delete greater/after 88   
cut -c-88    ####! only begin / less than  88 (fixed) characters of line ie: cat myfile.txt | cut -c-88 
wc -lc       ####!+ how many lines in the list / file ## ie: cat myfile.txt | wc -lc
du .         ####! directory sizes for this directory & all its subdirectories (ie: what's filling up diskspace ??)  ie: du . | sort -n  
fgrep -i 'mytext'  ####!+ all lines with 'mytext' anywhere on the line, ignore case. ie: cat myfile.txt | fgrep -i 'mytext '
sed '/./,$!d;s/[ \t]*$//'  ####! delete leading/beginning blanklines aswell as ending/trailing blanklines ie: cat myfile.txt | sed '/./,$!d;s/[ \t]*$//'
awk -v OFS=" " '$1=$1'   ####!+  delete/replace all multiple/duplicate/consecutive spaces with single space/blank, also deletes begin spaces. easy to view ie: cat myfile.txt | awk -v OFS=" " '$1=$1'
TIP: String together commands using the character : '|' ####!
|              ####! chain commands together :ie cat myfile.txt | fgrep 'mytext"  | wc -lc   ## this will select lines with 'mytext' in myfile.txt, and then show how many lines have "mytext" in it 
sort          ####! sort lines
sort -u       ####!+ sort lines and then delete duplicate lines
sort -k2      ####!+ sort on the second column
history  100  ####!  history of my 100 commands used in this terminal
>myfile.txt    ####!+ save results to myfile.txt in this directory (TIP: pls note there is no "|" ) ie: ls -al >myfile 
>myspreadsheet.csv  ####!+ save results to spreadsheet. result needs column delimiters, such as ';', but best is "|" as delimiter)  
u2d  ####!  TIP may need to run unix2dos or u2d , before looking at the file in Windows say notepad
diff -w myfile mysecondfile   ####!+ select differences in 2 files, but ignore differences of extra spaces or tabs (white space)  TIP "<" in the output means diff in 'myfile', ">" differences in 'mysecondfile'
______________Tools to help view the output
TIP: use these commands to temporarily view the output ####!
cat myfile  ####! show myfile
grep --color=auto -inHR 'mytext' *   ####! select/show in fancy colours/colors and linenumbers, 'mytext' 
grep --color mytext  *  ####! select/show in fancy colours/colors and linenumbers, 'mytext' 
head          ####! select beginning 10 lines of file ie: cat myfile| head | someMuppixcommand 
head -2       ####! select beginning (fixed) and second lines 
cut -c-2    ####!+ only select beginning and second characters of each line. ie: cut -c-77 to quickly view text, beginning 77 characters (not wrap long lines) 
sort -u    ####! delete duplicates, just the unique list
more     ####!
less -I  ####! view file of ANY Size & quickly move up and down & search (ignore case) ie: grep 'mytext' | less -I
-- spacebar   ####!next page
-- PageUp, PageDown ####!
--   <        ####!top of file
--   >        ####!bottom of file
--   /mytext   ####!search mytext forwards (ignore case)
--   /^mytext   ####! search lines beginning with 'mytext'
--   /mytext$   ####! search lines ending with 'mytext'
--   /myt[ex]   ####! search 'myte' or 'mytx'
--   ?mytext   ####!search mytext backwards
--   n     ####!next match Forwards
--   N     ####!next match backwards
--   999   ####!goto line 999 line
--   q        ####!Quit
--   CTRL z   ####!Quit 
tail -2    ####! select (fixed) end line and second from end line ie: tail -100 , end 100 lines
tail -f  myfile         ####! tails , selects end lines of the file & continues to select new updates if more lines are added . ie: a live log file  TIP:tail -f myfile| grep mytext  ## just select the lines with my Text
tail -300f myfile     ####! tails, initially select the end 300 lines , then select new updates 
tail -f file | grep 'mytext'    ####! tail the file, but only select lines with 'mytext' in it. very usefull for tailing changing log files
history      ####!list of commands
history 100 | grep 'mytext'     ####! out of the 100 commands, select those with 'mytext'
time mycommand       ####! See how long a command takes to execute ie: time fgrep 'mytext'
paste -s -d ' ' | xargs -n 2  ####! convert list of words to 2 columns TIP:if its a Windows file, shld  run  d2u 
nl -ba   ####! insert linenumbers at the beginning of each line 
cat -n       ####! insert linenumbers at beginning of each line ie: find out linenumbers with 'mytext' : cat myfile.txt| cat -n |fgrep 'mytext' 
fmt myfile        ####! format/left align all the text into 80 chars wide
______________Terminal keystrokes in Unix window
Up arrow  ####!  previous Linux command 
Down arrow ####! next Linux command
CTRL l   ####! clean line/statement
HOME     ####! cursor to beginning of line
END      ####! cursor to end of line
CTRL r   ####!  reverse command , enter what to search on , CTRL r again , goes to previous command#
CTRL k   ####!  deletes rest of line
CTRL d   ####! delete current character
CTRL c   ####!  STOP / Exit ie: cat mymassivefile    & then CTRL c or CTRL z
CTRL z   ####!  STOP / Exit
history   ####! commands
cut n paste   ####!   right click on blue banner of the Unix window, Edit , Paste or Mark
