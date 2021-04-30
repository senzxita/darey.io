# A SIMPLE TO-DO APPLICATION ON MERN WEBSTACK
MERN ---> MongoDB, ExpressJS, ReactJS, Node.js

- Get AWS instance up and running, connect to your local machine via ssh
- Run `sudo apt update` and `sudo apt upgrade`

## Install Nodejs & npm
- `sudo apt-get install -y nodejs` (This command also installs npm )
- Check the node and npm version `node -v` and `npm -v`
- Create a directory for your To-Do project using `mkdir  Todo` amnd change directory to the newly created directory `cd Todo`

![image](https://user-images.githubusercontent.com/20463821/116490410-394bf100-a88f-11eb-8389-950cbb313109.png)

- Run `npm init` command which initialises the project and automatically creates a package.json file 

![image](https://user-images.githubusercontent.com/20463821/116490458-5e406400-a88f-11eb-9474-6309f59ec9d3.png)

## Install ExpressJS
- Install expressjs using `npm install express` and create a file index.js `touch index.js`
- Install dotenv module using `npm install dotenv`

![image](https://user-images.githubusercontent.com/20463821/116490543-8f209900-a88f-11eb-9c6e-15667f92e7b2.png)

- Edit index.js file and save file
- Test the server is working using `node index.js`

![image](https://user-images.githubusercontent.com/20463821/116490743-06eec380-a890-11eb-9a72-15ad9dd41d85.png)

- Open port 5000 from AWS console

![image](https://user-images.githubusercontent.com/20463821/116490844-56cd8a80-a890-11eb-88cd-9bab1a3a0365.png)

- Confirm port works on browser using http://instance-public-ip:5000

![image](https://user-images.githubusercontent.com/20463821/116490937-95634500-a890-11eb-83fb-4e31198c584d.png)

## Create Routes
- `mkdir routes` to create a directory that houses api.js file.
- Create api.js file using `vim api.js` and write your code, save file afterwards.

![image](https://user-images.githubusercontent.com/20463821/116491890-d2c8d200-a892-11eb-907d-d5eeb6812697.png)

## Models and Schemas
- To create a Schema and model, mongoose needs to be installed. Go back to the root directory using `cd ..` and install Mongoose `npm install mongoose`.

![image](https://user-images.githubusercontent.com/20463821/116492165-829e3f80-a893-11eb-9e32-8a877caf2c85.png)

- Create models directory, change directory to the models directory and create todo.js in the directory 
`mkdir models && cd models && touch todo.js`
- Edit todo.js file and save
- Update api.js file in routes directory in order to use the new model.

![image](https://user-images.githubusercontent.com/20463821/116493689-250bf200-a897-11eb-9a7f-1a72d800be76.png)

## MongoDB

- Login to MongoDB else, signup 
- Make use of the free trial, which provides a sandbox 
- Create network access and enter your ip address

![image](https://user-images.githubusercontent.com/20463821/116495729-69998c80-a89b-11eb-9dc0-926786335c6c.png)

- Create a database in *Collections* tab, click on *Add My Own Data*
![image](https://user-images.githubusercontent.com/20463821/116496891-f9d8d100-a89d-11eb-8164-b181de7cc85d.png)

- Get the connection string of the database, Go to *Clusters* indicated by the green box, click on the *CONNECT* button.
![image](https://user-images.githubusercontent.com/20463821/116496135-47543e80-a89c-11eb-9d22-505201cae9e0.png)

- Click on *Connect your application* 
![image](https://user-images.githubusercontent.com/20463821/116496228-7c609100-a89c-11eb-8898-e1a705b9f9ca.png)

- Select Nodejs from the dropdown menu, copy the connection string which is indicated by the green box.
![image](https://user-images.githubusercontent.com/20463821/116496390-d7928380-a89c-11eb-9f78-abfbb2624864.png)

- In the terminal, create .env file in the root project directory `nano .env` paste the connection string copied and save the file.
- Replace <password> with your password and <MyFirstDatabase> with name of the database you just created.
- Update in index.js file to reflect .env, to enable Node.js to connect to the database.
- start the node server `node index.js`

![image](https://user-images.githubusercontent.com/20463821/116608105-b7110a80-a92a-11eb-9740-91c141cb4bc0.png)

## Testing Backend Code with RESTful API
- Making use of Postman to test our API, download and install Postman
- Create a token for your API on https://app.asana.com  
- Create a POST request to http://54.84.247.41:5000/api/todos, so that an item is added to the todo database
- Add the token created from asana to Postman, under Authorization tab

![image](https://user-images.githubusercontent.com/20463821/116699973-22f08300-a9be-11eb-9355-b64398a64cd2.png)

- Add a key: "Content-Type" and value: "application/json", under the Headers tab
![image](https://user-images.githubusercontent.com/20463821/116700316-8bd7fb00-a9be-11eb-9266-3511f5064af4.png)

- To add the item to the database, write the item in the Body tab (raw), file format JSON, and click on the save button.
![image](https://user-images.githubusercontent.com/20463821/116701014-55e74680-a9bf-11eb-8d0c-d148f1579015.png)

- Status 200 indicates that the POST request was succesful.
- Create a GET request that returns all existing returns from the Todo database, making use of the same token

![image](https://user-images.githubusercontent.com/20463821/116701983-6f3cc280-a9c0-11eb-94b2-51aa007414fc.png)

- Create a DELETE request that delete the content specified in the Todo database
???
????

## Frontend Creation
- Use `create-read-app` command to scaffold the TODO app `npx create-react-app client` 
- The command above creates a client directory in the Todo directory, this directory contains the React app
![image](https://user-images.githubusercontent.com/20463821/116708504-ad89b000-a9c7-11eb-8a29-f2ac2cbdae95.png)

- To test the react app, one has to install some dependencies
- Install concurrently, which enables one to run more than one command on the terminal `npm install concurrently --save-dev`
- Install nodemon, a dependency for the react app `npm install nodemon --save-dev`. Used to restart the server and load new changes.
- Go to Todo directory, edit script block in the package.json file and save
```
$ cd ..
$ nano package.json
```
## Configure Proxy 
- Go to the client directory, `cd client`
- Open package.json file and add "proxy": "http://54.84.247.41:5000" to the file, save the file afterwards. To enable us access the application directly from browser.
- In the Todo directory run `npm run dev`

![image](https://user-images.githubusercontent.com/20463821/116718014-71f3e380-a9d1-11eb-932c-46f8a4ffda91.png)

- Open port 3000 on your AWS console

![image](https://user-images.githubusercontent.com/20463821/116717710-17f31e00-a9d1-11eb-8aaf-14af9fab504c.png)

- To access the application your browser, use http://54.84.247.41:3000

![image](https://user-images.githubusercontent.com/20463821/116718298-bb443300-a9d1-11eb-9a81-a04da7c4ccb2.png)

## Create React Components
