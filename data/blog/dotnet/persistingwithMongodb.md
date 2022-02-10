---
title: Catalog 4. Persisting Dtos to Mongodb
date: '2021-10-16'
tags: ['.net', 'api', 'catalog']
draft: false
summary: Part 4 Catalog Api Project. Exploring .Net 6 , csharp 10  together with MongoDB
images: []
layout: PostLayout
---

## Overview

<TOCInline toc={props.toc} exclude="Overview" toHeading={2} />

## What You will Need

- Docker (https://www.docker.com/get-started)
- Postman (https://www.postman.com/downloads)

## Adding Persistence

If the rest api (_previous implementations_) is restarted, stopped or crashes when we are saving inMemory, all the data we have in the inmemory database is lost. We don't want a scenario which will losses our data when we restart the web server.

There a few persistence options, **Files** , **Databases** ( Relational, No-SQL). In this part post, let's use MongoDb database which is a No-Sql database. Some of the No-SQL benefits are:

1. No Need for a schema or SQL (_we need to enforce one_)
2. Low Latency
3. High Performance
4. Highly Scalable

## Using Postman

One you use postman on insomnia, you will get no result and an error that **ssl verification** failed.This is because .Net 5 uses self-signed certificates. You will need to disable ssl verification on postman to be able to use the Api.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/persistence/sslcertificate.png" />
  </div>
</div>

Press on the **Disable Ssl Verification** to disable it.

Once you disable ssl Verification, you will now be able to get the resource.

## Create a MongoDbItemsRepository

This repository will need to Implement the same Interface that the ImMemoryRepository have implemented. In order to implement the functionality using the external database, we will need to install a **Mongodb Client**.

### Add the Mongo client

Ensure that you are in the root folder of the application and execute the following statement.

```bash
dotnet add package MongoDB.driver
```

## Initialize the Repository by Implementing the Interface.

Note: All the method are not implemented, when you try to use them as they are you willget a NotImplementedException.

```csharp

using catalog.Entities;
using MongoDB.Driver;

namespace catalog.Repositories;

public class MongoDbItemsRepository : IInMemItemsRepository
{
   private const string databaseName = "catalog";
   private const string collectionName = "items";
   private readonly IMongoCollection<Item> itemsCollection;

   public MongoDbItemsRepository(IMongoClient mongoClient)
   {
	IMongoDatabase database = mongoClient.GetDatabase(databaseName);
        itemsCollection  = database.GetCollection<Item>(collectionName);
   }
    public void CreateItem(Item item)
    {
    }

    public void DeleteItem(Guid id)
    {
        throw new NotImplementedException();
    }

    public Item GetItem(Guid id)
    {
        throw new NotImplementedException();
    }

    public IEnumerable<Item> GetItems()
    {
        throw new NotImplementedException();
    }

    public void UpdateItem(Item item)
    {
        throw new NotImplementedException();
    }
}
```

## Get Mongodb Database Up and Running

Let's use a docker Image to create a docker container, which we will use for our application.

### Lets acquire and run a docker Image

```bash
 docker run -d --rm --name mongo -p 27017:27017  -v mongodbdata:/data/db mongo
```

- **docker run** will pull the image from the internet and create a container for you.
- **-d** flag tells docker to run the container in detached mode.
- **--name mongo** this name the container "mongo"
- **-p 27017:27017** binds the mongo container port to the local port in the system
- **-v mongodbdata:/data/db** This create a volume to store the data on the host machine, just incase you restart the container, the data will be intact.

We need to tell our dotnet Api to point to the created container instance.Lets add some configurations.

Open **appsettings.json** and add the following configurations.

```json
  ...
  "MongoDbSettings": {
    "Host": "localhost",
    "port": "27017",
    "User": "root"
  }
  ...
```

There are many ways to read the configurations from the dotnet Api. Lets Create a class that represents the configuration in a **settings** folder. Since we are creating a class, this gives us flexibility to perform calculation of the mongodb connection string using c# property.

```csharp
namespace Catalog.Settings;
public class MongoDbSettings {
  public string Host { get; set; }
  public string Port { get; set; }
  public string User { get; set; }
  public string Password { get; set; }

  public string ConnectionString { get {
    return $"mongodb://{User}:{Password}@{Host}:{Port}";
  } }
}

```

### Lets Register our MongoDB Client to The DI Container

We need to get the connections string and use the connection string to create a single instance of the mongodb for the application.

To read the configuration from the configuration file (**appsettings.json**).

```csharp
var settings = builder.Configuration.GetSection(nameof(MongoDbSettings)).Get<MongoDbSettings>();
```

To register MongoDB client to the DI container (**ServiceProvider**).

```csharp
builder.Services.AddSingleton<IMongoClient>(serviceProvider =>
{
var configuration = serviceProvider.GetRequiredService<IConfiguration>();
    return new MongoClient(settings.ConnectionString);
});

```

### Flip our Service: IInMemItemsRepository with MongoDbItemsRepository

Switch our service to start using our new Mongodbrepository. Now the implementation of IInMemItemsRepository will be injected with **MongoDbItemsRepository**.

```csharp
builder.Services.AddSingleton<IInMemItemsRepository, MongoDbItemsRepository>();
```

### Configure the Types

If you leave the default type provided by dotnet. The may end up having a representation that we don't want in the mongodb database. We need a way to tell mongoclient how to map between dotnet types and its types. Incompatible types includes the guid and DatetimeOffset.

We need to tell mongoclient to map Guids and DatetimeOffset as strings. This is how this is achieved. For this it was done in **Program.cs**.

```csharp
BsonSerializer.RegisterSerializer(new GuidSerializer(BsonType.String));
BsonSerializer.RegisterSerializer(new DateTimeOffsetSerializer(BsonType.String));
```

## Lets Implement MongoDbItemsRepository Methods

The final repository class looks like this.

```csharp
using catalog.Entities;
using MongoDB.Bson;
using MongoDB.Driver;

namespace catalog.Repositories;

public class MongoDbItemsRepository : IInMemItemsRepository
{
   private const string databaseName = "catalog";
   private const string collectionName = "items";
   private readonly IMongoCollection<Item> itemsCollection;

   private readonly FilterDefinitionBuilder<Item> filterbuilder = Builders<Item>.Filter;

   public MongoDbItemsRepository(IMongoClient mongoClient)
   {
	IMongoDatabase database = mongoClient.GetDatabase(databaseName);
        itemsCollection  = database.GetCollection<Item>(collectionName);
   }
    public async Task CreateItemAsync(Item item)
    {
	     await itemsCollection.InsertOneAsync(item);
    }

    public async Task DeleteItemAsync(Guid id)
    {
        var filter = filterbuilder.Eq(item => item.Id ,id);
	await itemsCollection.DeleteOneAsync(filter);
    }

    public async Task<Item> GetItemAsync(Guid id)
    {
        var filter = filterbuilder.Eq(item => item.Id ,id);
	return await itemsCollection.Find(filter).SingleOrDefaultAsync();
    }

    public async Task<IEnumerable<Item>> GetItemsAsync()
    {
       return await itemsCollection.Find(new BsonDocument()).ToListAsync();
    }

    public async Task UpdateItemAsync(Item item)
    {
        var filter = filterbuilder.Eq(existingitem => existingitem.Id, item.Id);
	await itemsCollection.ReplaceOneAsync(filter, item);
    }
}

```

### Testing The Create Method

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/persistence/location.png" />
  </div>
</div>
