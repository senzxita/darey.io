# MEAN Stack Deployment to Ubuntu in AWS

MEAN --> MongoDB, ExpressJS, AngularJS, Node.js
- Run and connect EC2 instance to your machie using ssh
- Update and upgrade the instance
- Install Node.js using `sudo apt install -y nodejs`
- Install MongoDB using `sudo apt install -y mongodb`
- Start the mongodb server after successful installation `sudo service mongodb start`, check status using `sudo service mongodb status` 
![image](https://user-images.githubusercontent.com/20463821/116609615-7a461300-a92c-11eb-8dec-afce7fdc3f64.png)

- Install npm package manager `sudo apt install -y npm`
- Install body-parser package `sudo npm install body-parser`
- Create *Books* directory and initialise npm in the directory to generate package.json file
```
$ mkdir Books && cd Books
$ npm init
```
- Create server.js file in Books directory `nano server.js`, copy and paste the code from the documentation, then save the file. 

