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

This is the workflow summary:

**Program.cs** is the application entry point -> **https://localhost:44327/Home/Index** -> Call Index action inside the Home controller **Controllers/HomeController.cs** -> Call Home view (**Views/Home/Index.cshtml**) -> the Index.cshtl view is Rendered inside the **Views/Shared/_Layout.cshtml** as defined in **_ViewStart.cshtml**

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

### 3.2. The application entry point (/Home/Index) calls the Index action inside the Home controller (Controllers/HomeController.cs)

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

### 3.4. The Views/Home/Index.cshtml view is rendered inside the Views/Shared/_Layout.cshtml view

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"] - WebApplication1</title>
    <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
    <link rel="stylesheet" href="~/css/site.css" asp-append-version="true" />
    <link rel="stylesheet" href="~/WebApplication1.styles.css" asp-append-version="true" />
</head>
<body>
    <header>
        <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
            <div class="container-fluid">
                <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">WebApplication1</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target=".navbar-collapse" aria-controls="navbarSupportedContent"
                        aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="navbar-collapse collapse d-sm-inline-flex justify-content-between">
                    <ul class="navbar-nav flex-grow-1">
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </header>
    <div class="container">
        <main role="main" class="pb-3">
            @RenderBody()
        </main>
    </div>

    <footer class="border-top footer text-muted">
        <div class="container">
            &copy; 2024 - WebApplication1 - <a asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
        </div>
    </footer>
    <script src="~/lib/jquery/dist/jquery.min.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

## 4. Adding docker support to the WebApp application

We right click on the project name and we select the menu option **Add->Docker support...**

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/ea5979e7-aa8b-43f3-a891-bb291da84add)

We select the Linux operating system

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/95ba4ba5-4e81-4f6a-9888-4a98d8e81559)

This is the Dockerfile automatically created

```
#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WebApplication1.csproj", "."]
RUN dotnet restore "./././WebApplication1.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "./WebApplication1.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./WebApplication1.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication1.dll"]
```

We right click on the project name and we select the option **Open in Terminal**

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/1648d596-53ef-4770-98e9-3deba5a9085a)

We build the WebApp docker image running this command

```
docker build -t webapplication1 .
```

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/496186c8-f493-4399-9d99-a0bcca11f46e)

We can verify in Docker Desktop the new image

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/287b555d-28f9-48ca-8e14-0ba99cedb9bf)

## 5. Run your application with protocol HTTP

We run the docker image with this command

```
docker run -d -p 8080:8080 --name myapp webapplication1
```

We confirm the application is running in the endpoing: http://localhost:8080

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/abe4693a-dc1a-4c6b-a417-85cc88701d80)

We also see the running docker image in Docker Desktop

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/c88615f9-4f46-4f18-92a3-ccbc59284839)

## 6. Run your application with protocol HTTPS

Creating and using a **self-signed certificate** for local development involves a few steps. 

This process can vary slightly depending on your operating system. I'll outline the general steps and provide guidance for **Windows**:

**6.1. Generate a Certificate Using PowerShell**:

You can use PowerShell to create a self-signed certificate:

```
New-SelfSignedCertificate -DnsName localhost -CertStoreLocation cert:\LocalMachine\My
```

We copy the **Thumbprint**. We need it to look for our certificate inside the Manage Certificates Computer application in the **Personal/Certificates** folder 

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/c66e3a06-e7b9-4e18-b391-e1ab0a54a85e)

**Thumbprint**: 8CC1EAD679D28909645182A946212D1126AB9192

This command creates a certificate for localhost and places it in the local machine's certificate store.

**6.2. Export the Certificate**:

We look for the application **Manage Computer Certificates** (**Microsoft Management Console (MMC)**) and we run it

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/dd8150f1-f14c-479a-9343-9b579417818a)

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/94f217de-bd15-4349-9ce4-00357005722b)

We search for the new certificate inside the **Personal/Certificates** folder, also **Issue To: localhot** and with the **Thumbprint**: 8CC1EAD679D28909645182A946212D1126AB9192

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/920401f4-b5e5-4974-9bf5-2a1f20609f47)

We also check the certificate **Thumbprint**: 8CC1EAD679D28909645182A946212D1126AB9192

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/24f77aaa-2d96-4aed-8045-b3d6daadef3e)

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/8098ca69-c1f1-4f07-ab97-472f142b6515)

You need to **export the certificate** to a **.pfx** file with a password. You can do this through the **Microsoft Management Console (MMC)** or PowerShell.

We select the certificate and then select the menu option **Action->All Tasks->Export...**

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/7010c69d-1a56-4ea2-a963-d49d0fe23c62)

We click on the Next button

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/3f30e236-389c-471e-a742-f47effd87868)

We select the option **Yes, export the private key**

![image](https://github.com/luiscoco/ASP-WebApp-.NET-8_and_deploy_to_Docker_Desktop/assets/32194879/06c4b24f-2781-42d4-845d-bcade7abacd4)




**6.3. Trust the Certificate**:

You can trust the certificate by adding it to the "Trusted Root Certification Authorities" store, either through MMC or PowerShell.

**Configure ASP.NET Core**:

In your **appsettings.json** or programmatic configuration, specify the path to the .pfx file and the password you used during export:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "Kestrel": {
    "Endpoints": {
      "Https": {
        "Url": "https://*:8081",
        "Certificate": {
          "Path": "certificate.pfx",
          "Password": "123456"
        }
      },
      "Http": {
        "Url": "http://*:8080"
      }
    }
  }
}
```

Also modify the Dockefile to copy the certificate file into the Docker image: **COPY ["certificate.pfx", "."]**

```
# See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WebApplication1.csproj", "."]
RUN dotnet restore "./WebApplication1.csproj"
COPY . .
WORKDIR "/src"
RUN dotnet build "WebApplication1.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "WebApplication1.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Copy the certificate file into the Docker image
COPY ["certificate.pfx", "."]

ENTRYPOINT ["dotnet", "WebApplication1.dll"]
```

For accessing both protocol HTTP and HTTPS comment this line: **app.UseHttpsRedirection();** in **Program.cs** file


