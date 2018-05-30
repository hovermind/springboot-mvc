## Create bat to start mongo service
Required: 
* MongoDB location
* MongoDB config location

`start_mongo_service.bat`
```
start cmd.exe /k "C:\MongoDB location\bin\mongod.exe -f C:\MongoDB config location\mongod.cfg"
```
You can use java CommandLine

## Create mongodb collection
run `start_mongo_service.bat` & open `cmd` (or you can use [MongoDB Compass](https://www.mongodb.com/products/compass))
```
mongo
use mydb
db.createColletion("account")
```

## Seed MongoDB
[Generate BCrypt password](https://www.dailycred.com/article/bcrypt-calculator) and insert into mongodb via cmd or [MongoDB Compass](https://www.mongodb.com/products/compass)
```
use mydb
db.account.insert({"_id" : "aaa", "password": "$2a$10$8W25wHzu28Pi/P3dDE/iUOt2Oqmu5bU4izV8qHYHOaugW1omUpED2",  "roles" : ["USER", "ADMIN"]})
db.account.insert({"_id" : "bbb", "password": "$2a$10$8W25wHzu28Pi/P3dDE/iUOt2Oqmu5bU4izV8qHYHOaugW1omUpED2", "roles" : ["USER", "SUPPORT"]})
```
