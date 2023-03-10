### Before you begin

**Linux Users Only** These setup steps (from developer dependencies to Git setup) are automated via a one-line `wget`. See details in the [Linux](https://github.com/adobe/brackets-shell/wiki/Building-Brackets-Shell#wiki-linux) section below.

**Clone Path Warning** The grunt scripts used by Brackets [do not work with paths that have spaces](https://github.com/adobe/brackets/issues/7181).  To avoid build problems, don't clone the source into a path containing spaces (e.g. "/users/user/document/test folder/github/brackets-shell" or "c:\users\user\documents\test folder\github\bracket-shell").

### General Prerequisites

* [Node.js](http://nodejs.org/download/)
* Additional requirements listed below for [Mac](https://github.com/adobe/brackets-shell/wiki/Building-Brackets-Shell#wiki-mac) and [Windows](https://github.com/adobe/brackets-shell/wiki/Building-Brackets-Shell#wiki-windows)
* Clone both [brackets-shell](https://github.com/adobe/brackets-shell) and [brackets](https://github.com/adobe/brackets) repositories as siblings in the same directory. (Grunt tasks in both projects assume the folders are siblings).

#### Install Dependencies

From the `brackets-shell` directory, run the following set of commands to install package dependencies and [Grunt](http://gruntjs.com)

```
npm install -g grunt-cli
npm install
grunt setup
```

Running ``grunt --help`` lists all the available tasks. Those tasks are listed below:

```
          full-build  Alias for "git", "create-project", "build",              
                      "build-branch", "build-num", "build-sha", "stage",       
                      "package" tasks.                                         
           installer  Alias for "full-build", "build-installer" tasks. (requires additional setup steps)
               build  Build shell executable. Run 'grunt full-build' to update 
                      repositories, build the shell and package www files.     
           build-mac  Build mac shell                                          
           build-win  Build windows shell                                      
                 git  Pull specified repo branch from origin *                 
        build-branch  Write www repo branch to config property                 
                      build.build-branch                                       
           build-num  Compute www repo build number and set config property    
                      build.build-number                                       
           build-sha  Write www repo SHA to config property build.build-sha    
               stage  Stage release files                                      
           stage-mac  Stage mac executable files                               
           stage-win  Stage win executable files                               
             package  Package www files                                        
        write-config  Update version data in www config.json payload           
     build-installer  Build installer                                          
 build-installer-mac  Build mac installer (requires additional setup steps)
 build-installer-win  Build windows installer (requires additional setup steps)
          set-sprint  Update occurrences of sprint number for all native       
                      installers and binaries                                  
                 cef  Download and setup CEF                                   
           cef-clean  Removes CEF binaries and linked folders                  
        cef-download  Download CEF, see curl-dir config in Gruntfile.js        
         cef-extract  Extract CEF zip                                          
        cef-symlinks  Create symlinks for CEF                                  
                node  Download Node.js binaries and setup dependencies         
            node-win  Setup Node.js for Windows                                
            node-mac  Setup Node.js for Mac OSX and extract                    
          node-clean  Removes Node.js binaries                                 
      create-project  Create Xcode/VisualStudio project                        
               setup  Alias for "cef", "node", "create-project" tasks.         
              jshint  Validate files with JSHint. *                            
                copy  Copy files. *                                            
               clean  Clean files and folders. *                               
                curl  Download files from the internet via grunt. *            
            curl-dir  Download collections of files from the internet via      
                      grunt. *                                                 
             default  Alias for "setup", "build" tasks.
```

**Building the installer** - as opposed to just the locally-executable binary - requires _additional setup_ not covered here. See [[Brackets Installer]] for details.


## Mac

#### Prerequisites

* [Git command line tools](http://git-scm.com/downloads)
* Python (should be installed by default--enter `python --version` in a Terminal window to verify)
* Xcode 4 required to build the project
  * In newer versions of Xcode, you might also need to install the "Command Line Tools" in Xcode from Preferences > Downloads and then select Xcode in the "Command Line Tools" drop-down in Preferences > Locations.
  * Running `xcode-select --install` in a Terminal window will install the required "Command Line Tools" without starting XCode

#### Setup
Open a Terminal window at the `brackets-shell` directory and run `grunt`. This will download the CEF and Node.js binaries (if needed), create symlinks for the CEF and Node.js directories, create the XCode project, then run a command line build.

You will need to run ``grunt setup`` later if new source files are added or if brackets-shell updates to a newer CEF build.

#### Building in XCode
Open appshell.xcodeproj in XCode and build the "Brackets" target.

#### Building from the command line
Open a Terminal window at the `brackets-shell` directory and run `grunt build`. (You will still need XCode installed, however).

#### Running
The build output is located at `xcodebuild/Release/Brackets.app` (release build) or  `xcodebuild/Debug/Brackets.app` (debug build).

When you launch this app, you will be prompted to select `index.html` (the main file for the Brackets HTML/JS/CSS source code). Navigate to your local copy of the brackets repo and select `src/index.html`. To avoid having to do this every time you launch, go to the brackets repo and run the `tools/setup_for_hacking.sh` script. This will add a symlink pointing from the compiled Brackets.app to the index.html file. The parameter to setup_for_hacking is the full path to Brackets.app. 


## Windows

#### Prerequisites
* Visual Studio 2010 (preferred) or 2012 are required to build the project. The free Visual Studio C++ Express works fine.
    * Note that if you're using VS 2010 or VS Express, you might need to install [Visual Studio 2010 SP1](http://www.microsoft.com/en-us/download/details.aspx?id=23691) to avoid link errors.
* Windows Vista or later
* [GitBash](http://code.google.com/p/msysgit/downloads/list) (I used Git-1.8.0-preview20121022.exe)
* [Python 2.7](https://www.python.org/downloads/) (Use the Windows X86 MSI or Windows X86-64 MSI)

Add Python to your path. The default python 2.7 install directory is `C:\Python27`.

#### Verify Prerequisites
* Start GitBash with **Run as Administrator**
* Enter `python --version`. You should see "Python 2.7.3".
* Enter `echo $VS100COMNTOOLS`. You should see something like ""C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\Tools\"

#### Setup
Open a GitBash shell and navigate to the `brackets-shell` directory. Run `grunt`. This will download the CEF binary (if needed), create symlinks for the CEF directories, create the Visual Studio solution file, then run a command line build.

You will need to _re-run_ ``grunt setup`` later if new source files are added or if brackets-shell updates to a newer CEF build.

#### Building in Visual Studio
Open appshell.sln in Visual Studio. NOTE: If you are using Visual Studio Express, you may get warnings that say some of the projects couldn't be loaded. These can be ignored.
Build the "Brackets" target.

#### Building from the command line
Open a GitBash window at the `brackets-shell` directory and run `grunt build`. (You will still need Visual Studio installed, however).

#### Running
The build output is located at `Release\Brackets.exe` (release build) or `Debug\Brackets.exe` (debug build).

When you launch this executable, you will be prompted to select an `index.html` file (the main file for the Brackets HTML/JS/CSS source code). Navigate to your local copy of the brackets repo and select `src\index.html`. To avoid having to do this every time you launch, go to the brackets repo and run `tools\setup_for_hacking.bat`. This will add a symlink pointing from the output folder to the index.html file. The parameter to setup_for_hacking is the full path to the folder containing Brackets.exe.

#### Troubleshooting
If grunt fails you may need to reset  

> git reset #HEAD  
> git checkout .

Make sure to run gitbash as administrator. Otherwise the symlinks/junctions for CEF directories will be missing after `grunt setup`.


## Linux

Currently, the core Brackets team only supports Debian/Ubuntu as a development environment and for our binary packages. We would like to support more distributions in the future, but we've already seen other open source contributors take Brackets to Arch Linux. For more information on the current development status, visit the [Linux wiki page](https://github.com/adobe/brackets/wiki/Linux-Version).

#### Preview Build

In Sprint 28, we release a Preview build for Linux (Debian/Ubuntu only). We're very close to parity with Mac and Windows. See the [Linux wiki page](https://github.com/adobe/brackets/wiki/Linux-Version) for more details.

#### Prerequisites

There are 2 options for installing prerequisites: (1) a one-line `wget` to setup the Git repositories and install depenencies or (2) setup Git separately and install dependencies on your own.

```
# One-line setup (recommended)
wget -N https://gist.github.com/jasonsanjose/5514813/raw/setup.sh; bash setup.sh; rm setup.sh
```

```
# Manual setup

# install git and dev dependencies
sudo apt-get install --assume-yes git libnss3-1d libnspr4-0d gyp gtk+-2.0
 
# install node and grunt
sudo apt-get install --assume-yes python-software-properties python g++ make
sudo add-apt-repository ppa:chris-lea/node.js -y
sudo apt-get update --assume-yes
sudo apt-get install --assume-yes nodejs
sudo npm install -g grunt-cli
```

#### Setup
Open a Terminal window at the `brackets-shell` directory and run `grunt`. This will download the CEF and Node.js binaries (if needed), create symlinks for the CEF and Node.js directories, create the Makefile, then run a command line build.

You will need to run ``grunt setup`` later if new source files are added or if brackets-shell updates to a newer CEF build.

#### Enabling symbols for debugging
If you would like to debug the shell, debug symbols need to be generated. In order to do that, open `appshell.gyp` in Brackets and goto https://github.com/adobe/brackets-shell/blob/master/appshell.gyp#L367. At the end add `-g,` to enable debug symbols. You could use `gdb` to debug the shell.

#### Building from the command line
Open a Terminal window at the `brackets-shell` directory and run `grunt build` or simply `make`.

#### Running
The build output is located at `out/Release/Brackets` (release build) or  `out/Debug/Brackets` (debug build).

When you launch this app, you will be prompted to select `index.html` (the main file for the Brackets HTML/JS/CSS source code). Navigate to your local copy of the brackets repo and select `src/index.html`. To avoid having to do this every time you launch, go to the brackets repo and run the `tools/setup_for_hacking.sh` script. This will add a symlink pointing from the compiled app to the index.html file. The parameter to setup_for_hacking is the full path to the compiled app.