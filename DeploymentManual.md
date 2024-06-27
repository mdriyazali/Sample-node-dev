<h1> Deployment Manual </h1>

##


<h3> Overview  </h3>


##

 <h4> This document outlines the steps taken to set up a CI/CD pipeline for deploying a Node.js application to an EC2 instance using GitHub Actions. It covers environment setup, configuration decisions, troubleshooting steps, and solutions to common issues encountered. </h4>



##


<h3> Enviroment Setup </h3>


1. Node.js and Dependencies

    
    - Decision: Use Node.js version 16 for compatibility with project dependencies.


    - Solution: Configured GitHub Actions workflow (ec2.yml) to use actions/setup-node@v2 with Node.js version 16.

##

2. Database configuration


    - Decision: Utilize PostgreSQL for database requirements with specific credentials (myuser, 123, mydb).

    
    - Solution: Configured environment variables (POSTGRES_*) in .env.test and ensured the correct connection URL (POSTGRES_DB_CONNECTION_URL) for testing purposes


##

3. GitHub Actions Workflow


    - Decision: Implement CI/CD pipeline triggered on pushes and pull requests to the main branch.


    - Solution: Created ec2.yml with jobs for building, testing, tagging releases, and deploying to EC2 instance.


##


<h3> Troubleshooting and Issue Resolution </h3>



1. Issue: Prisma migration failing to connect to database


    - Solution: Updated .env.test with correct PostgreSQL credentials and ensured the database server (localhost:5432) was accessible and running during CI pipeline execution.



##


2. Issue: npm package installation failing during deployment


    - Solution: Ensured dependencies were correctly installed by running npm install as part of the deployment step in ec2.yml. Verified SSH connection and permissions (SSH_PRIVATE_KEY secret) for accessing the EC2 instance.


##


3. Issue: GitHub Actions workflow failing with configuration errors


    - Solution: Adjusted workflow (ec2.yml) to fix syntax errors, ensure proper indentation, and correct environment variable usage (secrets.TOKEN, secrets.SSH_PRIVATE_KEY, etc.).


##


<h2> Deployment Process </h2>


1. Build and Test:


    - Cloned the repository and set up Node.js environment.

    - Installed project dependencies (npm install).

    - Ran linting, tests, and built the application (npm run lint, npm test, npm run build).


##

2. Tagging and Releasing

    - Tagged each successful build with a version derived from package.json (v${VERSION}).

    - Pushed tags to GitHub (git push origin v${VERSION}) to trigger release actions (create-release@v1).


##


3. Deployment to EC2


    - Established an SSH connection (ssh) to the EC2 instance.

    - Pulled the latest changes from the main branch (git pull origin main).

    - Re-installed dependencies and rebuilt the application (npm install, npm run build).

    - Restarted the application using PM2 (pm2 restart app).

##



<h2>  Conclusion </h2>

This manual summarizes our journey setting up a robust CI/CD pipeline using GitHub Actions for deploying our Node.js application to an EC2 instance. It documents the decisions, steps, and solutions we applied to ensure clarity and consistency in our deployment process
