# Siemens_PLC_NodeJS
Read siemens plc DB value and diaplay the value in Web browser

1)First install Node Js application:
  https://nodejs.org/en/download/prebuilt-installer
  chosse the nodejs version between V14.0.0 to 16.0.0 , download and install.

2)Set Up Your Project(choose your path)
  2.1_Create a new directory for your project:
    mkdir my-node-app
    cd my-node-app
  2.2_Initialize a new Node.js project(Run the cmd in the same path of the project)
    npm init -y
  2.3_Install Required Libraries(Run the cmd in the same path of the project)
    npm install nodes7
3)Creat a JavaScript file with .js extension(create file in the same path of the project)

4)Copy the code to the javascript file:

################################################

  const http = require('http');
  const nodes7 = require('nodes7');
  const conn = new nodes7();
  let concatenatedValues = ''; // Variable to store concatenated values
  
  // Define the variables for DB9 addresses from 992 to 1001 as CHAR
  const variables = {
    DB9_ADDR992: 'DB9,CHAR992', // Address 992
    DB9_ADDR993: 'DB9,CHAR993', // Address 993
    DB9_ADDR994: 'DB9,CHAR994', // Address 994
    DB9_ADDR995: 'DB9,CHAR995', // Address 995
    DB9_ADDR996: 'DB9,CHAR996', // Address 996
    DB9_ADDR997: 'DB9,CHAR997', // Address 997
    DB9_ADDR998: 'DB9,CHAR998', // Address 998
    DB9_ADDR999: 'DB9,CHAR999', // Address 999
    DB9_ADDR1000: 'DB9,CHAR1000', // Address 1000
    DB9_ADDR1001: 'DB9,CHAR1001'  // Address 1001
  };
  
  conn.initiateConnection({ port: 102, host: '192.168.1.86', rack: 0, slot: 1, debug: false }, connected);
  
  function connected(err) {
    if (err) {
      console.error("Connection error:", err);
      process.exit(1);
    }
  
    conn.setTranslationCB(tag => variables[tag]); // Set the translation for variable names
    conn.addItems(Object.keys(variables)); // Add all variables to read
    conn.readAllItems(valuesReady); // Read all items
  }
  
  function valuesReady(err, values) {
    if (err) {
      console.error("Error reading values:", err);
      return;
    }
  
    // Concatenate read values into a single string
    concatenatedValues = Object.values(values).join('');
    console.log("Concatenated values:", concatenatedValues);
  }
  
  // Create an HTTP server
  const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end(`
      <html>
        <head>
          <title>PLC Values</title>
        </head>
        <body>
          <h1>Read Values from PLC</h1>
          <p>${concatenatedValues}</p>
        </body>
      </html>
    `);
  });
  
  // Start the server
  const PORT = 3000;
  server.listen(PORT, () => {
    console.log(`Server running at http://localhost:${PORT}/`);
  });
  
################################################

5) now change the input of your plc
  PLC IP Address
  PLC DB No
  PLC Starting Address

6)Run your code in cmd (go to the project path and run the cmd)
  node filename.js 
  example 
  node index.js
  
7) view output in web browser
  http://localhost:300
  
