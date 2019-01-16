# Ruby/Rails Stuff

### Version Managers
- RVM (Ruby Version Manager): 
  - Overrides cd commands in order to load Ruby env variables
  - Rubies and Gemsets are loaded *when* switching directories
- rbenv
  - Uses **Shims** to execute commands (Similar to `package.json`)
  - Uses *bundler* to install gemsets
  
---
### Rake
- Task runner
- Scripting of admin level tasks
- `rake --tasks`
  - Gives info on apps (routes)
  - Cache
  - Asset
  - Database migrations

---
### Database Migrations
- Updating relational databases (schemas)

---
### Gems and bundle
- Ruby libraries
- bundle *is* a gem: tracks and manages gem installations
- `bundle install` (equivalent to `npm install`): Installs gems from *Gemfile*
- **test/spec** directory: test files
- **gemspec**: specifications of the gem
  - gem's files
  - test information
  - platform
  - version number
  - etc...

---
### Sprockets and the Asset Pipeline
  - Sprockets: gem for compliling and serving assets
  - Contains preprocessor pipeline for **Sass** and **Slim**
  - Concatenation, hashing, and minification of JS and CSS
  
---
### Rails
  - Technically a gem but referred to as a web application framework
  - Structure for MVC, routes, and configs
  - Provide helpers to generate migrations/models of the framework
  - Convention over configuration: Framework will assume/make decisions by convention instead of forcing the user to provide configuration files
  
---
### Models
  - *Usually* used as a one to one object model for rows of a database table
    - Only maps one row of a table to one row of another table
  - ActiveRecord - The **M** in MVC:
    - Data access via chainable queries
    - Data writing via model functions (`update`, `save`, or `destroy)
    - Migrations
    - Callbacks
    - Validation (Check that model matches requirements before it is written)

---
### Controllers
  - Class that receives and thus request after routing
  - Defines the actions to respond to different requests
  - Interacts with the model on behalf of view
  - **REST** actions: index, create/new, show, destroy, edit/update
    - Actions will receive a `params` object from requests and should be validated before actions
    
---
### Views
  - Contains data that user would receive from request 
  - *Convention: named after controller action* - Does not require explicit `render` call in controller action method
  - Will have both JSON and HTML views
  
---
### Routes
  - Action definer: Mapping URLs to methods in controller
  - Rails provides:
    - Helper functions for resources (controllers): Collection and member
    - Namespaces: Organization tool for grouping controllers
      - e.g. Grouping all the resources with *admin* rights
      ```
      namespace :admin do
          resources :articles, :comments
      end
      ```
    - `resources :symbol`: Automatically creates 7 actions for the symbol as follows:
      - e.g. when calling `resources :photos` the following will be created:
      - | HTTP Verb | Path             | Action  | Used For                             |
        |-----------|------------------|---------|--------------------------------------|
        | GET       | /photos          | index   | display list of all photos           |
        | GET       | /photos/new      | new     | return a view for creating new photo |
        | POST      | /photos          | create  | create a new photo                   |
        | GET       | /photos/:id      | show    | display a specific photo             |
        | GET       | /photos/:id/edit | edit    | return a view for editing photo      |
        | PUT       | /photos/:id      | update  | update a specific photo              |
        | DELETE    | /photos/:id      | destroy | delete a specific photo              |

---
### Punit Policies and Scopes
  - Pundit: gem that allows action policies to be defined as well as errors on failures
  - Policy method mathes action name
  - Scopes return limited ActiveRecord object of the model based on the current user
  - Removes permission logic from controllers
