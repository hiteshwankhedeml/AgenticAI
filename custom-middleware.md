# Custom Middleware

* In Prebuilt, we just had to use it, where and how it will get called was pre decided
* Here, we will have to specify where the middleware needs to get called
* By implementing hooks that run at specific points in the agent execution flow.
* Hooks ⇒ Extension points in custom middleware that let intercept, inspect or modify agent execution at specific stages
* **Two styles of hooks:**
  * **Node style hooks:**
    * Run sequentially at specific execution points
    * Use for logging, validation, and state updates
    * before\_agent, before\_model, after\_model, after\_agent
    * Here we get the state and runtime
  * **Wrap style hooks:**
    * Run around each model or tool call
    * wrap\_model\_call, wrap\_tool\_call
    * Here we get the exact request
  * We basically have to create a function with decorator like @before\_model or @after\_model and then we need to pass the same in middleware
  * For example in wrap\_model\_call based on the messages we can change the model to be used
* We can define middleware using decorator as well as class
* Class based:&#x20;
*

    <figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>
