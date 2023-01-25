# Installing_Hadoop_3.3.4_Win10-11NoWSL


### Before you begin...
- I strongly suggest you to remove your previous install of Hadoop from your computer (assuming you ran into errors) for a clean-install experience.

- Make sure you have downloaded the following files:
  - JDK 1.8_8u361 or better - [Direct link](https://drive.google.com/file/d/1MG3shs65Zpb-ZR_11GUM3WD7VSoGENfQ/view?usp=share_link)
  - WinRAR _(Ignore if already installed)_ - [Direct Link](https://www.win-rar.com/fileadmin/winrar-versions/winrar/winrar-x64-620.exe)
  - Hadoop 3.3.4.tar.gz - [Direct link](https://dlcdn.apache.org/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz)
  - bin.zip (For winutils config) - [Direct link](https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/raw/main/Resources/bin.zip)

---
### Procedure:
- Run the `jdk-8u361-windows-x64.exe` from the downloaded location and install it.

- Extract `hadoop-3.3.4.tar.gz` to `C:\` _(Do not extract it to any other folder as it will bring issues later)_

- Type `"Edit the system environment variables"` in the Start Menu. _[Not "Edit environment variables for your account"]_
  - Windows 11 Preview - <br /> <p align="left"><img src="https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/blob/main/Screenshots/EnvVarWin11.png?raw=true" width=50% height=50%></p>
  - Windows 10 Preview - <br /> <p align="left"><img style="float: right;" src="https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/blob/main/Screenshots/EnvVarWin10.png?raw=true" width=50% height=50%></p>

- Under both `User variables for %YOUR_USERNAME%` **and** `System variables`, click on `New...`
  - Preview: <br /> <p align="left"><img src="https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/blob/main/Screenshots/SysEnvVarBefore.png?raw=true" width=65% height=65%></p>

- Now create two environment variables with the following values in the before mentioned places:
  - Variable 1:
    - Variable name		: `JAVA_HOME`
    - Variable value	: `C:\Program Files\Java\jdk1.8.0_361`
  - Variable 2:
    - Variable name		: `HADOOP_HOME`
    - Variable value	: `C:\hadoop-3.3.4`

- It should look like this after completion:
  - Preview: <br /> <p align="left"><img src="https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/blob/main/Screenshots/SysEnvVarAfter.png?raw=true" width=65% height=65%></p>

- Now, under both the variable lists, open `Path`, add the following and click on `OK`:
  -  `C:\Program Files\Java\jdk1.8.0_361\bin`
  -  `C:\hadoop-3.3.4\bin`
  -  `C:\hadoop-3.3.4\sbin`
- It should look like this:
  - Preview: <br /> <p align="left"><img src="https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/blob/main/Screenshots/EnvVarWindow.png?raw=true" width=65% height=65%></p>

- Click on `OK` again to close the environment variable window.

- Now, extract `bin.zip` then copy the extracted `bin` folder and paste it to `C:\hadoop-3.3.4`. Click on `✔️ Replace the files in the destination` when prompted.

- Create a new folder in the following locations:
  - `data` in `C:\hadoop-3.3.4`
  - `namenode` in `C:\hadoop-3.3.4\data`
  - `datanode` in `C:\hadoop-3.3.4\data`

- Now, edit file `C:\hadoop-3.3.4\etc\hadoop\core-site.xml` and paste the following code:
```
<configuration>

   <property>
       <name>fs.defaultFS</name>
       <value>hdfs://localhost:9000</value>
   </property>

</configuration>
```

- Then, edit file `C:\hadoop-3.3.4\etc\hadoop\mapred-site.xml` and paste the following code:
```
<configuration>

    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
 
 </configuration>
```

- Then, edit file `C:\hadoop-3.3.4\etc\hadoop\hdfs-site.xml` and paste the following code:
```
<configuration>

    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
 
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/hadoop-3.3.4/data/namenode</value>
    </property>
 
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/hadoop-3.3.4/data/datanode</value>
    </property>
 
 </configuration>
```

- Then, edit file `C:\hadoop-3.3.4\etc\hadoop\yarn-site.xml` and paste the following code:
```
<configuration>

   <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
   </property>

   <property>
        <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name> 
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
   </property>

</configuration>
```

- Finally, edit file `C:\hadoop-3.3.4\etc\hadoop\hadoop-env.cmd`
  - Find `set JAVA_HOME=%JAVA_HOME%`
  - Replace it with `set JAVA_HOME=C:\Progra~1\Java\jdk1.8.0_361`
  - Save and close the editor

---
### Verification:

- Open CMD (It is always a better measure to 'Run as administrator')
  - Preview: <br /> <p align="left"><img src="https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/blob/main/Screenshots/CMDinStartSearch.png?raw=true" width=70% height=70%></p>
- Run the command `hdfs` it should output like the one below:
  - Preview: <br /> <p align="left"><img src="https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/blob/main/Screenshots/JAVA_HOME_Verification.png?raw=true" width=75% height=75%></p>
  - If there is an `Error: JAVA_HOME is incorrectly set` message just after you run the command, you might have INCORRECTLY set the `Environment variable` or `Path` or `hadoop-env.cmd` steps. Go back and verify.

- Format namenode:
  - Run the command `hdfs namenode -format`
  - The output should be like in the following - [Link](https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/blob/ef9c774df6c2de6bf4aaf9dfb71779fdda176a2d/CMD%20Outputs/namenodeformatsuccess.txt)

- Run the cluster
  - Execute the command `start-all.cmd` in the command prompt (CMD)
  - You should now get the following command prompt windows running:
    - Apache Hadoop Distribution - hadoop namenode
    - Apache Hadoop Distribution - hadoop datanode
    - Apache Hadoop Distribution - yarn resourcemanager
    - Apache Hadoop Distribution - yam nodemanager
  - You may get the following window during first-time use:
    - Preview (Reference): <br /> <p align="left"><img src="https://venzi.files.wordpress.com/2019/08/oracle-database-18c-xe-windows-firewall.png" width=70% height=70%></p>
    - Tick both `Private networks...` and `Public networks...` and click on `Allow access`
  - Give it a few moments to initialize.
  - Preview: <br /> <p align="left"><img src="https://brain-mentors.com/wp-content/uploads/2020/11/546.png" width=70% height=70%></p>
  
 - Verify execution:
   - Execute the command `jps`
   - You should get the following output:
   - Preview: <br /> <p align="left"><img src="https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/blob/main/CMD%20Outputs/jpsExecution.png?raw=true" width=70% height=70%></p>
   - If there are any one of them missing, check the respective window of the missed application to check for errors. There should not be any `SHUTDOWN_MSG: Shutting down %application% at %SystemName%/%IP_Address%`

---
### Accessing the UI:

- If all the things done till now are verified, you may attempt to access the UI.
- Open your preferred browser and enter the following address:
  - For accessing ResourceManager web UI: http://localhost:8088
    - Preview: <br /> <p align="left"><img src="https://brain-mentors.com/wp-content/uploads/2020/11/sdfsd-1536x510.png" width=80% height=80%></p>
  - For accessing NameNode web UI: http://localhost:9870
    - Preview: <br /> <p align="left"><img src="https://brain-mentors.com/wp-content/uploads/2020/11/asasas.jpg" width=80% height=80%></p>
- You can stop all the applications by running the command `stop-all.cmd` in any Command Prompt window. 
