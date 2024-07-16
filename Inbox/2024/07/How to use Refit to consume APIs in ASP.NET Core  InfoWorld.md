---
created: 2024-07-15T14:22:42 (UTC -03:00)
tags: []
source: https://www.infoworld.com/article/2514270/how-to-use-refit-to-consume-apis-in-asp-net-core.html
author: byJoydip Kanjilal


Contributor
---

# How to use Refit to consume APIs in ASP.NET Core | InfoWorld

> ## Excerpt
> how-to

---
how-to

Jul 04, 202410 mins

C#Development Libraries and FrameworksMicrosoft .NET

## Take advantage of Refit REST library to simplify API consumption and make your code cleaner, more efficient, and easier to maintain.

[Refit](https://github.com/reactiveui/refit) is an open-source library for .NET, .NET Core, and Xamarin that makes consuming REST APIs simpler and easier by enabling you to define the API endpoints as C# interfaces, thereby eliminating the need to create HTTP requests and parse HTTP responses manually.

In this article we will delve into the Refit library for .NET and see first-hand how it simplifies the development of [APIs](https://www.infoworld.com/article/3269878/what-is-an-api-application-programming-interfaces-explained.html). To use the code examples provided in this article, you should have Visual Studio 2022 installed in your system. If you don’t already have a copy, you can [download Visual Studio 2022 here](https://visualstudio.microsoft.com/vs/preview/#download-preview).

In sections below, we will implement two applications, a Contact API and a client application for consuming the Contact API. The Contact API application will comprise the following types:

-   Contact: This represents the model class.
-   IContactRepository: This represents the interface for the contact repository.
-   ContactRepository: This represents the contact repository class that contains methods to return contact data.
-   ContactsController: This represents the API controller used to expose Contact API endpoints to API clients.

The client application will use Refit to consume the Contact API and display the records retrieved at the console window.

## What is Refit? Why is it useful?

Refit is a type-safe, fast, REST library for .NET, .NET Core, and Xamarin that turns your REST API into an interface, making it easier to consume RESTful web services. Refit automatically transforms HTTP calls into C# interfaces using attributes to describe REST operations, thereby simplifying the process of connecting with APIs using minimal code.

To consume APIs using Refit, you need an interface that can interact with your API. Refit acts as a wrapper around the methods of this interface and handles HTTP requests and responses elegantly. Refit will automatically generate the required boilerplate code for you to access your APIs.

If you’re using Refit for the first time, you must first configure the HTTP client instance by specifying the base address, HTTP headers, serialization, and deserialization information, etc. The following code snippet shows how we can configure the HTTP client instance to connect to an endpoint in ASP.NET Core.

`string baseAddress = "http://localhost:59904/";`  
`HttpClient _client = new HttpClient();`  
`_client.BaseAddress = new Uri($"{BaseUrl}");`  
`_client.DefaultRequestHeaders.Accept.Clear();`  
`_client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));`  
`string url = BaseUrl + "api/authors";`  
`var response = await _client.GetAuthors(url);`  
`if (response.IsSuccessStatusCode)`  
`{      var result = await response.Content.ReadAsStringAsync();`  
    `var data = JsonSerializer.Deserialize<List<Author>>(result); }`

If you’re already using Refit, you need not be bothered with the boilerplate code, because Refit can handle all of these tasks with just a few lines of C# code.

`string baseAddress = "http://localhost:59904/";   var endpoint = RestService.For<IAuthorService>(baseAddress);   var contacts = await endpoint.GetAuthors();`

As you can see from the preceding code snippets, Refit can save us a lot of time and effort by eliminating the need to write the boilerplate code.

In the next sections, we’ll implement a simple web API in ASP.NET Core. After we create our API, we’ll implement a Refit client to consume it.

## Create an ASP.NET Core Web API project in Visual Studio 2022

To create an ASP.NET Core 8 Web API project in Visual Studio 2022, follow the steps outlined below.

1.  Launch the Visual Studio 2022 IDE.
2.  Click on “Create new project.”
3.  In the “Create new project” window, select “ASP.NET Core Web API” from the list of templates displayed.
4.  Click Next.
5.  In the “Configure your new project” window, specify the name and location for the new project. Optionally check the “Place solution and project in the same directory” check box, depending on your preferences.
6.  Click Next.
7.  In the “Additional Information” window shown next, select “.NET 8.0 (Long Term Support)” as the framework version and ensure that the “Use controllers” box is checked. We will be using controllers in this project.
8.  Elsewhere in the “Additional Information” window, leave the “Authentication Type” set to “None” (the default) and ensure the check boxes “Enable Open API Support,” “Configure for HTTPS,” and “Enable Docker” remain unchecked. We won’t be using any of those features here.
9.  Click Create.

We’ll use this ASP.NET Core Web API project to create our API in the sections below.

### Create the Contact model class

Create a new class named Contact in the Web API project you just created and enter the code given below.

`namespace Refit_Demo   {       public class Contact       {           public int Id { get; set; }           public string FirstName { get; set; }           public string LastName { get; set; }           public string Address { get; set; }           public string Phone { get; set; }       }   }`

We’ll use the Contact class in the next section to work with data.

### Create the ContactRepository class

Next, we’ll create a repository class to work with the Contact data. For the sake of simplicity and brevity, we’ll store our data in a list in memory. You can feel free to change this implementation to store the data in a database as per your requirements. The ContactRepository class implements the IContactRepository. This interface contains the declaration of two methods, namely, the GetContact and the GetContacts methods. While the former returns one contact record based on the id passed to it as a parameter, the latter returns all contacts.

The following code listing illustrates both the IContactRepository interface and the ContactRepository class.

`public interface IContactRepository   {     public Contact GetContact(int id);       public List<Contact> GetContacts();   }   public class ContactRepository: IContactRepository   {       private readonly List<Contact> contacts = new List<Contact>();       public ContactRepository()       {           contacts = new List<Contact>()           {               new Contact()               { Id =1, FirstName = "Keaton", LastName = "Underwood",                   Address = "12/3 ABC Road, Chicago, USA",               Phone = "1234567890"},               new Contact(){ Id = 2, FirstName = "John", LastName = "Smith",                   Address = "12/3 ABC Road, New York, USA",               Phone = "0987654321"}           };       }       public Contact GetContact(int id)       {           return contacts.SingleOrDefault(c => c.Id == id);       }       public List<Contact> GetContacts()       {           return contacts;       }   }`

You can register an instance of type IContactRepository with the services collection in the Program.cs using the following piece of code.

`builder.Services.AddScoped<IContactRepository, ContactRepository>();`

This will enable you to use dependency injection to create an instance of type IContactRepository in the application.

### Create the API controller

Let us now create the controller class for our Contacts API. To do this, create a new API controller named ContactsController and replace the generated code with the following code.

`using Microsoft.AspNetCore.Mvc;   namespace Refit_Demo.Controllers   {       [Route("api/[controller]")]       [ApiController]       public class ContactsController : ControllerBase       {           private readonly IContactRepository _contactRepository;           public ContactsController(IContactRepository contactRepository)           {               _contactRepository = contactRepository;           }           [HttpGet]           public async Task<List<Contact>> Get()           {               return await _contactRepository.GetContacts();           }           [HttpGet("{id}")]           public async Task<Contact> Get(int id)           {               return await _contactRepository.GetContact(id);           }       }   }`

Note how we have used constructor injection to create an instance of type IContactRepository in the preceding code listing.

In the next sections, we’ll create a console application project and build the Refit client that will consume our contacts API.

## Create a .NET Core console application project in Visual Studio

Follow the steps outlined below to create a new .NET Core console application project in Visual Studio.

1.  Launch the Visual Studio IDE.
2.  Click on “Create new project.”
3.  In the “Create new project” window, select “Console App (.NET Core)” from the list of templates displayed.
4.  Click Next.
5.  In the “Configure your new project” window, specify the name and location for the new project.
6.  Click Next.
7.  In the “Additional information” window shown next, choose “.NET 8.0 (Long Term Support)” as the framework version you want to use.
8.  Click Create.

We’ll use this .NET Core console application project to create our Refit API client.

### Install the Refit NuGet package

To install Refit into your project, select the project in the Solution Explorer window, then right-click and select “Manage NuGet Packages.”

In the NuGet Package Manager window, search for the Refit package and install it. Alternatively, you can install the package(s) via the NuGet Package Manager console by entering the commands shown below.

PM> Install-Package Refit

### Create the Refit API client

Now replace the generated code in the Program.cs file with the following code listing.

`using Refit;   string baseAddress = "http://localhost:59904/";   var contactsAPI = RestService.For<IContactService>(baseAddress);   var contacts = await contactsAPI.GetContacts(); foreach (var contact in contacts)   {       Console.WriteLine($"{contact.Id} | {contact.FirstName} |       {contact.LastName}");   }   Console.ReadLine();   [Headers("Accept: application/json", "Content-type: application/json")]   public interface IContactService   {       [Get("/api/contacts")]       public Task<Contact> GetContact(int id);       [Get("/api/contacts")]       public Task<List<Contact>> GetContacts();   }   public class Contact   {       public int Id { get; set; }       public string FirstName { get; set; }       public string LastName { get; set; }       public string Address { get; set; }       public string Phone { get; set; }   }`

## Execute the application

Since there are two applications in this example, you should run both one by one. First, run the API application followed by the API client console application. When both applications have been launched, you’ll observe the data retrieved from the Contacts API application displayed at the console as shown in Figure 1.

![Refit ASP.NET Core](https://b2b-contenthub.com/wp-content/uploads/2024/07/Refit-ASPNET-Core.png?w=1024)

Figure 1. Your Refit API client in action.

IDG

Refit is a great choice for implementing HTTP REST API clients. Refit greatly simplifies the boilerplate code required to connect to and work with REST APIs in your ASP.NET Core applications. One important point to note is that when you’re using Refit, all requests must be asynchronous. Refit does not support synchronous network calls.
