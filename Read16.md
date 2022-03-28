# Spring Security Architecture  
## Authentication and Access Control  
- Application security boils down to two more or less independent problems: authentication (who are you?) and authorization (what are you allowed to do?). Sometimes people say “access control” instead of "authorization", which can get confusing, but it can be helpful to think of it that way because “authorization” is overloaded in other places. Spring Security has an architecture that is designed to separate authentication from authorization and has strategies and extension points for both.  
### Authentication  
- The main strategy interface for authentication is **AuthenticationManager**, which has only one method:  
   ```
   public interface AuthenticationManager {

     Authentication authenticate(Authentication authentication)
       throws AuthenticationException;
   }
   ```
- An **AuthenticationManager** can do one of 3 things in its **authenticate()** method:   
   - Return an **Authentication** (normally with **authenticated=true**) if it can verify that the input represents a valid principal.  
   - Throw an **AuthenticationException** if it believes that the input represents an invalid principal.  
   - Return **null** if it cannot decide.  

- **AuthenticationException** is a runtime exception. It is usually handled by an application in a generic way, depending on the style or purpose of the application. In other words, user code is not normally expected to catch and handle it. For example, a web UI might render a page that says that the authentication failed, and a backend HTTP service might send a 401 response, with or without a **WWW-Authenticate** header depending on the context.  
- The most commonly used implementation of AuthenticationManager is ProviderManager, which delegates to a chain of AuthenticationProvider instances. An AuthenticationProvider is a bit like an AuthenticationManager, but it has an extra method to allow the caller to query whether it supports a given Authentication type:  
   ```
   public interface AuthenticationProvider {

	   Authentication authenticate(Authentication authentication)
			   throws AuthenticationException;

	   boolean supports(Class<?> authentication);
   }
   ```
- The Class<?> argument in the supports() method is really Class<? extends Authentication> (it is only ever asked if it supports something that is passed into the authenticate() method). A ProviderManager can support multiple different authentication mechanisms in the same application by delegating to a chain of AuthenticationProviders. If a ProviderManager does not recognize a particular Authentication instance type, it is skipped.
- A ProviderManager has an optional parent, which it can consult if all providers return null. If the parent is not available, a null Authentication results in an AuthenticationException.
- Sometimes, an application has logical groups of protected resources (for example, all web resources that match a path pattern, such as /api/**), and each group can have its own dedicated AuthenticationManager. Often, each of those is a ProviderManager, and they share a parent. The parent is then a kind of “global” resource, acting as a fallback for all providers
  

