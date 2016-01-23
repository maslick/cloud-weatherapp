# Instructions

### Prerequisites 
* Download HCP SDK:
  **https://tools.hana.ondemand.com/sdk/neo-javaee6-wp-sdk-2.75.8.4.zip**

```
###############################
# Install local server
###############################
NEO=/Users/maslick/Apps/FRI/opt/neo-javaee6-wp-sdk-2.75.8.4/tools/neo.sh
mkdir server
$NEO install-local -l server/
$NEO start-local -l server/
$NEO stop-local
```

```
###############################
# Deploy war to a local server
###############################
mvn clean package
$NEO deploy-local -s target/{war}.war -l server/
```

```
###############################
# Add local users
###############################
Edit {server}/config_master/com.sap.security.um.provider.neo.local/neousers.json

  {
    "Users": [
      {
        "UID": "test",
        "Password": "{SSHA}M8HLn42FcULAoa2TqlNnHH/sDbwrGHgW",
        "Roles": [
          "Employee",
          "Manager"
        ],
        "Attributes": [
          {
            "attributeName": "firstname",
            "attributeValue": "Pavel"
          },
          {
            "attributeName": "lastname",
            "attributeValue": "Maslov"
          },
          {
            "attributeName": "email",
            "attributeValue": "pavel.masloff@gmail.com"
          }
        ]
      }
    ]
  }
```

```
###############################
# Create local destinations
###############################
Edit {server}/config_master/service.destinations/destinations/openweathermap-destination

  URL=http://api.openweathermap.org/data/2.5/weather?APPID=efd5003f7f5485a8976587dc0c2898ae
  Name=openweathermap-destination
  ProxyType=Internet
  Type=HTTP
  Password=
  Authentication=NoAuthentication
  User=
  
```
