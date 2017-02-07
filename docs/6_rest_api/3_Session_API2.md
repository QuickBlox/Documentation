<span id="Signature_generation" class="on_page_navigation"></span>
# Signature generation
To authenticate your application you have to set valid a **auth_key** and generate a **signature** using your application **auth_secret**. Then pass generated **signature** in [Create Session request](3_Session_API.md) to obtain session token.

Signature is the [HMAC-SHA](http://en.wikipedia.org/wiki/HMAC) function of the body of the request, with a key **auth_secret**. Request body is formed as the sorted (sorting alphabetically, as symbols, not as bytes) by increase the string array 'parameter=value', separated with the symbol "&". For the parameters passed as a **user[login]=amigo30** is used just such a line of **user[login]=amigo30**.

Example of body: 
```
application_id=22&auth_key=wJHd4cQSxpQGWx5&nonce=33432&timestamp=1326966962
```

Example of body with user: 
```
application_id=22&auth_key=wJHd4cQSxpQGWx5&nonce=33432&timestamp=1326966962&user[login]=amigo30&user[password]=amigo30pass
```

Example of body with social token(Facebook): 
```
application_id=22&auth_key=wJHd4cQSxpQGWx5&keys[token]=AM46dxjhisdffgry26282352fdusdfusdfgsdf&nonce=33432&provider=facebook&timestamp=1326966962
```

<span id="Examples" class="on_page_navigation"></span>
# Examples
We collect examples of session creation request in different languages (not covered by QuickBlox SDKs) to help our users easily start QuickBlox integration:

* [Python](http://stackoverflow.com/questions/17962051/authentication-and-getting-a-session-token-from-quickblox-in-python)
* [PHP](http://stackoverflow.com/questions/14108986/how-to-generate-quickblox-authentication-signature-in-php)
* [Pascal](http://stackoverflow.com/questions/30477351/how-to-create-a-quickblox-session-in-pascal/30477396)