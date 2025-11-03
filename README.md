# iland_installation

### It is the instruction about how to install the iland on the Great Lakes

## 1. Install QT

Please visit the official Qt website at https://www.qt.io/download-dev to download Qt. Make sure to use your UMich email address to create a free account.

After registering, navigate to the Customer Portal, then go to the "All Downloads" page. From there, download the appropriate version: "Qt Online Installer for Linux x64."

<img width="498" alt="image" src="https://github.com/user-attachments/assets/e0f97685-da65-4aa9-9a73-61ae525c3a01" />

After downloading the Qt installer, transfer it to the Great Lakes cluster using your preferred method.


## 2. Intall QT on the Great Lakes

Next step is to install the qt online installer on the Great Lakes.

Firstly run the following code

```{cmd}
chmod +x qt-online-installer-linux-x64-4.8.1.run
./qt-online-installer-linux-x64-4.8.1.run
```

Then, turn on the basic Desktop of Great Lakes, click the installer. Then, sign in using the account that you created before.

![image](https://github.com/user-attachments/assets/bb91f3c9-ca15-4f52-8d45-d70df626f03e)

Choose the "Custom Installation"

![image](https://github.com/user-attachments/assets/3acc5012-71b3-4e88-b209-cbdf143c4ed8)

In the Select Components part, choose QT version 6.5.8, then wait the installation finished. (Please note that it may takes an hour to finish the installation).

<img width="814" alt="09271889dbe5ebac685f0216ea4999e" src="https://github.com/user-attachments/assets/51240202-5006-4fdd-a43c-2e37bfe0729b" />

run the following command.
```{cmd}
echo 'export PATH=$PATH:$HOME/Qt/6.5.8/gcc_64/bin' >> ~/.bashrc
source ~/.bashrc
```
Run the following code to check if the qt installation is successful.

```{cmd}
qmake --version
```

Run the above code to check if the qt installation is successful.

## 3. Get the  model source code.

Run the following code.

```{cmd}
git clone https://github.com/shengkai866/iland_installation.git
cd iland_installation
chmod +x iland_build.sh
chmod +x iland_source_code.sh
rm README.md
./iland_source_code.sh
cd ../src/ilandc
```
Then please go to the directory iland_model/src/ilandc and modify the ilandc.pro. Replace the line from 88-97 to the code in the following

![image](https://github.com/user-attachments/assets/7e96ed42-76c5-493e-a4ce-b33386531b34)
Originally Code

To 
```{cmd}
linux-g++ {
    # Use custom FreeImage from Spack installation
    INCLUDEPATH += /home/<UNIQUENAME>/.spack/opt/spack/gcc-13.2.0/freeimage/3.18.0-ko54/include
    LIBS += -L/home/<UNIQUENAME>/.spack/opt/spack/gcc-13.2.0/freeimage/3.18.0-ko54/lib
    LIBS += -lfreeimage
} else {
    # fallback: external freeimage (not used on linux)
    LIBS += -L$$THIRDPARTY_PATH/FreeImage -lFreeImage
}
```

#### replace "UNIQUENAME" to your uniquename.

![image](https://github.com/user-attachments/assets/1cc8c525-26c3-4a57-b622-2bb9165b70d9)


## 4. Build the model

This is the final step to build the model. Firstly, let us update the environment variable through runing the following code

```{cmd}
export LD_LIBRARY_PATH=/home/<UNIQUENAME>/.spack/opt/spack/gcc-13.2.0/freeimage/3.18.0-ko54/lib:$LD_LIBRARY_PATH
echo '/home/<UNIQUENAME>/.spack/opt/spack/gcc-13.2.0/freeimage/3.18.0-ko54/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

Remember to replace the “UNIQUENAME” to your uniquename, The following are example of the above code

```{cmd}
export LD_LIBRARY_PATH=/home/shengkai/.spack/opt/spack/gcc-13.2.0/freeimage/3.18.0-ko54/lib:$LD_LIBRARY_PATH
echo 'export LD_LIBRARY_PATH=/home/shengkai/.spack/opt/spack/gcc-13.2.0/freeimage/3.18.0-ko54/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

Finally, run the following code to finish the build-up (I assume that you are current in the directory that contains the iland_model, but not in it yet.

For example, you are in the directory "/home/name/iland_installation", the iland_istallation folder contains the iland_model

This process may takes more than half hour.

```{cmd}
./iland_build.sh
```

## 5. Run the Model

Every time you get into the Great Lakes and prepare to run the ILAND model, please run the following command first.

```{cmd}
module load gcc/13.2.0
module load spack
spack load freeimage
```
Then we can run the model, firstly go to the iland-model/build/ilandc folder, then run the following command

```{cmd}
./ilandc path/to/data/folder/example.xml years_simulation
```

##### Replace "path/to/data/folder/example/xml" to the folder that contains the data, also replace years_simulation to the number of the year you want to simulate. The following is an example of the command.

```{cmd}
./ilandc /home/shengkai/example_folder/project.xml
```

After you run the model, you can go to the data folder and find there is another folder called output. Inside the output folder, it is a sqlite database contains the output data.












