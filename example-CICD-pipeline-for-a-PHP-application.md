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

#### **2. `.gitlab-ci.yml` Configuration**
This file defines your CI/CD pipeline for GitLab. The pipeline includes building the Docker image, running tests, and deploying to Kubernetes.

```yaml
stages:
  - build
  - test
  - deploy

variables:
  DOCKER_REGISTRY: registry.gitlab.com
  DOCKER_IMAGE: $DOCKER_REGISTRY/$CI_PROJECT_PATH/app

before_script:
  - echo "Running before script: Install dependencies"
  - apt-get update && apt-get install -y docker-compose

# Step 1: Build Docker Image
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

# Step 2: Run Tests (Unit and Integration)
test:
  stage: test
  script:
    - echo "Running tests..."
    - docker-compose run --rm app vendor/bin/phpunit --coverage-text
  only:
    - merge_requests
    - main
    - develop

# Step 3: Deploy to Kubernetes
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

#### **3. Dockerfile**
A Dockerfile to containerize your PHP application.

```dockerfile
FROM php:8.1-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    libzip-dev \
    unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install pdo pdo_mysql zip

# Copy application files
COPY . /var/www/html

# Set working directory
WORKDIR /var/www/html

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer
RUN composer install

CMD ["php-fpm"]
```

#### **4. Docker Compose (Local Testing and Development)**
Use `docker-compose.yml` to manage services for local testing.

```yaml
version: '3'
services:
  app:
    build:
      context: .
    ports:
      - "8000:80"
    environment:
      - APP_ENV=development
    volumes:
      - .:/var/www/html
  mysql:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: app
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
```

#### **5. Kubernetes Deployment**
In the pipeline, the `deploy` stage uses **kubectl** to deploy the new Docker image to a Kubernetes cluster. The pipeline only deploys the latest image to production when manually triggered. This ensures that only approved changes make it to production.

- **Rolling Deployments**: Kubernetes allows rolling deployments, where containers are gradually replaced without downtime.
- **Rollback Strategy**: By using `--record`, you can track changes and easily rollback if something goes wrong using `kubectl rollout undo`.

### **Best Practices in This Example:**
1. **Automated Testing**: Every commit triggers a test run with PHPUnit to catch issues early.
2. **Dockerization**: The application is containerized, ensuring consistency between development, testing, and production environments.
3. **CI/CD Pipeline Automation**: The pipeline automates the process of building, testing, and deploying, minimizing manual intervention.
4. **Kubernetes for Zero-Downtime Deployments**: Kubernetes manages rolling updates, reducing downtime during production deployments.
5. **Manual Approval for Production**: The deployment to production requires manual approval, ensuring human oversight for critical environments.
6. **Version Control of Deployments**: The deployment is version-controlled, allowing for easy rollbacks in case of issues.

### **Pipeline Flow:**
1. **Commit/Push**: Developer pushes code to the `main` branch.
2. **CI Pipeline Trigger**:
   - **Build Stage**: Docker image is built and pushed to the registry.
   - **Test Stage**: PHPUnit tests are run inside a Docker container to ensure code correctness.
   - **Deploy Stage**: After manual approval, the application is deployed to a Kubernetes cluster using the newly built Docker image.
3. **Monitoring**: Once deployed, monitoring tools (e.g., Prometheus) track performance and error rates, allowing for quick responses to issues.

This setup ensures continuous integration, automated testing, and safe deployments with minimal downtime while being easily scalable.

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
