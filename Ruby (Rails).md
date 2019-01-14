# Ruby (on Rails) Stuff

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
