# Room
- Room provides an abstract layer over the SQLite Database to save and perform the operations on persistent data locally.  
### Difference Between Room and SQLite  
| Room      | SQLite  |
| ----------- | ----------- |
| No need of writing raw queries.      | Need to write raw queries.       |
| Compile Time verification of SQL queries.   | No, compile-time boilerplate verification of SQL queries.        |
| No need of converting the Data to Java Objects. As Room internally maps the Database objects to Java objects.      | Need to write SQL queries to convert the Data to Java Objects.       |
| This conveniently supports the integration with other Architecture components.   | This needs a lot of boiler plate code to integrate with the other Architecture Components.        |
| Room provides easier way to work with LiveData and perform the operations.      | SQLite doesn’t provide a direct way to access the LiveData, it needs external code to be written to access the LiveData.       |
| There is no need to change the code when the database schema gets changes.   | This needs to change its queries whenever the database schema gets changes        |

### Primary Components of Room  
- Data Entities: This Represents all the tables from the existing database. And annotated with @Entity.  
- DAO (Data Access Objects): This contains methods to perform the operations on the database. And annotated with @Dao.  
- Database Class: This provides the main access point to the underlying connection for the application’s persisted data. And this is annotated with @Database.  

### Room Library Architecture  
- The application first gets the Data Access Objects (DAOs) associated with the existing Room Database.  
- After getting DAOs, through DAOs it accesses the entities from the Database Tables.   
- And then it can perform the operations on those entities and persist back the changes to the Database.  

![room-architecture](images/room_architecture.png)  

### Steps to implement Room Database in Android Application  
- **Adding the required dependencies**   

   ```
   dependencies {
       def room_version = "2.4.2"

       implementation "androidx.room:room-runtime:$room_version"
       annotationProcessor "androidx.room:room-compiler:$room_version"
   }
   ```  
- **Creating Data Entity**  

   ```
   @Entity
   public class User {
       @PrimaryKey
       public int uid;

       @ColumnInfo(name = "first_name")
       public String firstName;

       @ColumnInfo(name = "last_name")
       public String lastName;
   }
   ```  
- **Creating Data Access Objects (DAOs)**  
   - The following code defines a DAO called UserDao. UserDao provides the methods that the rest of the app uses to interact with data in the user table.   
   
      ```
      @Dao
      public interface UserDao {
          @Query("SELECT * FROM user")
          List<User> getAll();

          @Query("SELECT * FROM user WHERE uid IN (:userIds)")
          List<User> loadAllByIds(int[] userIds);

          @Query("SELECT * FROM user WHERE first_name LIKE :first AND " +
                 "last_name LIKE :last LIMIT 1")
          User findByName(String first, String last);

          @Insert
          void insertAll(User... users);

          @Delete
          void delete(User user);
      }
      ```  
- **Creating the Database**   
   - Now creating the Database which defines the actual application’s database, which is the main access point to the application’s persisted data.    
   - This class must satisfy:  
      - The class must be abstract.  
      - The class should be annotated with @Database.  
      - Database class must define an abstract method with zero arguments and returns an instance of DAO.  
      
   ```
   @Database(entities = {User.class}, version = 1)
   public abstract class AppDatabase extends RoomDatabase {
       public abstract UserDao userDao();
   }
   ```
- **Usage of the Room database**  
   - Inside the MainActivity file we can create a database, by providing custom names for the database.  
      - get the instance of the application's database
      
         ```
         AppDatabase db = Room.databaseBuilder(getApplicationContext(),
                 AppDatabase.class, "database-name").build();
         ```  
      - create instance of DAO to access the entities  
      
         ```
         UserDao userDao = db.userDao();
         ```  
      - using the same DAO perform the Database operations  
         ```
         List<User> users = userDao.getAll();
         ```  
         

## Resources  
- [Rooms_Android_Developers](https://developer.android.com/training/data-storage/room)
- [Rooms_geeksforgeeks](https://www.geeksforgeeks.org/overview-of-room-in-android-architecture-components/)





