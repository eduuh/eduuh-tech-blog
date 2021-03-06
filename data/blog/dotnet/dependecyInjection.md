---
title: Catalog 2.  Dependecy Injection
date: '2021-10-15'
tags: ['.net', 'catalog', 'api']
draft: false
summary: Part 2 Catalog Api Project. Exploring .Net 6 , csharp 10  together with MongoDB
images: []
layout: PostLayout
---

## Overview

<TOCInline toc={props.toc} exclude="Overview" toHeading={2} />

## What you will Learn

1. What is Dependecy Injection?
2. How to register and Inject dependencies in .NET 5
3. How to implement Data Transfer Objects(DTOS)
4. How to map entities to DTOS using an Extension Method

## What is Dependency Injections

.Net Core support the Dependency Injection (DI) sofware design pattern, which is techinque for achieving **Inversion of Control (IoC)** between classes and their dependecies.A dependency is an object that another object depends on.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/dependecyinjection/dependcyinjection.png" />
  </div>
</div>

Examining the above **ItemsController** class which depends on **InMemoryRepository** class. A class can create an instance making this case **InMemoryRepository** the dependecy of the **ItemsController** class. This is a direct dependecy which result to some problems.

1. To replace the **InMemoryRepository** with a different implementation, the **ItemsController** class must be modified.
2. If **InMemoryRepository** has dependecies, they must also be configured by the Controller. In a large project with multiple classes depending on the **InMemoryRepository** dependency, the configuration code becomes scattered across the app.
3. This implementation is difficult to unit test. If the app unit testing may use a mock or stub **InMemoryRepository** class isn't possible with this approach.

## How does .Net DI work

Dependecy injection addresses these problems through:

- The use of an **interface** or **base class to abstract the dependecy implementation**.
- Registration of the dependecy in a service container. .Net core provides a built-in service container, **IServiceProvider.**
- Injection of the service into the constructor of the class where it's used. The framework takes on the responsibility of creating an instance of the dependecy and disposing of it when it's no longer needed.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/dependecyinjection/dotnetdi.png" />
  </div>
</div>

By using the DI patterm, the controller:

- Doesn't use the concrete type **InMemoryRepository**, only the **interface** it implements. That makes it easy to change the implementation that the controller uses without modifying the controller.
- Doesn't create an instance of **InMemoryRepository**, it's created by the DI container.

Let see how we can use Dependecy Injection to our advantage. First we are going to remove the Explicity dependecy from our controller.

Lets use Extract an interface from the IInMemItemsRepository. Move your cursor on the ClassName and Click on the Light ball. Click on the option to extract an interface.

You can now move the file in Its own file **IInMemItemsRepository.cs**

```csharp
public interface IInMemItemsRepository
{
    Item GetItem(Guid id);
    IEnumerable<Item> GetItems();
}
```

Lets Update the repository to use the interface.

```csharp
using catalog.Entities;
using catalog.Repositories;
using Microsoft.AspNetCore.Mvc;
namespace catalog.Controllers;
[ApiController]
[Route("items")]
public class ItemsController : IInMemItemsRepository
{

    // GET /items
    private readonly IInMemItemsRepository _repository;

    public ItemsController(IInMemItemsRepository repository)
    {
        _repository = repository;
    }
    [HttpGet]
    public IEnumerable<Item> GetItems()
    {
        return _repository.GetItems();
    }

    [HttpGet("{id}")]
    public ActionResult<Item> GetItem(Guid id)
    {
        var item = _repository.GetItem(id);
        if (item is null) return NotFound();
        return item;
    }
}
```

The next step is to register our Repository and IRepository to our Application Dependecy Class. I added the line **builder.Services.AddSingleton**. This ensures that our application creates a single instance of the InMemoryRepository.

**Program.cs** file will look as follows.

```csharp
using catalog.Repositories;
using Microsoft.OpenApi.Models;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new() { Title = "catalog", Version = "v1" });
});
// singlton: Is a way to specify how to create your app dependecies.
builder.Services.AddSingleton<IInMemItemsRepository, InMemItemsRepository>();
var app = builder.Build();
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c => c.SwaggerEndpoint("/swagger/v1/swagger.json", "catalog v1"));
}
app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();
```

Not Lets test the **get item by id** to ensure that the we can get an instance, hopefully we are now using a single instance of the InMemItemsRepository.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/dependecyinjection/getbyid.png" />
  </div>
</div>

We surely get the Object.

Its important to note that once you scroll down the swagger Page. We are exposing our Item Contract. This is offcourse not what we want. Lets abtract this away by using of DTOS. Create a new folder **DTO**.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/dependecyinjection/contract.png" />
  </div>
</div>

Lets create a **ItemDto**. In this case the dto will be similar to our Model class.

```csharp
using System;
namespace catalog.Entities;
public record ItemDto
{
    public Guid Id { get; init; }
    public string Name { get; init; }
    public decimal Price { get; init; }
    public DateTimeOffset CreatedDate { get; init; }
}

```

We could update the controller Method With the transformation from our contract to the new Object Dto as follows.

```csharp
    [HttpGet]
    public IEnumerable<ItemDto> GetItems()
    {
        return _repository.GetItems().Select(item => new ItemDto
        {
            Id = item.Id,
            Name = item.Name,
            Price = item.Price,
            CreatedDate = item.CreatedDate,
        });
    }
```

But we could be transformation in many places, which could not be idea. The ideal situation is to create an extension method (reusable code).

Lets create a file **Extensions.cs**

```csharp
using catalog.Entities;

namespace catalog;

public static class Extensions
{
    public static ItemDto AsDto(this Item item)
    {
        return new ItemDto
        {
            Id = item.Id,
            Name = item.Name,
            Price = item.Price,
            CreatedDate = item.CreatedDate,
        };
    }
}
```

Update the controller Files to look as Follows.

```csharp
using catalog.Entities;
using catalog.Repositories;
using Microsoft.AspNetCore.Mvc;
namespace catalog.Controllers;
[ApiController]
[Route("items")]
public class ItemsController : ControllerBase
{

    // GET /items
    private readonly IInMemItemsRepository _repository;

    public ItemsController(IInMemItemsRepository repository)
    {
        _repository = repository;
    }


    [HttpGet]
    public IEnumerable<ItemDto> GetItems()
    {
        return _repository.GetItems().Select(item => item.AsDto());
    }

    [HttpGet("{id}")]
    public ActionResult<ItemDto> GetItem(Guid id)
    {
        var item = _repository.GetItem(id);
        if (item is null) return NotFound();
        return item.AsDto();
    }
}

```

Now you expose the dto.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/dependecyinjection/dto.png" />
  </div>
</div>

Next post **Post, Put , Delete**
