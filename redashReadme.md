# Uptycs connection and Redash

Redash Version 7.0.0 onwards now supports Uptycs as a data source.
For more information on Redash, following is a link:
https://redash.io

Redash 7.0.0. detailed change log is given in following link:
https://github.com/getredash/redash/blob/master/CHANGELOG.md

For setting up Redash, please refer following link:
https://redash.io/help/open-source/setup


## Using Uptycs data source in Redash

For using uptycs data source, user need to download an api key from Uptycs.

### Download the API key from uptyc. 

Following are the steps for downloading API key:
 1. Connect to you tenant using your username and password. Uptycs URL example: https://<tenant>.uptycs.io
 2. Got to configuration -> Users
 3. Go to your username click on Edit 
 4. Create and Dowload "User API key"
 5. API key will have following JSON informaton:
 ```
 cat allinone_apikey.json 
{
  "domain": "allinone",
  "userId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  "active": true,
  "updatedAt": "2019-02-14T21:10:37.826Z",
  "secret": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  "customerId": "CUSTOMERIDXXXXXXXXXXXXXXX",
  "id": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  "createdAt": "2019-02-14T21:10:37.826Z",
  "key": KEYXXXXXXXXX"
}
```

From above JSON, please note save the customerId, secret and Key.

