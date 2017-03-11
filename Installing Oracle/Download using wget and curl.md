# Download using wget and curl

Downloading Oracle in linux is a bit tricky. Oracle provide a shell schript to download oracle, but for me it was taking more time to download, So here is the trick. 

> Do this in your GUI browser, like chrome or firefox.

1. First create an account in oracle and [Signin](https://login.oracle.com/mysso/signon.jsp).

	![](https://github.com/SqlAdmin/Oracle_CheatSheet/blob/master/Images/Oracle%20download-signin.png)

1. [Click here](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html) to download oracle for Linux and accept the license. 
	![](https://github.com/SqlAdmin/Oracle_CheatSheet/blob/master/Images/Oracle%20download-downlaod%20file.png)

2. Click the files you want to download and while downloading just pause it copy the download link.
	![](https://github.com/SqlAdmin/Oracle_CheatSheet/blob/master/Images/Oracle%20download-copy%20link.png)

3. Now go to your terminal, install wget or curl. 

	```sh 
   -- For Ubuntu
	 apt-get install wget curl

	-- For RedHat,Centos and Amazon Linux
	 yum install wget curl	


4. Now type the below command to download via wget. 

	```sh 
    wget <paste the like> -O filename.zip
    Eg: 
    wget http://download.oracle.com/otn/linux/oracle12c/122010/linuxx64_12201_database.zip?AuthParam=111111111_cb3a26b3a63c89b4405a1e32822d19fc -O file1.zip

![](https://github.com/SqlAdmin/Oracle_CheatSheet/blob/master/Images/Oracledownload-wget.png)