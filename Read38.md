# Intents
## Intent Filters  
- Filter announces ability of Activity to accept an implicit Intent.
- Filter puts conditions on the Intent that the Activity accepts.
- Declare one or more Intent filters for the Activity in AndroidManifest.xml 
   ```
   <activity android:name="ShareActivity">
   <intent-filter>
   <action android:name="android.intent.action.SEND"/>
   <category android:name="android.intent.category.DEFAULT"/>
   <data android:mimeType="text/plain"/>
   </intent-filter>
   </activity>
   ```
- The system may send a given Intent to an activity if that activity has an intent filter fulfills the following criteria of the Intent object:  
   - action : Match one or more action constants
      ```
      ○ android.intent.action.VIEW — matches any Intent with ACTION_VIEW
      ○ android.intent.action.SEND — matches any Intent with ACTION_SEND
      ```
   - category : Provides an additional way to characterize the activity handling the intent.
      ```
      ○ android.intent.category.BROWSABLE—can be started by web browser
      ○ android.intent.category.LAUNCHER—Show activity as launcher icon
      ```
   - data : Filter on data URIs, MIME type.
      ```
      ○ android:scheme="https"—require URIs to be https protocol
      ○ android:host="developer.android.com"—only accept an Intent from specified hosts
      ○ android:mimeType="text/plain"—limit the acceptable types of documents
      ```

- An Activity can have multiple filters
   ```
   <activity android:name="ShareActivity">
   <intent-filter>
   <action android:name="android.intent.action.SEND"/>
   ...
   </intent-filter>
   <intent-filter>
   <action android:name="android.intent.action.SEND_MULTIPLE"/>
   ...
   </intent-filter>
   </activity>
   ```
- A filter can have multiple actions & data
   ```
   <intent-filter>
   <action android:name="android.intent.action.SEND"/>
   <action android:name="android.intent.action.SEND_MULTIPLE"/>
   <category android:name="android.intent.category.DEFAULT"/>
   <data android:mimeType="image/*"/>
   <data android:mimeType="video/*"/>
   </intent-filter>
   ```
## Intent  
- An Intent is a description of an operation to be performed.
- An Intent is an object used to request an action from another app component via the Android system.  
- What can intents do?  
   - Start an Activity
      - A button click starts a new Activity for text entry
      - Clicking Share opens an app that allows you to post a photo
   - Start an Service  
      - Initiate downloading a file in the background
   - Deliver Broadcast
      - The system informs everybody that the phone is now charging

- There are two types of intents:  
   - Explicit Intent 
      - Starts a specific Activity
   - Implicit Intent 
      - Asks system to find an Activity that can handle this request

- Start an Activity with an explicit intent:
   - To start a specific Activity, use an explicit Intent
      - Create an Intent
         ```
         Intent intent = new Intent(this, ActivityName.class);
         ```
      - Use the Intent to start the Activity
         ```
         startActivity(intent);
         ```

- Start an Activity with implicit intent
   - To ask Android to find an Activity to handle your request, use an implicit Intent
      - Create an Intent
         ```
         Intent intent = new Intent(action, uri);
         ```
      - Use the Intent to start the Activity
         ```
         startActivity(intent);
         ```
- Implicit Intents Examples
   - Show a web page
      ```
      Uri uri = Uri.parse("http://www.google.com"); 
      Intent it = new Intent(Intent.ACTION_VIEW,uri); 
      startActivity(it);
      ```
   - Dial a phone number
      ```
      Uri uri = Uri.parse("tel:8005551234"); 
      Intent it = new Intent(Intent.ACTION_DIAL, uri); 
      startActivity(it);
      ```
- Two types of sending data with intents:
   - Data—one piece of information whose data location can be represented by an URI 
   - Extras—one or more pieces of information as a collection of key-value pairs in a Bundle

- Sending and retrieving data
   - In the first (sending) Activity:
      1. Create the Intent object
      2. Put data or extras into that Intent
      3. Start the new Activity with startActivity()
   - In the second (receiving) Activity: 
      1. Get the Intent object, the Activity was started with
      2. Retrieve the data or extras from the Intent object

- Put information into intent extras
   - putExtra(String name, int value) 
      - intent.putExtra("level", 406);
   - putExtra(String name, String[] value)
      - String[] foodList = {"Rice", "Beans", "Fruit"};
      - intent.putExtra("food", foodList);
   - putExtras(bundle);
      - if lots of data, first create a bundle and pass the bundle.

- Get data from intents
   - getData(); 
      - Uri locationUri = intent.getData();
   - int getIntExtra (String name, int defaultValue)
      - int level = intent.getIntExtra("level", 0);
   - Bundle bundle = intent.getExtras(); 
      - Get all the data at once as a bundle.



 
