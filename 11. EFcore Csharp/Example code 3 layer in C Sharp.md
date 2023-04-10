1.  Define your domain entities:

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
}

public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
}

```
2.  Create your data context:
```csharp
public class DataContext : DbContext
{
    public DbSet<Product> Products { get; set; }
    public DbSet<Category> Categories { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=MyDatabase;Integrated Security=True");
    }
}

```
3.  Implement your repository layer:
```csharp
public interface IRepository<TEntity> where TEntity : class
{
    IEnumerable<TEntity> GetAll();
    TEntity GetById(int id);
    void Add(TEntity entity);
    void Update(TEntity entity);
    void Delete(TEntity entity);
}

public class Repository<TEntity> : IRepository<TEntity> where TEntity : class
{
    private readonly DataContext _context;

    public Repository(DataContext context)
    {
        _context = context;
    }

    public IEnumerable<TEntity> GetAll()
    {
        return _context.Set<TEntity>().ToList();
    }

    public TEntity GetById(int id)
    {
        return _context.Set<TEntity>().Find(id);
    }

    public void Add(TEntity entity)
    {
        _context.Set<TEntity>().Add(entity);
        _context.SaveChanges();
    }

    public void Update(TEntity entity)
    {
        _context.Entry(entity).State = EntityState.Modified;
        _context.SaveChanges();
    }

    public void Delete(TEntity entity)
    {
        _context.Set<TEntity>().Remove(entity);
        _context.SaveChanges();
    }
}

public interface IProductRepository : IRepository<Product>
{
    IEnumerable<Product> GetByCategoryId(int categoryId);
}

public class ProductRepository : Repository<Product>, IProductRepository
{
    public ProductRepository(DataContext context) : base(context)
    {
    }

    public IEnumerable<Product> GetByCategoryId(int categoryId)
    {
        return _context.Products.Where(p => p.CategoryId == categoryId).ToList();
    }
}

public interface ICategoryRepository : IRepository<Category>
{
}

public class CategoryRepository : Repository<Category>, ICategoryRepository
{
    public CategoryRepository(DataContext context) : base(context)
    {
    }
}

```
4. Implement your services layer:
```csharp
public interface IProductService
{
    IEnumerable<Product> GetAll();
    Product GetById(int id);
    void Add(Product product);
    void Update(Product product);
    void Delete(Product product);
    IEnumerable<Product> GetByCategoryId(int categoryId);
}

public class ProductService : IProductService
{
    private readonly IProductRepository _productRepository;

    public ProductService(IProductRepository productRepository)
    {
        _productRepository = productRepository;
    }

    public IEnumerable<Product> GetAll()
    {
        return _productRepository.GetAll();
    }

    public Product GetById(int id)
    {
        return _productRepository.GetById(id);
    }

    public void Add(Product product)
    {
        _productRepository.Add(product);
    }

    public void Update(Product product)
    {
        _productRepository.Update(product);
    }

    public void Delete(Product product)
    {
        _productRepository.Delete(product);
    }

    public IEnumerable<Product> GetByCategoryId(int categoryId)
    {
        return _productRepository.GetByCategoryId(categoryId);
    }
}

public interface

```
5.  Implement your controller layer:
```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductController : ControllerBase
{
    private readonly IProductService _productService;

    public ProductController(IProductService productService)
    {
        _productService = productService;
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<Product>> GetById(int id)
    {
        var product = await _productService.GetById(id);
        if (product == null)
        {
            return NotFound();
        }
        return product;
    }

    [HttpGet]
    public async Task<ActionResult<List<Product>>> GetAll()
    {
        var products = await _productService.GetAll();
        return products;
    }

    [HttpPost]
    public async Task<ActionResult<Product>> Create(Product product)
    {
        await _productService.Add(product);
        return CreatedAtAction(nameof(GetById), new { id = product.Id }, product);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> Update(int id, Product product)
    {
        if (id != product.Id)
        {
            return BadRequest();
        }

        try
        {
            await _productService.Update(product);
        }
        catch (DbUpdateConcurrencyException)
        {
            if (await _productService.GetById(id) == null)
            {
                return NotFound();
            }
            else
            {
                throw;
            }
        }

        return NoContent();
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> Delete(int id)
    {
        var product = await _productService.GetById(id);
        if (product == null)
        {
            return NotFound();
        }

        await _productService.Delete(product);
        return NoContent();
    }
}

[ApiController]
[Route("api/[controller]")]
public class OrderController : ControllerBase
{
    private readonly IOrderService _orderService;

    public OrderController(IOrderService orderService)
    {
        _orderService = orderService;
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<Order>> GetById(int id)
    {
        var order = await _orderService.GetById(id);
        if (order == null)
        {
            return NotFound();
        }
        return order;
    }

    [HttpGet]
    public async Task<ActionResult<List<Order>>> GetAll()
    {
        var orders = await _orderService.GetAll();
        return orders;
    }

    [HttpPost]
    public async Task<ActionResult<Order>> Create(Order order)
    {
        await _orderService.Add(order);
        return CreatedAtAction(nameof(GetById), new { id = order.Id }, order);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> Update(int id, Order order)
    {
        if (id != order.Id)
        {
            return BadRequest();
        }

        try
        {
            await _orderService.Update(order);
        }
        catch (DbUpdateConcurrencyException)
        
        if (await _orderService.GetById(id) == null)
        {
            return NotFound();
        }
        else
        {
            throw;
        }
    }

    return NoContent();
}

[HttpDelete("{id}")]
public async Task<IActionResult> Delete(int id)
{
    var order = await _orderService.GetById(id);
    if (order == null)
    {
        return NotFound();
    }

    await _orderService.Delete(order);
    return NoContent();
}
}


```
6.  Finally, you can wire up your layers using dependency injection in your Startup.cs file:
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<MyDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));

    services.AddScoped<IProductRepository, ProductRepository>();
    services.AddScoped<IProductService, ProductService>();

    services.AddScoped<IOrderRepository, OrderRepository>();
    services.AddScoped<IOrderService, OrderService>();

    services.AddControllers();
}

```