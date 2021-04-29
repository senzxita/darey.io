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
- Install Express and use mongoose package to establish a schema for the database to store data `sudo npm install express mongoose`.
- Create a *apps* directory within the Books directory, and create a route.js file in the directory, copy and paste code from the documentation to the file, save file afterwards.
```
$ mkdir apps && cd apps
$ nano route.js
```
- Create *models* directory in the apps directory and create a book.js file in the models directory.
```
$ mkdir models && cd models 
$ nano book.js
```
 ## Access the routes with AngularJS
- Change directory to root project directory ie Book directory `cd ../..`
- Create public directory in the book directory `mkdir public`
- Create script.js in the public directory `nano script.js`. Copy, paste code and save file
- Create index.html file in the public directory `nano index.html`. Copy, paste code and save file
- Change directory to the project root directory
- Start server using `node server.js`
- Confirm it works using `curl -s http://54.235.60.72:3300`
- To ensure it works on the browser, open port 3300 on AWS console.
- Go to EC2 instance, select the instance, click on *security* The ports opened will be listed.
![image](https://user-images.githubusercontent.com/20463821/116619624-9b146580-a938-11eb-960b-9c46f9838382.png)
- Select the security group and select *Edit inbound rules* button
![image](https://user-images.githubusercontent.com/20463821/116619769-cdbe5e00-a938-11eb-933e-50cbf051ad47.png)

- Select *Add Rule* buuton and create a Custom TCP type on port 3300 and use 0.0.0.0/0 CIDR
![image](https://user-images.githubusercontent.com/20463821/116619946-0e1ddc00-a939-11eb-8701-0a84967da6a2.png)

- Go to browser to confirm http://54.235.60.72:3300
![image](https://user-images.githubusercontent.com/20463821/116620052-34dc1280-a939-11eb-88ed-141fb7d14d2d.png)

- Add some books to the database

![image](https://user-images.githubusercontent.com/20463821/116620843-4a057100-a93a-11eb-8bb6-5575b3033188.png)









  

