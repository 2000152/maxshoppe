# maxshoppe


steps to build an push docker containers to container registry in azure


docker build -t maxshoppe:v1 .

create Container registry and login into it

az acr login --name maxshoppe





docker tag resbook:v1 maxshoppe.azurecr.io/resbook:v1

docker push maxshoppe.azurecr.io/resbook:v1



Deploy using Web app of the image publish or up the container

container with ip and port 
https://maxshoppe.azurewebsites.net/

Database connection for maxshoppe


import the mssql database from azure latest

docker pull mcr.microsoft.com/mssql/server:2022-latest

create container with same registry as  maxshoppe


declare variables in advance settings of the repo image

{
"name": "DB_HOST",
"value": "db_ip",
"slotSetting": false
},
{
"name": "DB_NAME",
"value": "mydb",
"slotSetting": false
},
{
"name": "DB_PASSWORD",
"value": "Admin@1234",
"slotSetting": false
},
{
"name": "DB_USER",
"value": "sa",
"slotSetting": false
},


and publish and create instance from the image






