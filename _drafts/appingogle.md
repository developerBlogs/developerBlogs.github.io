# Creating an application on Third Parties Website.

 summary: In order to enable the third party SignIn such as Google SignIn in our application, we need to first register or create an app in the third party websites. 

The first process of integrating a third party SignIn such as Google SignIn is  to register or create an app in the third party websites. By doing this we will be able to get the credentials such as **client id** and **client secret** we need in order to integrate the third party sign in our application. In this section we will just cover this part so if you have already done this you can go on to the next section of integration process.

We will be going through this in a step wise manner so its gets easy to understand for all of us.



## Creating an application on Google 

### Step 1: Go to [Api Console](https://console.developers.google.com/apis/dashboard).

### Step 2: Create a Project

![Screenshot from 2020-07-31 09-52-39](/home/sajan/Pictures/Screenshot from 2020-07-31 09-52-39.png)



### Step 3: Go to [Oauth Consent Screen](https://console.developers.google.com/apis/credentials/consent)

Before generating our credentials such as Client Id and Client Secrets we need to configure the consent screen first. So, lets create that

![Screenshot from 2020-07-31 09-58-54](/home/sajan/Desktop/PersonalProjects/sajan_basnet_blog/assets/img/goauth1.png)

It will redirect us to the next page where we need to provide our application name. So, go ahead and name it and press **Save**

### Step 3: Go to [Credentials](https://console.developers.google.com/apis/credentials)

Press Create Credentials and select the Oauth Client ID options. Then in application type we need to select web application.



![Screenshot from 2020-07-31 10-05-56](/home/sajan/Desktop/PersonalProjects/sajan_basnet_blog/assets/img/project_cred.png)



We also need to add our **redirect url** where the user will be redirected to from the google authorization server .

For testing it in the development environment lets add it as **http://localhost:3000/auth/google_oauth2/callback**.



![Screenshot from 2020-07-31 10-07-41](/home/sajan/Desktop/PersonalProjects/sajan_basnet_blog/assets/img/add_uri.png)

Make sure that for production server we need to add another "**redirect_uri**".



### Step 4: Get the Credentials. 

After Step 3 process we will be provided with a **Client Id** and **Client Secret** which we will need in integration process.



![Screenshot from 2020-07-31 10-08-09](/home/sajan/Desktop/PersonalProjects/sajan_basnet_blog/assets/img/goauth4.png)



## Creating an application on Facebook

### Step 1: Go To [Facebook for Developers](https://developers.facebook.com/apps/)

Register for developer account if you haven't. 

### Step 2: Create an App ID

![Screenshot from 2020-07-31 11-19-45](/home/sajan/Desktop/PersonalProjects/sajan_basnet_blog/assets/img/fboauth01.png)



### Step 3: Add Platform

Go to **Setting> Basic** in the left menu bar and in the App Domains field add the value as **localhost**

![Screenshot from 2020-07-31 11-20-22](/home/sajan/Desktop/PersonalProjects/sajan_basnet_blog/assets/img/fboauth02.png)



Scroll down and **Add platform**. Select **Website** and  add the **site url** as **http://localhost:3000**

![Screenshot from 2020-07-31 11-06-35](/home/sajan/Desktop/PersonalProjects/sajan_basnet_blog/assets/img/fboauth3.png)



Note down the **App Secret** and **App ID**.



**Please feel free to give your feedback on the comment section below or ping me at* *<**a* *aria-label**=**"Send email"* *href**=**"mailto:sajanbasnet75@gmail.com"**><**i* *class**=**"icon fa fa-envelope"* *style**=**"**font-size:32px; margin: 0px 3px;**"**></**i**></**a**>* *or*  *<**a* *aria-label**=**"My LinkedIn"* *target**=**"_blank"* *href**=**"https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"**><**i* *class**=**"icon fa fa-linkedin-square"* *style**=**"**font-size:32px; margin: 0px 3px;**"* *aria-hidden**=**"true"**></**i**></**a**>**. Have a great time :smiley_cat:**