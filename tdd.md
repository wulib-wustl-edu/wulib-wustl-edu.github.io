---
layout: page
title: Test-Driven Development
permalink: /tdd/
---


### Test-Driven Development

---
---

#### Ruby On Rails

Testing is built into Ruby on Rails. A test directory is created as soon as you create a new Rails application.


Looking at the app/test directory:

        $ ls -F test
        application_system_test_case.rb
        test_helper.rb
        controllers/
        fixtures/
        helpers/
        integration/
        mailers/
        models/
        system/

Breakdown:

1. Helpers, Mailers, and Models directories should have tests related to each feature respectively.

2. The Controllers directory should hold tests for controller actions, routes, and views.

3. The Integration directory should hold tests for interactions between controllers.

3. The System directory are for full browser testing that mimic what a user would do with the application. These tests can test your Javascript as well.

4. Fixutres is a directory to help organize test data.

5. The test_helper.rb holds the default configuration for your tests.

6. The application_system_test_case.rb holds the default config for system tests.

---
---

#### Test Environment

Every Rails application has three environments: development, test, and production.

They are located at config/environments/*.rb

---
---

#### MiniTest & Rails

When you generate a model, controller, etc., an associated test is created in the corresponding test directory.

For example:

    $ bin/rails generate model resource title:string author:string

    create app/models/resource.rb
    create test/models/resource_test.rb
    create test/fixtures/resources.yml



