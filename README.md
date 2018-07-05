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
| -c | Case to convert a dynamic string to. Useful if wordlist is messy but you want to lowercase everything e.g. `-c lower` |
| -r | range of integers for fuzzing numeric files or IDs e.g. 1:300 will create numbers from 1 to 300 in place of the dynamic string |
| -i | list of HTTP error codes to ignore. By default this is set to "404,502,503,504"|
| -is | list of returned body sizes in bytes to ignore. e.g. "0,2,2067"|
| -f | Follow re-directs, off by default |
| -l | Simple rate limiting. Value is the number of miliseconds between requests |
| -n | Note field in results. Defaults to Server header but can be overridden with title or h1 to display the pages `<title>` or `<h1>` content |
| -H | Specify comma separated header values e.g. "Host: example.com, Content-type: application/json"
 
## Examples ##

Here we use the supplied wordlist and convert all strings to lower case and add a .php file extension. We're also asking for the page title instead of the Server header to be printed in the results
```
$ ./unc0ver -u http://www.example.com/ -w complete_wordlist.txt -c lower -e .php -n title                               
                                                                                                                                                
                       ___                                                                                                                      
     _   _ _ __   ___ / _ \__   _____ _ __                                                                                                      
    | | | | '_ \ / __| | | \ \ / / _ \ '__|                                                                                                     
    | |_| | | | | (__| |_| |\ V /  __/ |                                                                                                        
     \__,_|_| |_|\___|\___/  \_/ \___|_|                                                                                                        
                                                                                                                                                
404 09:20:58 http://www.example.com/review.php                                                                                                  
404 09:20:58 http://www.example.com/851.php Example Domain                                                                                      
404 09:20:58 http://www.example.com/addtocart_.php Example Domain                                                                               
404 09:20:58 http://www.example.com/212.php Example Domain                                                                                      
404 09:20:58 http://www.example.com/itemid.php Example Domain
```

Here we utilise ignore arguments with 2 supplied HTTP error codes and also asking to ignore all zero byte responses

```
$ ./unc0ver -u http://www.example.com/ -w complete_wordlist.txt -i "500|403" -is 0                                 
                                                                                                                                                
                       ___                                                                                                                      
     _   _ _ __   ___ / _ \__   _____ _ __                                                                                                      
    | | | | '_ \ / __| | | \ \ / / _ \ '__|                                                                                                     
    | |_| | | | | (__| |_| |\ V /  __/ |                                                                                                        
     \__,_|_| |_|\___|\___/  \_/ \___|_|                                                                                                        
                                                                                                                                                
404 09:24:43 http://www.example.com/! ECS (lga/1373)                                                                                            
404 09:24:43 http://www.example.com/4975 ECS (lga/13A5)                                                                                         
404 09:24:43 http://www.example.com/EmailChecker ECS (lga/131B)                                                                                 
404 09:24:43 http://www.example.com/CARLOS ECS (lga/13CE)                                                                                       
404 09:24:43 http://www.example.com/ItemId ECS (lga/137D)
```

