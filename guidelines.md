[<img src="https://merchant.jiomart.com/selfsignup/v1.0/images/JioMart-Partner-logo.jpg" width="333" height="175">](https://www.elsewhen.com/)

# Project Guidelines

- [Project Guidelines](#project-guidelines)
  - [1. Git](#1-git)
    - [1.1 Some Git rules](#11-some-git-rules)
    - [1.2 Git workflow](#12-git-workflow)
    - [1.3 Writing commit messages with valid format](#13-writing-commit-messages-with-valid-format)
  - [2. Documentation](#2-documentation)
  - [3. Environments](#3-environments)
  - [4. Dependencies](#4-dependencies)
  - [5. Testing](#5-testing)
  - [6. Structure and Naming](#6-structure-and-naming)
  - [7. Code style](#7-code-style)
    - [7.1 Some code style guidelines](#71-some-code-style-guidelines)
    - [7.2 Enforcing code style standards](#72-enforcing-code-style-standards)
  - [8. Logging](#8-logging)
  - [9. API](#9-api)
    - [9.1 API design](#91-api-design)

<a name="git"></a>

## 1. Git

<a name="some-git-rules"></a>

### 1.1 Some Git rules

There are a set of rules to keep in mind:

- Perform work in a feature branch.

  _Why:_

  > Because this way all work is done in isolation on a dedicated branch rather than the main branch. It allows you to submit multiple pull requests without confusion. You can iterate without polluting the master branch with potentially unstable, unfinished code. [read more...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)

- Branch out from **`master`** always

  _Why:_

  > This way, you can make sure that code in master will almost always build without problems, and lesser merge conflicts to resolve while merging.

- Never push into `jmp` or `master` branch. Make a Pull Request.

  _Why:_

  > It notifies team members that they have completed a feature. It also enables easy peer-review of the code and dedicates forum for discussing the proposed feature.

- Update your local branch and take a pull before pushing your feature and making a Pull Request.

  _Why:_

  > Fetches the latest code always

- Resolve potential conflicts while rebasing and before making a Pull Request.
- Delete local and remote feature branches after merging after a specific time i.e. 15 days

  _Why:_

  > It will clutter up your list of branches with dead branches. It ensures you only ever merge the branch back into (`jmpx2`, `jmpx3`, `jmp` or `master`) once. Feature branches should only exist for a while, when the work is still in progress.

- Before making a Pull Request, make sure your feature branch builds successfully and passes all tests (including code style checks).

  _Why:_

  > You are about to add your code to a stable branch. If your feature-branch tests fail, there is a high chance that your destination branch build will fail too. Additionally, you need to apply code style check before making a Pull Request. It aids readability and reduces the chance of formatting fixes being mingled in with actual changes.

- Protect these branches : `jmpx2`, `jmpx3`, `jmp` and `master` branch.

  _Why:_

  > It protects your production-ready branches from receiving unexpected and irreversible changes.

<a name="git-workflow"></a>

### 1.2 Git workflow

Because of most of the reasons above, we follow some common rules which are:

- Sync with remote to get changes you’ve missed.

  ```sh
  git checkout jmpx2
  git pull
  ```

  _Why:_

  > This will give you a chance to deal with conflicts on your machine while rebasing (later) rather than creating a Pull Request that contains conflicts.

- Make a Pull Request.
- Pull request will be accepted, merged and close by a reviewer.
- Remove your local feature branch if you're done after some time.

<a name="writing-good-commit-messages"></a>

### 1.3 Writing commit messages with valid format

Having a good guideline for creating commits and sticking to it makes working with Git and collaborating with others a lot easier. Here are some rules of thumb for committing in Fynd JMP eco-system.

One must follow [Fynd Commit Format](https://docs.fynd.engineering/docs/Fynd-Platform/common/commit-format). (if not used, won't be deployed)

_Note_: Can utilize this script [Command Line Commit Maker](https://github.com/maulik-fynd/fynd-tiny-tools/tree/main/fynd-git-commit-script)

- Format

  _Valid Commit Messages:_

  > ID: JMP-1008; DONE: 85; HOURS: 1; feat: add slot based delivery

  > ID: JMP-1008; DONE: 100; HOURS: 1; fix: add csv headers

  > ID: JMP-1008; DONE: 100; HOURS: 1; style: add csv headers

- Commit message types, examples could be:

  > **build**: Changes that affect the build system or external dependencies (example scopes: npm)

  > **ci**: Changes to our CI configuration files and scripts

  > **docs**: Documentation only changes

  > **feat**: A new feature

  > **fix**: A bug fix

  > **perf**: A code change that improves performance

  > **refactor**: A code change that neither fixes a bug nor adds a feature

  > **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)

  > **test**: Adding missing tests or correcting existing tests

- Do not end the subject line with a period.
- Use [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) in the subject line.

  _Why:_

  > Rather than writing messages that say what a committer has done. It's better to consider these messages as the instructions for what is going to be done after the commit is applied on the repository.

<a name="documentation"></a>

## 2. Documentation

- Comment your code. Try to make it as clear as possible what you are intending with each major section.
- If there is an open discussion on GitHub or stackoverflow about the code or approach you're using, include the link in your comment.
- Don't use comments as an excuse for a bad code. Keep your code clean.
- Don't use clean code as an excuse to not comment at all.
- Keep comments relevant as your code evolves.

<a name="environments"></a>

## 3. Environments

- Define separate `SIT`, `UAT` and `Production` environments if needed.

  _Why:_

  > Different data, tokens, APIs, ports etc... might be needed in different environments.

- Load your deployment specific configurations from environment variables and never add them to the codebase as constants

  _Why:_

  > You have tokens, passwords and other valuable information in there. Your config should be correctly separated from the app internals as if the codebase could be made public at any moment.

  _How:_

  > `.env` files to store your variables and add them to `.gitignore` to be excluded. Instead, commit a `.env.example` which serves as a guide for developers. For production, you should still set your environment variables in the standard way.

- It’s recommended to validate environment variables before your app starts. [Look at this sample](./configWithTest.sample.js) using `joi` to validate provided values.
  _Why:_
  > It may save others from hours of troubleshooting.

<a name="consistent-dev-environments"></a>

## 4. Dependencies

- Keep track of your currently available packages: e.g., `npm ls --depth=0`. [read more...](https://docs.npmjs.com/cli/ls)
- See if any of your packages have become unused or irrelevant: `depcheck`. [read more...](https://www.npmjs.com/package/depcheck)

  _Why:_

  > You may include an unused library in your code and increase the production bundle size. Find unused dependencies and get rid of them.

- Before using a dependency, check its download statistics to see if it is heavily used by the community: `npm-stat`. [read more...](https://npm-stat.com/)

  _Why:_

  > More usage mostly means more contributors, which usually means better maintenance, and all of these result in quickly discovered bugs and quickly developed fixes.

- Before using a dependency, check to see if it has a good, mature version release frequency with a large number of maintainers: e.g., `npm view async`. [read more...](https://docs.npmjs.com/cli/view)

  _Why:_

  > Having loads of contributors won't be as effective if maintainers don't merge fixes and patches quickly enough.

- If a less known dependency is needed, discuss it with the team before using it.
- Always make sure your app works with the latest version of its dependencies without breaking: `npm outdated`. [read more...](https://docs.npmjs.com/cli/outdated)

  _Why:_

  > Dependency updates sometimes contain breaking changes. Always check their release notes when updates show up. Update your dependencies one by one, that makes troubleshooting easier if anything goes wrong. Use a cool tool such as [npm-check-updates](https://github.com/tjunnone/npm-check-updates).

- Check to see if the package has known security vulnerabilities with, e.g., [Snyk](https://snyk.io/test?utm_source=risingstack_blog).

<a name="testing"></a>

## 5. Testing

- Place your test files next to the tested modules using `*.spec.js` naming convention, like `moduleName.spec.js`.

  _Why:_

  > You don't want to dig through a folder structure to find a unit test. [read more...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

- Put your additional test files into a separate test folder to avoid confusion.

  _Why:_

  > Folder name `__tests__` is standard and gets picked up by most JavaScript testing frameworks.

- Write testable code, avoid side effects, extract side effects, write pure functions

  _Why:_

  > You want to test a business logic as separate units. You have to "minimize the impact of randomness and nondeterministic processes on the reliability of your code". [read more...](https://medium.com/javascript-scene/tdd-the-rite-way-53c9b46f45e3)

  > A pure function is a function that always returns the same output for the same input. Conversely, an impure function is one that may have side effects or depends on conditions from the outside to produce a value. That makes it less predictable. [read more...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

- Use a static type checker

  _Why:_

  > Sometimes you may need a Static type checker. It brings a certain level of reliability to your code. [read more...](https://medium.freecodecamp.org/why-use-static-types-in-javascript-part-1-8382da1e0adb)

- Run tests locally before making any pull requests to `jmpx2`.

<a name="structure-and-naming"></a>

## 6. Structure and Naming

- Filenames are _case-sensitive_. To avoid confusion and cross-OS issues, it is standard practice to **always use lowercase letters**. It may include **dashes** (-). **Consistency is a must**.

  **Bad**

  ```
  .
  ├── controllers
  |   ├── productController.js
      ├── order_controller.js
      ├── paymentcontroller.js
  |   └── user.js
  ├── models
  |   ├── productModel.js
  |   └── user.js
  ```

  **Good**

  ```
  .
  ├── controllers
  |   ├── index.js
  |   ├── product.controller.js
  |   ├── order.controller.js
  |   ├── user-group.controller.js

  ```

- Use a single `config.js` file and don't make different config files for different environments.

  _Why:_

  > When you break down a config file for different purposes (database, API and so on); putting them in a folder with a very recognizable name such as `config` makes sense. Just remember not to make different config files for different environments. It doesn't scale cleanly, as more deploys of the app are created, new environment names are necessary.
  > Values to be used in config files should be provided by environment variables.

- Put your scripts in a `./scripts` folder. This includes `bash` and `node` scripts.

  _Why:_

  > It's very likely you may end up with more than one script, production build, development build, database feeders, database synchronization and so on.

<a name="code-style"></a>

## 7. Code style

<a name="code-style-check"></a>

### 7.1 Some code style guidelines

- We follow [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) for JavaScript.

- Include code style check in your build process.ƒ

- Use [ESLint - Pluggable JavaScript linter](http://eslint.org/) and Prettier to enforce code style.

- Always comment and keep them relevant as code changes. Remove commented blocks of code.

  _Why:_

  > Your code should be as readable as possible, you should get rid of anything distracting. If you refactored a function, don't just comment out the old one, remove it.

- Make your names search-able with meaningful distinctions avoid shortened names. For functions use long, descriptive names. A function name should be a verb or a verb phrase, and it needs to communicate its intention.

  _Why:_

  > It makes it more natural to read the source code.

- Organize your functions in a file according to the step-down rule. Higher level functions should be on top and lower levels below.

  _Why:_

  > It makes it more natural to read the source code.

<a name="enforcing-code-style-standards"></a>

### 7.2 Enforcing code style standards

- Use Prettier. Common configuration will be added soon here.

<a name="logging"></a>

## 8. Logging

To be added soon.

<a name="api"></a>

## 9. API

<a name="api-design"></a>

### 9.1 API design

Will be added soon.
