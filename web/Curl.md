# Curl getting started 
## view source 
curl www.sina.com

## get the page [the same as wget]
curl -o [fileName] www.sina.com

## auto direct
curl -L www.sina.com
You will visit www.sina.com.cn

## show http response header
curl -i www.sina.com
curl -I www.sina.com Only show the http response header

## -v : verbose, show the whole process connecting to the address.
curl -v www.sina.com

## --trace ： prite more detail
curl --trace output.txt www.sina.com
curl --trace-ascii output.txt www.sina.com

## send form information
curl example.com/form.cgi?data=xxx
curl -X POST --data "data=xxx" example.com/form.cgi
curl -X POST --data-urlendoce "data=April 1" example.com/form.cgi

## HTTP verb (default GET)
curl -X POST www.example.com
curl -X DELETE www.example.com

## file upload form
curl --form upload=@localFileName --form press=OK \[URL\]
localFileName is the path of the file

## add referer field in http request 
curl --referer http://www.example.com http://www.example.com

## User Agent 
iPhone4 User Agent:
Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_0 like Mac OS X; en-us) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8A293 Safari/6531.22.7
curl --user-agent "\[User Agent\]" \[URL\]

## user --cookie to send cookie
curl --cookie "name=xxx" www.example.com
curl -c cookies http://example.com
curl -b cookies http://example.com
-c means save cookie to "cookies"
-b means use "cookies" as cookie

## http requset header
curl --header "Content-Type:apaplication/json" http://example.com
curl -H "Content-Type:apaplication/json" http://example.com
--header == -H

## http basic authentication
curl --user name:password example.com



