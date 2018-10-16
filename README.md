# OCTOPUS example with rails 5.2

In this example, I use octopus for multiple databases management in the same project.
At the moment that this document is wrote, there is not stable version of the gem for rails 5.2, so I use the branch that has the compatibility proved:

```gem 'ar-octopus', :git => "https://github.com/thiagopradi/octopus.git", :branch => "feature/updating-octopus-versions"```

The documentation of this gem is find it in the url: https://github.com/thiagopradi/octopus

For testing, I used two databases (first-db_development and second-db_development) with the a same model (Users) and I wrote the configuration code in the users_controller.rb file

```
Octopus.setup do |config|
  config.environments = [:development, :production]
  config.shards = {:first_db => {:adapter => "postgresql", :database => first-db_development"} , :second_db => {:adapter => "postgis", :database => "second-db_development"}}
end
```

Other way to configure the databases, is creating the shard.yml file

```
octopus:
  environments:
    - development
    - production
  development:
    first_db_shard:
      host: localhost
      adapter: postgresql
      database: first-db_development

  production:
    second_db_shart:
      host: localhost
      adapter: postgis
      database: second-db_development
```

To select the model and database for query, I used the database symbol
```
User.using(:first_db).all
```

The result of the query is shown in the path
```
http://localhost:3000/users/
```