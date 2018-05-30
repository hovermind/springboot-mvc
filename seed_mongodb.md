## Seed MongoDB
[Generate password](https://www.dailycred.com/article/bcrypt-calculator) and insert into mongodb via cmd or [MongoDB Compass](https://www.mongodb.com/products/compass)
```
use mydb
db.account.insert({"_id" : "aaa", "password": "$2a$10$8W25wHzu28Pi/P3dDE/iUOt2Oqmu5bU4izV8qHYHOaugW1omUpED2",  "roles" : ["USER", "ADMIN"]})
db.account.insert({"_id" : "bbb", "password": "$2a$10$8W25wHzu28Pi/P3dDE/iUOt2Oqmu5bU4izV8qHYHOaugW1omUpED2", "roles" : ["USER", "SUPPORT"]})
```

## MongoDB document class
`MyAccount.java`
```
@Document(collection = "account")
public class MyAccount {

	@Id
	private String id;
	private String password;
	private String[] roles;
	
	// add getter setter
}
```
## MongoRepository (you can use MongoTemplate)
`IMyAccountRepository.java`
```

@Repository("myAccountRepository")
public interface IMyAccountRepository extends MongoRepository<MyAccount, String> {
	 MyAccount findById(String id);
}

```
