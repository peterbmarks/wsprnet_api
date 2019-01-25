# wsprnet_api - Documentation for using the API

At this point in time the API is invite only. Please send me a note if you want to be included. There are some infrastructure concerns that we need  to deal with before we open this up.

# Endpoints
Three endpoints are provided. One for spots and the second for statuses. A status record is the frequency, TX % data you see on the map when the station has no spots and comes from the WSPRNet client each user runs.

* `http://wsprnet.org/wsprnet/drupal/spots/json`
* `http://wsprnet.org/wsprnet/drupal/status/json`
* `http://wsprnet.org/drupal/wsprnet/paths/json` - summary data suitable for a map

You POST to these endpoints with the data containing json with parameters.

Parameters are similar to those on the database page form:

* callsign - transmitting callsign
* reporter - reporting callsign
* band - band code string
* unique - report one of each spot
* minutes - number of minutes back to report
* spotnum_start - for paging, start at a given spot sequence number
 
Spots are detailed, so same path and stations at different times will be in there. 
 
# Session Management
 
In order to access the endpoints the following has been created:

## 1. Login 
 
`POST http://wsprnet.org/drupal/rest/user/login`

Header:

`Content-Type: application/json`
 
Body:
 
`{`
`"name": "wsprnet_login"`
`"pass": "wsprnet_pass"`
`}`
 
 
## 2. Sesssion Cookie
 
Login will return a JSON body. In it find these properties:
 
`"sessid":"e8T0xDx-FkgT-Cwd6FPjdWaZqGxi8GXLFm1rPdSWI9Q"`
`"session_name":"SESS70f94c916a4e1b4938c6d4158a067062"`
`"token":"DUDx6gkw7UymmLxarDQjgTnjJ48LtTCqgqAsaT36YuF"`
 
Going forward add the header:
 
`Cookie: SESS70f94c916a4e1b4938c6d4158a067062=e8T0xDx-FkgT-Cwd6FPjdWaZqGxi8GXLFm1rPdSWI9Q`
 
i.e. Cookie: {sesssion_name}={sessid}
 
## 3. Logout
 
`GET http://wsprnet.org/drupal/services/session/token`

With your Cookie header from 2.
 
Returns a single token. Then:
 
`POST http://wsprnet.org/drupal/rest/user/logout.json`
 
Headers
 
`X-CSRF-Token: {token from above} `


`Cookie:{from step 2} Content-Type:application/json`
