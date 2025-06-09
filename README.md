✅ Step 1: Install and Configure Git
bash
Copy
Edit
# Check if Git is installed
git --version

# Configure Git with your name and email
git config --global user.name "VarunKumar-M"
git config --global user.email "varunkumsrm2494@gmail.com"
git config --global init.defaultBranch main
✅ Step 2: Generate SSH Key & Add to GitHub
bash
Copy
Edit
# Generate SSH key
ssh-keygen -t rsa -b 4096 -C "varunkumsrm2494@gmail.com" -f ~/.ssh/id_rsa -N ""

# Start ssh-agent and add the key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

# Copy the public key to GitHub -> Settings -> SSH and GPG keys
cat ~/.ssh/id_rsa.pub
➡️ Go to GitHub → Settings → SSH and GPG Keys → New SSH Key → Paste the copied key

✅ Step 3: Test SSH Connection
bash
Copy
Edit
ssh -T git@github.com
You should see something like:

bash
Copy
Edit
Hi VarunKumar-M! You've successfully authenticated, but GitHub does not provide shell access.
✅ Step 4: Create GitHub Repo Using Token
Go to GitHub → Settings → Developer settings → Personal access tokens → Generate new token

Select scopes: repo, workflow

Use the token in the following command:

bash
Copy
Edit
curl -H "Authorization: token YOUR_TOKEN" \
     -d '{"name":"hello-maven", "private":false}' \
     https://api.github.com/user/repos
Replace YOUR_TOKEN with your actual token.

✅ Step 5: Jenkins Pipeline Script for GitHub Maven Project
In Jenkins:

Click "New Item"

Choose "Pipeline"

Add the following to the pipeline script:

groovy
Copy
Edit
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository using HTTPS or SSH
                git branch: 'main', url: 'https://github.com/VarunKumar-M/hello-maven.git'
                // OR for SSH (preferred if using SSH setup):
                // git branch: 'main', url: 'git@github.com:VarunKumar-M/hello-maven.git'
            }
        }

        stage('Build') {
            steps {
                // Build the Maven project
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                // Run unit tests
                sh 'mvn test'
            }
        }

        // Optional additional stages like Deploy, SonarQube, etc.
    }
}
🔄 Optional GitHub Push Test (first-time setup)
bash
Copy
Edit
# Clone your repo (use SSH if you added your key)
git clone git@github.com:VarunKumar-M/hello-maven.git

cd hello-maven
echo "# Hello Maven" >> README.md
git add README.md
git commit -m "Initial commit"
git push origin main
