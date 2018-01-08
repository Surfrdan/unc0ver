# unc0ver
_An HTTP Recon tool_

unc0ver is a lightweight tool for performing HTTP reconnaissance. It is designed with the Unix principal of _do one thing and do it well._ It is constantly being updated so check back often for new features and bug fixes. Although I have included a wordlist, I highly recommend using unc0ver in combination with something like Daniel Miessler's excelent [SecLists](https://github.com/danielmiessler/SecLists) project in order to get the full benefit. 

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
| -w | Wordlist file e.g. wordlist.txt |
| -b | Include Body in output |
| -e | File Extension or other string to appear in URL after the dynamic string e.g. '.php' or '=9999' or '/further/directories' |
| -p | Prefix to dynamic string e.g. '?' or '&' Useful for querystrings |
| -c | Case to convert dynamic string to. Useful if wordlist is messy but you want to lowercase everything e.g. `-c lower` |
| -r | range of integers for fuzzung numeric files or IDs e.g. 1:300 will create numbers from 1 to 300 in place of the dynamic string |
| -f | Follow re-directs, off by default |
| -l | Simple rate limiting. value is number of seconds between requests. However due to Threading 12 requests are made at same time before the wait |
| -n | Note field in results. Defaults to Server header but can be overridden with title or h1 to display the pages `<title>` or `<h1>` content |
 
## Examples ##

Here we use the supplied wordlist and convert all strigns to lower case and add a .php file extension. We're also asking for the page title instead of the Server header to be printed in the results
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
404 09:20:58 http://www.example.com/itemid.php Example Domain                                                                                   ```

Here we utilise the grep command with an inverse match and 2 HTTP error codes to filter out unwanted results
```
$ ./unc0ver -u http://www.example.com/ -w complete_wordlist.txt | grep -v -E "500|403"                                  
                                                                                                                                                
                       ___                                                                                                                      
     _   _ _ __   ___ / _ \__   _____ _ __                                                                                                      
    | | | | '_ \ / __| | | \ \ / / _ \ '__|                                                                                                     
    | |_| | | | | (__| |_| |\ V /  __/ |                                                                                                        
     \__,_|_| |_|\___|\___/  \_/ \___|_|                                                                                                        
                                                                                                                                                
404 09:24:43 http://www.example.com/! ECS (lga/1373)                                                                                            
404 09:24:43 http://www.example.com/4975 ECS (lga/13A5)                                                                                         
404 09:24:43 http://www.example.com/EmailChecker ECS (lga/131B)                                                                                 
404 09:24:43 http://www.example.com/CARLOS ECS (lga/13CE)                                                                                       
404 09:24:43 http://www.example.com/ItemId ECS (lga/137D)                                                                                       ```

