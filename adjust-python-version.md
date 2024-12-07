# Upgrade/downgrade Python to desired version on Ubuntu Linux

To check the version of Python installed on your system run
```
python3 --version
```

**Note: Replace `*` in `python3.*` to desired version (e.g: python3.8, python3.9, ...)**

## Updating/downgrading Python to the desired version

### Step 1: Check if Python3.* is available for install
```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
```

Check if Python 3.* is available by running
```
apt list | grep python3.*
```

### Step 2: Install Python 3.*

Now you can install Python 3.* by running
```
sudo apt install python3.*
```

### Step 3: Set Python 3.* as default

Update the default Python by adding versions to an alternatives by running the below
**Note: change the priority to the desired number (e.g: 1, 2 or 3)**
```
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.* priority
```

Choose the selection corresponding to Python3.* (if not selected by default).
```
sudo update-alternatives --config python3
```

Now run `python3 --version` again and you should see the latest Python as the output.

## Fix pip and disutils errors

### Fix Python3-apt
Running pip in terminal will not work, as the current pip is not compatible with Python3.* and python3-apt will be broken, that will generate an error like
```
Traceback (most recent call last):   
    File "/usr/lib/command-not-found", line 28, in <module>     
        from CommandNotFound import CommandNotFound   
    File "/usr/lib/python3/dist-packages/CommandNotFound/CommandNotFound.py", line 19, in <module>     
        from CommandNotFound.db.db import SqliteDatabase   
    File "/usr/lib/python3/dist-packages/CommandNotFound/db/db.py", line 5, in <module>     
        import apt_pkg ModuleNotFoundError: No module named 'apt_pkg'
```

To fix this first remove the current version of `python3-apt` by running
```
sudo apt remove --purge python3-apt
```

Then do some cleanup
```
sudo apt autoclean
```

Finally, reinstall `python3-apt` by running
```
sudo apt install python3-apt
```

### Install pip & distutils
Running `pip` will still throw an error `pip: command not found`. We need to install the latest version of pip compatible with Python3.*

Install `distutils` 
```
sudo apt install python3-distutils
```

Now you can install `pip` by running
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python3.* get-pip.py
```

Now you can run `pip` and you should see the output of `pip --version`

# Delete desired version of Python on Ubuntu Linux

To list all python versions in default locations
```
ls /usr/bin/python*
```

To remove just python3.* package
```
sudo apt-get remove python3.*
```

To remove dependent packages
```
sudo apt-get remove --auto-remove python3.*
```

To remove configuration and/or data files of python3.*
```
sudo apt-get purge python3.*
```

To remove both configuration and/or data files of python3.* and it's dependencies
```
sudo apt-get purge --auto-remove python3.*
```