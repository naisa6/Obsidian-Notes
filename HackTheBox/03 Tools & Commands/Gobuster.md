sudo apt install golang-go
sudo apt install gobuster





Usage:

 gobuster [command]

Available Commands:

 completion Generate the autocompletion script for the specified shell
 dir Uses directory/file enumeration mode
 dns Uses DNS subdomain enumeration mode
 fuzz Uses fuzzing mode. Replaces the keyword FUZZ in the URL, Headers and the
request body
 gcs Uses gcs bucket enumeration mode
 help Help about any command
 s3 Uses aws bucket enumeration mode
 tftp Uses TFTP enumeration mode
 version shows the current version
 vhost Uses VHOST enumeration mode (you most probably want to use the IP address as
the URL parameter)
Flags:
--debug Enable debug output
--delay duration Time each thread waits between requests (e.g. 1500ms)
-h, --help help for gobuster
--no-color Disable color output
--no-error Don't display errors
-z, --no-progress Don't display progress
-o, --output string Output file to write results to (defaults to stdout)
-p, --pattern string File containing replacement patterns
-q, --quiet Don't print the banner and other noise
-t, --threads int Number of concurrent threads (default 10)
-u : Specify the URL
-v, --verbose Verbose output (errors)
-w, --wordlist string Path to the wordlist. Set to - to use STDIN.
--wordlist-offset int Resume from a given position in the wordlist (defaults to 0)
--url : The target URL. 
--wordlist : Path to the wordlist. 
-x : File extension(s) to search for.
For the -x switch, we can specify php and html to filter out all the unnecessary clutter that does not interest us. PHP and HTML files will most commonly be pages. We might get lucky and find an administrative panel login page that could help us find leverage against the target in combination with the credentials we extracted from the FTP server

We can use the common.txt wordlist which can be downloaded from here.

wget https://github.com/danielmiessler/SecLists/raw/master/Discovery/WebContent/common.txt -O /usr/share/wordlists/common.txt