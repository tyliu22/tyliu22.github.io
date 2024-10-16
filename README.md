If your Hugo-generated `index.html` file and other site contents are located in the `public` directory, but you want to use this directory for GitHub Pages, you can easily adjust the settings in GitHub to point to the `public` directory instead of the `docs` directory. However, GitHub Pages by default allows you to choose only from the root of the branch or the `docs` folder for hosting your site. To use content from the `public` directory directly, you need to manage how content gets into your GitHub repository.

## How to run the server

```
hugo server
```

Here's how to configure your project and GitHub repository so that your `public` folder can be used effectively for GitHub Pages:

### Option 1: Push the `public` folder to GitHub
This option involves pushing the contents of your `public` directory directly to the root of the `gh-pages` branch or another branch like `master` or `main` if you are using that for GitHub Pages.

1. **Build your site**:
   ```bash
   hugo  # This will regenerate your site and output to the public directory
   ```
   
2. **Navigate to your public directory**:
   ```bash
   cd public
   ```

3. **Initialize a git repository if it's not already a repo**:
   ```bash
   git init
   git remote add origin https://github.com/Tianyu-Liu11/your-repository.git
   ```

4. **Checkout to a new branch or use an existing one like `gh-pages`**:
   ```bash
   git checkout -b gh-pages  # Create and switch to gh-pages branch
   ```

5. **Add and commit changes**:
   ```bash
   git add .
   git commit -m "Deploy Hugo site"
   ```

6. **Push to GitHub**:
   ```bash
   git push origin gh-pages  # Pushes content to gh-pages branch
   ```

7. **Configure GitHub Pages**:
   - Go to your GitHub repository.
   - Click on “Settings” and navigate to "Pages" in the sidebar.
   - Under "Source", select the branch you pushed to (e.g., `gh-pages`) and save.

### Option 2: Automate with GitHub Actions
If you prefer automation, you can set up a GitHub Action to build your Hugo site and push the `public` directory to a specific branch like `gh-pages` automatically.

1. **Create a GitHub Action workflow file** in your repository under `.github/workflows/deploy.yml`:
   ```yaml
   name: Deploy Hugo Site

   on:
     push:
       branches:
         - main

   jobs:
     build-deploy:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         - name: Setup Hugo
           uses: peaceiris/actions-hugo@v2
           with:
             hugo-version: 'latest'
         - name: Build
           run: hugo
         - name: Deploy
           uses: peaceiris/actions-gh-pages@v3
           with:
             github_token: ${{ secrets.GITHUB_TOKEN }}
             publish_dir: ./public
   ```

This workflow triggers on a push to `main`, builds the Hugo site, and deploys the `public` directory to the `gh-pages` branch.

By following either of these methods, you can effectively host your site from the `public` folder using GitHub Pages. Remember, after setting up, always check your GitHub Pages settings to ensure it's pointing to the right branch and folder according to how you choose to deploy your site.
