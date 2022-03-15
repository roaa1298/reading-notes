# Spring
## Serving Web Content with Spring MVC

- **Spring** : is the most popular application development framework for enterprise Java. Millions of developers around the world use Spring Framework to create high performing, easily testable, and reusable code, its an open source Java platform. Spring was initially written by Rod Johnson and was first released under the Apache 2.0 license in June 2003.

### Starting with Spring Initializr:

   1- If you use Gradle, visit the Spring Initializr to generate a new project with the required dependencies (Spring Web, Thymeleaf, and Spring Boot DevTools).   
   
      ```
      plugins {
	    id 'org.springframework.boot' version '2.5.2'
	    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	    id 'java'
     }

     group = 'com.example'
     version = '0.0.1-SNAPSHOT'
     sourceCompatibility = '1.8'

     repositories {
      	mavenCentral()
     }

    dependencies {
        	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
         	implementation 'org.springframework.boot:spring-boot-starter-web'
	        developmentOnly 'org.springframework.boot:spring-boot-devtools'
	        testImplementation 'org.springframework.boot:spring-boot-starter-test'
    }

    test {
	     useJUnitPlatform()
     }
     ```

   2- manual initialization: following the steps below:
       1. navigate to https://start.spring.io.
       2. choose gradle and java as the language.
       3. click dependencies and select spring web, thymeleaf, and spring boot devtools.
       4. click generate.
       5. Download the resulting ZIP file, which is an archive of a web application that is configured with your choices.

### Create a Web Controller

  - HTTP requests are handled by a controller.
  - the controller identify by the @Controller annotation.
  - example on controller:   
     ```
     package com.example.servingwebcontent;

     import org.springframework.stereotype.Controller;
     import org.springframework.ui.Model;
     import org.springframework.web.bind.annotation.GetMapping;
     import org.springframework.web.bind.annotation.RequestParam;

    @Controller
    public class GreetingController {

	  @GetMapping("/greeting")
	  public String greeting(@RequestParam(name="name", required=false, defaultValue="World") String name, Model model) {
	      	model.addAttribute("name", name);
	      	return "greeting";
	    }

    }
    ```
### Spring Boot Devtools:

   - Enables hot swapping.
   - Switches template engines to disable caching.
   - Enables LiveReload to automatically refresh the browser.
   - Other reasonable defaults based on development instead of production.
### Run the Application

  - @Configuration: Tags the class as a source of bean definitions for the application context.
### Test the Application

   - Provide a name query string parameter by visiting http://localhost:8080/greeting?name=User.
   - The index.html resource is special because, if it exists, it is used as a "welcome page "serving-web-content/ which means it is served up as the root resource (that is, at http://localhost:8080/
   - restart the application, you will see the HTML at http://localhost:8080/.
## pring MVC and Thymeleaf: how to access data from templates
   - @Controller classes: are responsible for preparing a model map with data and selecting a view to be rendered.
   - in the case of Thymeleaf, it is transformed into a Thymeleaf context object.
### pring model attributes

  Spring MVC calls the pieces of data that can be accessed during the execution of views model attributes. The equivalent term in Thymeleaf language is context variables.

   - There are several ways of adding model attributes to a view in Spring MV:

      1. Add attribute to Model via its addAttribute method:
         ```
         @RequestMapping(value = "message", method = RequestMethod.GET)
          public String messages(Model model) {
            model.addAttribute("messages", messageRepository.findAll());
            return "message/list";
         }
        ```
   2. Return ModelAndView with model attributes included:
    
        ```
         @RequestMapping(value = "message", method = RequestMethod.GET)
         public ModelAndView messages() {
            ModelAndView mav = new ModelAndView("message/list");
            mav.addObject("messages", messageRepository.findAll());
            return mav;
        }
        ```
   3. Expose common attributes via methods annotated with @ModelAttribute:
        ```
         @ModelAttribute("messages")
        public List<Message> messages() {
            return messageRepository.findAll();
        }
        ```
   - in all these cases the messages attribute is added to the model and it will be available in Thymeleaf views.

   - In Thymeleaf, these model attributes can be accessed with: {attributeName}.
### Request parameters  

  - Request parameters are passed from the client to server, Like : https://example.com/query?q=Thymeleaf+Is+Great!

    If we have @Controller that sends a redirect with a request parameter:
         ```
         @Controller
        public class SomeController {
            @RequestMapping("/")
            public String redirect() {
                return "redirect:/query?q=Thymeleaf+Is+Great!";
            }
        }
        ```
  - we use the param to access q parameter, using brackets syntax: <p th:text="${param.q[0] + ' ' + param.q[1]}" th:unless="${param.q == null}">Test</p>
  - another way, using the special #request object: <p th:text="${#request.getParameter('q')}" th:unless="${#request.getParameter('q') == null}">Test</p>
  - we can add mySessionAttribute to session, Or by using #session, that gives us direct access to the javax.servlet.http.HttpSession object.
### ServletContext attributes  
- The ServletContext attributes are shared between requests and sessions.
- to access it use, #servletContext.
### Spring beans  
Thymeleaf allows accessing beans registered at the Spring Application Context with the @beanNamesyntax:  
      ```
      `<div th:text="${@urlService.getApplicationUrl()}">...</div>`
      ```  
@urlService refers to a Spring Bean registered at your context.

### References  
- [spring app basics](https://spring.io/guides/gs/serving-web-content/)
- [Spring MVC and Thymeleaf](https://www.thymeleaf.org/doc/articles/springmvcaccessdata.html)



        
