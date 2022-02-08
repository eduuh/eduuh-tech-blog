---
title: Catalog 1. Dotnet Rest API Basics
date: '2021-10-02'
tags: ['.net', 'catalog', 'api']
draft: false
summary: Part 1 Catalog Api Project. Exploring .Net 6 , csharp 10  together with MongoDB
images: []
layout: PostLayout
---

## Overview

<TOCInline toc={props.toc} exclude="Overview" toHeading={2} />

## .Net 6 Rest Api

A Rest API allows you to expose resources both inside or outside your network. The idea behing HTTP(workflow) is that a client sends an **HTTP request** to a server, and then the server responds to that client with a **response.** This is one of the most important concepts irrespective of whether you are building web APIS, websites or complex cloud applications.

In summary this is the lifecyle of a https request.

1. The communication starts on the client by sending a request to a web server.
2. The server receives the request and executes the logic behind the request.
3. Once the logic has successfully completed the server responds with a response.
4. The communications ends.

After that cycle, the server is no longer aware of the client and if the client sends another request, the server is not aware that it responded to a request earlier for the same client because **HTTP is stateless**.

In this case **.Net 6 Appliciation** will be the server and we will use **Swagger** as the client to consume the web Api.

## What you will learn

- How to create a .Net 6 web API project
- Using swagger UI to interact with the API

## What you need

- [The .Net 6 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/6.0).
- [Visual Studio code](https://code.visualstudio.com/) or any Preffered Editor.
- [MongoDB](https://www.mongodb.com/atlas/database)
- Understanding of csharp language 10 features

## Scenario

This is is what we will be creating in this post.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder Structure" src="/static/images/rest/restscenario.png"/>
  </div> 
</div>

## Check the Dotnet Version

First ensure that dotnet is installed.

```powershell
dotnet --version
# 6.0.101
```

## Trust dev-certificate

This will allow the web api to use a self signed certificate when developing. More information can be found [here](https://docs.microsoft.com/en-us/dotnet/core/additional-tools/self-signed-certificates-guide)

```PowerShell
dotnet dev-certs https --trust
```

## Starting the Creation

You have a folder in your hard drive where you store all your local repositories. You open **Visual Studio Code** and use the terminal to navigate this folder. Once in the folder, you create a project using the following statement.

```PowerShell
dotnet new webapi -n catalog
```

The folder structure of .Net 6 will appear as shown below. _.Net_ and _.Net core_ organize project and solutions in a slightly different way.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/rest/folderstructure.png"/>
  </div> 
</div>

## Visual Studio Code Debugging

This step visual studio code handles very well. Once you click on start debugging, vscode will request you to select the stack (_.Net Core_) you want to debug. Vscode will generate two files, **launch.json and tasks.json** in a .vscode folder.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Visual Studio Debugging" src="/static/images/rest/rest-vscodedebuggin.png"/>
  </div> 
</div>

The image above shows a debug session with the lauch.json file open. There are two section use to start debugging in different ways. You can **launch the application in debug mode** or you could **attach the debugger to a running process**.

## Run from Command Line

To start the application from command line, ensure you are in the root folder of the application and use the statement.

```bash
dotnet run
```

**dotnet run** will build your application and then start it. What about applying changes (when coding) to the running application without manually rebuilding? The answer is **Hot Reload**. In _visualstudio_ there is handy button called **Apply code changes** but when using Linux, these feature is provided through **dotnet watch**. [read more on Hot Reload](https://devblogs.microsoft.com/dotnet/introducing-net-hot-reload/)

```bash
dotnet watch run
# available in .Net 6 only
```

## Swagger UI

Allows engineers to get self-generated documentation for different platforms. Swagger UI is fully customizable tool that can be hosted in any enviroment. A great plus is that is enables developers to save a lot of time for API documentation.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/rest/rest-swagger.png"/>
  </div> 
</div>

## Launch and Build Tricks

### Avoid New Instance Api From Opening Other Browser Tabs

Opening a new tab instance when developing a web API can be a lot. It's important to disable this functionality. Go to the _vscode/lanch.json_ setting and remove the section

```json
 "serverReadyAction": {
                "action": "openExternally",
                "pattern": "\\bNow listening on:\\s+(https?://\\S+)"
            },

```

### Update the Build Task

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

Lets start by creating a record type of item with the following properties.

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

Let's create a repository class. A repository class have the following functionalities.

- Get **all the records**
- Get **A single record**,
- **Creating a record**
- **Updating a record**
- **Deleting a Record**.

For our Catalog class, let's just create a repository that can Get records.

_For now the datastore is readonly list. we will replace this with MongoDB._

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

Let's update the Implementation and solve the problem using **dependency injection, singleton** in the [next post](http://localhost:3000/blog/dotnet/dependecyInjection).
