# unc0ver
_An HTTP Recon tool_

unc0ver is a lightweight tool for performing HTTP reconnaissance. It is designed with the Unix principle of _do one thing and do it well._ It is constantly being updated so check back often for new features and bug fixes. Although I have included a wordlist, I highly recommend using unc0ver in combination with something like Daniel Miessler's excellent [SecLists](https://github.com/danielmiessler/SecLists) project in order to get the full benefit. 

## Dependencies ##
unc0ver is written for Python 3 and requires the following libraries to be present:

* requests
* multiprocessing
* urllib3
* bs4

## Usage ##

| Arg | Description |
| --- | ----------- |
| -u | Base URL for requests to be made to |
| -a | Randomise User-Agent header from some common browser IDs |
| -w | Wordlist file e.g. wordlist.txt |
| -b | Include Body in output |
| -e | File Extension or other string to appear in URL after the dynamic string e.g. '.php' or '=9999' or '/further/directories' |
| -p | Prefix to dynamic string e.g. '?' or '&' Useful for querystrings |
| -c | Case to convert a dynamic string to. Supports 'upper', 'lower' and 'first'|
| -r | range of integers for fuzzing numeric files or IDs e.g. 1:300 will create numbers from 1 to 300 in place of the dynamic string |
| -i | list of HTTP error codes to ignore. By default this is set to "404,502,503,504"|
| -is | list of returned body sizes in bytes to ignore. e.g. "0,2,2067", also supports range syntax e.g. 100:130 to ignore anything between 100 and 130 bytes |
| -f | Follow re-directs, off by default |
| -l | Simple rate limiting. Value is the number of miliseconds between requests |
| -n | Note field in results. Defaults to Server header but can be overridden with title or h1 to display the pages `<title>` or `<h1>` content |
| -H | Specify comma separated header values e.g. "Host: example.com, Content-type: application/json"
| -D | Debug mode
 
## Examples ##

Here we use the supplied wordlist and convert all strings to lower case and add a .php file extension. We're also asking for the page title instead of the Server header to be printed in the results
```
$ ./unc0ver -u https://www.yahoo.com/ -w wordlists/complete_wordlist.txt -c lower -e .php -n title

                           ___
         _   _ _ __   ___ / _ \__   _____ _ __
        | | | | '_ \ / __| | | \ \ / / _ \ '__|
        | |_| | | | | (__| |_| |\ V /  __/ |
         \__,_|_| |_|\___|\___/  \_/ \___|_|
           An HTTP Recon Tool - By Surfrdan

302 06:53:59 0s 107B [160/63786]   text/html; charset=UTF-8 https://www.yahoo.com/config.php None None
302 06:54:05 0s 0B [910/63786]   text/html; charset=UTF-8 https://www.yahoo.com/redirect.php None None
302 06:54:06 0s 107B [1324/63786]   text/html; charset=UTF-8 https://www.yahoo.com/config.php None None
400 06:54:08 0s 220B [1687/63786]   text/html https://www.yahoo.com/322.php <title>Bad Request</title> None
400 06:54:09 0s 3191B [2133/63786]   text/html; charset=iso-8859-1 https://www.yahoo.com/metro.php <title>Yahoo - 400 Bad Request</title> None
```

Here we utilise ignore arguments with 2 supplied HTTP error codes and also asking to ignore all zero byte responses

```
$ ./unc0ver -u http://www.example.com/ -i 500,403 -is 0                                                                                                   [12/12]

                           ___
         _   _ _ __   ___ / _ \__   _____ _ __
        | | | | '_ \ / __| | | \ \ / / _ \ '__|
        | |_| | | | | (__| |_| |\ V /  __/ |
         \__,_|_| |_|\___|\___/  \_/ \___|_|
           An HTTP Recon Tool - By Surfrdan

404 06:50:00 0s 1270B [120/63786]   text/html; charset=UTF-8 http://www.example.com/153 ECS (dca/24D9) None
404 06:50:00 0s 1270B [121/63786]   text/html; charset=UTF-8 http://www.example.com/1041 EOS (vny006/0454) None
404 06:50:00 0s 1270B [122/63786]   text/html; charset=UTF-8 http://www.example.com/1392 EOS (vny006/0451) None
404 06:50:00 0s 1270B [123/63786]   text/html; charset=UTF-8 http://www.example.com/! ECS (dca/24C0) None
404 06:50:00 0s 1270B [124/63786]   text/html; charset=UTF-8 http://www.example.com/1849 EOS (vny006/0452) None
404 06:50:00 0s 1270B [125/63786]   text/html; charset=UTF-8 http://www.example.com/SiteInformation EOS (vny006/0452) None
```

