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

**Program.cs**: This is the entry point of the application. 

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

