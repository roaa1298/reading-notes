# Purely functional programming  
## What is functional programming? 
- It is a declarative style of programming rather than imperative. The basic objective of this style of programming is to make code more concise, less complex, more predictable, and easier to test compared to the legacy style of coding. Functional programming deals with certain key concepts such as pure function, immutable state, assignment-less programming etc.   
## Functional programming vs Purely Functional programming:  
- Pure functional programming languages don’t allow any mutability in its nature whereas a functional style language provides higher-order functions but often permits mutability at the risk of we failing to do the right things, which put a burden on us rather than protecting us. So, in general, we can say if a language provides higher-order function it is functional style language, and if a language goes to the extent of limiting mutability in addition to higher-order function then it becomes purely functional language. Java is a functional style language and the language like Haskell is a purely functional programming language.  
- Higher-order functions: In functional programming, functions are to be considered as first-class citizens. That is, so far in the legacy style of coding, we can do below stuff with objects.   
     - We can pass objects to a function.
     - We can create objects within function.
     - We can return objects from a function.
     - We can pass a function to a function.
     - We can create a function within function.
     - We can return a function from a function.

- Pure functions: A function is called pure function if it always returns the same result for same argument values and it has no side effects like modifying an argument (or global variable) or outputting something.  
- Lambda expressions: A Lambda expression is an anonymous method that has mutability at very minimum and it has only a parameter list and a body. The return type is always inferred based on the context. Also, make a note, Lambda expressions work in parallel with the functional interface. The syntax of a lambda expression is:  
   ```
   (parameter) -> body
   ```
- In its simple form, a lambda could be represented as a comma-separated list of parameters, the –> symbol and the body.  
## How to Implement Functional Programming in Java?  
    ```
    // Java program to demonstrate
    // anonymous method
    import java.util.Arrays;
    import java.util.List;
    public class GFG {
    	public static void main(String[] args)
	    {

	    	// Defining an anonymous method
	    	Runnable r = new Runnable() {
		    	public void run()
		    	{
		    		System.out.println(
		    			"Running in Runnable thread");
		   	}
		   };

		     r.run();
		    System.out.println(
		    	"Running in main thread");
 	   }
    }
    ```
- Output:   
   ```
   Running in Runnable thread
   Running in main thread
   ```
- We have worked many times with loops and iterator so far up to Java 7 as follows:  
   ```
   // Java program to demonstrate an
   // external iterator
   import java.util.Arrays;
   import java.util.List;
   public class GFG {
   	public static void main(String[] args)
   	{
	   	List<Integer> numbers
		   	= Arrays.asList(11, 22, 33, 44,
						   	55, 66, 77, 88,
							   99, 100);

		   // External iterator, for Each loop
		   for (Integer n : numbers) {
			   System.out.print(n + " ");
		   }
	   }
   }
   ```
- output:
   ```
   11 22 33 44 55 66 77 88 99 100
   ```
- Above was an example of forEach loop in Java a category of external iterator, below one is again example and another form of external iterator.    
   ```
   // Java program to demonstrate an
   // external iterator
   import java.util.Arrays;
   import java.util.List;
   public class GFG {
    	public static void main(String[] args)
	   {
		   List<Integer> numbers
		   	= Arrays.asList(11, 22, 33, 44,
							   55, 66, 77, 88,
							   99, 100);

		   // External iterator
		   for (int i = 0; i < numbers.size(); i++) {
			   System.out.print(numbers.get(i) + " ");
		   }
	   }
   }
   ```
- output:  
   ```
   11 22 33 44 55 66 77 88 99 100
   ```
- We can transform the above examples of an external iterator with an internal iterator introduced in Java 8, as follows:   
   ```
   // Java 8 program to demonstrate
   // an internal iterator

   import java.util.Arrays;
   import java.util.List;

   public class GFG {
   	 public static void main(String[] args)
	   {
		   List<Integer> numbers
			   = Arrays.asList(11, 22, 33, 44,
							   55, 66, 77, 88,
							   99, 100);

		   // Internal iterator
		   numbers.forEach(number
						   -> System.out.print(
							   number + " "));
	   }
   }
   ```
- output:  
   ```
   11 22 33 44 55 66 77 88 99 100
   ```
- Here, the functional interface plays a major role. Wherever a single abstract method interface is expected, we can pass lambda expression very easily. Above code could be more simplified and improved as follows:   
   ```
   numbers.forEach(System.out::println);
   ```




