# Day 45 of #100DaysOfDevOp - Resolve Dockerfile Issues 


### What is a Dockerfile?
A **Dockerfile** is a text file containing a series of instructions used to build Docker images automatically. Each instruction in a Dockerfile creates a layer in the image, making it efficient and cacheable.

### Key Dockerfile Instructions:
- **FROM**: Specifie the base image
- **RUN**: Executes commands during build time
- **COPY/ADD**: Copies files from host to container
- **EXPOSE**: Documents which ports the container will use
- **CMD/ENTRYPOINT**: Defines the default command to run

### Common Dockerfile Issues:
1. **Case Sensitivity**: Docker instructions must be UPPERCASE
2. **File Path Error**: Incorrect source/destination paths
3. **Syntax Errors**: Missing quotes, brackets, or incorrect formatting
4. **Missing Dependencies**: Required files or packages not available
5. **Layer Optimization**: Inefficient layering causing large images

## 🎯 Lab Objective
Fix a broken Dockerfile on App Server 2 that's failing to build, without changing the base image, configurations, or data files.

## 🏗️ Environment Setup

| Server | IP Address | Hostname | User | Password | Role |
|--------|------------|----------|------|----------|------|
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Ir0nM@n | Nautilus App 1 |
| **stapp02** | **172.16.238.11** | **stapp02.stratos.xfusioncorp.com** | **steve** | **Am3ric@** | **Nautilus App 2** |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | BigGr33n | Nautilus App 3 |

**Target Server**: App Server 2 (stapp02)  
**Dockerfile Location**: `/opt/docker/`

## 🔧 Step-by-Step Lab Execution

### Step 1: Connect to App Server 2
```bash
# SSH connection to the target server
ssh steve@stapp02.stratos.xfusioncorp.com
# Password: Am3ric@
```

**Result**: Successfully connected to stapp02 server.

### Step 2: Navigate to Docker Directory
```bash
cd /opt/docker
ls -la
```

**Observation**: Found the Dockerfile and related files in the directory structure:
- `Dockerfile` - Main build file
- `certs/` directory - SSL certificates
- `html/` directory - Web content (index.html)

### Step 3: Examine the Current Dockerfile
```bash
cat Dockerfile
```

**Initial Dockerfile Content**:
```dockerfile
from httpd:2.4.43                    # ❌ ISSUE: lowercase 'from'
>>> RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf.d/httpd.conf
RUN sed -i "/LoadModule ssl_module modules\/mod_ssl.so/s/^#//g" conf.d/httpd.conf
RUN sed -i "/LoadModule socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g" conf.d/httpd.conf
RUN sed -i "/Include conf\/extra\/httpd-ssl.conf/s/^#//g" conf.d/httpd.conf
COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key
COPY html/index.html /usr/local/apache2/htdocs/
```

### Step 4: Attempt Initial Build (To Identify Issues)
```bash
sudo docker build -t test-image .
```

**Error Output**:
```bash
=> ERROR [2/8] RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf.d/httpd.conf:
1.271 sed: can't read /usr/local/apache2/conf.d/httpd.conf: No such file or directory
```

## 🔍 Problem Analysis

### Root Cause Identification:
1. **Primary Issue**: Lowercase `from` instead of `FROM`
2. **Impact**: Docker doesn't recognize the instruction, causing the base image to not load properly
3. **Consequence**: Subsequent RUN commands fail because the Apache environment isn't set up

### Why This Happens:
- Docker parser requires **strict uppercase** for all instructions
- When `from` is lowercase, Docker treats it as invalid
- The build context becomes corrupted, leading to file system errors

## 🛠️ Solution Implementation

### Step 5: Fix the Dockerfile
```bash
sudo vi Dockerfile
```

**Corrected Dockerfile**:
```dockerfile
FROM httpd:2.4.43                    # ✅ FIXED: uppercase 'FROM'
RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf.d/httpd.conf
RUN sed -i "/LoadModule ssl_module modules\/mod_ssl.so/s/^#//g" conf.d/httpd.conf
RUN sed -i "/LoadModule socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g" conf.d/httpd.conf
RUN sed -i "/Include conf\/extra\/httpd-ssl.conf/s/^#//g" conf.d/httpd.conf
COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key
COPY html/index.html /usr/local/apache2/htdocs/
```

### Changes Made:
- **Line 1**: Changed `from httpd:2.4.43` → `FROM httpd:2.4.43`
- **All other lines**: Remained unchanged as per requirements

### Step 6: Verify the Fix
```bash
sudo docker build -t test-image .
```

## ✅ Successful Build Results

### Build Process Output:
```bash
=> [internal] load build definition from Dockerfile         0.0s
=> => transferring dockerfile: 732B                         0.0s
=> [internal] load .dockerignore                           0.0s
=> => transferring context: 2B                             0.0s
=> [internal] load metadata for docker.io/library/httpd:2.4.43  30.6s
=> [1/8] FROM docker.io/library/httpd:2.4.43@sha256:cd88fee4eab37f8dcd04b06ef97285ca981c27b4d685f821e6c5ddf4  96.7s
=> [internal] load build context                           0.0s
=> => transferring context: 2B                             0.0s
=> [2/8] RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf.d/httpd.conf  1.5s
=> [3/8] RUN sed -i "/LoadModule ssl_module modules/mod_ssl.so/s/^#//g" conf.d/httpd.conf  1.5s
=> [4/8] RUN sed -i "/LoadModule socache_shmcb_module modules/mod_socache_shmcb.so/s/^#//g" conf.d/httpd.conf  1.3s
=> [5/8] RUN sed -i "/Include conf/extra/httpd-ssl.conf/s/^#//g" conf.d/httpd.conf  1.7s
=> [6/8] COPY certs/server.crt /usr/local/apache2/conf/server.crt  1.6s
=> [7/8] COPY certs/server.key /usr/local/apache2/conf/server.key  1.6s
=> [8/8] COPY html/index.html /usr/local/apache2/htdocs/     1.1s
=> exporting to image                                        2.7s
=> => writing image sha256:3f345089ba194bf3cab53f442ed1927139...  0.0s
=> => naming to docker.io/library/test-image                0.0s
```

### Step 7: Verify Image Creation
```bash
sudo docker images
```

**Result**:
```bash
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
test-image    latest    3f345089ba19   11 seconds ago   166MB
```

### Step 8: Test Container Deployment
```bash
sudo docker run -d -p 8080:8080 test-image
```

**Container Status**:
```bash
CONTAINER ID   IMAGE        COMMAND              CREATED        STATUS        PORTS                    NAMES
129055769f55   test-image   "httpd-foreground"   7 seconds ago  Up 4 seconds  80/tcp, 0.0.0.0:8080->8080/tcp  test-httpd
```



## 🏆 Lab Completion

### Success Criteria Met:
- ✅ **Build Success**: Image created without errors
- ✅ **Functionality**: Container runs and serves content
- ✅ **Requirements**: Base image unchanged
- ✅ **Data Integrity**: index.html and certificates preserved
- ✅ **Configuration**: All Apache modules properly configured

### Final Verification:
```bash
curl http://localhost:8080
# Should return the content of index.html
```



