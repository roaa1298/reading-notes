# Amazon S3  
- Amazon S3 is an object storage service that offers industry-leading scalability, data availability, security, and performance.  
- Amazon S3 stores and protects any amount of data for a range of use cases, such as (data lakes, websites, mobile applications, backup and restore, archive, enterprise applications, IoT devices, and big data analytics). 
- Amazon S3 is an object storage service that stores data as objects within buckets. An object is a file and any metadata that describes the file. A bucket is a container for objects.    
- To store your data in Amazon S3, you first create a bucket and specify a bucket name and AWS Region. Then, you upload your data to that bucket as objects in Amazon S3. Each object has a key (or key name), which is the unique identifier for the object within the bucket.  

## Features of Amazon S3  
- **Storage classes** : Amazon S3 offers a range of storage classes designed for different use cases.  
- **Storage management** : Amazon S3 has storage management features that you can use to manage costs, meet regulatory requirements, reduce latency, and save multiple distinct copies of your data for compliance requirements.  
- **Access management** : Amazon S3 provides features for auditing and managing access to your buckets and objects. By default, S3 buckets and the objects in them are private. You have access only to the S3 resources that you create.  
- **Data processing** : To transform data and trigger workflows to automate a variety of other processing activities at scale.  
- **Storage logging and monitoring** : Amazon S3 provides logging and monitoring tools that you can use to monitor and control how your Amazon S3 resources are being used.  
- **Analytics and insights** : Amazon S3 offers features to help you gain visibility into your storage usage, which empowers you to better understand, analyze, and optimize your storage at scale.  
- **Strong consistency** : Amazon S3 provides strong read-after-write consistency for PUT and DELETE requests of objects in your Amazon S3 bucket in all AWS Regions.  

## configure your application with Amplify Storage  
### Provision backend storage  
- To provisioning storage resources in the backend, execute this command:

   ```
   amplify add storage 
   ```
- To push your changes to the cloud, execute this command: 

   ```
   amplify push
   ```
- Upon completion, amplifyconfiguration.json will be updated to reference a newly provisioned S3 bucket.

### Install Amplify Libraries  
- Add these libraries into the dependencies block in the build.gradle (Module: app) file:  

   ```
   dependencies {
       implementation 'com.amplifyframework:aws-storage-s3:1.35.4'
       implementation 'com.amplifyframework:aws-auth-cognito:1.35.4'
   }
   ```
### Initialize Amplify Storage  
- Add the following code to your onCreate() method in your application class:

   ```
   Amplify.addPlugin(new AWSCognitoAuthPlugin());
   Amplify.addPlugin(new AWSS3StoragePlugin());
   ```
- To initialize the Amplify Auth and Storage categories you call Amplify.addPlugin() method for each category. To complete initialization call Amplify.configure().  

### Uploading data to your bucket  
- To upload to S3 from a data object, specify the key and the data object to be uploaded. 
 
   ``` 
   private void uploadFile() {
       File exampleFile = new File(getApplicationContext().getFilesDir(), "ExampleKey");

       try {
           BufferedWriter writer = new BufferedWriter(new FileWriter(exampleFile));
           writer.append("Example file contents");
           writer.close();
       } catch (Exception exception) {
           Log.e("MyAmplifyApp", "Upload failed", exception);
       }

       Amplify.Storage.uploadFile(
               "ExampleKey",
               exampleFile,
               result -> Log.i("MyAmplifyApp", "Successfully uploaded: " + result.getKey()),
               storageFailure -> Log.e("MyAmplifyApp", "Upload failed", storageFailure)
       );
   }
   ```
- Upon successfully executing this code, you should see a new folder in your bucket, called public. It should contain a file called ExampleKey, whose contents is Example file contents.  

## Resources:  
- [storage](https://docs.amplify.aws/lib/storage/getting-started/q/platform/android/)
- [Introduction to Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html)
