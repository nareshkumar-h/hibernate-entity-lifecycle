# Hibernate Entity Lifecycle

#### Different States of an Object
Given an instance of an object that is mapped to Hibernate, it can be in any one of four different states:
* Transient
* Managed
* Detached
* Removed

#### 1. Transient Object
* Transient objects exist in heap memory. 
* Hibernate does not manage transient objects or persist changes to transient objects.
* To persist the changes to a transient object, you would have to ask the session to save the transient object to the database, at which point Hibernate assigns the object an identifier and marks the object as being in persistent state.

* Example - Transient Object
```
User user = new User();
user.setName("Naresh");
user.setEmail("nareshkumarh@live.com");
user.setPassword("pass123");
```


#### 2. Persistent Object
* Persistent objects exist in the database, and Hibernate manages the persistence for persistent objects.
* If fields or properties change on a persistent object, Hibernate will keep the database representation up to date when the application marks the changes as to be committed.
```
session.save(user);
```

#### 3. Detached Object
* Detached objects have a representation in the database, but changes to the object will not be reflected in the database, and vice-versa
* A detached object can be created by closing the session that it was associated with, or by evicting it from the session with a call to the session's evict() method.

```
session.save(user);
session.close();
```

#### 3.1 Detached Object to Persistent Object
* In order to persist changes made to a detached object, the application must reattach it to a valid Hibernate session. 
* A detached instance can be associated with a new Hibernate session when your application calls one of the load, refresh, merge, update(), or save() methods on the new session with a reference to the detached object. 
* After the call, the detached object would be a persistent object managed by the new Hibernate session.

#### 4. Removed Object
* Removed objects are objects that are being managed by Hibernate (persistent objects, in other words) that have been passed to the session’s remove() method. 
* When the application marks the changes held in the session as to be committed, the entries in the database that correspond to removed objects are deleted.
```
session.remove(user);
```


#### Entity Lifecycle Diagram

![alt Entity Lifecycle](Entity-LifeCycle-State-tiny.png)



#### Summary
* Newly created POJO object will be in the transient state. Transient object doesn’t represent any row of the database i.e. not associated with any session object. It’s plain simple java object.
* Persistent object represent one row of the database and always associated with some unique hibernate session. Changes to persistent objects are tracked by hibernate and are saved into database when commit call happen.
* Detached objects are those who were once persistent in past, and now they are no longer persistent. To persist changes done in detached objects, you must reattach them to hibernate session.
* Removed objects are persistent objects that have been passed to the session’s remove() method and soon will be deleted as soon as changes held in the session will be committed to database.
