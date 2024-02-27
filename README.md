# custom-component-package

Here's a step-by-step procedure from generating a personal access token (PAT) to publishing and installing a ui-components package using the GitHub Package Registry:

1. Generate Personal Access Token (PAT):
Go to your GitHub account settings.
Navigate to "Developer settings" > "Personal access tokens."
Click "Generate token."
Enter a name for your token and select the necessary scopes:
write:packages for publishing packages.
read:packages for installing packages.
Click "Generate token."
Copy the generated token.
2. Set up Repository Secrets:
In your GitHub repository, go to "Settings" > "Secrets."
Click "New repository secret."
Name the secret, e.g., NPM_PUBLISH_TOKEN, and paste the token as the value.
Click "Add secret."

#You may need to login to github packages to effectivly use PAT
```
npm login --scope=@SCOPE --auth-type=legacy --registry=https://npm.pkg.github.com
```
write scope. Enter You github username.
Password: <Enter_PATH_token> 

3. Update GitHub Actions Workflow:
Assuming you are using GitHub Actions for your CI/CD workflow:

name: Publish Package

on:
  release:
    types:
      - created

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '14'

      - name: Install Dependencies
        run: npm ci

      - name: Authenticate with GitHub Packages
        run: echo "//npm.pkg.github.com/:_authToken=${{ secrets.NPM_PUBLISH_TOKEN }}" >> ~/.npmrc

      - name: Build and Publish Package
        run: npm publish --access public  # Adjust the command as needed

      # Add any other necessary steps for your build process

4. Update package.json:
Ensure your package.json includes the correct package name and version:

json
Copy code
{
  "name": "@yourusername/ui-components",
  "version": "1.0.0",
  // other package configuration
}
5. Publish the Package:
Commit and push your changes to trigger the workflow.
Create a new release on GitHub, or push a tag to trigger the workflow if you're using a release event.
6. Install the Package in Another Project:
Assuming you want to install the package in another project:

bash
Copy code
# Create an .npmrc file in your project's root directory
echo "//npm.pkg.github.com/:_authToken=${YOUR_PAT}" >> .npmrc
@yourusername:registry=https://npm.pkg.github.com/

# Install the package
npm install @yourusername/ui-components
Replace YOUR_PAT with the personal access token generated for read access (read:packages). This token can be the same or a different one used for publishing.

Following these steps should allow you to generate a token, publish a package, and install it in another project using the GitHub Package Registry. Ensure that you handle tokens securely and follow best practices for managing secrets and access tokens.
