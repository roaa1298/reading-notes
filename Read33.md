# GraphQL @connection   
## One-to-many Connections  
- One-to-many connections require an index created with the @index on a connected model. In this schema, the Comment model schema has a secondary index created with the @index called byPost . In the Post model comments field, the @hasMany uses this index named byPost to find comments with a matching postID .  
   
   ```
   type Post @model {
     id: ID!
     title: String!
     comments: [Comment] @hasMany(indexName: "byPost", fields: ["id"])
   }

   type Comment @model {
     id: ID!
     postID: ID! @index(name: "byPost", sortKeyFields: ["content"])
     content: String!
   }
   ```
- The id field referenced by @hasMany is the id on the Post type.   
- we can now query a post with its associated comments and get back a nicely shaped data package in one simple query:  

   ```
   mutation CreatePost {
     createPost(input: {title: "Hello world!"}) {
       comments {
         items {
           postID
           content
           id
         }
       }
       title
       id
     }
   }
   ```   

## CompletableFuture  
- CompletableFuture is used for asynchronous programming in Java. Asynchronous programming is a means of writing non-blocking code by running a task on a separate thread than the main application thread and notifying the main thread about its progress, completion or failure.   
- This way, your main thread does not block/wait for the completion of the task and it can execute other tasks in parallel.   
- Having this kind of parallelism greatly improves the performance of your programs.  

### Creating a CompletableFuture  
- We can create a CompletableFuture simply by using the no-arg constructor:  

   ```
   CompletableFuture<String> completableFuture = new CompletableFuture<String>();
   ```  
- This is the simplest CompletableFuture that we can have. All the clients who want to get the result of this CompletableFuture can call CompletableFuture.get() method.
- The get() method blocks until the Future is complete. So, this call will block forever because the Future is never completed.   
   ```
   String result = completableFuture.get()
   ```
- We can use CompletableFuture.complete() method to manually complete a Future:  

   ```
   completableFuture.complete("Future's Result")
   ```
- All the clients waiting for this Future will get the specified result. And, Subsequent calls to completableFuture.complete() will be ignored.
