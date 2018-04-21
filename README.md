## Kaggle-2018-Data-Science-Bowl

The notebook can be run directly on colab.

Final result:

* Stage1: LB = 0.428, Rank = 500
* Stage2: LB = 0.471, Rank = 275

Methods work on increasing LB:

* Original model from https://github.com/matterport/Mask_RCNN (LB = 0.324)
* Set IMAGE\_MIN\_DIM, IMAGE\_MIN\_DIM = 512 (LB = 0.372)
* Deal with overlap result, with rle method modified (LB = 0.396)
* Data augumentation: flip (LB = 0.410)
* Data augumentation: flip + blur + crop (LB = 0.428)

No improvement methods we've also tried:

* Increase the epochs to 70 (loss decreases a little, while LB doesn't change at all)
* Set IMAGE\_MIN\_DIM, IMAGE\_MIN\_DIM = 1024 (Error)
* Training dataset fix from https://github.com/lopuhin/kaggle-dsbowl-2018-dataset-fixes (No improvement on LB)
* Ensemble the model of U-Net and MASK-RCNN (the results get worse)
* Change training method, where heads = 60, all = 40 (get worse LB)

The following contentes is about A Brief Note of Using Jupyter on Remote Machine and Setting up a AWS Server for GPU Computing. It's cited from the github of [Xiaoyu Cai][6]. Thanks to his help.

## **Running a Jupyter notebook on Clear.**
### For Linux and Mac user

 * Open a new terminal window
 * Type in command: 
```
ssh yourNetId@ssh.clear.rice.edu
```
Then type in your password, which is the same password you use when you logging in Canvas.
 * Install Jupyter, if you have installed, skip to next step.
```
pip install --user jupyter
```
This command will install Jupyter under user's home directory. If you do not include **--user** flag, you will encounter permission denied error.
 * Start your Jupyter notebook
```
cd ~ 
cd .local/bin
jupyter notebook 
```

Perhaps you would like to designate a certain port for Jupyter, if so, use:

```
jupyter notebook --port = the port you want
```
Then you can see your Jupyter notebook is running, find something like 

>  http://localhost:8889/?token=fd4bb9b48f8c71bf2b4d6c94d273ac1db57aae5db1f8658f

And copy the port token, which is "8889" and "fd4bb9b48f8c71bf2b4d6c94d273ac1db57aae5db1f8658f", we will use it later.

* Do not shutdown the terminal you have opened and open a new terminal.
* Type in the command below:

```
ssh -N -f -L localhost:12345:localhost:[the port (4 number digit by default) you saw/chose in step 4] yourNetId@ssh.clear.rice.edu
```

For example:

```
ssh -N -f -L localhost:12345:localhost:8889 yourNetId@ssh.clear.rice.edu
```

The flag -L means that the local machine forward the local port to the remote port. For more information see [here][1] 

* Open your browser, type in **localhost:12345** in address bar.
* Copy the token you saved in step 4 into the text field on the top of the page.

### For Windows user
* Download Putty from [here][2]. After you get used to Putty, you may want to try XShell.
* Open Putty Click **"Session"** on the left side of the window. Type in **netId@ssh.clear.rice.edu** under **"Host Name"**
* Click **"ssh"** expand the tree then click **"tunnel"**.
* Click the checkbox of **"Local ports accept..."**
* In **"Source Port"** choose any port you like. For example: 12345
* In **"Destination"**, type in **localhost:[the port you would like choose]**, then click **"add"**. 
* Back to the **"Session"** tab, click **"Open"**.
* Type in your password
* Install Jupyter, if you have installed, skip to next step.
```
pip install --user jupyter
```
This command will install Jupyter under user's home directory. If you do not include **--user** flag, you will encounter permission denied error.

10. Start your Jupyter notebook
```
cd ~ 
cd .local/bin
jupyter notebook 
```

Perhaps you would like to designate a certain port for Jupyter, if so, use:

```
jupyter notebook --port = the port you chose in Putty
```
Then you can see your Jupyter notebook is running, find something like 

>  http://localhost:8889/?token=fd4bb9b48f8c71bf2b4d6c94d273ac1db57aae5db1f8658f

And copy the token, which is ""8889" and "fd4bb9b48f8c71bf2b4d6c94d273ac1db57aae5db1f8658f", we will use it later.

* Open your browser, type in **localhost:12345** in address bar.
* Copy the token you saved in step 10 into the text field on the top of the page.

## How to start a AWS EC2 GPU server machine
### Always remember to stop or terminate your AWS machine after using it
### Apply credit of AWS educate program
* Create an account on aws.amazon.com, find the account ID for step 2.
* Go to [here][3], click **Student** and fill in the form with your @rice.edu emaill address. If you use the link in Github student pack, you will get more than \$100 credit.
* If approved your will get \$100 credit for AWS. 

### Start a AWS EC2 GPU Server for Deep Learning
* You have to raise the limit before you create an instance. To raise the limit, go to [AWS support page][4], create a case. For **"Regarding"**, select **"Service Limit Increase"**. For **"Limit Type"**, select **"EC2 instance"**. For **"region"**, select **"Ohio"**. For **"Primary Instance Type"**, select **"p2.xlarge"**. Note that, in p2.xlarge, there is a Tesla K80 GPU, which is suffient for personal use. If you would like to use more than one GPU at the same time, you may want to use **p2.x8large**.
* Go back to the console, click **"Service"** then click **"EC2"**
* Click the blue button **"create instance"**.
* In the AMI webpage, select **"Deep Learning AMI"**.
* Choose **p2.xlarge** on the next page, then click **"Review and Launch"**
* Create or use exists key.
* After some minutes, your EC2 instance is ready. Go back to the console, click **"Service"** then click **"EC2"**, then click **"Instance"** on the left side. Select the instance you created, click **"connect"**, you can find the instruction of connecting the machine.

### How to running Jupyter on AWS EC2 (using private key)
The way we run Jupyter notebook on AWS EC2 is almost the same, however, we have to use a private key to log in EC2. 

#### For Linux/Mac
Whenever you use **ssh** command, remember add **-i "youkey.pem"**. For example:
```
ssh -i "yourkey.pem" username@remoteMachineAddress
```

#### For Windows
* Expand **"ssh"** on the left side of Putty window, and click **"Auth"**.
* Click **"Browse"** then select your key pair file.
* Then Click **"Open"**.

#### After logging in
Use 
```
conda env list
```
to see the virtual environment you have on the EC2 machine. Activate anyone you like and run your code.

## Some other notes
If we close a terminal that is connecting with a remote machine, the connection will be down and the task running in that terminal will be stopped. It sounds not good for us because sometimes we may need a lot of hours to run a program (it usual if you run some big deep learning models), we have to keep our computer open and connecting with WIFI all the time.

To prevent such things, you may want to use **Screen**, for more information, see the [man page][5] of it or just Google some tutorial about it.


  [1]: https://linux.die.net/man/1/ssh
  [2]: https://www.putty.org/
  [3]: https://www.awseducate.com/Registration
  [4]: https://console.aws.amazon.com/support/home
  [5]: https://www.gnu.org/software/screen/manual/screen.html
  [6]: https://github.com/AllenCX/Notes/blob/master/comp540_cloud_note.md
