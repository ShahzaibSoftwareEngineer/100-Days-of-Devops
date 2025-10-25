# Day 69: Installing Jenkins Plugins 🔌

## 📚 Theory Overview

### What are Jenkins Plugins?

Jenkins plugins are extensions that add functionality to Jenkins. They allow you to customize and extend Jenkins capabilities without modifying the core system. Think of them as apps for your smartphone - each plugin adds specific features to Jenkins.

### Why Do We Need Plugins?

- **Version Control Integration**: Connect Jenkins with Git, GitLab, GitHub, Bitbucket, etc.
- **Build Tools**: Support for Maven, Gradle, NPM, Docker, and more
- **Cloud Integrations**: AWS, Azure, GCP integrations
- **Notifications**: Slack, Email, Microsoft Teams alerts
- **Security**: LDAP, SAML, OAuth authentication
- **Pipeline Enhancement**: Advanced pipeline features and visualization

### Common Jenkins Plugins

1. **Git Plugin**: Integrates Git version control with Jenkins
2. **GitLab Plugin**: Connects Jenkins with GitLab repositories and CI/CD
3. **Docker Plugin**: Build and deploy Docker containers
4. **Pipeline Plugin**: Create complex CI/CD pipelines as code
5. **Blue Ocean**: Modern UI for Jenkins pipelines

---

## 🎯 Today's Task

**Objective**: Install Git and GitLab plugins in Jenkins

**Environment**: Nautilus DevOps Jenkins Server

---

## 🛠️ Implementation Steps

### Step 1: Access Jenkins UI
```
URL: http://localhost:8080
Username: admin
Password: Adm!n321
```

### Step 2: Navigate to Plugin Manager
1. Click **Manage Jenkins** (left sidebar)
2. Click **Plugins** or **Manage Plugins**
3. Select **Available plugins** tab

### Step 3: Search and Select Plugins
- Search for **"Git"** → Check the checkbox ✅
- Search for **"GitLab"** → Check the checkbox ✅

### Step 4: Install Plugins
1. Click **Install** button at the bottom
2. Enable: ☑️ **"Restart Jenkins when installation is complete and no jobs are running"**
3. Wait for installation to complete

### Step 5: Verify Installation
After Jenkins restarts, verify plugins are installed:

**Via UI:**
- Go to **Manage Jenkins → Plugins → Installed plugins**
- Search for "git" and "gitlab"

---

## 📸 Screenshots

*Screenshot 1: Jenkins Dashboard*
*Screenshot 2: Plugin Manager - Available Plugins*
*Screenshot 3: Installation Progress*
*Screenshot 4: Installed Plugins Verification*

---

## 💡 Key Learnings

1. **Plugin Dependencies**: Some plugins require other plugins to work. Jenkins automatically handles these dependencies during installation.

2. **Restart Requirement**: Complex plugins often need a Jenkins restart to fully activate.

3. **Update Center**: Jenkins periodically checks for plugin updates. It's good practice to keep plugins updated for security and features.

4. **Plugin Compatibility**: Always check plugin compatibility with your Jenkins version.
