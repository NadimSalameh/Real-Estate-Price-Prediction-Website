# Real-Estate-Price-Prediction-Website
 In This data science project I will create  a Real Estate Price 
prediction website. I will first build a model using **Sklearn**
and **Linear Regression** using Banglore Home Prices dataset from *kaggle.com*.
Second step would be to write a python **flask
server** that uses the saved model to serve **http requests**. 
   Third component is the website built in **HTML**, **CSS** and 
   **javascript** that allows user to enter home square ft area,
 bedrooms etc and it will call python flask server to retrieve
  the predicted price. And Finally I will deploy the app to the **Cloud** 
**(AWS EC2")**

During model building I will use many data science concepts such as **data load and cleaning**,
**Outlier detection and Removal**, **Feature Engineering**, 
**Dimensionality Reduction**,** Gridsearchcv**for hyperparameter tunning, 
**k fold cross validation** etc.
 



## Authors

- [@NadimSalameh](https://github.com/NadimSalameh)


## Technologies and Tools

* Python

* Numpy and Pandas for data cleaning

* Matplotlib for data visualization

* Sklearn for model building

* Jupyter notebook, visual studio code 

* Python flask for http server

* HTML/CSS/Javascript for UI

* AWS , EC2 

![Alt text](https://github.com/NadimSalameh/Real-Estate-Price-Prediction-Website/blob/main/Home%20Price%20Prediction.png "Home Price Prediction")

## Deploy The App to the Cloud ( AWS EC2)

* Create EC2 instance using amazon console, also in security group add a rule to allow HTTP incoming traffic
* Now connect to your instance using this command :

```console
ssh -i "C:\Users\Viral\.ssh\Banglore.pem" ubuntu@ec2-3-133-88-210.us-east-2.compute.amazonaws.com*
```
* nginx setup:
i.Install nginx on EC2 instance using these commands:
```console
sudo apt-get update
sudo apt-get install nginx
```
ii.Above will install nginx as well as run it. Check status of nginx using:
``` console
sudo service nginx status
```
iii. Here are the commands to start/stop/restart nginx:
``` console
sudo service nginx start
sudo service nginx stop
sudo service nginx restart
```
iv.Now when you load cloud url in browser you will see a message saying "welcome to nginx" This means your nginx is setup and running.

* Now you need to copy all your code to EC2 instance. You can do this either using git or copy files using winscp. We will use winscp. You can download winscp from here: https://winscp.net/eng/download.php

* Once you connect to EC2 instance from winscp, you can now copy all code files into /home/ubuntu/ folder. The full path of your root folder is now: /home/ubuntu/BangloreHomePrices

* After copying code on EC2 server now we can point nginx to load our property website by default. For below steps:
i.Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this:
``` console
server {
    listen 80;
        server_name bhp;
        root /home/ubuntu/BangloreHomePrices/client;
        index app.html;
        location /api/ {
             rewrite ^/api(.*) $1 break;
             proxy_pass http://127.0.0.1:5000;
        }
}

```
ii. Create symlink for this file in /etc/nginx/sites-enabled by running this command:
```console
sudo ln -v -s /etc/nginx/sites-available/bhp.conf
```
iii.Remove symlink for default file in /etc/nginx/sites-enabled directory:
```console
sudo unlink default
```
iv.Restart nginx:
```console
sudo service nginx restart
```

* Now install python packages and start flask server:
``` console
sudo apt-get install python3-pip
sudo pip3 install flask numpy scikit-learn
python3 /home/ubuntu/BangloreHomePrices/client/server.py

 ```