# Getting Started with Create React App

# Steps

## <span style="color:lightgreen">PRE-SETUP</span>
<br/>

### * Create a folder that will hold multiple projects; do not create the project subfolders, just this one parent folder. The CLI for creating the projects will name and create the folders for you.

<br/>

## <span style="color:lightgreen">SETTING UP REACT PROJECT</span>
<br/>

### * Run the create-react-app package
Notice: Specifying the template as typescript causes the js (eg. index.js and App.js) files to be tsx files instead (eg. index.tsx and App.tsx).
```DOS
npx create-react-app reactweb --template typescript
```
### * Open Visual Studio from the new react folder, created above, reactweb
  
![](public/images/image_001.jpg)

<br/>

### * Install Bootstrap
```DOS
npm install bootstrap
```

### * Edit index.tsx file to import the Bootstrap CSS file
<br/>

#### Index.tsx file
```TS
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import "bootstrap/dist/css/bootstrap.min.css";                                                  <====

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```
### * Create a child directory under src called main and move App.tsx and App.css to the new directory. Be sure to update the import statement in index.tsx. 
![](public/images/image_002.jpg)


### In the App.css file, replace the contents with this. 
<br/>

#### App.css file
```CSS
.logo {
  height: 150px;
  cursor: pointer;
}

.subtitle {
  font-style: italic;
  font-size: x-large;
  color: coral;
}

.themeFontColor {
  color: coral;
}

tr {
  cursor: pointer;
}

```
### * In the main folder, create a header.tsx file with the content below. 
### NOTE: Copy over the file called GloboLogo.png from Pluralsight course and drop it in the main folder so that the reference to in the header.tsx file works.
<br/>

#### Header.tsx file
```TSX
import logo from "./GloboLogo.png";

type Args = {
  subtitle: string;
};

const Header = ({ subtitle }: Args) => {
  return (
    <header className="row mb-4">
      <div className="col-5">
        <img src={logo} className="logo" alt="logo" />
      </div>
      <div className="col-7 mt-5 subtitle">{subtitle}</div>
    </header>
  );
};

export default Header;

```


### Next edit the App.tsx file from the 1st section below to the 2nd one
<br/>

#### App.tsx file (previous code)
```TSX
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.tsx</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;

```
### NOTE: When you type in the header element, the import Header line is added automatically

<br/>

#### App.tsx file (replacement code)
```TSX
import React from 'react';
import logo from './logo.svg';
import './App.css';
import Header from './Header';

function App() {
  return (
    <div className="container">
      <Header subtitle="Providing houses all over the freakin world!!"></Header>
    </div>
  );
}

export default App;

```
### Test the changes by running npm start
![](public/images/image_003.jpg)



## <span style="color:lightgreen">SETTING UP API PROJECT</span>
<br/>
### *Make a new folder at the same level as the reactweb folder, call it api
<br>

### * Move to the api folder and run the following command:
#### &emsp; - The dotnet command will create a new project <br/> &emsp; - The name it will use is based on the name of it's folder. <br/> &emsp; - The type of project will be a webapi project and will have minimal scaffolding

```DOS
dotnet new webapi -minimal

```

### * Open VS Code from the folder, if you are prompted with the picture below, click on Yes to add the C# extension for building and debugging
<br/>

![](reactweb/public/images/image_004.jpg)


If you don't get the popup, then make sure you have installed the C# extension from OmniSoft ...

![](reactweb/public/images/image_005.jpg)

### * Your files should look like this, which shows it is in fact a minimal insstallation
<br/>

### As you can see there are no controllers, basically the whole API is written in the program file.

![](reactweb/public/images/image_006.jpg)


### This is the initial program file when creating a new project wih a minimal install

* The 1st line creates the builder object to configure services (dependency injection) and the middleware
* The next section configures the dependency injection that will add Swagger
    * The 1st line of this section adds what ApiExlporer to use for Swagger  
    * The 2nd line adds Swagger, which can now scan for all endpoints because of the Api Explorer that was injected
* The next thing after that, it creates an app object from the builder object built above, to then add middleware for the HTTPS pipeline
  * The 1st middleware section is for running Swagger, and only for the dev environment
    * Use Swagger creates a standardized definition of our APIs and puts that into JSON format
    * THe 2nd one, SwaggerUI, takes that JSON and transofrms it into a UI
  * The 2nd section is adding the ability to rediret
  * The 3rd section creates an array of temperature descriptions for the mock API that was built for this template
  * The 4th section defines an endpoint that responds to a get request directly without the need for controllers and a routing table
    * The 1st parameter is the relative URL of the endpoint
    * The 2nd parameter is a lambda to execute once the endpoint is hit
      * In this case it is returning a collection of weather forecast objects with a random temperature and summary. (a WeatherForecast object is created and assigned to the forecast variable, and the forecast variable is what is returned
    * The WithName makes it possible to refer to this endpoint by name
  * The final section, app.run(), is what then runs everything and in this case creates the active endpoint.


#### Program.cs
```C#
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

var summaries = new[]
{
    "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
};

app.MapGet("/weatherforecast", () =>
{
    var forecast =  Enumerable.Range(1, 5).Select(index =>
        new WeatherForecast
        (
            DateTime.Now.AddDays(index),
            Random.Shared.Next(-20, 55),
            summaries[Random.Shared.Next(summaries.Length)]
        ))
        .ToArray();
    return forecast;
})
.WithName("GetWeatherForecast");

app.Run();

record WeatherForecast(DateTime Date, int TemperatureC, string? Summary)
{
    public int TemperatureF => 32 + (int)(TemperatureC / 0.5556);
}
```

### * Edit the launchSettings.json file to:
* Remove the IIS settings part and the IIS Express part too.
* Remove the non protected HTTP server
* Change the port to 4000 (since the react app is on 3000, this easier to remember)

It should look like this

```JSON
{
  "profiles": {
    "api": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "launchUrl": "swagger",
      "applicationUrl": "https://localhost:4000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

### * Run the project, by typing the following from the terminal

```DOS
dotnet run
```
![](reactweb/public/images/image_007.jpg)

<br/>

![](reactweb/public/images/image_008.jpg)

![](reactweb/public/images/image_009.jpg)

![](reactweb/public/images/image_010.jpg)


## <span style="color:lightgreen">DEBUGGING</span>
Since we are using two applications using two different technology stacks we will be switching between both the react app and the api app in seprate instances of VS Code.

## <span style="color:lightblue">REACT (Debugging)</span>

### Open up React app and click on the create a launch.json file link. This will be the debugging configuration files to choose what URL and port to use for debugging (eg localhost:3000/)

![](reactweb/public/images/image_011.jpg)

#### * Once you click on it, you need to choose what browser you want to use for debugging

![](reactweb/public/images/image_012.jpg)

This will then bring you to the launch.json file.


#### launch.json
![](reactweb/public/images/image_013.jpg)

Edit the JSON file to used the proper host for debugging

```JSON
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "msedge",
            "request": "launch",
            "name": "Launch Edge against localhost",
            "url": "http://localhost:3000",
            "webRoot": "${workspaceFolder}"
        }
    ]
}
```


Prove it works by start it up, using the command below 

```
npm start
```
Then click on the debug button ... a separate instance of edge will be launched 

![](reactweb/public/images/image_014.jpg)

Move the browser out of the way, and set a breakpoint, and re-fresh the browswer to prove the debugging works. Press play to continue.

![](reactweb/public/images/image_015.jpg)

## <span style="color:lightblue">API (Debugging)</span>

In the previous section for setting up debugging for the React project, you had to create a launch.json file to setup the debugging configuration. You do not have to do that for this project though, as
* The launch.json was created already by .NET. when you made the project and
* You don't have to worry about what process to attach the debug to as well. Dotnet will automatically attach the debug to the dotnet process.

With this launch.json file, you get two configurations vs just the one like we saw in the one for React.
* The 1st section, called .NET Core Launch will build and run the project; you don't have to have it already running
* The 2nd section, called .NET Core attach, will assume the program is already running.

### launch.json
```JSON
{
    "version": "0.2.0",
    "configurations": [
        {
            // Use IntelliSense to find out which attributes exist for C# debugging
            // Use hover for the description of the existing attributes
            // For further information visit https://github.com/OmniSharp/omnisharp-vscode/blob/master/debugger-launchjson.md
            "name": ".NET Core Launch (web)",
            "type": "coreclr",
            "request": "launch",
            "preLaunchTask": "build",
            // If you have changed target frameworks, make sure to update the program path.
            "program": "${workspaceFolder}/bin/Debug/net6.0/api.dll",
            "args": [],
            "cwd": "${workspaceFolder}",
            "stopAtEntry": false,
            // Enable launching a web browser when ASP.NET Core starts. For more information: https://aka.ms/VSCode-CS-LaunchJson-WebBrowser
            "serverReadyAction": {
                "action": "openExternally",
                "pattern": "\\bNow listening on:\\s+(https?://\\S+)"
            },
            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            },
            "sourceFileMap": {
                "/Views": "${workspaceFolder}/Views"
            }
        },
        {
            "name": ".NET Core Attach",
            "type": "coreclr",
            "request": "attach"
        }
    ]
}
```

So to start the debugging
* Set a breakpoint
* Move the debug section in VS Code and launch the debugger. To get past the breakpoint click on continue at the top. The browswer will then automatically open, you then put in the Get endpoint to prove the app still works.

![](reactweb/public/images/image_016.jpg)

And then access an enpoint from a browser to prove the debugging works.

![](reactweb/public/images/image_017.jpg)

## <span style="color:lightgreen">CREATE SQLITE DATABASE & TABLE & DATA</span>
<br/>

### <span style="color:lightblue">Install EF Packages


### * Install the proper version of SQL Lite by specifying the version # of >NET that it supposed to run with

```DOS
dotnet add package Microsoft.EntityFrameworkCore.Sqlite --version 6.*
```

After this install, create a using section for the Microsoft.EntityFrameworkCore namespace in the project file, so that you don't have to add the using statement over and over at the top of the pages that use it across the app. If you find that you have to install because it is not included with the Sqlite package then install it like this.


### * Install Entity Framework Core
```DOS
dotnet add package Microsoft.EntityFrameworkCore --version 6.*

```

### api.csproj
```C#
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

  <ItemGroup>
    <Using Include="Microsoft.EntityFrameworkCore" />
  </ItemGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite" Version="6.*" />
    <PackageReference Include="Swashbuckle.AspNetCore" Version="6.2.3" />
  </ItemGroup>

</Project>
```

### <span style="color:lightblue">Create the Entity
* Data folder - Create this to store the database information, and 
* HouseEntity.cs - create this file in the data folder to create a new entity.
  * Entities are classes that are modeled arund the tables and columns we haev in the database.
* Fill the class out as follows, note the following:
  * No need for using statements at the top
  * The name of the table will be House
  * The field names below are what you will see in the table. The ? means that the data can be null
  * Because the 1st field is called Id, it will be the primary key assigned by the database, when records are inserted

```C#
public class HouseEntity
{
    public int Id { get; set; }
    public string? Address { get; set; }
    public string? Country { get; set; }
    public string? Description { get; set; }
    public int Price { get; set; }
    public string? Photo { get; set; }
}
```

### <span style="color:lightblue">Create the DB Context
* Entities are used by a class called the database context
* Use this DB Context class to query and manipulate data
* HouseDbContext.cs - Create this file in the data folder, with the content below:
  * The property of the type DbSet of HouseEntity called houses , is a  DbSet collection that represents the database table itself, which will have all the columns defined in the entity. Notice that the initial value is set to an empty DbSet, using the convenient Set method.
  * The OnConfiguring section configures which database to use. Notice that it is an override in the HouseDbContext class for the OnConfiguring method.
      * In the 1st two lines we determine where the DB file should be (Sqlite uses a file not a server like SQL Server). The LocalApplicationData is a different path on different computers, in Windows 11 for me, it is the following: C:\Users\Carlo\AppData\Local
      * In the final line, we are using the DbContext's Options Builder that we passed in to tell EF Core we are using SQLite. We pass in a connection string that points to the path and specifies the file name of the DB.

### HouseDbContext.cs
  ```C#
public class HouseDbContext : DbContext
{
    public DbSet<HouseEntity> Houses => Set<HouseEntity>();
    
    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
        var folder = Environment.SpecialFolder.LocalApplicationData;
        var path = Environment.GetFolderPath(folder);
        options.UseSqlite($"Data Source={Path.Join(path, "houses.db")}");
    }
}
  ```
### <span style="color:lightblue">Get Data Ready for Migration

### * Seed Prep - This step creates the data that will be used to create the migration scripts explained in the next section for migration scripts setup 
* SeedData.cs - Create this file in the data folder
* Load the following into it ...
  * Notice this is a static class as it is just a functional method and does not need an instance
  * The ModelBuilder object that is passed to the Seed method is part of EF Core.
  * The remaining code seeds the HouseEntities
    * The HasData method will check if the listed data is already present in the DB.If not it will create it. BUT not directly; it will be seeded by a migration (covered in the next section).

### SeedData.cs
```C#
public static class SeedData
{
    public static void Seed(ModelBuilder builder)
    {
        builder.Entity<HouseEntity>().HasData(new List<HouseEntity> {
            new HouseEntity {
                Id = 1,
                Address = "12 Valley of Kings, Geneva",
                Country = "Switzerland",
                Description = "A superb detached Victorian property on one of the town's finest roads, within easy reach of Barnes Village. The property has in excess of 6000 sq/ft of accommodation, a driveway and landscaped garden.",
                Price = 900000
            },
            new HouseEntity
            {
                Id = 2,
                Address = "89 Road of Forks, Bern",
                Country = "Switzerland",
                Description = "This impressive family home, which dates back to approximately 1840, offers original period features throughout and is set back from the road with off street parking for up to six cars and an original Coach House, which has been incorporated into the main house to provide further accommodation. ",
                Price = 500000
            },
            new HouseEntity
            {
                Id = 3,
                Address = "Grote Hof 12, Amsterdam",
                Country = "The Netherlands",
                Description = "This house has been designed and built to an impeccable standard offering luxurious and elegant living. The accommodation is arranged over four floors comprising a large entrance hall, living room with tall sash windows, dining room, study and WC on the ground floor.",
                Price = 200500
            },
            new HouseEntity
            {
                Id = 4,
                Address = "Meel Kade 321, The Hague",
                Country = "The Netherlands",
                Description = "Discreetly situated a unique two storey period home enviably located on the corner of Krom Road and Recht Road offering seclusion and privacy. The house features a magnificent double height reception room with doors leading directly out onto a charming courtyard garden.",
                Price = 259500
            },
            new HouseEntity
            {
                Id = 5,
                Address = "Oude Gracht 32, Utrecht",
                Country = "The Netherlands",
                Description = "This luxurious three bedroom flat is contemporary in style and benefits from the use of a gymnasium and a reserved underground parking space.",
                Price = 400500
            }
        });
    }
}
```
### * DBContext - Update this class to call the seed method when the DBContext is loaded
  * Call the seed method from the Seed class, by adding this code at the end. 
  * This is another override to the DBContext's method, this time the OnModelCreating method.
  * Notice that it takes in the ModelBuilder as the parameter

```C#
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        SeedData.Seed(modelBuilder);
    }
```
### * The DBContext class should now look like this

### HouseDbContext.cs
  ```C#
public class HouseDbContext : DbContext
{
    public DbSet<HouseEntity> Houses => Set<HouseEntity>();
    
    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
        var folder = Environment.SpecialFolder.LocalApplicationData;
        var path = Environment.GetFolderPath(folder);
        options.UseSqlite($"Data Source={Path.Join(path, "houses.db")}");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        SeedData.Seed(modelBuilder);
    }
}
  ```

### * NOTE:
  * So far, all we have done is model our existing database, but that is JUST a model
    * We have not added any instructions yet on how to create it.
    * The DbContext, is just a class, nothing has been created to actually call the OnModelCreating event yet, passing in the modelBuilder.
  * To create these instructions, we will need to add a migration.
  * Before we so that though, 1st add another package for this. 

### <span style="color:lightblue">Migration Scripts Setup 
* EFCore Design Package - Installation
  * This wil faciliate working with migrations
  * Run the following from the terminal to install it

```DOS
dotnet add package Microsoft.EntityFrameworkCore.Design --version 6.*
```
  * Migration sql scripts - create this utilizing the above package and running the following command
    * Note:
      * Initial is just a name of the migration, we can call it anything, we are calling it initial since it is the initial seeding
      * Only the data for the migration has been created at this time, it has yet to be updated/inserted into the database.

```DOS
dotnet ef migrations add initial
```

* After the scripts for migration has been completed, a Migrations folder will be automatically created at the root, with the migration SQL statements for the insert.
* All of the code for how to create the database and seed the initial data is there
* All that is left is to run it

### <span style="color:lightblue">Run Migration Scripts

### * Seed the Data (Execute the Migration Scripts)
* Execute the following from the terminal

```DOS
dotnet ef database update
```
If you get an error that the package can't be found, then install it

```DOD
dotnet tool install --global dotnet-ef
```

### * The database has been created and seeded
  * Prove it is working by checking out the app folder shown above to see if the file is present and/or
  * Confirm using the SQLite Explorer to open the database and check it out

![](reactweb/public/images/image_018.jpg)


## <span style="color:lightgreen">ENDPOINTS</span>
* Add the Endpoints to retrieve data from the Database

### <span style="color:lightblue">Program.cs - Update the file
* DBContext - Add the DBContext to the Dependency Injection container in the Program.cs file, so that it will become available in our application.
* AddDbContext - Use this special extension method to do this

```C#
builder.Services.AddDbContext<HouseDbContext>(options => options.UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking));
```
* Delete the summaries variable section from the file
* Delete the body of the get for the weather endpoint (we will replace with our own)
* Rename the get to /houses
* Add the following inside the houses get method

```C#
app.MapGet("/houses", (HouseDbContext dbContext) =>
dbContext.Houses);
```

The file should look like this:

### Program.cs
```C#
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
builder.Services.AddDbContext<HouseDbContext>(options => options.UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking));

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.MapGet("/houses", (HouseDbContext dbContext) =>
dbContext.Houses);

app.Run();
```

NOTE: If you try to run this now, you will get an error. The DBContext class requires a default constructor. There for add the following to that class:

```C#
    public HouseDbContext(DbContextOptions<HouseDbContext> options) : base(options) { }
```
The DBContext class should now look like this

### HouseDbContext.cs
  ```C#
public class HouseDbContext : DbContext
{
    public HouseDbContext(DbContextOptions<HouseDbContext> options) : base(options) { }
    public DbSet<HouseEntity> Houses => Set<HouseEntity>();
    
    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
        var folder = Environment.SpecialFolder.LocalApplicationData;
        var path = Environment.GetFolderPath(folder);
        options.UseSqlite($"Data Source={Path.Join(path, "houses.db")}");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        SeedData.Seed(modelBuilder);
    }
}
  ```

# <span style="color:lightgreen">SEPARATION OF CONCERNS</span>
* Entity Objects
* DTO Objects
* Repository Layer

Enity Objects - You should never send the actual Entity straight across the wire, directly. The Entity is the closest thing to the database, with items in it related to the database schema.

DTO Objects - Convert the Entity Object into a DTO (Data Transfer Obect) 1st,before sending across the wire. This is at a minimum, we will look at the repository layer later. The DTO object is just for sending across the wire here, it would still be converted into a client object representing that same data, but for the client. The following is a representation of this concept:

* The HouseEntity is converted to a House DTO
* The House DTO is converted to House React object

![](reactweb/public/images/image_019.jpg)

The following is an example of how to convert the Entity Object into a DTO
* Create the DTO object
* Send the DTO object in the endpoint

  * DTO Object
    * HouseDto.cs - Add this class to the Data folder
    * Put in the following code:
      * Note: Instead of a class, we are using a new feature from C# 9, a record. The main difference between a class type and a record type in C# is that a record has the main purpose of storing data, while classes define responsibility. Records are immutable, while classes are not; therefore records are faster than classes.

### HouseDto.cs
```C#
public record HouseDto(int Id, string? Address, string? Country, int Price);
```
* Next update the Program.cs file to convert the Entity Object to this DTO object

### From:
```C#
app.MapGet("/houses", (HouseDbContext dbContext) =>
dbContext.Houses);
```
### To:
```C#
app.MapGet("/houses", (HouseDbContext dbContext) =>
dbContext.Houses.Select(h => new HouseDto(h.Id, h.Address, h.Country, h.Price)));
```

* The completed Program.cs file should look like this:

### Program.cs
```C#
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
builder.Services.AddDbContext<HouseDbContext>(options => options.UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking));

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.MapGet("/houses", (HouseDbContext dbContext) =>
dbContext.Houses.Select(h => new HouseDto(h.Id, h.Address, h.Country, h.Price)));

app.Run();
```

* Confirm it still works, notice the use of the JSON extension used, called JSON Lite

![](reactweb/public/images/image_020.jpg)

![](reactweb/public/images/image_021.jpg)


Repository

In the above example, the endpoint is doing the work, not only is converting the Entity object into a DTO object, it is also doing a select statement.

Good practice is to separate the concerns even more, so that the endpoint is just an endpoint.

* HouseRepository.cs
  * Create this file to do the work that the Program file was doing.
    * Convert the Entity to DTO
    * GetAll - Make this the method that queries from the DB through the entity and return the DTO to the caller
  * Add an interface to the class, for the class to inherit from. The reason behind this, is for dependency injection, you will be able to swap it out with another if needed such as for testing, etc.
    * The DB Context will be injected into the repository instead of the program file and
    * The repository type (not the repository itself, hence why we made the interface so we can create different repositories as long as it. As long as the repository ,can inherit from the IRepository you can create other repositories, like for testing et. ) will be injected into the program file instead of the DBContext


*Program.cs
* Update the depencency injection containter, to register the repository using its interface

```C#
builder.Services.AddScoped<IHouseRepository, HouseRepository>();
```

* Update the endpoint, to instead of injecting the DBContext, inject the repository interface instead. We inject the repository Interface and not the actual repository, because the registration above already has what repository to use when injecting the interface. So this way you can easily swap out repositories in the registration section without having to change code (IDEAL for unit testing)

### From:
```C#
app.MapGet("/houses", (HouseDbContext dbContext) =>
dbContext.Houses.Select(h => new HouseDto(h.Id, h.Address, h.Country, h.Price)));
```

### To:
```C#
app.MapGet("/houses", (IHouseRepository repo) => repo.GetAll());
```
The Program file should now look like this:

### Program.cs
```C#
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
builder.Services.AddDbContext<HouseDbContext>(options => options.UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking));
builder.Services.AddScoped<IHouseRepository, HouseRepository>();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.MapGet("/houses", (IHouseRepository repo) => repo.GetAll());

app.Run();
```
* Run the application to make sure it still works

![](reactweb/public/images/image_022.jpg)


# <span style="color:lightgreen">CONSUMING THE ENDPOINTS WITH USESTATE</span>

The next goal on the React application is to display the list of houses coming from the API. 

Since we're using Typescript, we need a type in a central place that represents a house DTO. 

* types folder - Create this folder in the src folder
* house.ts - Add this file to the new types folder.
  * Create the house object and export the new type with properties that are similar to the DTO on the API side. 
  * The file should look like this:

### house.ts
```C#
export type House = {
    id: number;
    address: string;
    country: string;
    description: string;
    price: number;
    photo: string;
  };
```

* config.ts - Create this file directly under the src directory
  * Define an object with configuration information
  * The file should like the following:
      * The 1st line adds a central place to store the base URL of the API
      * The 2nd gives us the needed export for other pages to import that information

### config.ts
```ts
const Config = {
  baseApiUrl: "https://localhost:4000",
};

export default Config;
```

* House Folder - Create this folder inside the src folder, that will hold all that center around houses
* HouseList.tsx - Add this file to the folder and update it with the following
  * A constant to return the list of houses in TSX
      * useState - Setup
        * useState variable - Setup the useState function assigns the House array to the houses variable holding the state.
        * The setHouse method - Assigns the current state (because of this the state will always show as changed whenever the page refreshes. We will address that in the next section.)
      * fetchHouses - create this function
        * Sends the API request to the server configured in the config.ts file, to the houses endpoint
          * Returns the data to the houses state variable
        * setHouses sets the state by comparing the state of the houses state variable to the previous state, (the state changes with every new render (this will be fixed in the next section to only re-render the 1st time)
      * fetchHouses - run the function
      * return - return the TSX code that loops through all of the records in the houses collection in the tbody element


### HouseList.tsx
```C#
import { useState } from "react";
import config  from "../config";
import { House } from "../types/house";

const HouseList = () => {
  const [houses, setHouses] = useState<House[]>([]); 

  const fetchHouses = async () => {
    const rsp = await fetch(`${config.baseApiUrl}/houses`);
    const houses = await rsp.json();
    setHouses(houses);
  };
  fetchHouses();

  return (
    <div>
      <div className="row mb-2">
        <h5 className="themeFontColor text-center">
          Houses currently on the market
        </h5>
      </div>
      <table className="table table-hover">
        <thead>
          <tr>
            <th>Address</th>
            <th>Country</th>
            <th>Asking Price</th>
          </tr>
        </thead>
        <tbody>
          {houses.map((h) => (
            <tr key={h.id}>
                <td>{h.address}</td>
                <td>{h.country}</td>
                <td>{h.price}</td>
            </tr>
            ))}
        </tbody>
      </table>
    </div>
  );
};

export default HouseList;
```
* If you test this right now you will get an issue with CORS, so you now have to update the Program.cs file
  * Add the CORS() service
  * Add authorization for the client in the middleware

API PROJECT - ALLOW CROSS-ORIGIN RESOURCE SHARING

Adding CORS service
```C#
builder.Services.AddCors();
```

Adding authorization to the client in the middleware
```C#
app.UseCors(p => p.WithOrigins("http://localhost:3000").AllowAnyHeader().AllowAnyMethod().AllowCredentials());
```

The program file should now look like this:

### Program.cs
```C#
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
builder.Services.AddCors();
builder.Services.AddDbContext<HouseDbContext>(options => options.UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking));
builder.Services.AddScoped<IHouseRepository, HouseRepository>();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseCors(p => p.WithOrigins("http://localhost:3000").AllowAnyHeader().AllowAnyMethod().AllowCredentials());

app.UseHttpsRedirection();

app.MapGet("/houses", (IHouseRepository repo) => repo.GetAll());

app.Run();
```
* Test this by launching the API app 1st and then the React app

![](reactweb/public/images/image_023.jpg)

* REACT NOTE

  * You will see it work, but
    * We are using the state feature to detect when to run the fetch
    * The method that assigns the state keeps assigning a new state every time the page refreshes. So it will continuously keep running the fetch.
    * (If you go into the dev tools you can see it fetching over and over). 


  * Therefore
    * We will need to do some recoding
      * The state comparison should not be indefinite, and instead compare to an empty array, and only run therefore if empty. 
      * Also, while we are at it, we should refactor for separation of concerns, we should not be fetching from the API in the same component that renders the return set from the API



# <span style="color:lightgreen">CONSUMING ENDPOINTS <br> &nbsp; - SEPARATON OF CONCERNS AND  <br> &nbsp; - USING THE EFFECT FEATURE FOR API REQUESTS</span>
* Separation of concerns
  * Begin by moving the fetch to a different folder
    * Hooks - Create this folder inside the src folder
    * HouseHooks.ts - Create this typescript file to do the fetch with the following code:
      * Move the three imports here from the HouseList.tsx file to here
      * Create a function that returns a House array and uses the useState and fetching logic in the HouseList.tsx file
      * Return the array of houses
      * Export the function so that other components can use it
      * The completed code should look like this

### HouseHooks.ts
```C#
import { useState } from "react";
import config  from "../config";
import { House } from "../types/house";

const useFetchHouses = (): House[] => {
  const [houses, setHouses] = useState<House[]>([]); 

  const fetchHouses = async () => {
    const rsp = await fetch(`${config.baseApiUrl}/houses`);
    const houses = await rsp.json();
    setHouses(houses);
  };

return houses;
}

export default useFetchHouses;
```
* The completed stripped down version of HouseList.tsx should now look like this:

### HouseList.tsx
```C#
import useFetchHouses from "../hooks/HouseHooks";

const HouseList = () => {
  const houses = useFetchHouses();

  return (
    <div>
      <div className="row mb-2">
        <h5 className="themeFontColor text-center">
          Houses currently on the market
        </h5>
      </div>
      <table className="table table-hover">
        <thead>
          <tr>
            <th>Address</th>
            <th>Country</th>
            <th>Asking Price</th>
          </tr>
        </thead>
        <tbody>
          {houses.map((h) => (
            <tr key={h.id}>
                <td>{h.address}</td>
                <td>{h.country}</td>
                <td>{h.price}</td>
            </tr>
            ))}
        </tbody>
      </table>
    </div>
  );
};

export default HouseList;
```
* Even though we have done the separation of concerns at this point, we are still refetching on each re-render because of using the useState feature.

* Therefore, we need to use the useEffect hook, that is built into React
* useEffect
    * The 1st parameter takes a function
    * The 2nd prameter takes in an array of dependencies
      * When one of the configured dependencies changes the funtion will fire
      * The empty function looks like this

```TS
useEffect (() => {
 <the function>
}, [dependencies]);
```
  * Putting this to use in the HouseHooks.ts fetching logic, would look like this
  * Notice that in this case the dependency is an empty array, so we are saying to only run the code that is inside it if the array is empty.

  ```TS
import { useEffect, useState } from "react";
import config  from "../config";
import { House } from "../types/house";

const useFetchHouses = (): House[] => {
  const [houses, setHouses] = useState<House[]>([]); 

  useEffect (() => {
    const fetchHouses = async () => {
        const rsp = await fetch(`${config.baseApiUrl}/houses`);
        const houses = await rsp.json();
        setHouses(houses);
      };
      fetchHouses();
   }, []);


return houses;
}

export default useFetchHouses;
  ```
* Test it out by going in to the dev tools and validating the request is only firing once.

![](reactweb/public/images/image_024.jpg)

# <span style="color:lightgreen">CONSUMING THE ENDPOINTS WITH USEQUERY AND AXIOS</span>
## PUSHING DATA TO THE SCREEN
* We will now code for showing updates made to the page you are sitting on, while other users are updating the data from their sessions, and you are just looking at it. It will refresh with the new data, similar to SignalR.
* The useEffect method that was used above for helping us control state worked just find for your state, but that state is JUST at the session level, per request. It is not a global state, so that the state of the data is shared for ALL requests, one state for ALL requests, not one state per request.
* Even if there were no other requests, the useEffect method above, is not JUST per request, per session, but it is also per component. Each differenct component would be using their own version of the fetch, their own version of the state, causing the same fetch over and over per each component used. EG. if the useEffect is used in 10 separate components, the data will be fetched 10 different times.
* Also, the fetch method above does not have a retry mechanism for glitches with network connections, etc. where the problem is usually temporary.
* To solve all of the above problems we need:
  * A shared state
  * Caching
  * Controlled re-fetches (some way of invalidating the cache and a re-fetch occurs)
  * Retries
* Theoretically, we could write the code for all of this ourselves, but it would be very difficult to get it right and more importantly we would just be re-inventing the wheel anyway. So ...
  * React Query - We will use the React Query for the above ... so install this to get started ALSO, instead of using fetch .. 
  * Axios - Install Axios, a libarary that makes working with HTTP requests way more robust than using the simple fetch API provided by JavaScript AND it integrates better with React Query

```DOD
npm install axios react-query
```
# HouseHooks.ts

* To get started, after the packages have been installed, use the code from the packages to replace the code in the custom hook ...

  * Replace the code in the HouseHooks.ts file
    * Replace the acode in the useFetchHouses with the following code that uses React Query, and remove the house array typing for the return type, we want to infer the return type instead

```C#
const useFetchHouses = () => {
  const useFetchHouses = () => {
    return useQuery<House[], AxiosError>("houses", () =>
      axios.get(`${Config.baseApiUrl}/houses`).then((resp) => resp.data)
    );
  };
}
```
The file used to look like this, but should now look like the next one below:

### HouseHooks.ts (Previous)
```TS
import { useEffect, useState } from "react";
import config  from "../config";
import { House } from "../types/house";

const useFetchHouses = (): House[] => {
  const [houses, setHouses] = useState<House[]>([]); 

  useEffect (() => {
    const fetchHouses = async () => {
        const rsp = await fetch(`${config.baseApiUrl}/houses`);
        const houses = await rsp.json();
        setHouses(houses);
      };
      fetchHouses();
   }, []);


return houses;
}

export default useFetchHouses;
```
The new code for React Query and Axios:
* The code looks a lot cleaner
* We don't need a call to useState anymore, that is because ReactQuery is taking care of managing the state for us
* ReactQuery exposes its functionality using hooks
  * useQuery is used to define the query, something that is fetching data
  * useQuery is generic and the code below is using two generic parameters
    * The 1st is the type of data we expect to get, in this case an array of house types
    * The 2nd is the type we expect to get back when something goes wrong, and in this case the type is Axios Error which is built into Axios
  * useQuery has two parameters
    * The 1st parameter is a string; the fetched data will be cached on the server with the name in that string, in this case "houses" SO ... when another component uses this same query with that string, it will pull the data from the cache (9f it is still valid of course) and not requery the database. This other component can also be a component that uses our custom useFetchHouses hook.
  * The 2nd parameter is a function that actually gets the data; React Query doesn't care how you do it, but since Axios is an easier way to do it, we are using it to do the fetch instead of fetch.
    * So the get request uses Axios, not fetch
      * The 1st parameter is the URL it should use to get the data from
      * When it is done fetching, the 2nd part of the get deserializes the return set


### HouseHooks.ts (new)
```TS
import axios, { AxiosError } from "axios";
import { useQuery } from "react-query";
import Config from "../config";
import { House } from "../types/house";

const useFetchHouses = () => {
  return useQuery<House[], AxiosError>("houses", () =>
    axios.get(`${Config.baseApiUrl}/houses`).then((resp) => resp.data)
  );
};

export default useFetchHouses;
```

# HouseList.tsx
* We must now modify the HouseList.tsx file
  * We are now getting an object back from the HouseHooks that reflects the state of the request that is being executed
  * To get to the houses returned from the API, we have to destructure our data, which is of type house . So where we had the houses object at the top and in the tsx for mapping, we replace that with the data object.
    *NOTE: Where we do the mapping we have to account for the possibility of the data being undefined while the request is still executing, hence the special syntax to see if the data is still falsey before we do the mapping
  * The file should now look like this:

```TS
import useFetchHouses from "../hooks/HouseHooks";

const HouseList = () => {
  const {data} = useFetchHouses();

  return (
    <div>
      <div className="row mb-2">
        <h5 className="themeFontColor text-center">
          Houses currently on the market
        </h5>
      </div>
      <table className="table table-hover">
        <thead>
          <tr>
            <th>Address</th>
            <th>Country</th>
            <th>Asking Price</th>
          </tr>
        </thead>
        <tbody>
          {data && data.map((h) => (
            <tr key={h.id}>
                <td>{h.address}</td>
                <td>{h.country}</td>
                <td>{h.price}</td>
            </tr>
            ))}
        </tbody>
      </table>
    </div>
  );
};

export default HouseList;
```

# index.tsx
* Add a line for adding React Query relies on an object of type  queryClient, so we have to instantiate that to an object to then used the QueryClientProvider and make it available to all child components

* Test to see if the application still works

![](reactweb/public/images/image_025.jpg)

* We mentioned re-tries at the beginning of this session, but there is no need for special code to do this. All of that can be done by simply configuring React Query. If you want to use the defaults they are as follows:
  * Cached data will be marked as stale right after the the fetch
  * But the data will only be fetched
    * when a new component is rendered that uses the query or 
    * or the browser is getting a focus after another window got the focus, this might solve the problem too of a user looking at stale data 
    * or when the network is reconnected, because the connection was previously lost
    * The default for cache is 5 minutes
    * Retries will be done 3 times automatically
    * To change the defaults go to React Query documentation


# <div style="margin-top: 50px; color:lightgreen">FORMATTING</div>
* In the screenshot above, the prices are not formatted.
* We will format this in this section using the JavaScript International namespace ... we will add this in the config.ts file ...

* The 1st parameter specifies the culture to used for the format
* The other parameter is a configuration object
  * style = currency
  * currency = USD
  * maximum # of fraction digits = 0
* Of course, don't forget to export it so other components can import it

```TS
const currencyFormatter = Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
  maximumFractionDigits: 0,
});

export { currencyFormatter };
```
The completed should now look like this

### config.ts
```TS
const Config = {
  baseApiUrl: "https://localhost:4000",
};

const currencyFormatter = Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
  maximumFractionDigits: 0,
});

export default Config;
export { currencyFormatter };
```

* Next, edit the HouseList.tsx fle to use the new International Namespace configuration for currency
  * Add an import to deconstruct the currencyFormatter object created above in the config.ts file

```TS
import { currencyFormatter } from "../config";
```

* Format the price
```TS
...
<td>{currencyFormatter.format(h.price)}</td>
...
```

* While we are formatting the price, let's also add the status and isSuccess objects, that come back with the data object. As a reminder the axios get method returns a response object called resp in our method (see the HouseHooks.ts file). Included in that ovject is the data object shown below, and also the status and isSuccess objects as well.

* To use it, deconstruct them along with the data object returned back from the useFetchHouses() method at the top

```TS
const { data, status, isSuccess } = useFetchHouses();
```
* The status file is a string with the following values

![](reactweb/public/images/image_026.jpg)


* The isSuccess is a simple boolean; what is great about this is that you don't have to check the string for a specific value to determine if the query/response worked.

* The completed file should look like this

HouseList.tsx
```TS
import useFetchHouses from "../hooks/HouseHooks";

const HouseList = () => {
  const { data, status, isSuccess } = useFetchHouses();

  return (
    <div>
      <div className="row mb-2">
        <h5 className="themeFontColor text-center">
          Houses currently on the market
        </h5>
      </div>
      <table className="table table-hover">
        <thead>
          <tr>
            <th>Address</th>
            <th>Country</th>
            <th>Asking Price</th>
          </tr>
        </thead>
        <tbody>
          {data && data.map((h) => (
            <tr key={h.id}>
                <td>{h.address}</td>
                <td>{h.country}</td>
                <td>{h.price}</td>
            </tr>
            ))}
        </tbody>
      </table>
    </div>
  );
};

export default HouseList;
```

* To start using the objects we added above, status and isSuccess, we will create an API Status component to use them (API because it is related to the API since that is what we want to get the status on)
* APIStatus.tsx - So create this file in the src folder; is is the src folder and not in the other folders, because could be used by all of the other components to check the status.

* The code should look like this:
  * Args - Create a type for the status properties, we call it Args, because this will be called by other components where they will pass in the status object that as we said above can only have one of these values
  * Create the method that will return the HTML that will be based on the value of the status object that was passed in to this component. The destructured "status" out object in the method is the parameter for passing the status obect, then based on the string value returns the appropriate html.
  

```C#
type Args = {
  status: "idle" | "success" | "error" | "loading";
};

const ApiStatus = ({ status }: Args) => {
  switch (status) {
    case "error":
      return <div>Error communicating with the data backend</div>;
    case "idle":
      return <div>Idle</div>;
    case "loading":
      return <div>Loading..</div>;
    default:
      throw Error("Unknown API state");
  }
};

export default ApiStatus;
```



# <div style="margin-top: 50px; color:lightgreen">NEXT SESSION</div>


# <div style="margin-top: 50px; color:lightgreen">NEXT SESSION</div>


# <div style="margin-top: 300px; color:lightgreen">THE END</div>

## <span style="color:lightgreen">PADDING</span>
<br/>
<br/>
===================================================================================

<br/>
<br/>
<br/>
<br/>
<br/>















































# <span style="color:lightgreen">ADDED BY REACT</span>

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).


## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).
