Here’s a **complete GitHub README-style file** (formatted perfectly for VS Code and GitHub).
You can copy and save it as:

📄 **`Day72-Jenkins-Parameterized-Builds.md`**

---

````markdown
# 🚀 Day 72: Jenkins Parameterized Builds  

## 🧠 Overview  
In Jenkins, **Parameterized Builds** allow developers to provide inputs to jobs dynamically during execution.  
Instead of hardcoding configurations (like environment or build stage), parameters make builds flexible, reusable, and more adaptable for different environments — such as **Development**, **Staging**, and **Production**.  

Jenkins supports several types of parameters, including:
- 📝 **String Parameter** – For simple text values.  
- 📋 **Choice Parameter** – Dropdown list with predefined options.  
- 🔘 **Boolean Parameter** – For true/false values.  

This feature is especially useful in **CI/CD pipelines**, where the same build process is used across multiple environments with only small differences in configuration.

---

## ⚙️ Lab Steps  

### 1️⃣ Login to Jenkins  
- Open Jenkins in your browser.  
- Log in using:  
  ```text
  Username: admin  
  Password: Adm!n321
````

---

### 2️⃣ Create a New Jenkins Job

1. Click **“New Item”** on the Jenkins Dashboard.
2. Enter the job name:

   ```
   parameterized-job
   ```
3. Select **Freestyle project** and click **OK**.

---

### 3️⃣ Enable Parameterized Build

* Scroll down and check the box:
  ✅ **This project is parameterized**

---

### 4️⃣ Add Build Parameters

#### 🧩 String Parameter

* **Name:** `Stage`
* **Default Value:** `Build`

#### 🧩 Choice Parameter

* **Name:** `env`
* **Choices:**

  ```
  Development
  Staging
  Production
  ```

---

### 5️⃣ Add Build Step (Execute Shell)

Add the following script under **Build → Execute shell**:

```bash
echo "Stage: $Stage"
echo "Env: $env"
```

This will print out the parameters entered during the build.

---

### 6️⃣ Save and Build the Job

1. Click **Save**
2. Click **Build with Parameters**
3. Select:

   * `Stage = Build`
   * `env = Staging`
4. Click **Build**

---

### 7️⃣ Verify the Console Output

Go to **Build History → Console Output** and confirm the results:

```bash
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/parameterized-job
[parameterized-job] $ /bin/sh -xe /tmp/jenkinsXXXXXX.sh
+ echo Stage: Build
Stage: Build
+ echo Env: Staging
Env: Staging
Finished: SUCCESS
```


---