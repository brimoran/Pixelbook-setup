# Pixelbook Linux set up

Provide 30 GB of disk space, install will take 15 GB.

To see what we are dealing with here:

```cat /etc/debian_version```

It's Debian Buster 11.4.

And what is already installed:

```apt list --installed```

## Git

Set up git config:

```git config --global user.name "John Doe"```

```git config --global user.email johndoe@example.com```

Confirm:

```
git config --list
```

Create SSH key:

```
ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist
```

If none exist:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Show contents of public SSH key to copy to add to web git service:

```
cat ~/.ssh/id_rsa.pub
```

Add SSH key to webservice and then clone repo using SSH in the area you want it on your computer (for example create a Version Controlled folder in your home directory):

e.g. with Gitlab

```
git clone git@gitlab.com:YOURGITUSERNAME/YOURREPO.git

```

Add public key to any servers you need to access:

```
ssh-copy-id -i ~/.ssh/id_rsa.pub YOURUSERNAMEE@SERVERIPADDRESS
```

## Vim

Let's install VimPlug:

```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Then copy .vimrc which I store in a git repository to home folder:

e.g. navigate to folder and then

```
cp .vimrc ~/
```

And then install the plug ins from within Vim by typing:

```
:PlugInstall
```
## Other dot files

Copy .bashrc from git repo.

e.g. navigate to folder and then

```
cp .bashrc ~/
```

## Install LaTeX

Using this method http://www.tug.org/texlive/acquire.html to ensure we have the latest version (didn't check the version in the repository but the version in stretch caused me some trouble with datatool:

Download the latest tar.gz:

http://www.tug.org/texlive/acquire-netinstall.html

Use ChromeOS files app to copy to the Linux container and back in the terminal unpack:

```tar -xzf install-tl-unx.tar.gz```

``cd`` into the unpacked folder and:

```sudo ./install-tl``` (sudo seems to be necessary on Pixelbook)

The install will take a long time.

When complete there will be a prompt to add to PATH, MANPATH and INFOPATH.  Test by temporarily adding to path:

```PATH=/usr/local/texlive/2019/bin/x86_64-linux:$PATH``` (check that this path is correct)

... and test that ```pdflatex --version``` works.  If it does then add the path to the .bashrc file:

```cd```

```vim .bashrc``` place the line at the end of the file along with:

```MANPATH=/usr/local/texlive/2019/texmf-dist/doc/man```                                                                               
```INFOPATH=/usr/local/texlive/2019/texmf-dist/doc/info```

Then save and quit Vim.

## Install R

```sudo apt-get update```

```sudo apt-get upgrade```

The version in the repo is too old so:

Install the dependencies necessary to add a new repository over HTTPS:

```sudo apt install dirmngr apt-transport-https ca-certificates software-properties-common gnupg2```

Enable the CRAN repository and add the CRAN GPG key to your system by running the following commands:

```apt-key adv --keyserver keyserver.ubuntu.com --recv-key '95C0FAF38DB3CCAD0C080A7BDC78B2DDEABC47B7'```

```sudo add-apt-repository 'deb http://cloud.r-project.org/bin/linux/debian bullseye-cran40/'```

```sudo apt update```

Then install R:

```sudo apt install r-base r-base-dev```

Check the installed version if you wish:

```R --version```

Then install stuff required the packages we will install.

```sudo apt-get install curl libcurl4-openssl-dev libxml2-dev libssl-dev libgdal-dev libproj-dev xorg libx11-dev libglu1-mesa-dev libcairo2-dev libudunits2-dev libv8-dev libprotobuf-dev libjq-dev protobuf-compiler cmake```


## Install useful R packages

Enter R as a superuser in interactive mode:

```sudo R```

Let's install the packages we need in one command:

```install.packages(c("ggplot2","tidyverse","knitr","ggthemes","scales","ggmap","plotly","ggfortify","leaflet","leaflet.extras","rgdal","forecast","treemapify","dbscan","survival","googleVis","rmarkdown","flexdashboard","highcharter","devtools","maptools","mapview","treemap","networkD3","visNetwork","DiagrammeR","DT","ggcorrplot","Hmisc","anomalize", "fpp2", "h2o", "sweep", "timetk", "xgboost", "prophet", "survminer","ggwordcloud", "ggsn", "formattable", "IMD", "car", "maps", "this.path", "triangle", "fuzzyjoin"))`````

This will take a while.

After it completes, to enable mapshot to work (untested but necessary on MacOS):

webshot::install_phantomjs()

And then for ggmap

register_google(key = "[your key]", write = TRUE) # get key from Google Cloud Console, APIs & Services > Credentials


```q()``` to exit R


## Install R Studio

Download the deb:

https://www.rstudio.com/products/rstudio/download/#download

```sudo dpkg -i DEB```

where DEB is the path to the downloaded deb.

This failed at this point for me so:

```sudo apt --fix-broken install```
 
 again which worked fine.

## Python stuff

### pip

``sudo apt install python3-pip``

### pandas

``sudo apt-get install python3-pandas``

### xlsx2csv

To convert xlsx files to csv easily:

```sudo apt-get install xlsx2csv```

## Additional goodies

### Pandoc

Download deb file: https://github.com/jgm/pandoc/releases

Copy into Linux

```sudo dpkg -i DEB```

where DEB is the path to the downloaded deb.

### Poppler

```sudo apt-get install poppler-utils```

### Ghostscript

```sudo apt-get install ghostscript```

### Sublime Text

Sometimes it's nice to have an alternative to Vim...

https://www.sublimetext.com/docs/3/linux_repositories.html

Install the GPG key:

```wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -```

Add stable channel:

```echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list```

Update sources and install:

```sudo apt-get update```

```sudo apt-get install sublime-text```

### Zathura

It's a bit of a pain having to flip back and forth to a browser to view compiled pdfs.  I prefer to use Zathura from the terminal or from Vim, especially because it uses Vim key bindings.

```sudo apt-get install zathura```

### Feh

To launch an image viewer from the terminal:

```sudo apt-get install feh```

## 7z

To extract common compression format:

```sudo apt-get install p7zip-full```

## Filezilla

For FTP:

```sudo apt-get install filezilla```

## MAME tools

To manage chd files:

```sudo apt install -y --no-install-recommends mame-tool```

## Mosh

for more stable connections to remote servers:

```sudo apt install -y mosh```

## OCRmyPDF

To OCR PDFs example: ocrmypdf -l eng input.pdf output.pdf

```sudo apt install -y ocrmypdf```

## Rename

For flexible tools to rename file in BASH:

```sudo apt install -y rename```

## Parallel

To use multiple course in bash scripts:

```sudo apt install -y parallel```

# Firefox

To scrape websites using R:

```sudo apt-get install firefox-esr```
