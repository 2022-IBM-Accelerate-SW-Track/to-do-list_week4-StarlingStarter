# Project Week 4: To-do list application (Cont.)
## Introduction
As of now, you have completed Project Week 3 and should now have a fully functional React Application with the ability to navigate to an **About me** page and a **Home** page hosting the Todo List Application. The Todo List Application itself allows users to provide a task w/ a due date, create a list w/ those user inputs to be displayed on a webpage, mark those task as complete and remove them from the list, and etc. Currently, all data which is essentially our tasks in this case lives in the front-end (Todo List Application). Everytime we refresh the page or restart our application that data is lost. This is where the backend component comes into play. The backend component will allow us to communicate to our front-end component (Todo List Application) using express and save our data inside a database (json file) where data will not be lost after a page refresh or application restart. For Project Week 4, you will go thru the process of initializing and creating a backend component, using express to communicate with the front-end component, and using axios to communicate with backend component.

## Requirements
Feature requirements (Week 4 task is complete when you):
+ Create and Initialize an Express application (Backend)
+ Create a connection between the Backend and Front-end
+ Create a json file to represent your database
+ Create a **POST** request to submit data to a json file

Implementation requirements:
+ Use [**Express**](https://www.npmjs.com/package/express) w/in the backend component
+ Use [**Axios**](https://www.npmjs.com/package/axios) w/in the front-end component (Todo List Application)

## Instructions

### Express APP (Backend)
1. In this step, we will be going thru the process of creating an Express application w/ in our Todo List Application. **Note:** From here on out, the term Backend will correspond to our Express Application, Front-end will correspond to our Todo List Application, and vice versa.
      + Navigate to our project's root directory and run the following command w/in the terminal. **Hint:** Essentially, this is the directory where our `src` and `public` folders are located.
        1. Create a new folder called `backend` that will essentially host our Express application by running the following command: `mkdir backend`
      + Navigate to the newly created `backend` folder and run the following commands w/in the terminal. **Hint:** Currently, this directory should be empty with no such sub-folders or files present. **Hint** Run the command `cd backend` or similar to change directory.
        1. Run the following command to initialize your directory with some basic information: `npm init`\
           **Note:** Accept all default configuration values by pressing **Return** or **Enter** until fully complete.
        2. Run the following command to install Express as a dependency: `npm install express`
        3. Run the following command to install cors as a dependency: `npm install cors`\
           **Note:** Cors allows us to relax the security applied to an API and you can learn more about this module [**here**](https://www.section.io/engineering-education/how-to-use-cors-in-nodejs-with-express/)
        5. Create a new file called `index.js` out of which we'll run our Express server by running this command: `touch index.js`\
           **Note:** If this command doesn't work, look into creating the file thru a file explorer or VS code.
         
      + The file structure of your project should now look similar to what is shown on the screenshot below:
        <img width="299" alt="Screen Shot 2022-06-23 at 6 25 55 AM" src="https://user-images.githubusercontent.com/57464095/175310108-65d0525c-c0b4-4432-8c12-a01ce7a0c05e.png">
           
2. In this step, we will be using Express to create a simple web server that will then be ran on a specified port.\
   **Note:** As you follow along w/ these sub-steps, place each snippet of code below the other.
      + Navigate to `backend/index.js`
        1. Implement the code snippet provided below:
           ```
           const express = require("express"),
                  app = express(),
                  port = process.env.PORT || <port>,
                  cors = require("cors");
           const bodyParser = require('body-parser');
           const fs = require("fs");
           ```
           **Note:** This snippet of code is importing external modules and setting the environment variables. Make sure to replace `<port>` with a port number of your choosing such as **8080** or **3001** keep note of this port number for future usage. Click on the following links [**express**](https://expressjs.com/en/5x/api.html), [**cors**](https://expressjs.com/en/resources/middleware/cors.html), [**body-parser**](https://expressjs.com/en/resources/middleware/body-parser.html), and [**fs**](https://nodejs.dev/learn/the-nodejs-fs-module) to learn more about these modules and their usage.
        2. Implement the code snippet provided below:
           ```
           app.use(cors());
           app.use(bodyParser.json({ extended: true }));
           app.listen(port, () => console.log("Backend server live on " + port));
           ```
           **Note:** This snippet of code sets up our express application and returns a message back to console once our application is running.
        3. Implement the code snippet provided below:
           ```
           app.get("/", (req, res) => {
           res.send({ message: "Connected to Backend server!" });
           });
           ```
           **Note:** This snippet of code returns a message once a **GET** request to the specified route is made.
         4. Implement the code snippet provided below:
            ```
            app.post("/add/item", addItem)
            ```
            **Note:** This snippet of code makes a call the `addItem` function once a **POST** request to the specified route is made.
         5. Implement the code snippet provided below:
            ```
            function addItem (request, response) {
            let id = request.body.jsonObject.id
            let task = request.body.jsonObject.task
            let curDate = request.body.jsonObject.currentDate
            let dueDate = request.body.jsonObject.dueDate
            var newTask = {
              ID: id,
              Task: task,
              Current_date: curDate,
              Due_date: dueDate
            }
            const jsonString = JSON.stringify(newTask)

            var data = fs.readFileSync('database.json');
            var json = JSON.parse(data);
            json.push(newTask);
            fs.writeFile("database.json", JSON.stringify(json), function(err, result) {
              if (err) { console.log('error', err) }
              else { console.log('Successfully wrote to file') }
            });
            response.send(200)
            }
            ```
            **Note:** This snippet of code takes in a request body from the Todo List Application which represents a `todo` item. The body is then converted into a new json object called `newTask` to represent the new `todo` item. The new json object is finally appended to a json list located in a file called `database.json` to represent our `todos` list.
           
3. In this step, we will be creating a json file to act as our database and hold data submitted from our Front-end application once a user clicks on the **Add** button.
      + Navigate to the `backend` directory. **Hint:** This is the directory that only contains package.json, package-lock.json, and index.js files.
        1. Create a new file called `database.json` out of which we'll store the data received from the front-end by running this command: `touch database.json`\
           **Note:** If this command doesn't work, look into creating the file thru a file explorer or VS code.
      + Navigate to `backend/database.json`
        1. Implement the code snippet provided below:
           ```
           []
           ```
           **Note:** The square brackets must be placed within this json file or we will receive an error when trying to append `todo` items within a list. Square brackets corresponds to an array.
      + The file structure of your project should now look similar to what is shown on the screenshot below:
        <img width="302" alt="Screen Shot 2022-06-23 at 11 27 59 AM" src="https://user-images.githubusercontent.com/57464095/175369370-5a323053-deff-43a3-ad1c-bca2918135f8.png">

### Todo List APP (Front-End)
1. In this step, we will be implementing axios in order to submit requests to the Express Application as well as to receive a response.
      + Navigate to our project's root directory and run the following command w/in the terminal. **Hint:** Essentially, this is the directory where our `src` and `public` folders are located.
        1. Run the following command to install Axios as a dependency: `npm install axios`
      + Navigate to `src/component/AddTodo.js`
        1. Import the Axios library at the top of our file:
           ```
           import Axios from "axios";
           ```
        2. In the handleSubmit function, implement the code snippets provided below before performing the **addTodo** action:
           ```
           const jsonObject = {
              id: this.state.id,
              task: <value representing the task content>,
              currentDate: <value representing the date/time task was added>,
              dueDate: <value representing the date/time task is due>
           };
           ```
           **Note:** This snippet of code is creating a json object that will be used as a body request to be sent to the `addItem` function located in our Express application. Place this code snippet below the code snippet above and make sure to replace the comments w/ the updated values for the following remaining keys: `task`, `currentDate`, and `dueDate`.
           ```
           Axios({
              method: "POST",
              url: "http://localhost:<port>/add/item",
              data: {jsonObject},
              headers: {
                 "Content-Type": "application/json"
              }
           }).then(res => {
              console.log(res.data.message);
           });
           ```
           **Note:** This snippet of code is making a **POST** request the `addItem` function located in our Express Application and returning a response message. Make sure to replace `<port>` with the port number that was used in the Express Application process such as **8080** or **3001**.

## Running Application
Upon completion of Week 4 Lab Project, all the necessary components and functions should be implemented in order to successfully send and receive data between the Client Side (Todo List Application) and Server side (Express Application). Now we will go thru the steps in simplifying the process of setting up and and running your applications. 

1. Navigate to `package.json` file located in our project's root directory. **Hint:** Essentially, this is the directory where our `src` and `public` folders are located.
   + Add the following scripts to the `scripts` property and save the file.
     ```
     "install-backend": "cd backend && npm install",
     "install-both": "npm install & npm run install-backend",
     "backend": "cd backend && node index.js",
     "start-both": "npm run backend & npm start"
     ```
   + The `package.json` file `scripts` property should now look similar to the screen shot shown below:
     ![Screen Shot 2022-06-24 at 12 31 47 AM](https://user-images.githubusercontent.com/57464095/175486138-37ee5abb-1409-4305-aeaa-eb821dff3781.png)

     **Note:** This configuration will allow us install all dependencies needed for both our Front-end and Backend application as well as running both application from one directory instead of creating multiple terminals.
     
2. Navigate to our project's root directory once again and run the following commands w/in the terminal. **Hint:** Essentially, this is the directory where our `src` and `public` folders are located.
  + Run `npm run install-both` to install all dependencies for both applications (Todo List Application and Express Application)
  + Run `npm run start-both` to start up both applications (Todo List Application and Express Application)

**Optional:** To Test and see if your Express Application was implemented correctly, run the following command: `npm run backend`\
**Note:** Make sure all processes are terminated before running this command.
  + There should be no error message and a message similar to the screenshot provided below should be displayed:
    ![Screen Shot 2022-06-24 at 12 40 20 AM](https://user-images.githubusercontent.com/57464095/175487997-f8b2bd8c-8ee6-41bb-83da-f82f39c92dea.png)

## Pre-session Material
Here is a [**link**](https://ibm.ent.box.com/file/975198401278) to the pre-session material that was provided to you earlier.
