Here is how you can add the images to your `README.md` file in the appropriate sections. I have updated your provided instructions and added the image references with the correct markdown syntax:

---

```markdown
# Capstone Project: E-Commerce Platform Deployment with Git, Linux, and AWS

## Scenario
You have been assigned to develop an e-commerce website for a new online marketplace named "MarketPeak." This platform will feature product listings, a shopping cart, and user authentication. Your objective is to utilize Git for version control, develop the platform in a Linux environment, and deploy it on an AWS EC2 instance. You can find a suitable website template here to kickstart your development.

---

## Tasks

### 1. Implement Version Control with Git

#### 1.1. Initialize Git Repository
Begin by creating a project directory named "MarketPeak_Ecommerce".

Inside this directory, initialize a Git repository to manage your version control.

```bash
mkdir MarketPeak_Ecommerce
cd MarketPeak_Ecommerce
git init
```

#### 1.2. Obtain and Prepare the E-Commerce Website Template
Instead of developing the website from scratch, you'll use a pre-existing e-commerce website template. This approach allows you to focus on the deployment and operational aspects, rather than on web development. The actual web development is done by web/software developers on the project.

- **Download a Website Template**: Visit Tooplate or any other free template resource, and download a suitable e-commerce website template. Look for templates that are ready to use and require minimal adjustments.
- **Prepare the Website Template**: Extract the downloaded template into your project directory, `MarketPeak_Ecommerce`.
- **Customize the Template (Optional)**: If desired and you have basic web development skills, you can make minor customizations to the template to tailor it to "MarketPeak". This might include updating logos, changing color schemes, or modifying text to better fit the marketplace's brand identity.

#### 1.3. Stage and Commit the Template to Git
Add your website files to the Git repository.
Set your Git global configuration with your username and email.
Commit your changes with a clear, descriptive message.

```bash
git add .
git config --global user.name "YourUsername"
git config --global user.email "youremail@example.com"
git commit -m "Initial commit with basic e-commerce site structure"
```

#### 1.4. Push the code to your GitHub repository
After initializing your Git repository and adding your e-commerce website template, the next step is to push your code to a remote repository on GitHub.

1. **Create a Remote Repository on GitHub**: Log into your GitHub account and create a new repository named "MarketPeak_Ecommerce". Leave the repository empty without initializing it with a README, .gitignore, or license.
2. **Link Your Local Repository to GitHub**: In your terminal, within your project directory, add the remote repository URL to your local repository configuration.

```bash
git remote add origin https://github.com/your-git-username/MarketPeak_Ecommerce.git
```

> *Note*: Make sure you replace "your-git-username" with your actual Git username.

3. **Push Your Code**: Upload your local repository content to GitHub.

```bash
git push -u origin main
```

---

### 2. AWS Deployment

#### 2.1. Set Up an AWS EC2 Instance
1. Log in to the AWS Management Console.
2. Launch an EC2 instance using an Amazon Linux AMI.
3. Connect to the instance using SSH.

#### 2.2. Clone the repository on the Linux Server
Before deploying your e-commerce platform, you need to clone the GitHub repository to your AWS EC2 instance. This process involves authenticating with GitHub and choosing between two primary methods of cloning a repository: SSH and HTTPS. To see the SSH or HTTP link to clone your repository, navigate to your repository in GitHub console.

- **Select the code as highlighted in the image below**:

![SSH Clone](2.2%20ssh.png)

#### SSH Method:
1. On your EC2 instance, generate SSH keypair using `ssh-keygen`.
2. Display and copy your public key.

```bash
cat /home/ubuntu/.ssh/id_rsa.pub
```

> *Note*: Your SSH public key will be different.

3. Add the SSH public key to your GitHub account.

![SSH Key GitHub Account](ssh%20key%20github%20account.gif)

4. Use the SSH clone URL to clone the repository:

```bash
git clone git@github.com:yourusername/MarketPeak_Ecommerce.git
```

#### HTTPS Method:
For repositories that you plan to clone without setting up SSH keys, use the HTTPS URL. GitHub will prompt for your username and password:

```bash
git clone https://github.com/yourusername/MarketPeak_Ecommerce.git
```

#### 2.3. Install a Web Server on EC2
Apache HTTP Server (httpd) is a widely used web server that serves HTML files and content over the internet. Installing it on Linux EC2 server allows you to host the MarketPeak E-commerce site:

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

#### 2.4. Configure httpd for Website
To serve the website from the EC2 instance, configure `httpd` to point to the directory on the Linux server where the website code files are stored. Usually in `/var/www/html`.

1. **Prepare the Web Directory**: Clear the default `httpd` web directory and copy the `MarketPeak Ecommerce` website files to it.

```bash
sudo rm -rf /var/www/html/*
sudo cp -r ~/MarketPeak_Ecommerce/* /var/www/html/
```

2. **Reload httpd**: Apply the changes by reloading the httpd service.

```bash
sudo systemctl reload httpd
```

#### 2.5. Access Website from Browser
With `httpd` configured and website files in place, the MarketPeak Ecommerce platform is now live on the internet:

Open a web browser and access the public IP of your EC2 instance to view the deployed website.

![Access Website](2.5%20access%20website.png)

---

### 3. Continuous Integration and Deployment Workflow

To ensure a smooth workflow for developing, testing, and deploying your e-commerce platform, follow this structured approach. It covers making changes in a development environment, utilizing version control with Git, and deploying updates to your production server on AWS.

#### Step 1: Developing New Features and Fixes
1. **Create a Development Branch**: Begin your development work by creating a separate branch. This isolates new features and bug fixes from the stable version of your website.

```bash
git branch development
git checkout development
```

2. **Implement Changes**: On the development branch, add your new features or bug fixes. This might include updating web pages, adding new products, or fixing known issues.

#### Step 2: Version Control with Git
1. **Stage Your Changes**: After making your changes, add them to the staging area in Git.

```bash
git add .
```

2. **Commit Your Changes**: Securely save your changes in the Git repository with a commit. Include a descriptive message about the updates.

```bash
git commit -m "Add new features or fix bugs"
```

3. **Push Changes to GitHub**: Upload your development branch with the new changes to GitHub.

```bash
git push origin development
```

#### Step 3: Pull Requests and Merging to the Main Branch
1. **Create a Pull Request (PR)**: On GitHub, create a pull request to merge the development branch into the main branch. This process is crucial for code review and maintaining code quality.

2. **Review and Merge the PR**: Review the changes for any potential issues. Once satisfied, merge the pull request into the main branch, incorporating the new features or fixes into the production codebase.

```bash
git checkout main
git merge development
```

3. **Push the Merged Changes to GitHub**: Ensure that your local main branch, now containing the updates, is pushed to the remote repository on GitHub.

```bash
git push origin main
```

#### Step 4: Deploying Updates to the Production Server
1. **Pull the Latest Changes on the Server**: SSH into your AWS EC2 instance where the production website is hosted. Navigate to the website's directory and pull the latest changes from the main branch.

```bash
git pull origin main
```

2. **Restart the Web Server (if necessary)**: Depending on the nature of the updates, you may need to restart the web server to apply the changes.

```bash
sudo systemctl reload httpd
```

#### Step 5: Testing the New Changes
Access the Website: Open a web browser and navigate to the public IP address of your EC2 instance. Test the new features or fixes to ensure they work as expected in the live environment.

---

### 4. Capstone Submission

#### 4.1. Document Your Process
Provide a detailed `README.md` file documenting the steps you took to deploy the entire flow.
Include any troubleshooting or challenges you faced and how you overcame them.

#### 4.2. Share Git Repository
Share the Git repository containing the `README.md` file as your submission on darey.io.
```

This updated `README.md` includes the images for your SSH cloning, GitHub SSH key setup, and website access steps. Make sure the images are in the same directory as your `README.md` file, and they will display correctly when viewed in GitHub or a markdown viewer.
