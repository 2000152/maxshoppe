**maxshoppe**

1.steps to build an push docker containers to container registry in azure

docker build -t maxshoppe:v1 .

2.create Container registry and login into it

 az acr login --name maxshoppe

 docker tag resbook:v1 maxshoppe.azurecr.io/resbook:v1

 docker push maxshoppe.azurecr.io/resbook:v1

you can run by contianer instance or using web ip, both have been achieved below

container with ip and port https://maxshoppe.azurewebsites.net/

D**atabase connection for maxshoppe**

import the mssql database from azure latest

 docker pull mcr.microsoft.com/mssql/server:2022-latest

create container with same registry as maxshoppe

declare variables in advance settings of the repo image

{
"name": "DB_HOST",
"value": "db_ip", 
"slotSetting": false
},
{ "name": "DB_NAME", 
"value": "mydb",
"slotSetting": false },
{ "name": "DB_PASSWORD",
"value": "Password@123",
"slotSetting": false },
{ "name": "DB_USER",
"value": "sa", 
"slotSetting": false },

you can test you connection with db tools
