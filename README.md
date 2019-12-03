# React-Rails-Installation-Guide
Installing React on Rails 6 for CVWO 19/20 winter assignment.

## Introduction

#### What is _React_?
React is a JavaScript library created by Facebook. It is a User Interface (UI) library, which more simply put is a tool for building UI components (also known as frontend).

#### What is _Ruby on Rails_?
Ruby on Rails, also known as Rails, is a server-side (also known as backend) web application framework written in the Ruby language.

#### What is _React on Rails_?
Since the release of Rails v6.0 in August 2019, Webpacker (a javascript compiler) is built in to Rails by default. This has made it easier for React and Rails to be integrated together, giving rise to a full-stack framework known unofficially as React on Rails. Given the widespread popularity of React, we have decided to include it in this winter assignment.

## Installation and Setup
### 1. Install and Setup Ruby on Rails

Install and setup the latest version of Ruby on Rails with a database.

We recommend following this [Installation Guide by GoRails](https://gorails.com/setup/).

- If asked to choose the method of installing Ruby, we recommend using rbenv as it is used by CVWO projects, though it is not a requirement.

- For the choice of database, we recommend PostgreSQL as it is used by CVWO projects, but it is not a requirement.

At this point, you should be able to start building a simple Rails application (without React). You might want to checkout [this introductory tutorial to Ruby on Rails](https://guides.rubyonrails.org/getting_started.html) before moving on, especially if you have no prior Rails experience.

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
**At this point, you are able to create a simple Rails application without React.**

### 3. Set up React

Install the React Rails gem
```
bundle add react-rails
```

Generate a Rails installation
```
rails g react:install
```

You are now done with setup! Time to test your react components

### 4. 

### 5. Generating React Components

Generate your first component:
```
rails g react:component HelloWorld greeting:string
```

You can also generate your component in a subdirectory:
```
rails g react:component my_subdirectory/HelloWorld greeting:string
```

Note: Your component is added to `app/javascript/components/` by default.

### Credit
- GoRails: https://gorails.com/setup
- Official documentation for the React Rails gem: https://github.com/reactjs/react-rails
- Getting started with Rails 6 and React by Spike Burton: https://medium.com/swlh/getting-started-with-rails-6-and-react-afac8255aecd
