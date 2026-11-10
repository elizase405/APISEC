Often times security controls like web applications firewalls (WAFs) and rate-limiting can block your attacks. Security controls may differ from one API provider to the next, but at a high level, they will have some threshold for malicious activity that will trigger a response. WAFs, for example, can be triggered by a wide variety of things, like:
- Too many requests for resources that do not exist.
- Too many requests within a certain amount of time
- Common attack attempts, like SQL injection and XSS attacks
- Abnormal behavior, like tests for authorization vulnerabilities

The following measures can be effective at evading or bypassing these restrictions.
#### String Terminators
Null bytes and other combinations of symbols are often interpreted as string terminators used to end a string. If these symbols are not filtered out, they could terminate the API security control filters that may be in place. When you are able to successfully send a null byte it is interpreted by many back-end programming languages as a signifier to stop processing. If the null byte is processed by a back-end program that validates user input, then the security control could be bypassed because it stops processing the following input. Here is a list of potential string terminators you can use:
- %00
- 0x00
- //
- ;
- %
- !
- ?
- []
- %5B%5D
- %09
- %0a
- %0b
- %0c
- %0e
e.g
String terminators can be placed in different parts of the request, like the path or POST body, to attempt to bypass any restrictions in place. For example, in the following injection attack to the user profile endpoint, the null bytes entered into the payload could bypass filtering.
POST /api/v1/user/profile/update
[…]
{ “uname”: “hapihacker”, “pass”: "%00'OR 1=1" }
In this example, input validation measures might be bypassed due to the null byte placed before the SQL injection attempt.
#### Case Switching
If a security control is built around the literal spelling and case of the components within a request, then use case switching. Case switching is the literal switching of the case of the letters within the URL path or payload.
e.g
POST /api/myprofile 
[…] 
{uid=§0001§} 
 An API may leverage rate-limiting to only allow 100 requests per minute. Based on the length of the uid value, you know that to brute force it you’ll need to send 10,000 requests to exhaust all possibilities. To bypass rate-limiting or other WAF controls you may be able to simply alter the URL path by switching upper and lower-case letters in the path:
POST /api/myProfile 
POST /api/MyProfile 
POST /aPi/MypRoFiLe
Each of these path iterations could cause the API provider to handle the request differently, potentially bypassing the rate limit. In some cases, a control like rate-limiting may disappear completely with a different spelling or the new spelling may have its own renewed rate limit. In the case where rate-limiting is no longer applied, you can just send over as many requests as you need to the endpoint with the casing switched. In the case where rate-limiting is renewed, you could use the BurpSuite Pitchfork attack to pair a certain number of attacks to a certain number of brute-force attempts.
For example:
POST /api/myprofile paired with uid 001 - 100
POST /api/Myprofile paired with uid 101 - 200
POST /api/mYprofile paired with uid 201 - 300
If you were using Burp Suite’s Intruder for this attack, you could set the Attack Type to Pitchfork and use the same value for both payload positions. This tactic allows you to use the smallest number of requests required to brute force the uid.
#### Encoding Payloads
To take your WAF-bypassing attempts to the next level, try encoding payloads. Encoded payloads can often trick WAFs while still being processed by the target application or database. Even if the WAF or an input-validation measure blocks certain characters or strings, they might miss encoded versions of those characters. Alternatively, you could also try double encoding your attacks.

Imagine that the API provider has a control in place that provides one round of decoding to requests that are received.

URL Encoded Payload: %27%20%4f%52%20%31%3d%31%3b
API Provider URL Decoder: ' OR 1=1;
WAF Rules detect a fairly obvious SQL Injection attack and block the payload.

Double URL Encoded Payload: %25%32%37%25%32%30%25%34%66%25%35%32%25%32%30%25%33%31%25%33%64%25%33%31%25%33%62
API Provider URL Decoder: %27%20%4f%52%20%31%3d%31%3b
This could perhaps pass by a bad WAF rule to later be decoded and interpreted.
#### Evasion with Wfuzz
Wfuzz also has some great capabilities for payload processing. You can find its payload processing documentation under the advanced section at [https://wfuzz.readthedocs.io/](https://wfuzz.readthedocs.io/).

If you need to encode a payload, you’ll need to know the name of the encoder you want to use. To see a list of all Wfuzz encoders, use the following:
$ wfuzz -e encoders

|          |              |                                                                                                                          |
| -------- | ------------ | ------------------------------------------------------------------------------------------------------------------------ |
| Category | Name         | Summary                                                                                                                  |
| hashes   | base64       | Encodes the given string using base64                                                                                    |
| url      | urlencode    | Replaces special characters in strings using the %xx escape. Letters, digits, and the characters '_.-' are never quoted. |
| default  | random_upper | Replaces random characters in strings with capitals letters                                                              |
| hashes   | md5          | Applies a MD5 hash to the given string                                                                                   |
| default  | none         | Returns all characters without changes                                                                                   |
| default  | hexlify      | Converts every byte of data to its corresponding two-digit hex representation                                            |
Next, to use an encoder, add a comma to the payload and specify its name:
$ wfuzz -z file,wordlist/api/common.txt,base64 http://hapihacker.com/FUZZ
In this example, every payload would be base64-encoded before being sent in a request.

The encoder feature can also be used with multiple encoders. To have a payload processed multiple encoders specify them with a hyphen. For example, if you were to have a single payload “TEST” with the encoding applied like this:  
$ wfuzz -z list,TEST,base64-md5-none
Then you would receive one payload encoded to base64, another payload encoded by MD5, and a final payload in its original form (none=not encoded). This would result in three different payloads.

Note that if you had three payloads, using a hyphen for three encoders would send nine total requests like this:
$ wfuzz -z list,a-b-c,base64-md5-none
In this case, the list of payloads was individually processed three different times. Each letter (a, b, and c)  was base64 encoded, hashed with md5, and sent in the original form.

If, instead, you want each payload to be processed by multiple encoders, separate the encoders with an @ sign:
wfuzz -z list,TEST1,base64@base65@md5
In this example, Wfuzz would first create an MD5 hash of the payload (TEST1) and then base64 encode it twice.

To dive deeper into the topic of WAF bypassing, I recommend checking out the incredible Awesome-WAF GitHub repo ([https://github.com/0xInfection/Awesome-WAF](https://github.com/0xInfection/Awesome-WAF)), where you’ll find a ton of great information.