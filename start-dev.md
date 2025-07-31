# Openblocks Development Server Setup

## Prerequisites Setup

### 1. Install Java 17
```powershell
winget install Oracle.JDK.17
# or
winget install Microsoft.OpenJDK.17
```

### 2. Install Maven
```powershell
# Download Maven
Invoke-WebRequest -Uri "https://repo1.maven.org/maven2/org/apache/maven/apache-maven/3.9.6/apache-maven-3.9.6-bin.zip" -OutFile "maven.zip"

# Extract Maven
Expand-Archive -Path "maven.zip" -DestinationPath "C:\" -Force

# Set environment variables
$env:MAVEN_HOME = "C:\apache-maven-3.9.6"
$env:PATH += ";C:\apache-maven-3.9.6\bin"

# Verify installation
mvn --version
```

### 3. Install Dependencies
```powershell
# Frontend dependencies
cd client; npm install

# Backend dependencies
cd server/node-service; npm install
```

### 4. Start Required Services
```powershell
# Start MongoDB
docker run -d --name openblocks-mongodb -p 27017:27017 -e MONGO_INITDB_DATABASE=openblocks mongo

# Start Redis
docker run -d --name openblocks-redis -p 6379:6379 redis
```

### 5. Build Java Backend
```powershell
cd server/api-service; mvn clean package -DskipTests
```

## Start Development Servers

### Terminal 1 - Frontend:
```powershell
cd client; $env:API_PROXY_TARGET="http://localhost:8080"; $env:NODE_SERVICE_API_PROXY_TARGET="http://localhost:6060"; npm start
```

### Terminal 2 - Node.js Service:
```powershell
cd server/node-service; npm run dev
```

### Terminal 3 - Java Backend:
```powershell
cd server/api-service; java "-Dpf4j.mode=development" "-Dspring.profiles.active=openblocks" "-Dpf4j.pluginsDir=openblocks-plugins" -jar openblocks-server/target/openblocks-server-1.0-SNAPSHOT.jar
```

## Access Points
- **Frontend:** http://localhost:3000
- **Java Backend API:** http://localhost:8080
- **Node.js Service:** http://localhost:6060
- **MongoDB:** localhost:27017
- **Redis:** localhost:6379