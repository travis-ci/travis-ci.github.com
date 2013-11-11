---
title: Contributing to Travis CI
layout: en
permalink: contributing/
---

So you want to contribute to Travis CI? Awesome! This page contains some best
practices and guidelines we follow for writing code.

The ideas and layout of this document was (originally) borrowed from
Thoughtbot's own [guides](https://github.com/thoughtbot/guides), but the
content itself has been adapted to fit with our code and practices.

First, some high level guidelines:

- Be consistent.
- Don't rewrite existing code to follow this guide.
- Don't violate a guideline without a good reason.
- A reason is good when you can convince a teammate.

In general, code that doesn't follow this guide can be updated when it is touched for other reasons, unless that makes it break another rule (mainly “be consistent”).

A note on the language:

- “Avoid” means don't do it unless you have a good reason.
- “Don't” means there's never a good reason.
- “Prefer” indicates a better option and its alternative to watch out for.
- “Use” is a positive instruction.

## Code Review

We recommend reading [Thoughtbot's Code Review guide](https://github.com/thoughtbot/guides/tree/master/code-review).

## Best Practices

### General

- Don't duplicate the functionality of a built-in library.
- Don't swallow exceptions or “fail silently”.
- Don't write code that guesses at future functionality.
- [Exceptions should be exceptional](http://www.readability.com/~/yichhgvu).
- [Keep the code simple](http://www.readability.com/~/ko2aqda2).

### Object-Oriented Design

- Avoid global variables.
- Avoid long parameter lists.
- Limit collaborators of an object (entities an object depends on).
- Limit an object's dependencies (entities that depend on an object).
- Prefer composition over inheritance.
- Prefer small methods. Between one and five lines is best.
- Prefer small objects with a single, well-defined responsibility. When an
  object exceeds 100 lines, it may be doing too many things.
- [Tell, don't ask](http://robots.thoughtbot.com/post/27572137956/tell-dont-ask).

### Ruby

- Avoid optional parameters. Does the method do too much?
- Avoid monkey-patching.
- Prefer classes to modules when designing functionality that is shared by multiple models.
- Prefer `private` when indicating scope. Use `protected` only with comparison methods like `def ==(other)`, `def <(other)` and `def >(other)`.

### Ruby Gems

- Declare dependencies in the `<PROJECT_NAME>.gemspec` file.
- Reference the `gemspec` in the `Gemfile`.
- Use [Bundler](http://bundler.io/) to manage the gem's dependencies.

### Rails

Note: We don't use the full Rails stack that much at Travis CI, but we use its components (especially Active Record). These guidelines apply to the components when used outside of the full Rails stack, as well.

Some of these guidelines also transfer to other web frameworks.

- Avoid bypassing validations with methods live `save(validate: false)`,
  `update_attribute` and `toggle`.
- Avoid instantiating more than one object in controllers.
- Avoid naming methods after database columns in the same class.
- Don't change a migration after it has been merged into master if the desired
  change can be solved with another migration.
- Don't reference a model class directly from a view.
- Don't use instance variables in partials. Pass local variables to partials
  from view templates.
- Don't use SQL or SQL fragments (`where('inviter_id IS NOT NULL')`) outside of
  models.
- If there are default values, set them in migrations.
- Keep `db/schema.rb` or `db/development_structure.sql` under version control.
- Every migration that changes the schema should be accompanied with a change
  to `db/schema.rb` or `db/development_structure.sql` as well.
- Use only one instance variable in each view.
- Use SQL, not Active Record models, in migrations.
- Use the `.ruby-version` file convention to specify the Ruby version and patch
  level for a project.
- Use `_url` suffixes for named routes in mailer views and
  [redirects](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30).
  Use `_path` suffixes for named routes everywhere else.
- Validate the associated `belongs_to` object (`user`), not the database column
  (`user_id`).

### Testing

- Avoid `any_instance` in rspec-mocks and mocha. Prefer [dependency
  injection](http://en.wikipedia.org/wiki/Dependency_injection).
- Avoid using instance variables in tests.
- Disable real HTTP requests to external services.
- Don't test private methods.
- Use [stubs and
  spies](https://github.com/thoughtbot/guides/blob/master/best-practices/not%20mocks)
  in isolated tests.
- Use a single level of abstraction within scenarios.
- Use an `it` example or test method for each execution path through the method.
- Use [assertions about state](https://speakerdeck.com/skmetz/magic-tricks-of-testing-railsconf?slide=51) for incoming messages.
- Use stubs and spies to assert you sent outgoing messages.
- Use a [Fake](http://robots.thoughtbot.com/post/219216005/fake-it) to stub requests to external services.
- Use integration tests to execute the entire app.
- Use non-[SUT](http://xunitpatterns.com/SUT.html) methods in expectations when possible.

### Bundler

- Specify the [Ruby version](http://bundler.io/v1.3/gemfile_ruby.html) to be used on the project in the `Gemfile`.
- Use a [pessimistic version](http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle) in the `Gemfile` for gems that follow semantic versioning, such as `rspec`, `factory_girl` and `capybara`.
- Use a [versionless](http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle) `Gemfile` declaration for gems that are safe to update often, such as pg, thin and debugger.
- Use an [exact version](http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle) in the `Gemfile` for fragile gems, such as Rails.

## Style

### Git

- Avoid merge commits by using a [rebase workflow](https://github.com/thoughtbot/guides/tree/master/protocol#merge).
- Prefix feature branch names with your initials when committing to the main repositories (`travis-ci/*` and `travis-pro/*`).
- Squash multiple trivial commits into a single commit.
- Write a [good commit message](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).

### Formatting

- Avoid inline comments.
- Break long lines after 80 characters.
- Delete trailing whitespace.
- Don't include spaces after `(`, `[` or before `]`, `)`.
- Don't misspell.
- Don't vertically align tokens on consecutive lines.
- If you break up an argument list, keep the arguments on their own lines and
  closing parenthesis on its own line.
- If you break up a hash, keep the elements on their own lines and closing
  curly brace on its own line.
- If you break up an array or a hash, add a comma after the last entry.
- Indent continued lines with two spaces.
- Indent private methods equal to public methods.
- Use two spaces for indentation (no tabs).
- Use an empty line between methods.
- Use newlines around multi-line blocks.
- Use spaces around operators, after commas, after colons and semicolons,
  around `{` and before `}`.
- Use
  [Unix-style](https://github.com/thoughtbot/guides/blob/master/style/%60n%60)
  line endings.
- Use [uppercase for SQL key words and lowercase for SQL
  identifiers](http://www.postgresql.org/docs/9.2/static/sql-syntax-lexical.html#SQL-SYNTAX-IDENTIFIERS).

### Naming

- Avoid abbreviations.
- Avoid types in names (`user_array`).
- Name the enumeration parameter the singular of the collection.
- Name variables, methods and classes to reveal intent.
- Treat acronyms as words in names (`XmlHttpRequest`, not `XMLHTTPRequest`), even if the acronym is the entire name (`class Html`, not `class HTML`).

### Organization

- Order methods so that caller methods are earlier in the file than the methods they call.
- Order methods so that methods are as close as possible to other methods they call.

### Ruby

- Avoid conditional modifiers (lines that end with conditionals, such as
  `do_this if foo`).
- Avoid multiple assignments per line (`one, two = 1, 2`).
- Avoid organizational comments (`# Validations`).
- Avoid ternary operators (`boolean ? true : false`). Use multi-line `if`
  instead to emphasize code branches.
- Avoid explicit return statements.
- Avoid using semicolons.
- Don't use `self` explicitly anywhere except class methods (`def self.method`)
  and assignments (`self.attribute =`).
- Prefer `find` over `detect`.
- Prefer `reduce` over `inject`.
- Prefer `map` over `collect`.
- Prefer `select` over `find_all`.
- Prefer single quotes for strings.
- Use `_` for unused block parameters.
- Use `%{}` for single-line strings needing both interpolation and
  double-quotes.
- Use `&&` and `||` for Boolean expressions.
- Use `{...}` for single-line blocks. Use `do..end` for multi-line blocks.
- Use `?` suffix for predicate methods.
- Use `CamelCase` for classes and modules, `snake_case` for variables and
  methods, `SCREAMING_SNAKE_CASE` for constants.
- Use `def self.method`, not `def Class.method` or `class << self`.
- Use `def` with parentheses when there are arguments.
- Use `each`, not `for`, for iteration.
- Use heredocs (`<<EOS`/`<<-EOS`) for multi-line strings.

### ERb

- When wrapping long lines, keep the method name on the same line as the ERb
  interpolation operator and keep each method argument on its own line.
- Prefer double quotes for attributes.

### HTML

- Prefer double quotes for attributes.

### Rails

- Avoid `member` and `collection` routes.
- Use private instead of protected when defining controller methods.
- Name date columns with `_on` suffixes.
- Name datetime columns with `_at` suffixes.
- Name initializers for their gem name.
- Order ActiveRecord associations alphabetically by attribute name.
- Order ActiveRecord validations alphabetically by attribute name.
- Order model contents: constants, macros, public methods, private methods.
- Put application-wide partials in the
  [`app/views/application`](http://asciicasts.com/episodes/269-template-inheritance)
  directory.
- Use `def self.method`, not the `scope :method` DSL.
- Use the default `render 'partial'` syntax over `render partial: 'partial'`.

### Testing

- Avoid the `private` keyword in specs.
- Prefer `eq` to `==` in RSpec.
- Seperate setup, exercise, verification and teardown phases with newlines.
- Use RSpec's [`expect` syntax](http://myronmars.to/n/dev-blog/2012/06/rspecs-new-expectation-syntax).
- Use `not_to` instead of `to_not` in RSpec expectations.

#### Unit Tests

- Don't prefix `it` block descriptions with 'should'.
- Name outer `describe` blocks after the method under test. Use `.method` for class methods and `#method` for instance methods.

### Go

Use `go fmt` to format your code. Go uses tabs instead of spaces for indenting.
