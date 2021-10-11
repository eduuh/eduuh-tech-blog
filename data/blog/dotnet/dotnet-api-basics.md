---
title: One Dotnet Basics, Part 1
date: '2021-10-02'
tags: ['.net', 'api']
draft: false
summary: Catalog Api Project
images: []
layout: PostLayout
---

## .Net 6 Rest Api

Allows you to expose resources both inside or outside your network. The idea behing HTTP is that a client sends an HTTP requests to a server, and then the server responds to that client. This is one of the most important concepsts, irrespective of whether you are building web APIS, websites or complex cloud applications.

In summary this is the lifecyle of a https request.

1. The communication starts.
2. The client sends a request to the server.
3. The server receiver the request.
4. The server most likely does something(executes some logic/code)
5. The server responds to the client.
6. The communications ends.

After that cycle, the server is no longer aware of the client and if the client sends another request, the server is not aware that it responded to a request earlier for the same client because **HTTP is stateless**.

In this case **.Net 6 Appliciation** will be the server and we will use **Swagger** as the client to consume the web Api.

## What you will learn

- How to create a .Net 5 web API project
- Using swagger UI to interact with the API

## What you need

- The .Net 6 SDK
- vscode
- A basic Understanding of csharp language

## Scenario

This is is what we will be creating in this post.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder Structure" src="/static/images/rest/restscenario.png"/>
  </div> 
</div>

## Check the Dotnet Version

```powershell
dotnet --version
# 6.0.100-rc.1.21458.32
```

## Trust Dev-Certificates

```PowerShell
dotnet dev-certs https --trust
```

## Create the Project

Create the project using the command:

```PowerShell
dotnet new webapi -n catalog
```

.Net 6 folder structure

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/rest/folderstructure.png"/>
  </div> 
</div>

## Visual Studio Code Debugging

## Run From CommandLine

## Swagger

## Launch and Build

### Avoid New Instance Api From Opening Other Windows

Opening a new tab instance when developing a web API can be a lot. It's important to disable this functionality. Go to the vscode/lanch.json setting and remove the section

```json
 "serverReadyAction": {
                "action": "openExternally",
                "pattern": "\\bNow listening on:\\s+(https?://\\S+)"
            },

```

## Up the Build Task

Update the vscode task file build to default to building. Now you can perform vscode build by pressing keybinding **Ctrl+Shift+B**.

```JSON
{
            "label": "build",
            "command": "dotnet",
            "type": "process",
            "args": [
                "build",
                "${workspaceFolder}/catalog.csproj",
                "/property:GenerateFullPaths=true",
                "/consoleloggerparameters:NoSummary"
            ],
            "problemMatcher": "$msCompile",
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
```

## Let write Some Code

we will start by creating a type of item with the following properties.

```CSharp
using System;
namespace catalog.Entities;
public record Item
{
    public Guid Id { get; init; }
    public string Name { get; init; }
    public decimal Price { get; init; }
    public DateTimeOffset CreatedDate { get; init; }
}

```

Note: This is not how you should create a repository class. It serves as a good starting point to understanding how to work with a dotnet core API.

Let's create a repository class. A repository class how functionality of how to get **all the records** , **A single record**, **creating a record** , **Updating a record** and **Deleting a Record**. For our Catalog class, let's just create a repository that can Get records.

```CSharp
using catalog.Entities;

namespace catalog.Repositories;

public class InMemItemsRepository
{
    private readonly List<Item> items = new()
    {
        new Item { Id = Guid.NewGuid(), Name = "Potion", Price = 8, CreatedDate = DateTimeOffset.UtcNow },
        new Item { Id = Guid.NewGuid(), Name = "Iron Sword", Price = 10, CreatedDate = DateTimeOffset.UtcNow },
        new Item { Id = Guid.NewGuid(), Name = "Bronze Shield", Price = 16, CreatedDate = DateTimeOffset.UtcNow },
        new Item { Id = Guid.NewGuid(), Name = "Knife", Price = 20, CreatedDate = DateTimeOffset.UtcNow },
    };
    public IEnumerable<Item> GetItems()
    {
        return items;
    }

    public Item GetItem(Guid id)
    {
        return items.Where(item => item.Id == id).SingleOrDefault();
    }
}

```

Create an Items Controller in the controllers folder.

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
    private readonly InMemItemsRepository repository;

    public ItemsController()
    {
        repository = new InMemItemsRepository();
    }

    [HttpGet]
    public IEnumerable<Item> GetItems()
    {
        return repository.GetItems();
    }

    [HttpGet("{id}")]
    public  Item GetItem(Guid id)
    {
        var item = repository.GetItem(id);
        return item;
    }
}

```

At this point, let's test the API.

```
Press F5 in visual studio code
```

Once you refresh the swagger page you should see the following In your browser

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/rest/catalog.png"/>
  </div> 
</div>

Let's test getting all the Catalog from the Swagger UI.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/rest/getallitems.png"/>
  </div> 
</div>

Let's grab one Id from the GetAllItems and Test It out.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/rest/getoneitem.png"/>
  </div> 
</div>

Something Wrong happens. We get an Error 204 meaning our application is returning a null value.

**Why is the application returning null ?**

This is because of the implementation. When we make a new Request, The controller is Initializes an InmemoryController instance where all the Items have unique guids. This makes our getbyid route not find a matching Item.

### Let's handle the null response.

Notice that once we handle the null by returning a Notfound how the compiler complains. This is because of NotFound() returns a type that is different from the **item** type that the controller method expects.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/rest/returnType.png"/>
  </div> 
</div>

We need to have a way to return multiple types from a controller. Dotnet MVC has a type to do this. All we need is to update the method signature to match the following.

Once we test the route, we will now get a Notfound return with a 404 status code shown below.

```csharp
   [HttpGet("{id}")]
    public ActionResult<Item> GetItem(Guid id)
    {
        var item = repository.GetItem(id);
        if (item is null) return NotFound();
        return item;
    }
```

Let's update the Implementation problem using **dependency injection, singleton** in the next post.
