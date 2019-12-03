# React-Rails-Installation-Guide
Installing React on Rails 6 for CVWO 19/20 winter assignment

### 1. Install and Setup Ruby on Rails

Install and setup the latest version of Ruby on Rails with a database.

We recommend this setup guide by GoRails: https://gorails.com/setup/.

- We recommend installing Ruby using rbenv as it is used by CVWO projects, but it is not a requirement.

- For the choice of database, we recommend PostgreSQL as it is used by CVWO projects, but it is not a requirement.


### 2. Create a new Rails app with React

- If you are using SQLite 3
```
rails new my-app --webpack=react
```

- If you are using MySQL
```
rails new my-app --webpack=react -d mysql
```

- If you are using PostgreSQL
```
rails new my-app --webpack=react -d postgresql
```
