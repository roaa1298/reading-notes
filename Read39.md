# Location
- Using the Google Play services location APIs, our app can request the last known location of the user's device.  
- We use the fused location provider to retrieve the device's last known location.
- The fused location provider is one of the location APIs in Google Play services. It manages the underlying location technology and provides a simple API so that you can specify requirements at a high level, like high accuracy or low power. It also optimizes the device's use of battery power.  
- We will make a single request for the location of a device using the **getLastLocation()** method in the fused location provider. 

## Set up Google Play services  
- We have to download and install the Google Play services component by using the SDK Manager to access the fused location provider, and add the library to our project.

   ```
   implementation 'com.google.android.gms:play-services-location:19.0.1'
   ```   
- Specify app permissions
- To protect user privacy, apps that use location services must request location permissions.  
- The system includes multiple permissions related to location. Which permissions you request, and how you request them, depend on the location requirements for your app's use case.  

## Create location services client  
- In our activity we will create an instance of the Fused Location Provider Client.  

   ```
   private FusedLocationProviderClient fusedLocationClient;

   // ..

   @Override
   protected void onCreate(Bundle savedInstanceState) {
       // ...

       fusedLocationClient = LocationServices.getFusedLocationProviderClient(this);
   }
   ```
- Once we have created the Location Services client you can get the last known location of a user's device.  

## Get the last known location  
- When our app is connected to these you can use the fused location provider's getLastLocation() method to retrieve the device location.
- The precision of the location returned by this call is determined by the permission setting we put in our app manifest.   

   ```
   fusedLocationClient.getLastLocation()
           .addOnSuccessListener(this, new OnSuccessListener<Location>() {
               @Override
               public void onSuccess(Location location) {
                   // Got last known location. In some rare situations this can be null.
                   if (location != null) {
                       // Logic to handle location object
                   }
               }
           });
           ```
- The getLastLocation() method returns a Task that you can use to get a Location object with the latitude and longitude coordinates of a geographic location.  
- The location object may be null in the following situations:
   - Location is turned off in the device settings.
   - The device never recorded its location (new device or a device that has been restored to factory settings).
   - Google Play services on the device has restarted.

## Choose the best location estimate  
- getLastLocation() gets a location estimate more quickly and minimizes battery usage that can be attributed to your app.  
- getCurrentLocation() gets a fresher, more accurate location more consistently. 

## Resourses
- [Location](https://developer.android.com/training/location/retrieve-current)

