# Hibernate - get vs load method

#### Scenario:
* Retreive a row in the users table
```
select * from users where id = 101;
```

#### Get Method
* Get method always hit database.
* Get method never returns a proxy, it either returns null or fully initialized Object.
* Use get method to determine if an instance exists or not because it can return null if instance doesn’t exists in cache and database.
```
int userId = 101;
User user = (User) session.get(User.class, userId);
```

#### Load Method
* load() method may not always hit the database, depending upon which method is called.
* load() method may return proxy, which is the object with ID but without initializing other properties, which is lazily initialized.
* Use load method to retrieve instance only if you think that instance should exists and non availability is an error condition.
* Throws "ObjectNotFoundException" if userId "101" doesn't exists in User table.
```
int userId = 101;
User user = (User) session.load(User.class, userId);
```


#### Performance - Get vs Load
* get() method could suffer performance penalty if only identifier method like getId()  is accessed. 
* So consider using load method if  your code doesn't access any method other than identifier or you are OK with lazy initialization of object, if persistent object is not in Session Cache because load() can return proxy.



#### Example:
```
package com.naresh.bankingapp.dao;

import org.hibernate.Session;
import org.hibernate.SessionFactory;

import com.naresh.bankingapp.dao.impl.UserDAOImpl;
import com.naresh.bankingapp.model.User;
import com.naresh.bankingapp.util.HibernateUtil;

public class TestUserGetLoad {

	static UserDAO userDAO = new UserDAOImpl();

	static SessionFactory sf = HibernateUtil.getSessionFactory();

	public static void main(String[] args) {

		int userId = 9;
		testLoad(userId);
		testGet(userId);
	}

	private static void testLoad(int userId) {
		System.out.println("Test Load");
		Session session = sf.openSession();
		User user = session.load(User.class, userId);
		System.out.println(user);
		//System.out.println(user.getId());
		session.close();
	}
	
	private static void testGet(int userId) {
		System.out.println("Test get");
		Session session = sf.openSession();
		User user = session.get(User.class, userId);
		System.out.println(user);
		session.close();
	}

}
```
