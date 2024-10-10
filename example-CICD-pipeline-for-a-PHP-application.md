What is a CI CD pipeline?
continuous integration and continuous deployment
Overview. A continuous integration and continuous deployment (CI/CD) pipeline is a series of steps that must be performed in order to deliver a new version of software. 
CI/CD pipelines are a practice focused on improving software delivery throughout the software development life cycle via automation.




Here’s an example of a **CI/CD best practice** setup for a PHP application, using **GitLab CI**, **Docker**, and **Kubernetes** for deployment:

### Example CI/CD Pipeline for a PHP Application

#### **1. Directory Structure**
```
- app/
    - src/
    - tests/
    - Dockerfile
- .gitlab-ci.yml
- docker-compose.yml
```

Yes, you can integrate the **pre-commit checks** into your CI/CD pipeline to enforce code quality checks at every stage automatically, in addition to or as an alternative to local pre-commit hooks.

Here’s how you can add **pre-commit checks** like **PHPStan**, **PHPCS**, and **PHPUnit** to your CI/CD pipeline to ensure that code quality is enforced during the CI build process. This way, even if a developer skips or doesn't run the pre-commit hook locally, the checks will still run in the CI pipeline.

### Updated CI/CD Example with Pre-commit Checks in GitLab CI

#### **1. Pre-commit Checks in CI/CD Pipeline (`.gitlab-ci.yml`)**

In this updated configuration, the **build** stage will include **code quality checks** (PHPStan, PHPCS, and PHPUnit tests) as part of the CI pipeline. The CI pipeline will fail if any of these checks fail.

```yaml
stages:
  - quality
  - build
  - test
  - deploy

variables:
  DOCKER_REGISTRY: registry.gitlab.com
  DOCKER_IMAGE: $DOCKER_REGISTRY/$CI_PROJECT_PATH/app

before_script:
  - echo "Running before script: Install dependencies"
  - apt-get update && apt-get install -y docker-compose

# Step 1: Pre-commit Quality Checks (PHPStan, PHPCS, PHPUnit)
pre-commit:
  stage: quality
  script:
    - echo "Running code quality checks with PHPStan, PHPCS, and PHPUnit..."
    - composer install
    - vendor/bin/phpstan analyse
    - vendor/bin/phpcs --standard=PSR12
    - vendor/bin/phpunit --testsuite unit
  only:
    - merge_requests
    - main
    - develop

# Step 2: Build Docker Image
build:
  stage: build
  script:
    - echo "Building the Docker image..."
    - docker build -t $DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA .
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $DOCKER_REGISTRY
    - docker push $DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA
  only:
    - main
    - develop

# Step 3: Run Tests (Integration Tests)
test:
  stage: test
  script:
    - echo "Running tests..."
    - docker-compose run --rm app vendor/bin/phpunit --coverage-text
  only:
    - merge_requests
    - main
    - develop

# Step 4: Deploy to Kubernetes
deploy:
  stage: deploy
  environment:
    name: production
    url: https://my-app.com
  script:
    - echo "Deploying to Kubernetes..."
    - kubectl set image deployment/app app=$DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA --record
  only:
    - main
  when: manual  # Manual trigger for production deployment
```

### Explanation of Pipeline Stages:

1. **Quality Stage (Pre-commit checks)**:
   - This stage runs **PHPStan**, **PHPCS**, and **PHPUnit** to enforce code quality before building and deploying the application.
   - If any of these checks fail, the pipeline will fail, preventing poor-quality code from being built or deployed.
   
2. **Build Stage**:
   - If the quality checks pass, the Docker image is built and pushed to the registry.

3. **Test Stage**:
   - Run additional tests, like integration or end-to-end tests, using the Docker container.

4. **Deploy Stage**:
   - Deploy the application to Kubernetes only if all previous stages succeed.

#### **2. Dockerfile**
To run the quality checks and tests inside the CI pipeline, make sure your **Dockerfile** includes all the required dependencies for PHPStan, PHPCS, and PHPUnit.

```dockerfile
FROM php:8.1-fpm

# Install required dependencies
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    libzip-dev \
    unzip \
    git \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install pdo pdo_mysql zip

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Install PHPStan, PHPCS, and PHPUnit
COPY composer.json composer.lock /var/www/html/
RUN composer install --no-dev

# Set working directory
WORKDIR /var/www/html

CMD ["php-fpm"]
```

#### **3. Composer Configuration**
Ensure that **PHPStan**, **PHPCS**, and **PHPUnit** are installed as development dependencies in your `composer.json`.

```json
{
  "require-dev": {
    "phpstan/phpstan": "^1.0",
    "squizlabs/php_codesniffer": "^3.6",
    "phpunit/phpunit": "^9.5"
  }
}
```

### Benefits of Integrating Pre-commit Checks into CI/CD:

1. **Consistency Across Environments**:
   - Even if a developer skips running the pre-commit checks locally, the CI/CD pipeline will still enforce them in a consistent environment.

2. **Automatic Code Quality Enforcement**:
   - PHPStan and PHPCS ensure that the code adheres to quality standards, while PHPUnit ensures that the code passes all unit tests before deployment.

3. **Reduced Risk of Bugs**:
   - Running unit tests and static analysis during the CI/CD process helps catch bugs early, reducing the chances of introducing bugs into production.

4. **Early Feedback for Developers**:
   - The pipeline will fail immediately if any code quality issues are detected, providing developers with early feedback.

5. **Reliable Deployments**:
   - Only high-quality code that passes all checks and tests will be built and deployed, increasing reliability in production environments.

By integrating these checks directly into the pipeline, you automate the process and maintain consistent code quality throughout the development lifecycle.
---

In a CI/CD pipeline, stages are defined as sequential steps in which specific actions or jobs are performed. The stages you mentioned (**quality**, **build**, **test**, and **deploy**) represent the typical lifecycle of a CI/CD process. These stages are executed in sequence, each one dependent on the success of the previous stage. If a stage fails, the pipeline stops, preventing bad code from moving forward.

### Example Overview of Stages:

1. **Quality**: Ensures that the code meets quality standards (linting, code style, static analysis).
2. **Build**: Compiles or builds the application (if applicable).
3. **Test**: Runs unit, integration, or other tests.
4. **Deploy**: Deploys the application to a server or environment (production, staging).

### Running Stages Manually

If you want to manually run the stages (or the jobs within them), you need to understand how CI/CD works for your platform. For example, in **GitLab**, the stages are defined in a `.gitlab-ci.yml` file, and each stage consists of jobs. The jobs within a stage can be manually triggered or rerun from the UI.

If you are not using a CI tool and want to run these stages locally, you can execute the scripts for each stage one by one.

### How to Manually Run Stages (Example with Shell Scripts)

For demonstration purposes, here's an example of how you can manually run stages by using simple shell scripts in a **bash** environment.

#### 1. **Quality Stage**

This stage runs static analysis tools like **PHPStan**, **PHPCS**, or linters.

```bash
# quality.sh
#!/bin/bash
echo "Running code quality checks..."
phpstan analyse
phpcs --standard=PSR12 src/
echo "Code quality checks completed."
```

Run it manually by executing:

```bash
./quality.sh
```

#### 2. **Build Stage**

This stage compiles or builds the application. In the case of PHP, this may involve creating autoload files, setting up environment variables, or preparing build artifacts.

```bash
# build.sh
#!/bin/bash
echo "Building the application..."
composer install --no-dev --optimize-autoloader
echo "Build completed."
```

Run it manually:

```bash
./build.sh
```

#### 3. **Test Stage**

This stage runs your tests, such as unit tests or integration tests using **PHPUnit** or similar tools.

```bash
# test.sh
#!/bin/bash
echo "Running tests..."
phpunit --testsuite unit
phpunit --testsuite integration
echo "Tests completed."
```

Run it manually:

```bash
./test.sh
```

#### 4. **Deploy Stage**

This stage deploys the application to your server. This could involve uploading files, running database migrations, or restarting services.

```bash
# deploy.sh
#!/bin/bash
echo "Deploying the application..."
# Example deploy commands:
scp -r ./build/ user@server:/var/www/app/
ssh user@server 'cd /var/www/app && php artisan migrate'
echo "Deployment completed."
```

Run it manually:

```bash
./deploy.sh
```

### Running Stages Sequentially

To simulate a CI/CD pipeline manually, you can chain the scripts to ensure that each one only runs if the previous one succeeds.

```bash
#!/bin/bash

# Run quality checks
./quality.sh
if [ $? -ne 0 ]; then
  echo "Quality checks failed."
  exit 1
fi

# Run build
./build.sh
if [ $? -ne 0 ]; then
  echo "Build failed."
  exit 1
fi

# Run tests
./test.sh
if [ $? -ne 0 ]; then
  echo "Tests failed."
  exit 1
fi

# Deploy
./deploy.sh
if [ $? -ne 0 ]; then
  echo "Deployment failed."
  exit 1
fi

echo "Pipeline completed successfully."
```

In this script:
- `./quality.sh` runs the quality stage.
- If it succeeds (`$?` is the exit code, which is `0` on success), it moves on to `./build.sh`.
- If any stage fails, the pipeline stops, just like in a CI/CD system.

### Example in GitLab CI/CD

Here’s how the same concept could be defined in a **GitLab CI/CD** pipeline (`.gitlab-ci.yml`):

```yaml
stages:
  - quality
  - build
  - test
  - deploy

quality:
  stage: quality
  script:
    - phpstan analyse
    - phpcs --standard=PSR12 src/
  only:
    - main

build:
  stage: build
  script:
    - composer install --no-dev --optimize-autoloader
  only:
    - main

test:
  stage: test
  script:
    - phpunit --testsuite unit
    - phpunit --testsuite integration
  only:
    - main

deploy:
  stage: deploy
  script:
    - scp -r ./build/ user@server:/var/www/app/
    - ssh user@server 'cd /var/www/app && php artisan migrate'
  only:
    - main
  when: manual
```

In this pipeline:
- Each stage is defined separately (`quality`, `build`, `test`, `deploy`).
- The pipeline will stop if any stage fails (unless explicitly configured to continue).
- The `deploy` stage is marked `manual`, meaning it will require human intervention to trigger.

### Summary
- **Stages**: Each stage in a CI/CD pipeline represents a logical grouping of jobs (quality checks, build, testing, deployment).
- **Manual Execution**: You can simulate running stages manually by creating individual shell scripts and chaining them together to mimic a CI/CD process.
- **Automated CI/CD**: Tools like GitLab CI, Jenkins, and GitHub Actions allow you to define stages that run automatically based on events (like code pushes).
