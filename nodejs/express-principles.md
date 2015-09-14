*Need to be more succinct, but the general idea is here.*

- the express application should know what environment it's in (development, test, production, etc) and alter it's functionality accordingly
- global variables that differ between environments should be stored in environment specific configuration files
- modules that require configuration should have their own configuration scripts (mongoose, passport, seraph, etc.)
- the start script (server.js) should call the required configuration scripts and start the server in around 100 lines
- the majority of feature/functionality changes should not require a developer to modify the start or configuration scripts
