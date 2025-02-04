# Omnetpp-API-Fix
Guide to install and fix omnetpp to run API scripts using the libcurl modules

# OMNeT++ install
Go to https://omnetpp.org/download/ and select the correct download distribution based on your OS. (For most users that have Windows x64, this link will suffice: https://github.com/omnetpp/omnetpp/releases/download/omnetpp-6.1.0/omnetpp-6.1.0-windows-x86_64.zip)

After the download has been successful, make sure to copy the contents of the archive in a safe location that **DOES NOT CONTAIN ANY SPACE**, for example OMNeT++ can be installed under **Desktop**, but not under **Program Files**

# First Run
## Install and Configure OMNeT++
Inside the omnetpp folder, you can find an executable called **mingwenv.cmd**, double-click on it to run.
Once open, make sure to run the **/.configure** command

![image](https://github.com/user-attachments/assets/dcd5c7b5-3553-4029-a54f-2cda19ee06b9)

Once the command has finished, make sure to run the **make clean && make** command
![image](https://github.com/user-attachments/assets/76d18704-da3f-4b8d-8d37-922adcb6760e)

## Setup additional settings
After all the commands have finished, navigate to **src/scave/resultitems.cc** inside your OMNeT folder. 
Inside **resultitems.cc** change the line 
>const SCAVE_API std::string NULLSTRING = "";

to

>extern const SCAVE_API std::string NULLSTRING = "";

## Configure the libcurl library to be able to run API calls
OMNeT++ comes with the libcurl library preinstalled, so other external uses of the library are not needed at all and might cause issues when running the simulations.

To make sure that OMNeT++ can access the libcurl library in your projects, make sure to add it to your **PATH environment variables**:

Press the **Windows** key and type env. The Environment Variables menu should pop up:

![image](https://github.com/user-attachments/assets/b16ffd3c-3ff9-4220-aeb1-3bc2b2391c7f)

Once open, go to **Environment Variables**:

![image](https://github.com/user-attachments/assets/cfde6563-aa7d-4cb1-a26d-04601f46062e)

Now click on **Path** and then on **Edit**

![image](https://github.com/user-attachments/assets/0c12a1de-42a6-465d-8d5c-ecd3df1ace50)

Click on **New** and type:
>{path_to_omnet}\tools\win32.x86_64\mingw64\bin\libcurl-4.dll

Where **path_to_omnet** is the path to your OMNeT++ folder.

# Run OMNeT++
Whenever you want to run OMNeT++, just type **omnetpp** in the mingwenv console

## VERY IMPORTANT
For each Task that requires the use of APIs, make sure to add **libcurl-4.dll** (from {path_to_omnet}\tools\win32.x86_64\mingw64\bin\libcurl-4.dll) to your project's folder.

# Basic API Task
For this task, we will solve the following objective: 
>Implement a module in OMNeT++ to fetch data from an online service (e.g.,weather data, or a sample JSON endpoint).

We will try to fetch the current weather on a given location using APIs from https://openweathermap.org/api.

For this, you need an account and the API key associated with that account.

The endpoint to fetch the current weather status from a given location is https://api.openweathermap.org/data/2.5/weather?q={location}&appid={api_key}, where {location} and {api_key} are set by your own choice. I recommend running the endpoint in the browser to see that you are able to fetch information about the weather.

Moving on to the project, add a **Source File**. The content of it can be taken from https://drive.google.com/file/d/1p5sOgzqUhlfJUjY_Vit8RV9Yr8EUNc1I/view?usp=sharing. Make sure to replace your_api_key with the actual key

Now add a **NED** file. The content of it can be taken from https://drive.google.com/file/d/1oHSCUlQMwKpGv0OrdgkX1T-IeM4HFNQ0/view?usp=sharing. This will create a recurrent network that sends and receive messages from itself.

Now add a **Initialization** file. The content of it can be taken from https://drive.google.com/file/d/1CxQgxrKarMKXnB9SQ3QJQkxN-8cMUzMY/view?usp=sharing. It uses Bucharest as the default location for the API call.

Now just run the simulation and if it has been configured properly, you should see the response content of the API call like this:

![image](https://github.com/user-attachments/assets/2b06f778-4e5e-4ffd-b48a-e489b3d374a7)

# Other API Tasks
A very important aspect when running other API tasks (which in essence are pretty similar to this one, make sure to disable the SSL verification before calling the API:

>
        curl_easy_setopt(curl, CURLOPT_SSL_VERIFYPEER, 0L);
        curl_easy_setopt(curl, CURLOPT_SSL_VERIFYHOST, 0L);

    

