## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/LexLuc/LexLuc.github.io/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/LexLuc/LexLuc.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

# Stardust Software Installation & Environment Configuration on Windows

## Preparation
1. Install Python 2.7
   1. Visit [Python Official Website](https://www.python.org/ftp/python/2.7.14/python-2.7.14.amd64.msi) to download 64-bit python2;
   2.  Run the .exe file you've just download by double clicking, and finish installation follow the instructions;
       (Make sure to tick "add to path" during installation)
   3. Check version after finishing by command: 
     
      \>`py -2 --version`
   4. To find out more about python2, see [Python2 Documents](https://docs.python.org/2/);
2. Install & Build Virtual Environment
   1. Open a cmd and type commands below:
     
      \>`cd C:\Users\xxx\` // xxx is your user name on Windows, same notation below.
      1. Installation
      
         \>```py -2 -m pip install virtualenv```
        
      2. Build Environment
        
         \>```mkdir Envs```
        
         \>`cd Envs`
        
         \>`py -2 -m virtualenv stardust`
      3. Activate Environment
      
         \>`C:\Users\xxx\Envs\stardust\Script\activate`
        
         Note: Make sure there is a **(stardust)** on the right side of the command prompt looks like 
         
         `(stardust) C:\Users\xxx\Envs\>`
         
         Then, if you type `where python`, the feedback should be `C:\Users\xxx\Envs\stardust\Scripts\python.exe` on the first line.
         
      4. Deactivate Environment
         
         \> `deactivate`
4. Install Pycharm
   1. Visit [Pycharm Official Website](https://www.jetbrains.com/pycharm/download/#section=windows) and download Pycharm Professional version.
   2. Install Pycharm follow the installer instruction;
   3. To activate Pycharm, ask your successors for help;
   4. To get started with Pycharm, see [Pycharm Documents](https://www.jetbrains.com/help/pycharm/first-steps.html).
3. Install GitHub
   1. Visit <https://git-scm.com/download/win> to download git with command prompt only; 
      Alternatively, you can choose GUI or portable version if you like;
   2. Run the .exe file you've just downloaded, and finish installation follow the instructions.
   3. Open git.cmd (windows command prompt) or git.bash (linux command prompt) as you like, and type command:
      
      \> `git --version`
      
      It should show you the version of the git that was downloaded just now;
      
      Then type:
      
      \> `git clone https://github.com/callzhang/stardust_server`
      
      to get our project files;
   5. For more usage about git, see [Git documents](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
4. Install Project Dependencies
   
   Open git.cmd as Administrator and go through instructions below:
   1. Install Aliyun API
      1. Activate Virtual Environment *stardust* if inactive
         (See 2.i.c);
      2. Go to the directory *\stardust_server\* and decompress the file *aliyun_api.tar.gz*;
      3. Install API according to the *readme.txt*.
   2. Install Shaply
      1. Visite website: <https://www.lfd.uci.edu/~gohlke/pythonlibs/#shapely> and download "Shapely‑1.6.3‑cp27‑cp27m‑win_amd64.whl" onto working directory
      2. Input command:
         
         \> `pip install Shapely‑1.6.3‑cp27‑cp27m‑win_amd64.whl`
         
   3. Install Other Dependences
      1. Change working directory to the directory *stardust_server* which contains project files;
      2. Edit file "requirements.txt" by deleting the line "shapely" as we've already installed that;
      3. Download & install Microsoft Visual C++ Compiler for Python 2.7 from [HERE](https://www.microsoft.com/en-us/download/details.aspx?id=44266);
      4. Download stdint.h from [HERE](https://github.com/mattn/gntp-send/blob/master/include/msinttypes/stdint.h) and save it to the path `C:\Users\xxx\AppData\Local\Programs\Common\Microsoft\Visual C++ for Python\9.0\VC\include`
      5. Run command:
         
         \> `pip install -r requirements.txt`
         
      6. After it finished, you can check which packet has been installed by typing:
         
         \> `pip list`
4. Install postgreSQL
   1. Visit [postgreSQL Offical Website](https://www.postgresql.org/download/windows/) to download postgreSQL (version >= 9.6.6), Here We take Graphical installer by BigSQL as an example;
   2. Run the installer and follow the installer prompt to finish installing as usual;
   3. To find more about postgreSQL, visit website [postgreSQL Documents](https://www.postgresql.org/docs/).
5. Install Redis for Windows
   1. Visit https://github.com/MicrosoftArchive/redis/releases to download MSI installer;
   2. Run the installer and finish the installation.

## Environment Configuration
1. Configure Local Settings
   1. Create a copy of file *settings_local.py.tmpl* and rename it as "settings_local.py";
   2. Edit the new file *settings_local.py*:
      * Modify the line `#DEBUG = False` as `DEBUG = True`;
      * Add a new line with `PYBOSSA_REDIS_CACHE_DISABLED = True`;
      * Save & close the file.
   3. Create a copy of file *alembic.ini.template* and rename it as *alembic.ini*;
2. Create User and Database for Pybossa
   1. Run postgreSQL service from "start" button;
      
      If the service was launched successfully, you can verify this by:
      *  Tap keyboard shortcut "Windows + R";
      *  Input "services" and run;
      *  Check out if the service postgreSQL does exist and is running.
   2. Open a cmd and run:
      
      \> `psql -U postgres -h localhost -p 5432`
      
      if there is a warning like:
      
      ```WARNING: Console code page (850) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
      ```
      
      then exit the psql and type:
      
      \> `cmd /c chcp 1252` // will silence the warning
      \> `psql -U postgres -h localhost -p 5432`
   3. Now you will see the prompt start with `postgre=#` on each new line, type:
      postgre=#`CREATE USER pybossa WITH PASSWORD 'tester';`
      postgre=#`CREATE DATABASE pybossa;`
      postgre=#`GRANT ALL PRIVILEGES ON DATABASE pybossa TO pybossa`;
      
3. Configure Database
   1. Open our project *stardust_server* on Pycharm;
   2. On menu bar, select *View->Tool Windows->Database*;
   3. On the right side, select *Database-> **+** ->Datasource->postgreSQL*
   4. Fill in the blanks:
      * Host:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`localhost`
      * Database:`pybossa`
      * User:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`pybossa`
      * Password:`tester`
      
      Then click `Test Connection`, it should say "Successful".
   5. See menu bar again and select *View->Tool Windows->Terminal*;
   6. In the terminal window, activate Virtual Environment *stardust* first, then run:
   
      \> `python cli.py db_create`
      
      It should show you that several SQL statements has been executed.
      (If there's a error message similar to "<packet_name\> not found", just install corresponding packet by:
      \> `pip install <packet_name>`)
4. Start Redis Service
   1. Open cmd as Administrator;
   2. Change working directory to *stardust_server*;
   3. Copy file sentinel.conf under *stardust_server\contrib* to where the Redis was installed; By default, just type
      ...\stardust_server>`cp contrib\sentinel.conf C:\Program Files\Redis`
   4. Then, change working directory to *Redis*; By default, type
      
      \> `cd C:\Program Files\Redis`
   5. Install sentinel as system service with config file:

      \> `redis-server --service-install --service-name Sentinel_1 sentinel.conf --sentinel`
   6. Run Sentinel_1 service:
      
      \> `redis-server --service-run --service-name Sentinel_1 sentinel.conf --sentinel`
   7. Start Redis server with config file:
   
      \> `redis-server redis.windows.conf`
      
      To verify whether Redis service is running, just check it on *service* (refer to method mentioned in session 2.1)
   *  If you'd like to remove the Sentinel_1 service, here's the command:
      
      \> `sc delete Sentinel_1`
   
