
## MongoTemplate

Repositories are more convenient but templates give you more fine-grained control over what to execute.

Generally recommend approach:
 - Start with the repository abstract and just declare simple queries using the query derivation mechanism or manually defined queries.
 - For more complex queries, add manually implemented methods to the repository (as documented here). For the implementation use MongoTemplate.


The mapping between Mongo documents and domain classes is done by delegating to an implementation of the interface MongoConverter. Spring provides two implementations, SimpleMappingConverter(deprecated) and MongoMappingConverter, but you can also write your own converter.
The default converter implementation used by MongoTemplate is MongoMappingConverter.    

**MongoOperations interface has all the methods available to Mongo Collection.**   

**The MongoTemplate class implements the MongoOperations interface.**    

**The preferred way to reference the operations on MongoTemplate instance is via its interface MongoOperations.**    




Details:    
[Introduction to MongoTemplate](https://docs.spring.io/spring-data/data-document/docs/current/reference/html/#mongo-template)    
[MongoTemplate.java API doc](https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/core/MongoTemplate.html)


## Instantiating MongoTemplate

#### Method -1 : extending AbstractMongoConfiguration
```
@Configuration
public class MongoConfig extends AbstractMongoConfiguration {
  
    @Override
    protected String getDatabaseName() {
        return "test";
    }
  
    @Override
    public Mongo mongo() throws Exception {
        return new MongoClient("127.0.0.1", 27017);
    }
  
    @Override
    protected String getMappingBasePackage() {
        return "org.baeldung";
    }
}
```
Note: We didn’t need to define MongoTemplate bean as it’s already defined in [AbstractMongoConfiguration](https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/config/AbstractMongoConfiguration.html)

#### Method - 2: without extending AbstractMongoConfiguration
```
@Configuration
public class SimpleMongoConfig {
  
    @Bean
    public Mongo mongo() throws Exception {
        return new MongoClient("localhost");
    }
 
    @Bean
    public MongoTemplate mongoTemplate() throws Exception {
        return new MongoTemplate(mongo(), "test");
    }
}
```

#### Inserting and retrieving documents using the MongoTemplate
```
import static org.springframework.data.mongodb.core.query.Criteria.where;
import static org.springframework.data.mongodb.core.query.Criteria.query;

...

Person p = new Person("Bob", 33);
mongoTemplate.insert(p);

Person qp = mongoTemplate.findOne(query(where("age").is(33)), Person.class);


```

#### Updating documents using the MongoTemplate
```
import static org.springframework.data.mongodb.core.query.Criteria.where;
import static org.springframework.data.mongodb.core.query.Query;
import static org.springframework.data.mongodb.core.query.Update;

...


WriteResult wr = mongoTemplate.updateMulti(new Query(where("accounts.accountType").is(Account.Type.SAVINGS)),
                                                            new Update().inc("accounts.$.balance", 50.00),
                                                            Account.class);

```

#### Querying for documents using the MongoTemplate
```
import static org.springframework.data.mongodb.core.query.Criteria.where;
import static org.springframework.data.mongodb.core.query.Query.query;

...

List<Person> result = mongoTemplate.find(query(where("age").lt(50).and("accounts.balance").gt(1000.00d)), Person.class);


```




