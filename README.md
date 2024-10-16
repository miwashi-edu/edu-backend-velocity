# edu-backend-velocity

> I added config.js, this is a good practice to separate the reading of environment in a separate file.
>
> Rules: Work til first fail, then remove the folder and start over. Everything must be written, not copy/pasted.
>

## level-1

### setup
```bash
cd ~
cd ws
rm -rf backend-velocity #Om den finns
mkdir backend-velocity
cd backend-velocity
npm init -y
mkdir src
touch ./src/service.js
touch ./src/app.js
npm pkg set scripts.start="node ./src/service.js"
npm pkg set scripts.dev="node --watch ./src/service.js"
npm install express
npm install dotenv
```
###  heredoc config.js

```bash
cat > ./src/config.js << 'EOF'
require('dotenv').config();
const PORT = process.env.PORT || 3000;
module.exports = {PORT};
EOF
```

###  heredoc app.js

```bash
cat > ./src/app.js << 'EOF'
const express = require('express');
const app = express();
app.use(express.json());

app.get('/', (req,res) => {
    res.json({success: true});
});

module.exports = app;
EOF
```

###  heredoc service.js

```bash
cat > ./src/service.js << 'EOF'
const { PORT } = require('./config.js');
const app = require('./app.js');

app.listen(PORT, () => {
    console.log(`http server listening on port ${PORT}`);
});
EOF
```



### add to git
```bash
git init
git add .
git commit -m "Initial commit"
```


```mermaid
C4Context
    title Backend Architecture

    UpdateLayoutConfig($c4ShapeInRow="1", $c4BoundaryInRow="1")

    System_Boundary(Backend, "Backend System") {
        Boundary(ServiceAppBoundary, "Service & App File") {
            Component(Service, "Service", "starts the server and other services")
            Component(App, "App", "handles configurations")
        }

        Boundary(Subsystems, "Subsystems") {
            System(Routes, "Routes", "API")
            System(Controller, "Controller", "controls flow")
            System(Domain, "Domain", "Handles business logic")
        }
        
    }

    
    Rel(Routes, Controller, "Imports")
    Rel(Controller, Domain, "Imports")
    Rel(Service, App, "Imports")
    Rel(App, Routes, "Imports")
```
