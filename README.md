# crypto-bot
Mounting a crypto bot in GCP for automated trades using GCP, Binance and Jupyter. Is a very simple code using scalping trading strategy during a sideways market (sideways drift), but at the same time, with a bullish attitude in the long term.

CONFIGURING GCP AND INSTALLING JUPYTER.

Step 1 : Create a free account in Google Cloud with 300$ credit


For this step, you will have to put your payment information and verify your account. It’s the most simple step. If you fail this step, close your laptop and think where you are going in life.

Step 2 : Create a new project
Click on the three dots shown in the image below and then click on the + sign to create a new project.

![1*SbaYPKE5_MmARsRlHU0qkg](https://user-images.githubusercontent.com/12050212/177019065-fe9dd8f1-19db-4761-abd6-03f6ba88d441.png)


Step 3 : Create a VM instance
Click on the three lines on the upper left corner, then on the compute option, click on ‘Compute Engine’

![1*5Z7XbsnxB0Cb3g6UEAVuKw](https://user-images.githubusercontent.com/12050212/177019081-5ff22c31-07fd-41fd-b908-975f380c47f7.png)

Now click on ‘Create new instance’. Name your instance, select zone as ‘ us-west1-b’. Choose your ‘machine type’. (I chose 8v CPUs).
Select your boot disk as ‘Ubuntu 16.04 LTS’. Under the firewall options tick both ‘http’ and ‘https’ (very important). Then, choose the disk tab and untick ‘ Delete boot disk when instance is deleted’.

![1*fg1j6JOFABOxflvcfXQ7Cw](https://user-images.githubusercontent.com/12050212/177019110-90618697-c393-432a-87c1-cbcff00f14a0.png)


If you click on ‘customize’, you will be able to find options for using GPUs. You can choose between 2 NVIDIA GPUs.

![1*LMBZwYTq9Ivk6fW4a2bqOw](https://user-images.githubusercontent.com/12050212/177019116-625ca1a9-466d-45dc-9dfb-10739f7b6461.png)


Some firewall settings:-

![1*gQURWhpn4s3bHDwUfErztg](https://user-images.githubusercontent.com/12050212/177019129-1082d3a6-1ad3-4893-b843-af2a39f10833.png)

Now click on ‘Create’ and your instance is ready!

![1*fxFUT_XW-xRvTLB6uPl07g](https://user-images.githubusercontent.com/12050212/177019134-3dca9f12-42b9-4f17-8f92-d09fc0f2a87d.png)

Your new VM instance should look something like this. Note down the External IP.
IMPORTANT : DON’T FORGET TO STOP YOUR GPU INSTANCE AFTER YOU ARE DONE BY CLICKING ON THE THREE DOTS ON THE IMAGE ABOVE AND SELECTING STOP. OTHERWISE GCP WILL KEEP CHARGING YOU ON AN HOURLY BASIS.

![1*eb1aMVZCc085W1qi7gtpLg](https://user-images.githubusercontent.com/12050212/177019142-24a55363-f123-4310-8126-ece18bb2e7d9.png)

Step 4: Make external IP address as static
By default, the external IP address is dynamic and we need to make it static to make our life easier. Click on the three horizontal lines on top left and then under networking, click on VPC network and then External IP addresses.

![1*5K6zNphpR1cUtxIryfaxkw](https://user-images.githubusercontent.com/12050212/177019146-28400e70-7d18-48ad-a0e2-6d828af29604.png)

Change the type from Ephemeral to Static.

![1*pwRPSLksp-pm1m4wyWWpVw-2](https://user-images.githubusercontent.com/12050212/177019159-7b03efdd-cb24-4d4f-83da-9df4c911ef69.png)

Step 5: Change the Firewall setting
Now, click on the ‘Firewall rules’ setting under Networking.

![1*5K6zNphpR1cUtxIryfaxkw-2](https://user-images.githubusercontent.com/12050212/177019166-d7c7543e-48d7-4702-b61a-08fd53e2bdbd.png)

Click on ‘Create Firewall Rules’ and refer the below image:

![1*R3jRo09kec4ygt1fUcZ_uA](https://user-images.githubusercontent.com/12050212/177019175-dce8dced-9ab8-4e46-b07b-c906d47e2e79.png)

Under protocols and ports you can choose any port. I have chosen tcp:5000 as my port number. Now click on the save button.

Step 6: Start your VM instance
Now start your VM instance. When you see the green tick click on SSH. This will open a command window and now you are inside the VM.

![1*J1iV3ZeGc_SgfuRQHzWBnQ](https://user-images.githubusercontent.com/12050212/177019181-d9e8649b-7f61-4bdb-9939-7d648ed70928.png)

Step 7 : Install Jupyter notebook and other packages
In your SSH terminal, enter:
wget http://repo.continuum.io/archive/Anaconda3-4.0.0-Linux-x86_64.sh
bash Anaconda3-4.0.0-Linux-x86_64.sh
and follow the on-screen instructions. The defaults usually work fine, but answer yes to the last question about prepending the install location to PATH:
Do you wish the installer to prepend the 
Anaconda3 install location to PATH 
in your /home/haroldsoh/.bashrc ? 
[yes|no][no] >>> yes
To make use of Anaconda right away, source your bashrc:
source ~/.bashrc
Now, install other softwares :
pip install tensorflow
pip install keras

Step 8: Set up the VM server
Open up a SSH session to your VM. Check if you have a Jupyter configuration file:
ls ~/.jupyter/jupyter_notebook_config.py
If it doesn’t exist, create one:
jupyter notebook --generate-config
We’re going to add a few lines to your Jupyter configuration file; the file is plain text so, you can do this via your favorite editor (e.g., vim, emacs). Make sure you replace the port number with the one you allowed firewall access to in step 5.
c = get_config()
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.port = <Port Number>
It should look something like this :
  
![1*SwFnrGUO0gWSdO6z8oly_A](https://user-images.githubusercontent.com/12050212/177019194-d01f5be7-5fbc-4e7d-a0ed-b5d4432cbb78.png)

Step 9: Launching Jupyter Notebook
  To run the jupyter notebook, just type the following command in the ssh window you are in :
jupyter-notebook --no-browser --port=<PORT-NUMBER>
Once you run the command, it should show something like this:
  
![1*dEi_LCzhpsRy7cDRppVE-A](https://user-images.githubusercontent.com/12050212/177019201-a737ac06-907b-423a-83e2-a0491a4bc02f.png)

Now to launch your jupyter notebook, just type the following in your browser:
http://<External Static IP Address>:<Port Number>
where, external ip address is the ip address which we made static and port number is the one which we allowed firewall access to.
  
![1*7ELRH-iVecVLtFo66jduxQ](https://user-images.githubusercontent.com/12050212/177019213-1d6414d2-7fee-4443-912f-55d28e3e7a39.png)

Congratulations! You successfully installed jupyter notebook on GCP!
  
BINANCE SETUP.

Step 1: Go to API Management.
  
Step 2: Create new API key

Step 3: Copy API key and API secret

Step 4: Enable Reading and Enable Spot & Margin Trading
  
![Screen Shot 2022-07-03 at 2 04 05 AM](https://user-images.githubusercontent.com/12050212/177027342-4b07e20c-a525-4ac8-8234-939bdf61b066.png)

ON TERMINAL AND JUPYTER.
  
Step 1: On terminal do: pip install python-binance

Step 2: Install pandas: pip install pandas

Step 3: On Jupyter, create a new python file and call it cryptoBot

Step 4: Add the code in the new python file (I added it in a separate row each line of code, but you can add it all in the same row)

  ![Screen Shot 2022-07-03 at 2 41 26 AM](https://user-images.githubusercontent.com/12050212/177028353-0ec8419f-dfd3-4bd1-b4c4-922a83b6ddb5.png)
  
Step 5: On Jupyter, create another python file called Binance_keys
 
Step 6: In Binance_keys, paste the API key and API secret previously copied
 
![Screen Shot 2022-07-03 at 2 46 58 AM](https://user-images.githubusercontent.com/12050212/177028461-8be14e68-e11b-4e9e-925c-46217e870f43.png)

Step 7: Return to cryptoBot file, and finish the code.
  
![Screen Shot 2022-07-03 at 3 09 04 AM](https://user-images.githubusercontent.com/12050212/177029158-9df5c2b5-5548-41dc-90d3-5208bcf6a64b.png)

On Jupyter you execute each line by clicking on Run button. And the order execution for each line, will be according to the numeration.
  
![Screen Shot 2022-07-03 at 3 15 41 AM](https://user-images.githubusercontent.com/12050212/177029355-138fa178-a049-485f-a570-87437ae446a3.png)

