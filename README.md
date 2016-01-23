# Instructions

### Prerequisites 
* Download HCP SDK:
  **https://tools.hana.ondemand.com/sdk/neo-javaee6-wp-sdk-2.75.8.4.zip**
* Extract into a local folder
```

###############################
# Install local server
###############################
NEO=/Users/maslick/Apps/FRI/opt/neo-javaee6-wp-sdk-2.75.8.4/tools/neo.sh
mkdir server
$NEO install-local -l server/
```

### Deploying to a local server 
```
###############################
# Start/stop server
###############################
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

### Deploying to HANA cloud
* Create a file named ``weatherapp.properties``:
```
################################################
# General settings - relevant for all commands #
################################################

# Your account name
account=p1941753077trial

# Application name
application=weatherapi

# User for login to hana.ondemand.com.
user=pavel.maslov@cloud.si

# URL of the landscape admin server. Optional. Defaults to hana.ondemand.com.
host=hanatrial.ondemand.com

#################################################################
# Deployment descriptor settings - relevant only for deployment #
#################################################################

# List of file system paths to *.war files and folders containing them
source=target/weatherapp.war
```

* In order to deploy the war file, run: 
```
mvn clean package
$NEO deploy weatherapp.properties
```
This will deploy the war file to HANA cloud

* Create a file named ``destinations``
```
Name=openweathermap-destination
URL=http://api.openweathermap.org/data/2.5/weather?APPID=efd5003f7f5485a8976587dc0c2898ae
ProxyType=Internet
Type=HTTP
Authentication=NoAuthentication
CloudConnectorVersion = 2
```

* Run this to upload destinations to HANA cloud:
```
$NEO put-destination -h hanatrial.ondemand.com -u pavel.maslov@cloud.si -a p1941753077trial --localpath destinations
```

* Start the app:
```
$NEO start weatherapp.properties
```
