# rails_postgres_docker_demo
Docker container running Ruby on Rails and PostgreSQL foundation files

Files go along with the blog post
https://stackslabs.blogspot.com/2023/01/running-ruby-on-rails-with-postgres-in.html

## Instructions
Clone this repository, and cd into the directory. Once there follow the instructions below.
Feel free to play around with the folder and variable names. 

### Set Up the Rails Application
This will build a rails application accorcing to the options you supply.
```
docker-compose run --no-deps web rails new . --force --database=postgresql
docker-compose build
```

### Set Up the datbase.yml file
```
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  host: <%= ENV.fetch('DB_HOST', 'localhost') %>
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>

development:
  <<: *default
  database: app_development
test:
  <<: *default
  database: app_test

production:
  <<: *default
  database: app_production
  username: app
  password: <%= ENV["APP_DATABASE_PASSWORD"] %>
  ```
  ### Create and Migrate the databases
  Open up a terminal from your docker app for what will probably be the web-1 container and run the following, which should feel pretty familair. 
  ```
  rake db:create
  rake db:migrate
  ```
  
  Now you should be able to go to http://localhost:3000 and have a rails app using PostgreSQL running in Docker containers.
  

