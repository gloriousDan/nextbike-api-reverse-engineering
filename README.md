# nextbike-api-reverse-engineering

This API documentation is what I got from reverse engineering the nextcloud app. It's mainly based on the earlier webview based version (pre-4.x) but so far it looks like the latest app uses the same API calls.

## Base URL
* Base URL: https://api.nextbike.net
* Webview URL; https://webview.nextbike.net

## Obtaing API key
It seems as they use hardcoded API keys in their new, (probably) native app. In the old webview based app, the endpoint `https://webview.nextbike.net/getAPIKey.json` returns a key that is always the same. Maybe they change it when bumping versions in the app or regularly in the webview endpoint.

You can either get the the api key via the endpoint or use the hardcoded one.

* **Keys**

    * https://webview.nextbike.net/getAPIKey.json
    * from binary: `rXXqTgQZUPZ89lzB`

## Get API Key
----
Returns API key as JSON

* **Full URL**

  https://webview.nextbike.net/getAPIKey.json

* **Method:**

  `GET`

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ "apiKey" : "-key-" }`

* **Sample Call:**

  `curl https://webview.nextbike.net/getAPIKey.json`

## Login
---
Logs user in and returns user info and server time

* **URL**

    /api/login.json

* **Method:**

    `POST`

* **Form Data Params**

    * `apikey=[string]`
    * `mobile=[integer]`
    * `pin=[integer]`
    * `show_errors=[integer]`

* **Success Response:**

  * **Code:** 200 <br />
  *  **Content:** 
    ```JSON
    {
    "server_time":1543956595,
    "user":{
        "mobile":"<mobile>",
        "loginkey":"<loginkey>",
        "rfid_uids":[ "" ],
        "active":true,
        "lang":"EN",
        "domain":"de",
        "currency":"EUR",
        "credits":0,
        "free_seconds":0,
        "ticket_ids":[ ],
        "subscriptions":[ ],
        "partner_ids":[ ],
        "partners":[ ],
        "payment":"pp",
        "team_owner":false,
        "team_id":0,
        "max_bikes":4,
        "screen_name":"<user's name>",
        "newsletter":false,
        "accepted_terms":[ "de" ],
        "needs_email_verification":false,
        "required_actions":[ ],
        "is_vcn_active":true,
        "countRecoverPin":"0"
        }
    }
    ```
* **Error Response:**

    * **Code:** 404 <br />
    * **Content:** `{"server_time":1543957338,"error":{"code":1,"message":"user not found or login failed"}}`

* **Sample Call:**

    `curl -X POST -F 'mobile=023421337' -F 'pin=666421' -F 'apikey=rXXqTgQZUPZ89lzB' -F 'show_errors=1' https://api.nextbike.net/api/login.json`

## Rent bike
---
Starts rental of bike and returns bike data

* **URL**

    /api/rent.json

* **Method:**

    `POST`

* **Form Data Params**

    * `apikey=[string]`
    * `bike=[integer]`
    * `loginkey=[string]`
    * `show_errors: [integer]`

* **Success Response:**

  * **Code:** 200 <br />
  *  **Content:** 
    ```JSON
    {
        "server_time":1543959258,
        "rental":{
            "id":63446297,
            "show_close_lock_info":false,
            "invalid":false,
            "return_via_app":false,
            "return_to_official_only":true,
            "city_id":7,
            "city":"Wiesbaden",
            "domain":"de",
            "bike":"<bike number>",
            "boardcomputer":22604,
            "customer_comment":"",
            "electric_lock":true,
            "biketype":15,
            "lock_types":[ "fork_lock" ],
            "code":"<lock code>",
            "start_place":4728239,
            "start_place_lat":50.050828206632,
            "start_place_lng":8.2480899381638,
            "start_place_name":"Kurt-Schumacher-Ring (Campus Ost)",
            "start_rack":0,
            "end_place":0,
            "end_place_lat":0,
            "end_place_lng":0,
            "end_place_name":null,
            "start_time":1543959257,
            "end_time":0,
            "price":100,
            "price_service":0,
            "rfid_uid":0,
            "review_state":0,
            "trip_type":0,
            "app_campaign_picture":"",
            "app_campaign_picture_link":"",
            "app_campaign_uid":0,
            "ad4x20":null,
            "break":false,
            "rating":0,
            "customer_rfids":[ ],
            "gps_tracking":false
        }
    }
    ```

* **Error Response:**

    * **Code:** 404 <br />
    * **Content:** `{"server_time":1543959093,"error":{"code":2,"message":"bike %s not found"}}`

* **Sample Call:**

    `curl -X POST -F 'apikey=rXXqTgQZUPZ89lzB' -F 'bike=<bike number>' -F 'loginkey=<loginkey>' -F 'show_errors=1' https://api.nextbike.net/api/rent.json`

## Return Bike
---
Ends rental at given station

* **URL**

    /api/return.json

* **Method:**

    `POST`

* **Form Data Params**

    * `apikey=[string]`
    * `loginkey=[string]`
    * `bike=[integer]`
    * `comment: [string]` *note: for experience*
    * `place: [integer]` *note: arrival station*
    * `show_errors: [integer]`

* **Success Response:**

  * **Code:** 200 <br />
  *  **Content:** 
    ```JSON
    {
        "server_time":1543961916,
        "rental":{
            "id":63446718,
            "show_close_lock_info":false,
            "invalid":true,
            "return_via_app":false,
            "return_to_official_only":true,
            "city_id":7,
            "city":"Wiesbaden",
            "domain":"de",
            "bike":"<bike number>",
            "boardcomputer":22604,
            "customer_comment":"",
            "electric_lock":true,
            "biketype":15,
            "lock_types":[ "fork_lock" ],
            "code":"<lock code>",
            "start_place":4728239,            "start_place_lat":50.050828206632,
            "start_place_lng":8.2480899381638,
            "start_place_name":"Kurt-Schumacher-Ring (Campus Ost)",
            "start_rack":0,
            "end_place":4728239,
            "end_place_lat":50.050828206632,
            "end_place_lng":8.2480899381638,
            "end_place_name":"Kurt-Schumacher-Ring (Campus Ost)",
            "start_time":1543961901,
            "end_time":1543961916,
            "price":100,
            "price_service":0,
            "rfid_uid":0,
            "review_state":0,
            "trip_type":0,
            "app_campaign_picture":"",
            "app_campaign_picture_link":"",
            "app_campaign_uid":0,
            "ad4x20":null,
            "break":false,
            "rating":0
        }
    }
    ```
* **Sample Call:**

    `curl -X POST -F 'apikey=rXXqTgQZUPZ89lzB' -F 'bike=<bike number>' -F 'loginkey=<loginkey>' -F 'comment=Lock+broken' -F 'place=<station number>' -F 'show_errors=1' https://api.nextbike.net/api/rent.json`

## Map Discovery
---
Map endpoint has list of all the bikes

* **URL**

    /maps/nextbike-official.json

* **Method:**

    `GET`

*  **URL Params**

   **Optional:**
 
   `countries=[csv]`

* **Sample Call:**

    `curl https://api.nextbike.net/maps/nextbike-official.json?countries=pl,de`