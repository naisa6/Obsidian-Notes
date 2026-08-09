command | parameter(s) | - explanation | flags available
cp source destination - copy file from source to destination 
- -v verbose
mv source destination - move file from source to destination. If moving without specifying directory, renames the file. 

nano 
- Ctrl+W - find 
- Ctrl+K - remove current line where cursor is
- Ctrl+O - output file 

less file - output text in a space within the terminal (q to exit)
| - pipe can be used to combine two commands and pipe. e.g. cat sources.list | grep src will only display lines that have 'src' in them

grep text - grep
- -i case insensitive

echo "text" - echoes onto the screen
\> - directs/ redirects things e.g.  echo "123TEST" > test, then cat test will print 123TEST
touch filename - create files
mkdir filename - make directory
chown username:usergroup filename - change ownership of a file
chmod  +x (or with numbers) - change file permissions
rm filename - but cannot remove directory files 
- -r recursive 
- -f force
adduser username

