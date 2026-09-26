Task Manager – ASP.NET Core + EF Core + SQL Server
A simple full-stack task management app:
Backend: ASP.NET Core Web API (.NET 8)
Database: SQL Server (LocalDB by default)
ORM: Entity Framework Core
Frontend: Plain HTML/CSS/JavaScript served from `wwwroot`
Project structure
```
TaskManagementApp/
├── TaskManagementApp.csproj
├── Program.cs
├── appsettings.json
├── appsettings.Development.json
├── Controllers/
│   └── TasksController.cs
├── Models/
│   └── TaskItem.cs
├── Data/
│   └── AppDbContext.cs
├── Properties/
│   └── launchSettings.json
└── wwwroot/
    ├── index.html
    ├── css/style.css
    └── js/app.js
```
Prerequisites
Visual Studio 2022 (Community edition is fine) with the ASP.NET and web development workload installed.
SQL Server LocalDB – this is installed automatically with Visual Studio's "ASP.NET and web development" workload (via SQL Server Express LocalDB component). You do NOT need a full SQL Server install.
If you'd rather use a full SQL Server instance instead of LocalDB, just change the connection string in `appsettings.json`.
How to open and run this project in Visual Studio
Create the project folder structure exactly as shown above, and copy each file's contents into the matching file (instructions per-file are below). The easiest path:
Open Visual Studio → Create a new project → ASP.NET Core Web API → name it `TaskManagementApp`.
When prompted, choose .NET 8.0, and uncheck "Use controllers" is fine to leave checked (we use controllers), and uncheck "Enable OpenAPI support" (we add Swagger manually) — or leave it checked, it won't conflict.
Once the project is created, delete the sample `WeatherForecastController.cs` and `WeatherForecast.cs` files if Visual Studio generated them.
Then add the files below into the matching folders (right-click project → Add → New Folder / New Item).
Install NuGet packages (Visual Studio usually restores these automatically from the `.csproj`, but if not):
Tools → NuGet Package Manager → Manage NuGet Packages for Solution
Install:
`Microsoft.EntityFrameworkCore.SqlServer`
`Microsoft.EntityFrameworkCore.Design`
`Microsoft.EntityFrameworkCore.Tools`
`Swashbuckle.AspNetCore` (for Swagger; usually included by default in the Web API template)
Create the database migration (this generates the SQL Server tables from the `TaskItem` model):
Open Tools → NuGet Package Manager → Package Manager Console
Run:
```
     Add-Migration InitialCreate
     Update-Database
     ```
This creates a `Migrations` folder and applies the schema to your local SQL Server (LocalDB) database named `TaskManagementDb`.
(Note: `Program.cs` also calls `db.Database.Migrate()` automatically at startup, so once the `Migrations` folder exists, just running the app will keep the database up to date too.)
Run the project (press `F5` or click the green ▶ Run button).
The app will open your browser automatically to the task list UI.
Swagger API docs are available at `/swagger` while in development mode.
Where each file goes
File	Location in project
`TaskManagementApp.csproj`	Project root (already created by Visual Studio; just make sure `PackageReference` entries match)
`Program.cs`	Project root
`appsettings.json`	Project root
`Models/TaskItem.cs`	New folder `Models` → new file `TaskItem.cs`
`Data/AppDbContext.cs`	New folder `Data` → new file `AppDbContext.cs`
`Controllers/TasksController.cs`	Folder `Controllers` (already exists) → new file `TasksController.cs`
`wwwroot/index.html`	Folder `wwwroot` (already exists) → new file `index.html`
`wwwroot/css/style.css`	New folder `css` inside `wwwroot` → new file `style.css`
`wwwroot/js/app.js`	New folder `js` inside `wwwroot` → new file `app.js`
API endpoints
Method	Route	Description
GET	`/api/tasks`	Get all tasks
GET	`/api/tasks/{id}`	Get one task
POST	`/api/tasks`	Create a task
PUT	`/api/tasks/{id}`	Update a task
DELETE	`/api/tasks/{id}`	Delete a task
Troubleshooting
"Cannot open database ... requested by the login" – run `Update-Database` again in Package Manager Console, or delete the `Migrations` folder and re-run `Add-Migration InitialCreate` + `Update-Database`.
LocalDB not found – open Visual Studio Installer → Modify → make sure "SQL Server Express LocalDB" is checked under the ASP.NET and web development workload.
Port already in use – edit `Properties/launchSettings.json` and change the port numbers.
