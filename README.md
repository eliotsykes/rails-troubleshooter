# Ruby on Rails Troubleshooter

Troubleshoot common Ruby on Rails issues

<!-- MarkdownTOC depth=0 autolink=true bracket=round -->

- [Database](#database)
  - [Migrations](#migrations)
    - [Ensure test db is migrated](#ensure-test-db-is-migrated)
    - [Applying a migration then editing it](#applying-a-migration-then-editing-it)
- [Testing](#testing)
  - [RSpec + DatabaseCleaner](#rspec--databasecleaner)
    - [Ensure `spec/support` directory is correctly configured](#ensure-specsupport-directory-is-correctly-configured)
- [Contributors](#contributors)

<!-- /MarkdownTOC -->


# Database

## Migrations

### Ensure test db is migrated

Migrations in Rails live in the `db/migrations` folder. They need to be applied to each database environment your app has. Usually the 3 environments are development, test, and production.

A common problem is migrations have been run on the development database by calling `rake db:migrate`, but, depending on the Rails version, this may mean the migrations have **not** been applied to the test database.

To ensure your migrations are applied to the test database, run this command:

```
rake db:migrate RAILS_ENV=test
```

### Applying a migration then editing it 

Once a migration file has been run against a database, and **after** running it, if the migration file is **then** edited to change the work the migration does, then running `rake db:migrate` again will **not** apply those changes to the database. This is correct behaviour.

If you've encountered this on a production database with important data that cannot be lost, ask for help from an experienced developer if you're unsure how to handle this.

Alternatively, if this is encountered only on the test and/or development database, the 2 most common ways to fix this are:

1. Undo the edits made to the original migration file and save it
2. Create a new migration file with the required database changes
3. Apply the new database migration with  
`rake db:migrate RAILS_ENV=insert_the_environment_here`

**OR**

1. Rollback the migration on the database(s) with  
`rake db:rollback STEP=1 RAILS_ENV=insert_the_environment_here`
2. Edit the original migration file to contain the required changes and save it
3. Apply the edited migration with  
`rake db:migrate RAILS_ENV=insert_the_environment_here`


# Testing

## RSpec + DatabaseCleaner

### Ensure `spec/support` directory is correctly configured

When configuring DatabaseCleaner to clear out the test database between specs, it is common to put the DatabaseCleaner configuration into its own file in the `spec/support` directory.

If the database isn't being cleared out between specs, it may be because the files in the `spec/support` directory are not being required by RSpec. Check that the following line is not commented out in `spec/rails_helper.rb`:

```
# Line below must not be commented out to ensure support files are required by RSpec
Dir[Rails.root.join("spec/support/**/*.rb")].each { |f| require f }
```

---

# Contributors

Suggestions for common Rails issues to add to this README welcome in the issue tracker or as pull requests.

- Eliot Sykes https://eliotsykes.com/
- Your name here, contributions welcome!
