# React-Rails-Installation-Guide
Installing React on Rails 6 for CVWO 19/20 winter assignment

### 1. Install and Setup Ruby on Rails

Install and setup the latest version of Ruby on Rails with a database.

We recommend this setup guide by GoRails: https://gorails.com/setup/.

- We recommend installing Ruby using rbenv as it is used by CVWO projects, but it is not a requirement.

- For the choice of database, we recommend PostgreSQL as it is used by CVWO projects, but it is not a requirement.


### 2. Create the Rails application

The following commands create a new Rails application called `my-app` in the current directory.

- If you are using PostgreSQL
```
rails new my-app --webpack=react -d postgresql
```

- If you are using SQLite 3 (Rails default)
```
rails new my-app --webpack=react
```

- If you are using MySQL
```
rails new my-app --webpack=react -d mysql
```

Move to the application directory
```
cd my-app
```

If you setup MySQL or Postgres with a username/password, modify the config/database.yml file to contain the username/password that you specified

Create the Database
```
rake db:create
```

### 3. Set up React

Install the React Rails gem
```
bundle add react-rails
```

Generate a rails installation
```
rails g react:install
```

### Credit
- GoRails: https://gorails.com/setup
- Official documentation for the React Rails gem: https://github.com/reactjs/react-rails
- Getting started with Rails 6 and React by Spike Burton: https://medium.com/swlh/getting-started-with-rails-6-and-react-afac8255aecd
