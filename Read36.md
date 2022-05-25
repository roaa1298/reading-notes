# Cognito
- Amazon Cognito lets us add user sign-up, sign-in, and access control to our web and mobile apps quickly and easily.  
- Amazon Cognito User Pools provide a secure identity store that scales to millions of users.  
- With Amazon Cognito, your users can sign in through social identity providers such as Apple, Google, Facebook, and Amazon, and through enterprise identity providers such as SAML and OpenID Connect.  
## Authentication  
- The Amplify Auth category provides an interface for authenticating a user.  
- The Amplify CLI helps us to create and configure the auth category with an authentication provider.  
### Configure Auth Category  
- To start provisioning auth resources in the backend, we should go to our project directory and execute the command:

   ```
   amplify add auth
   ```
- Enter the following when prompted:  

   ```
   ? Do you want to use the default authentication and security configuration?
       `Default configuration`
   ? How do you want users to be able to sign in?
       `Username`
   ? Do you want to configure advanced settings?
       `No, I am done.`
   ```
- To push our changes to the cloud, execute the command:  

   ```
   amplify push
   ```
- Upon completion, **amplifyconfiguration.json** should be updated to reference provisioned backend auth resources.  

### Install Amplify Libraries  
- Add the following dependency to our app's build.gradle and click "Sync Now".

   ```
   dependencies {
       implementation 'com.amplifyframework:aws-auth-cognito:1.35.4'
   }
   ```  
### Initialize Amplify Auth 
- Add the Auth plugin before calling Amplify.configure :

   ```
   // Add this line, to include the Auth plugin.
   Amplify.addPlugin(new AWSCognitoAuthPlugin());
   Amplify.configure(getApplicationContext());
   ```  
### Check the current auth session  
- We can now check the current auth session.  

   ```
   Amplify.Auth.fetchAuthSession(
       result -> Log.i("AmplifyQuickstart", result.toString()),
       error -> Log.e("AmplifyQuickstart", error.toString())
   );
   ```  
- The isSignedIn property of the authSession will be false since we haven't signed in to the category yet.  

### Sign in  
- The Auth category can be used to register a user, confirm attributes like email/phone, and sign in with optional multi-factor authentication.  
- The default CLI flow requires a username, password and a valid email id as parameters to register a user.  
- Invoke the following api to initiate a sign up flow.  

   ```
   AuthSignUpOptions options = AuthSignUpOptions.builder()
       .userAttribute(AuthUserAttributeKey.email(), "my@email.com")
       .build();
   Amplify.Auth.signUp("username", "Password123", options,
       result -> Log.i("AuthQuickStart", "Result: " + result.toString()),
       error -> Log.e("AuthQuickStart", "Sign up failed", error)
   );
   
- A confirmation code will be sent to the email id provided during sign up. Enter the confirmation code received via email in the confirmSignUp call.  

   ```
   Amplify.Auth.confirmSignUp(
       "username",
       "the code you received via email",
       result -> Log.i("AuthQuickstart", result.isSignUpComplete() ? "Confirm signUp succeeded" : "Confirm sign up not complete"),
       error -> Log.e("AuthQuickstart", error.toString())
   );
   ```
### Sign in a user  
- Implement a UI to get the username and password from the user.  
- After the user enters the username and password we can start the sign in flow by calling the following method:  

   ```
   Amplify.Auth.signIn(
       "username",
       "password",
       result -> Log.i("AuthQuickstart", result.isSignInComplete() ? "Sign in succeeded" : "Sign in not complete"),
       error -> Log.e("AuthQuickstart", error.toString())
   );
   ```  
- if the sign in flow is completed successfully, then we have now registered a user and authenticated with that user's username and password with Amplify.  

## Resources 
- [Authentication](https://docs.amplify.aws/lib/auth/getting-started/q/platform/android/)
   
