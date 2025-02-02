# Foodoramix
Find a perfect recipe with your own list of ingredients.

## Getting started
### Database
This project is built to work with a MySQL database.
If you want to run a mysql database on docker, you can type :

`docker run --name mysql-foodoramix -p 3306:3306 -e MYSQL_ROOT_PASSWORD=yourpassword -d mysql`

Create a database **foodoramix**
Create a user **foodoramix_client** identified with the password **FqxTFS93f5!GiMR#**
Or, you can use the user **root** with the password you defined earlier.

Open a terminal in the project repository and run `npm run initDB`
Wait until the message _Loading done !_ appears. Then, kill the run with ctrl+c

Now, your database should be loaded with recipes and all the others needed data.

### API
To run the api, you need 2 terminals.

In the first, run `npx tsc --watch` and in the second `node dist/index.js`.

You should see a message in your terminal saying that the server is listening.

If you open your favorite browser and open the given link (http://127.0.0.1:3000 for example) you should see the welcome message.

You can now start playing with API functionalities.

The file *Insomnia_export.json* is a file containing route use example. Import it in insomnia to try the routes with the correct schemas.

## Functionalities

The objective of this application is to create a database of recipes, accessible to everyone, where each user can create/edit recipe.
If the user likes a recipe, He can add it to his favorites for an easier access.

#### For recipes 
- To see all recipes, the user has to do a GET on /recipes/. The list of recipes will be displayed in the browser in JSON format.
- To see a specific recipe, the user has to do a GET on /recipes/id. The specific recipe will be displayed in the browser.
- To create a recipe, the user has to be logged in and do a POST on /recipes/ with a body containing a recipe in JSON format. A recipe must have a title, an URL, a list of ingredients and a list of instructions.
- To update a recipe, the user has to be logged in and do a PATCH on /recipes/id (id of recipe to modify) with a body containing a recipe in JSON format (similar to the POST)

#### For favorites
- To add a favorite, the user has to be connected and do a POST on /favorites/ with a body containing the id of a recipe.
- To see all his favorites, the user has to be connected and do a GET on /favorites/.
- To remove a recipe from his favorites, the user has to be connected and do a DELETE on /favorites/id (id of recipe to remove).

#### For account
- To create an account, the user has do a POST on /account/signin/ with a JSON body containing an email and a password.
- To be connected, the user has to do a POST on /account/login/ with a JSON body containing an email and a password.
- To see the data of his account, the user has to do a GET on /account/. It will display his email.
- To change his data, the user has to do a PATCH on /account/ with a JSON body containting an email and a password.

## Test & Code coverage
This is the last report of our code coverage :
![report](assets/code_coverage.png)

This is the number of tests passing (all) :

![report_tests](assets/tests_passing.png)

## Documentation
[Click here to see the report](assets/api-documentation.pdf)

## Checkpoints report for the project

### Input validation

- [x] Strictly and deeply validate the type of every input (`params, querystring, body`) at runtime before any processing. **[1 point]** 🔵
> We built json and types schema to validate the inputs and outputs for each routes.

- [x] Ensure the type of every input can be inferred by Typescript at any time and properly propagates across the app. **[1 point]** 🔵
> We defined the strict type checking as true, and we built a script to generate TS types (can be run with `npm run generateSchemas`)

- [x] Ensure the static and runtime input types are always synced. **[1 point]** 🔵
> We defined the strict type checking as true.

### Authorisation

- [x] Check the current user is allowed to call this endpoint. **[1 point]** 🔵
> We added a session allowing to check if the user is logged in and can access endpoint.

- [x] Check the current user is allowed to perform the action on a specific resource. **[1 point]** 🔵
> We defined secure policy checking if the user can perform the requested action or not.

- 🔴 Did you build or use an authorisation framework, making the authorisation widely used in your code base? **[1 point]**
> Not done

- 🔴 Do you have any way to ensure authorisation is checked on every endpoint? **[1 point]**
> It is pretty easy to forget authorising some action.
> For obvious reasons, it may lead to security issues and bugs.
> At work, we use `varvet/pundit` in our `Ruby on Rails` stack. It can raise exception just before answering the client if authorisation is not checked.
> https://github.com/varvet/pundit#ensuring-policies-and-scopes-are-used
>
> Not done

### Secret and configuration management

- [x] Use a hash for any sensitive data you do not need to store as plain text. 🔵
> Password is hashed before being stored.

- [x] Store your configuration entries in environment variables or outside the git scope. **[1 point]** 🔵
> We used dotenv library with a .env file.

- [x] Do you provide a way to list every configuration entries (setup instructions, documentation, requireness... are appreciated)? **[1 point]**
> We wrote a README file with different parts (actually this file)

- [x] Do you have a kind of configuration validation with meaningful error messages? **[1 point]**
> We used dotenv with a `getOrThrow` method.

### Package management

- [x] Do not use any package with less than 50k downloads a week. 🔵

- 🔴 Did you write some automated tools that check no unpopular dependency was installed? If yes, ensure it runs frequently. **[1 point]**
> Not done

- [x] Properly use dependencies and devDevepencies in your package.json. **[0.5 points]**
> Manual verification of each package usage.

### Automated API generation

- [x] Do you have automated documentation generation for your API (such as OpenAPI/Swagger...)? **[1 point]** 🔵
> We used swagger to dynamically generate the documentation.
> [Click here to see the report](assets/api-documentation.pdf)

- [x] In addition to requireness and types, do you provide a comment for every property of your documentation? **[1 point]**
> We have added description inside the routes.

- 🟠 Do you document the schema of responses (at least for success codes) and provide examples of payloads? **[1 point]**
> With schemas and descriptions

- 🔴 Is your documentation automatically built and published when a commit reach the develop or master branches? **[1 point]**
> Not done

### Error management

- [x] Do not expose internal application state or code (no sent stacktrace in production!). **[1 point]** 🔵
> Custom error handling and reply.

- 🔴 Do you report errors to Sentry, Rollbar, Stackdriver… **[1 point]**
> Not done

### Log management

- 🔴 Mention everything you put in place for a better debugging experience based on the logs collection and analysis. **[3 points]**
> Not done

- 🔴 Mention everything you put in place to ensure no sensitive data were recorded to the log. **[1 point]**
> Not done

### Asynchronous first

- [x] Always use the async implementations when available. **[1 point]** 🔵
> We used async/await any time we could.

- [x] No unhandled promise rejections, no uncaught exceptions… **[1 point]** 🔵
> Global exception management in the fastify lib.

### Code quality

- [x] Did you put a focus on reducing code duplication? **[1 point]**
> Reuse of schemas, code review & re factorisation, global methods.

- 🟠 Eslint rules are checked for any pushed commit to develop or master branch. **[1 point]**
> Eslint is checked in the IDE.

### Automated tests

- 🟠 You implemented automated specs. **[1 point]** 🔵
> We tried to create a github action, but we had issues with the database tests.

- [x] Your test code coverage is 75% or more.  **[1 point]** 🔵
  ![report](assets/code_coverage.png)

- 🟠 Do you run the test on a CD/CI, such as Github Action? **[1 point]**
> See first question