---
title: OpenBazaar REST API Reference

language_tabs:
  - shell
  - python
  - node

toc_footers:
  - <a href='https://openbazaar.org'>Download OpenBazaar</a>
  - <a href='https://github.com/OpenBazaar/OpenBazaar-Server'>Github Repo for OpenBazaar-Server 1.x</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the OpenBazaar-Server REST API! 

OpenBazaar is a peer-to-peer decentralized marketplace... a fusion of BitTorrent, eBay, and Bitcoin! OpenBazaar the application is split into two components: the client and server. The client accesses all of the server functionality using the REST and websocket APIs. This documentation covers the server component REST API.

## List of API calls

### Authentication

- [x] POST /login 

### Images

- [ ] GET /get_image  
- [ ] POST /upload_image  

### Profile

- [x] GET /profile  
- [ ] POST /profile  
- [ ] POST /social_accounts  
- [ ] DELETE /social_accounts  
- [ ] GET /get_followers  
- [ ] GET /get_following  
- [ ] POST /follow  
- [ ] POST /unfollow  
- [ ] POST /make_moderator  
- [ ] POST /unmake_moderator  

### Settings 

- [ ] POST /settings  
- [ ] GET /settings  
- [ ] GET /shutdown  

### Ordering

- [ ] GET /get_listings  
- [ ] GET /contracts  
- [ ] POST /contracts  
- [ ] DELETE /contracts  
- [ ] POST /purchase_contract  
- [ ] POST /confirm_order  
- [ ] POST /complete_order
- [ ] GET /order_messages  
- [ ] GET /get_ratings  
- [ ] GET /btc_price  
- [ ] POST /refund  
- [ ] GET /sales  
- [ ] GET /get_purchases  
- [ ] POST /check_for_payment  
- [ ] GET /get_order  
- [ ] POST /release_funds  

### Network

- [ ] GET /connected_peers  
- [ ] GET /routing_table  

### Notifications

- [ ] GET /notifications  
- [ ] POST /mark_notification_as_read  

### Dispute resolution

- [ ] POST /dispute_contract  
- [ ] POST /close_dispute  
- [ ] GET /get_cases  

### Chat

- [ ] POST /broadcast  
- [ ] GET /get_chat_messages  
- [ ] GET /get_chat_conversations  
- [ ] DELETE /chat_conversation  
- [ ] POST /mark_chat_message_as_read  
- [ ] POST /mark_discussion_as_read  

# Authentication

> To authorize API calls, first login to the server:

```shell
# With shell, login to the server and retrieve the header containing the Twisted session cookie
curl "http://localhost:18469/api/v1/login"
  --data "username=username&password=password"
  --dump-header header
```

```python
# With requests, login to the server and retrieve the Twisted session cookie
import requests

s = requests.Session()
auth = {'username': 'username', 'password': 'password'}
login = s.post('http://localhost:18469/api/v1/login', data=auth)
# Print the response
print login.json()
# Print the header containing the Twisted session cookie
print login.headers
```

```javascript
var request = require('request');

var login = {
    url: 'http://localhost:18469/api/v1/login',
    method: 'POST',
    form: {username: "username", password: "password"},
    jar: true
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(response.headers);
        console.log(response.body);
    }
}

request(login, callback);
```

> The resulting header file should look something like this:

```shell
HTTP/1.1 200 OK
Date: Thu, 17 Nov 2016 03:56:27 GMT
Content-Length: 17
Content-Type: application/json
Server: TwistedWeb/16.1.0
Set-Cookie: TWISTED_SESSION=0df9b71a84cd850b5723476b7aff0475; Path=/
``` 

```python
# Print of the response
{u'success': True}

# Print of the header
{'Date': 'Thu, 17 Nov 2016 07:04:22 GMT', 'Set-Cookie': 'TWISTED_SESSION=90bc1399cef451d82e7281d5933378e3; Path=/', 'Content-Length': '17', 'Content-Type': 'application/json', 'Server': 'TwistedWeb/16.1.0'}
```

```json
{ "date": "Thu, 17 Nov 2016 12:01:00 GMT",
  "connection": "close",
  "content-type": "application/json",
  "content-length": "17",
  "server": "TwistedWeb/16.1.0",
  "set-cookie": [ "TWISTED_SESSION=9f0bd1fb63a28df702b7cf2e58d52047; Path=/" ] }

{"success": true}
```

OpenBazaar-Server uses session cookies to make authenticated API calls. To obtain the session cookie, you firstly need to login to the server.

`localhost` can be replaced with the IP address of wherever the server component is hosted. Make sure that the server is run with `-a 0.0.0.0` flag to accept API calls from any location (or replace `0.0.0.0` with a specific IP address).

<aside class="notice">
You must replace <code>username</code> and <code>password</code> with the username and password set in the <code>ob.cfg</code> configuration file in OpenBazaar-Server.
</aside>

# Profile

## GET Profile

```shell
curl "http://localhost:18469/api/v1/profile"
  --cookie header
```

```python
import requests

# Open an authenticated session
s = requests.Session()
payload = {'username': 'username', 'password': 'password'}
login = s.post('http://localhost:18469/api/v1/login', data=payload)

# GET /profile
profile = s.get('http://localhost:18469/api/v1/profile')
print profile.json()
```

```javascript
```

> The above call returns JSON structured like this:

```json
{
 "profile": {
  "social_accounts": {
   "twitter": {
    "username": "https://twitter.com/cmosthestore",
    "proof_url": ""
   },
   "instagram": {
    "username": "https://instagram.com/cmosthestore",
    "proof_url": ""
   }
  },
  "moderation_fee": 0.0,
  "moderator": false,
  "nsfw": false,
  "vendor": true,
  "guid": "074265875f569b33b68dbee6834d1c4fd2438fa0",
  "background_color": 3092024,
  "secondary_color": 5591909,
  "location": "UNITED_STATES",
  "short_description": "Simply the best!",
  "primary_color": 7433862,
  "email": "cmos.the.store@gmail.com",
  "website": "",
  "handle": "@cmosthestore",
  "text_color": 16777215,
  "last_modified": 1470754647,
  "public_key": "6a2f42013d03e586ea909743053ca06d6b861d34de6797b3fef45691f973444a",
  "about": "&lt;h1&gt;Cmos the Store&lt;/h1&gt;\n<h2>About Us</h2><p><b>Cmos the Store </b>has a mission to sell you the cheapest 'most awesome' products we can find. We guarantee you will not find better prices anywhere else for the items we sell.</p><p>OpenBazaar is about the freedom of trade and no fees. Our mission is to also associate OpenBazaar with the place you go to get high quality goods at a cheap price you can't find anywhere else.</p><p>All of our transactions are 'moderator-preferred'. We also commit to answer your questions in chat within 12 hours from Monday to Friday, and within 24 hours on the weekend.</p><h3><b>Cat tax</b></h3><p><img src=\"http://www.catgifpage.com/gifs/310.gif\" width=\"400px\"></p><h3>Cena Toll</h3><p><img src=\"https://media.giphy.com/media/Vzku9jyuef09G/giphy.gif\" width=\"400px\"></p>\n<h2>Shop At Our Sister Stores!</h2>\n<p><img src=\"http://i.imgur.com/OLdavrh.png\" width=\"400px\"></p>\n<p><b>Highgarden</b> - The largest collection of women's clothing on OpenBazaar and the lowest prices. ob://b3df09bd79a27f54cbfd007e6237c41884377612/store</p>\n<p><img src=\"http://i.imgur.com/dSyW4y4.jpg\" width=\"400px\"></p>\n<p><b>The Grid</b> - Consumer electronics, accessories, and parts. ob://1b0a60112b919c141d0b89ef4c855432ec218251/store</p>",
  "name": "Cmos the Store",
  "header_hash": "4efb6e41a1a79f6cb1200b1bc177089cdcec5557",
  "pgp_key": "",
  "avatar_hash": "8d1281c49fe1fedfd37beef3454bde30b1aab414"
 }
}
```

This endpoint retrieves the profile information of the node.

### HTTP Request

`GET http://localhost:18469/api/v1/profile`

## GET Profile of a target node

```shell
curl "http://localhost:18469/api/v1/profile?guid=a06aa22a38f0e62221ab74464c311bd88305f88c"
  --cookie header
```

> The above call returns JSON structured like this:

```json
{
    "profile": {
        "social_accounts": {
            "twitter": {
                "username": "@openbazaar", 
                "proof_url": ""
            }, 
            "facebook": {
                "username": "OpenBazaarProject", 
                "proof_url": ""
            }
        }, 
        "moderation_fee": 0.0, 
        "moderator": false, 
        "nsfw": false, 
        "vendor": true, 
        "guid": "a06aa22a38f0e62221ab74464c311bd88305f88c", 
        "background_color": 1846074, 
        "secondary_color": 3362410, 
        "location": "UNITED_STATES", 
        "short_description": "Trade free.", 
        "primary_color": 4285834, 
        "email": "project@openbazaar.org", 
        "website": "https://openbazaar.org/", 
        "handle": "@openbazaar", 
        "text_color": 16777215, 
        "last_modified": 0, 
        "public_key": "42741d5d3a87140c4b850078ec2b625504ee3a711657e42531f5978e4fcf53fd", 
        "about": "<h2>Trade free.</h2><p>OpenBazaar is a decentralized network for trade with no fees and no restrictions.</p><p>Trade happens directly between peers.<i> No middleman</i>.</p><p>If you want to show support for the core developers who built OpenBazaar, check out our products for sale in the store tab.</p>", 
        "name": "OpenBazaar", 
        "header_hash": "5f858909c8304c03d00dd27fe525321cff03c52a", 
        "pgp_key": "", 
        "avatar_hash": "55456e9efbafb5139977d1f86313eaac3293a88b"
    }
}
```

This endpoint retrieves the profile information of a target node on the network.

### HTTP Request

`GET http://localhost:18469/api/v1/profile?guid=<guid>`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
guid | _blank_ | The guid of the target node.

<aside class="notice">
If the <code>guid</code> parameter isn't included in the call, the server will return its own profile infomration!
</aside>

