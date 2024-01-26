---
share: true
---


# Development Guide

Let this document serve as a guideline and a general SOP for the development
of the project.

## Table of Contents

1. [Project Structure](#project-structure)
2. [Development Setup](#development-setup)
3. [Development Workflow](#development-workflow)
4. [Code Style](#code-style)
5. [Testing](#testing)
6. [Documentation](#documentation)
7. [Deployment](#deployment)

## Project Structure

* __dm_frontend__: Frontend framework written in [Angular](https://angular.io)
* __dm_scraper__: Automated scraping engine written with [Scrapy](https://scrapy.org)
* __dm_api__: API service written in [Starlette](https://www.starlette.io)
* __dm_db__: Database service utilizing [PostgreSQL](https://www.postgresql.org)

![](../media/architecture.png)

> Simplified architecture diagram of the project. Each service is deployed as a docker container.

## Development Setup

[VSCode Developer Setup](VSCode%20Developer%20Setup.md)

### Required Prerequisites

* [Docker](https://www.docker.com/)
  * [Docker Desktop](https://www.docker.com/products/docker-desktop) or 
  * [Docker Engine](https://docs.docker.com/engine/install/)
* [Docker Compose](https://docs.docker.com/compose/)

### Installation Guide
1. Debian/Ubuntu 
      ```bash
      # Install Docker
     #Add Docker's official GPG key:
      sudo apt-get update
      sudo apt-get install ca-certificates curl gnupg
      sudo install -m 0755 -d /etc/apt/keyrings
      curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
      sudo chmod a+r /etc/apt/keyrings/docker.gpg

      # Add the repository to Apt sources:
      echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
        $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update

      sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 
      
      # update your user group to docker-ce
      sudo groupadd docker
      sudo usermod -aG docker $USER 

      # restart shell
      docker run hello-world
      ```
2. Windows
    * [Docker Desktop](https://www.docker.com/products/docker-desktop)

3. MacOS
    * [Docker Desktop](https://www.docker.com/products/docker-desktop)
### Frontend Prerequisites

* [Node.js](https://nodejs.org/en/download/)
* [Angular CLI](https://angular.io/cli)

### Installation Guide
1. Debian/Ubuntu 
     ```bash
    # Install Node.js and nvm 
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
    .  ~/.nvm/nvm.sh 
    nvm install node --lts

    # Install Angular CLI
    sudo npm install -g @angular/cli
      ```

### Backend Prerequisites

* [Python >=3.11](https://www.python.org/downloads/release/python-3117/)
* [Poetry](https://python-poetry.org/)

### Installation Guide

1. Debian/Ubuntu

    ```bash
    # Install Python 3.11.x 
    sudo apt update && sudo apt install build-essential libssl-dev zlib1g-dev \
      libbz2-dev libreadline-dev libsqlite3-dev curl \
      libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
    
    curl https://pyenv.run | bash 

    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
    echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(pyenv init -)"' >> ~/.bashrc 
    
    source ~/.bashrc 
    
    pyenv install 3.11
    
    pyenv --version

    # Install Poetry
    curl -sSL https://install.python-poetry.org | python3 -


    ```
2. MacOS 
    ```zsh 
    brew update
    brew install pyenv

    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
    echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
    echo 'eval "$(pyenv init -)"' >> ~/.zshrc
    
    exec "$SHELL"

    pyenv install 3.11
  
    curl -sSL https://install.python-poetry.org | python3 -
   ````

3. Windows
    * [Python 3.11.x](https://www.python.org/downloads/release/python-3117/)
    * [Poetry](https://python-poetry.org/docs/#installation) (Powershell)
      ```powershell
      (Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | py -
      ```

## Development Workflow

  High-Level:

  1. Create a new feature branch from `master`
  2. Make changes to the codebase
  3. Create a pull request to `master`
  4. Review and merge pull request
 
  __Detailed (CLI):__
  
  1. Create a new feature branch from `master`
      * `git checkout -b feature/<feature-name>`

  2. Make changes to the codebase
      * Commit often and with descriptive commit messages
	      * `git add <file1> <file2> <dir1> <etc...>`
	      * `git commit -m <commit message>`
      * Push changes to remote branch
	      * `git push origin feature/<feature-name>`
  
  3. Create a pull request to `master`
      * Create a pull request on GitHub
      * Assign a reviewer
      * Resolve any conflicts

  4. Review and merge pull request
      * Review code changes
      * Merge pull request
      * Delete remote branch
      * `git branch -d feature/<feature-name>`

__Detailed (Github Desktop):__

1. Create a new feature branch from `master`
    * `Branch > New Branch`
    * `Branch > Choose a branch to merge into > master`
    * `Branch > Create Pull Request`
    * `Branch > Publish Branch`

2. Make changes to the codebase
    * Commit often and with descriptive commit messages
    * Push changes to remote branch
    * `Branch > Push to origin`

3. Create a pull request to `master`
    * Create a pull request on GitHub
    * Assign a reviewer
    * Resolve any conflicts

4. Review and merge pull Request
    * Review code changes
    * Merge pull request
    * Delete remote branch
    * `Branch > Delete Branch`
## Code Style

Adhering to a consistent code style is crucial for maintaining readability and
scalability of our codebase. We use a combination of linters and formatters 
to enforce a consistent style across our Node.js and Python code.

__Node.js:__

We use [ESLint](https://eslint.org/) for linting our JavaScript code. It helps us
catch errors and inconsistencies in our code before they become problems.
Our ESLint configuration is based on the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript).

For formatting, we use [Prettier](https://prettier.io/). It integrates well with
ESLint and ensures that our code is consistently formatted.

__Python:__

For Python, we use [ruff](https://github.com/astral-sh/ruff) for linting and formatting.

__See the following [Python Style Guide](Python%20Style%20Guide.md) for a detailed breakdown of required styling guide.__

__General Guidelines:__

* Write clear and concise code comments.
* Follow the DRY (Don't Repeat Yourself) principle.
* Use meaningful variable and function names.
* Keep functions and methods short and focused on a single task.

Remember, the goal of having a code style is to make the code easier to read and 
understand. When in doubt, prioritize readability over cleverness.

## Testing
Testing is a critical part of our development process. It ensures that our code works 
as expected and helps prevent regressions. We use a combination of unit tests, 
integration tests, and end-to-end tests to validate our code.

__Unit Tests:__

Unit tests are small, isolated tests that verify the behavior of a single function 
or method. We use [Jest](https://jestjs.io/) for unit testing in Node.js and 
[pytest](https://docs.pytest.org/en/latest/) for Python.

__Integration Tests:__

Integration tests verify that different parts of our system work together correctly. 
This could involve testing interactions between our API and database, for example.

__End-to-End Tests:__

End-to-end tests simulate real user scenarios to ensure the entire system works 
together as expected. For our frontend, we use tools like [Cypress](https://www.cypress.io/) 
or [Protractor](https://www.protractortest.org/).

__Test Coverage:__

We aim for a high level of test coverage to ensure that most of our code is validated by 
tests. We use tools like [Istanbul](https://istanbul.js.org/) for JavaScript and 
[Coverage.py](https://coverage.readthedocs.io/) for Python to measure our test coverage.

__Continuous Integration:__

Our Continuous Integration (CI) system automatically runs our test suite on every pull 
request. This helps catch any issues before they're merged into `master`.

Remember, the goal of testing is to catch errors and ensure that our code is working as
expected. It's important to write thorough tests and keep them updated as the codebase evolves.

## Documentation
Good documentation is crucial for understanding, using, and contributing to our codebase. 
It helps onboard new team members, facilitates collaboration, and serves as a reference for future development.

__Code Comments:__

Inline comments should be used to explain the purpose of complex code blocks or algorithms. 
They should be concise and meaningful. 

__Function/Method Documentation:__

Every function or method should have a comment explaining what it does, its parameters, 
its return value, and any exceptions it may raise. For Python, we follow the 
[Google Python Style Guide](https://google.github.io/styleguide/pyguide.html) for docstrings. 
For JavaScript, we use [JSDoc](https://jsdoc.app/) style comments.

__README Files:__

Each project should have a README file at the root of the repository, explaining the 
purpose of the project, how to set up the development environment, how to run tests, 
and how to deploy the project.

__API Documentation:__

If the project exposes an API, it should be documented using a standard like [OpenAPI](https://www.openapis.org/). 
The documentation should include all available endpoints, their required parameters, 
and their responses.

__Wiki:__

For more extensive documentation, such as architecture overviews, 
design decisions, and process guides read the [Wiki]()
