Designing a code API with 3 layers (repository, services, controller) using Entity Framework can be done in several steps:

1.  Define your domain entities: Begin by defining the domain entities that represent your business entities. These should be classes that model your business objects and should have all the properties and behaviors that are required.
    
2.  Create your data context: Once you have defined your domain entities, create a data context that derives from DbContext class. This context will be responsible for managing connections to the database and translating between your domain entities and the underlying database tables.
    
3.  Implement your repository layer: The repository layer is responsible for encapsulating the data access code and providing a clean and consistent interface for accessing the data. Create interfaces for your repositories, and then implement these interfaces with concrete classes. The concrete classes should inherit from a base repository class, which contains the implementation for the common data access methods (such as adding, updating, and deleting entities).
    
4.  Implement your services layer: The services layer is responsible for implementing the business logic that operates on the data. Create interfaces for your services, and then implement these interfaces with concrete classes. The concrete classes should use the repository layer to access the data and implement the business logic as required.
    
5.  Implement your controller layer: The controller layer is responsible for handling incoming requests and returning responses. Create your controller classes, and then inject your services into the controller constructor. This way, the controller can access the services and invoke their methods to handle the incoming requests.
    
6.  Configure dependency injection: Use a dependency injection container to wire up the dependencies between the layers. This will help to ensure that the correct instances of your repository and service classes are injected into your controllers.
    
7.  Write your API endpoints: Finally, write the API endpoints that map incoming requests to the appropriate controller methods. Use the appropriate HTTP verbs (GET, POST, PUT, DELETE) to map the requests to the appropriate methods.
    

With these steps, you should have a well-designed code API with 3 layers (repository, services, controller) using Entity Framework. This design pattern will help you to separate your concerns and create a modular and maintainable codebase.