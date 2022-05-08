# zephyrExport
Workaround to bulk export Tests in Zephyr Squad from Jira JQL filter

👉 **Why would we need to export Test Cases from Zephyr?**
To reuse them in another project or to share them with the customer, for example. 

The problem is that Zephyr Squad don't allow natively to bulk-export the test from a JQL Filter through its GUI.
If we have tons of test cases to export, do it one by one is not an option. 

**Solution:** A workaround using the Zephyr API and Postman to generate the export via a personalized JQL Filter that allows us to export the whole set in just a minute.

## How to use this method Step by Step

### Step 0: Preconditions
- Download the postman collection from [here](https://github.com/matias-pirez/zephyrExport/)
- Download and install Postman
- Import the Collection "Export Zephyr.json" as a file

### Step 1: Build your JQL Filter in JIRA
Make the JQL Filter that returns the set of tests you want to bulk export. 
For example:
```
project = "MYPROJECT" AND issuetype = Test AND status = Open AND priority = Critical AND labels = Regression
```

### Step 2: In Zephyr: Obtain the x-acpt header
- Go to the project in Jira
- Go to Zephyr > Tests
- Open the browser dev tools (F12) and go to Network tab
- Export 1 test manually
- Find the "export" API call in Network tab
- Find and copy the value of the **x-acpt** header


![Screenshot of the X-acpt header in Zephyr](https://i.imgur.com/wOlzhD7.png)

### Step 3: In Postman: Set the collection variables values
- Open the collection "Zephyr Export"
- Go to "Variables"
- Set the values from jqlQuery and X-acpt to the values obtained in steps 1 and 2 respectively
- Save the changes (CTRL + S)


It should look like this
![Variables set in postman](https://i.imgur.com/kGtpnF6.png)

### Step 4: In Postman: Send the request POST / Export With JQL
- Don't forget to set the projectId in the request Body. This should be a number. If you don't know it, you can obtain it from the request payload in Step 2.
- Send the request
- Wait a few seconds. From 10 to 30 seconds or more depending on the number of tests to export.
- You should get a 200 OK response with a code. 
- If you get a 401 you need to repeat step 2 and update the x-acpt


### Step 5: In Postman: Send the request GET / Download XLSX result
- Send the request
- You should obtain 200 OK with a result with a lot of characters. 

### Step 6: Save the result to a file
Save the response to a file. It should detect it as an .xlsx file. 

And that's it, you'll have an excel file with all the tests and its fields. Ready to import to another project 😎
