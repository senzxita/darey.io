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


