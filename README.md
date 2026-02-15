# BlazorTrainingPostgres

Building on https://github.com/richardlucas761/BlazorTraining but using a Postgres database rather than SQL Server, Postgres 
database training.

## TODO

### Broken / wrong approach / simpler implementation?

Perhaps the wrong approach here to copy-and-paste the parts of BlazorTraining but without the SQL Server database project, 
currently broken and may just abandon this and try something simpler for Postgres database training.

### Logins

Postgres login differs from SQL Server as it uses a specific username and password rather than using an account.

How do I configure this without committing the superuser password to source control?

Should I create a username / password with fewer rights than the super user account and use that instead?

Should I create a specific database other than the default 'postgres' database?

```"BlazorWebAppMoviesContext": "Host=localhost;Port=5432;Database=postgres;Username=postgres;Password=yourpassword"```
