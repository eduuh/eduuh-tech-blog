---
title: Catalog 3. Post, delete , update (API)
date: '2021-10-16'
tags: ['.net', 'api', 'catalog']
draft: false
summary: Part 3 Catalog Api Project. Exploring .Net 6 , csharp 10  together with MongoDB
images: []
layout: PostLayout
---

## What you will Learn

1. How to create resources with Post.
2. How to Validate the values of DTO properties.
3. How to Update resources with Put.
4. How to delete resources with Delete

## Create

### Update the Interface

Add a Create Method to the interface.

```csharp
void CreateItem(Item item);
```

To create some values may be generated on the server while other are supplied as DTO, therefore we need to add a **createDto**.

```csharp
namespace catalog.Dtos;

public record CreateItemDto
{
    public string Name { get; init; }
    public decimal Price { get; init; }
}
```

Got to the repository class and Implement the interface. You will need to update the **createMethod** as follows.

```csharp
    public void CreateItem(Item item)
    {
        items.Add(item);
    }
```

Lastly we need a route to expose this functionality. Lets add a create route in the items controller.

```csharp
 [HttpPost]
    public ActionResult<ItemDto> CreateItem(CreateItemDto itemDto)
    {
        Item item = new()
        {
            Id = Guid.NewGuid(),
            Name = itemDto.Name,
            Price = itemDto.Price,
            CreatedDate = DateTimeOffset.UtcNow
        };
        _repository.CreateItem(item);

        return CreatedAtAction(nameof(GetItem), new { id = item.Id }, item.AsDto());
    }
```

Lets test the create Functionality.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/crud/create.png" />
  </div>
</div>

### Creating Invalid Object.

If you try to post an invalid object i.e an object with no name, our api will accept the value and save inconsistent data. This is something we need to avoid .Net providea concept of **data annotations** to add validation to our fields.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/crud/invalidobject.png" />
  </div>
</div>

### Add Data Annotation

we are going to add Data Annotation to the **CreateItemDto** to tell .Net core, to ensure that all the fields are required.

This is how the Dto will look like.

```Javascript
public record CreateItemDto
{
    [Required]
    public string Name { get; init; }
    [Required]
    [Range(1,1000)]
    public decimal Price { get; init; }
}
```

Once the validation have been added, when you try to post invalid objects, you will get validation errors.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/crud/validationerrors.png" />
  </div>
</div>

## Update

### Update the Interface

Add a Update Method to the interface.

```csharp
void UpdateItem(Item item);
```

### Create an Update Dto

Create the Data transfer object.

```csharp
public record UpdateItemDto
{
    [Required]
    public string Name { get; init; }
    [Required]
    [Range(1,1000)]
    public decimal Price { get; init; }
}
```

### Update the Repository

The InMemoryRepository is the concrete class that implement the interface. Once we add a method on the interface, its a requirement for use to implement it in the classes implementations

The update method will look as follows.

```csharp
public void UpdateItem(Item item)
    {
        var index = items.FindIndex(existingitem => existingitem.Id == item.Id);
	items[index] = item;
    }
```

### Update the Controller

Then we need to add an Update route in the controller. The signature of the method will look as follows.

```csharp

    [HttpPut("{id}")]
    public ActionResult UpdateItem(Guid id, UpdateItemDto itemDto)
    {
        var exstingitem = _repository.GetItem(id);

        if (exstingitem is null) return NotFound();

        Item updatedItem = exstingitem with { Name = itemDto.Name, Price = itemDto.Price };

        _repository.UpdateItem(updatedItem);

        return NoContent();

    }
```

### Test the Controller

The final step is to test the controller to ensure that the controller routes are working as expected.

You will see that the controller will be working alright.

<div className="flex flex-wrap -mx-2 overflow-hidden xl:-mx-2">
  <div className="my-1 px-2 w-full overflow-hidden xl:my-1 xl:px-2 xl:w-1/2">
    <img alt="Folder structure" src="/static/images/crud/updatetest.png" />
  </div>
</div>

## Delete

### Update the Interface

Add a Delete Method to the interface. The method takes in and Guild Id and delete it. For delete we do not need to have a DTo since we are passing in a simple string.

```csharp
void Delete(Guid id);
```

### Update the Repository

The method implementation is implemented as follows.

```csharp
 public void DeleteItem(Guid id)
    {
        var index = items.FindIndex(existingitem => existingitem.Id == id);
	items.RemoveAt(index);
    }
```

### Update the Controller

The controller method will be implemented as follows.

```csharp
 // delete /items/
    [HttpDelete("{id}")]
      public ActionResult DeleteItem(Guid id){

          var existingItem = _repository.GetItem(id);
	  if(existingItem is null) return NotFound();

	  _repository.DeleteItem(id);

	  return NoContent();
      }
```

That was all, Next lets look on how to persist the data in a mongoDb database.
