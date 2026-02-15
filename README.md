# BlazorTrainingPostgres

Continuing on from https://github.com/richardlucas761/BlazorTraining but using a Postgres database instead, Postgres database 
training.

It works but it's got some issues to fix.

## TODO

### No equivalent of the SQL Server Database Project exists for Postgres

As there is no (SQL Server) Database Project then how do we create the Movies database table?

(Data loading is simpler in this solution as it automatically seeds the database with sample movies if the table is empty)

We want a Postgres equivalent of this TSQL from BlazorTraining:

```
CREATE TABLE [dbo].[Movie]
(
    [MovieId]       INT IDENTITY(1,1)   NOT NULL, 
    [Title]         NVARCHAR(255)       NOT NULL, 
    [ReleaseDate]   DATE                NOT NULL, 
    [Genre]         NVARCHAR(50)        NOT NULL, 
    [Price]         DECIMAL(18, 2)      NOT NULL,
    [Rating]        NVARCHAR(5)         NOT NULL, 
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([MovieId])
)
```

(Conveniently ignoring the column descriptions present in the original BlazorTraining SQL Server database project)

This works but is probably not best practice.

```
DROP TABLE IF EXISTS public."Movie";

CREATE TABLE IF NOT EXISTS public."Movie"
(
    "MovieId" bigint NOT NULL,
    "Title" character varying(255) COLLATE pg_catalog."default" NOT NULL,
	"ReleaseDate" date NOT NULL,
	"Genre" character varying(50) COLLATE pg_catalog."default" NOT NULL,
	"Price" decimal(18,2) NOT NULL,
	"Rating" character varying(5) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT "PK_Movie" PRIMARY KEY ("MovieId")
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public."Test"
    OWNER to postgres;
```

#### Postgres best practice

* Don't use the default ```postgres``` database, create a new ```Movies``` database perhaps?
 Assume ```postgres``` is the equivalent of *master* or *model* in SQL Server perhaps?
* Discover the Postgres equivalent of ```INT IDENTITY(1,1)```
* Discover how to create table and column descriptions
* Create a new user account for accessing the ```Movies``` database rather than using the super user name and password, is this 
 an account you allocate roles to or something else?
* Configure appsettings.json so the new user name and password are not stored unencrypted in source control
* Find out what *tablespaces* are in Postgres and how this should be configured
* Postgres equivalent of ```"Application Name=BlazorTrainingPostgres;"```

### serviceDependencies.json still references mssql?

```
{
  "dependencies": {
    "mssql1": {
      "type": "mssql",
      "connectionId": "ConnectionStrings:BlazorWebAppMoviesContext"
    }
  }
}
```
