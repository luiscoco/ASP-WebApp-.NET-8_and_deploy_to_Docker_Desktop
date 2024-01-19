# How to create my first ASP WebApp with .NET 8 and how to deploy to Docker Desktop (in your local laptop)

## 1. Run Visual Studio and create the WebApp

We run Visual Studio 2022 Community Edition and we create a new project

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/793f6d74-610c-484f-a743-b84f6c7b25a0)

We select the project template

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/1d390e08-0871-4ecc-a884-601387e00a68)

We input the location and the project name

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/e5e64261-83aa-407d-af77-b199a17d5140)

We chose the new project main features

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/d5e90836-95b7-4a1c-ba04-695a5dd8c08b)

## 2. Project folder structure and main files description

### 2.1. Default Folder Structure

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/1213f16a-b3aa-4b8a-b4d2-ef4c3c72e8f3)



**wwwroot**

This folder contains static files like HTML, CSS, JavaScript, and image files. It's the root for web server requests.

**Controllers**

In an MVC (Model-View-Controller) application, this folder contains the controller classes. Controllers handle user input and interactions, and return responses.

**Models**

This folder holds the classes that represent the data of the application and the logic to manage that data. It's part of the MVC pattern.

**Views**

In MVC, the Views folder contains the Razor view files (.cshtml). These files combine HTML markup with C# code for dynamic rendering of content.

**Properties**

Contains configuration files, like launchSettings.json, which includes settings for launching the app, like environment variables and application URL.

**Dependencies**

This section in your solution explorer lists the packages, frameworks, and projects your app depends on.

**Optional Folders**

**Areas**: Used for organizing related functionality into a group, like admin panels.

**Migrations**: For Entity Framework Core, contains database migration files.

**Data**: Contains data access related code, often includes a **DbContext** class for **Entity Framework Core**.

## 3. Workflow after running the WebApp

### 3.1. Program.cs file is the entry point of the application

It sets up the web host and starts the application.

```csharp
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddControllersWithViews();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseRouting();

app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

### 3.2. The application entry point calls the Index action inside the Home controller

```csharp
using Microsoft.AspNetCore.Mvc;
using System.Diagnostics;
using WebApplication1.Models;

namespace WebApplication1.Controllers
{
    public class HomeController : Controller
    {
        private readonly ILogger<HomeController> _logger;

        public HomeController(ILogger<HomeController> logger)
        {
            _logger = logger;
        }

        public IActionResult Index()
        {
            return View();
        }
```

### 3.3. The Index action inside the Home controller calls the Views/Home/Index.cshtml view

```cshtml
@{
    ViewData["Title"] = "Home Page";
}

<div class="text-center">
    <h1 class="display-4">Welcome</h1>
    <p>Learn about <a href="https://learn.microsoft.com/aspnet/core">building Web apps with ASP.NET Core</a>.</p>
</div>
```
