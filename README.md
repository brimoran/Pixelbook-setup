# Pixelbook Linux set up

To see what we are dealing with here:

```cat /etc/debian_version```

It's Debian Stretch 9.9.

And what is already installed:

```apt list --installed```

## Install R

The version in the repo is too old so:

Install the dependencies necessary to add a new repository over HTTPS:

```sudo apt install dirmngr apt-transport-https ca-certificates software-properties-common gnupg2```

Enable the CRAN repository and add the CRAN GPG key to your system by running the following commands:

```sudo apt-key adv --keyserver keys.gnupg.net --recv-key 'E19F5F87128899B192B1A2C2AD5F960A256A04AF'```

```sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/debian stretch-cran35/'```

```sudo apt update```

Then install stuff required by R and the packages we will install, followed by r-base

```sudo apt-get install curl libcurl4-openssl-dev libxml2-dev libssl-dev libgdal-dev libproj-dev xorg libx11-dev libglu1-mesa-dev libcairo2-dev libudunits2-dev libv8-dev libprotobuf-dev libjq-dev protobuf-compiler phantomjs r-base```

Check the installed version if you wish:

```R --version```

## Install useful R packages

Enter R as a superuser in interactive mode:

```sudo R```

Let's install the packages we need in one command:

```install.packages(c("ggplot2","tidyverse","knitr","ggthemes","scales","ggmap","plotly","ggfortify","leaflet","leaflet.extras","rgdal","forecast","treemapify","dbscan","survival","googleVis","rmarkdown","flexdashboard","highcharter","devtools","maptools","mapview","treemap","networkD3","visNetwork","DiagrammeR","DT","ggcorrplot"))```

This will take a while.

```q()``` to exit R

## Install LaTeX

We have the space so a full install:
```
sudo apt-get install texlive-full
```

## Install R Studio

Download the deb:

https://www.rstudio.com/products/rstudio/download/#download

```sudo dpkg -i $DEB```

where $DEB is the path to the downloaded deb.

This failed at this point for me so I restarted the terminal and:

```sudo apt --fix-broken install```

```sudo apt-get install python3-qtpy```

then

```sudo dpkg -i $DEB```
 
 again which worked fine.

## Additional goodies

### Pandoc

Download deb file: https://github.com/jgm/pandoc/releases/tag/2.7.3

Copy into Linux

```sudo dpkg -i $DEB```

where $DEB is the path to the downloaded deb.

### Poppler

```sudo apt-get install python-poppler```

```sudo apt-get install poppler-utils```

### Visual Code Studio

Sometimes it's nice to have an alternative to Vim... Sublime Text looks too ugly for some reason so for a light weight editor we'll use Visual Code Studio instead:

Enable the package repository by creating this file using Vim:

```sudo vim /etc/apt/sources.list.d/vscode.list```

And then add this into the file:

```deb [arch=amd64] http://packages.microsoft.com/repos/vscode stable main```

Save file and quit Vim.

Add the keys:

```
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
```
Update repository and install:

```
sudo apt-get update
sudo apt-get install code
```

### Zathura

It's a bit of a pain having to flip back and forth to a browser to view compiled pdfs.  I prefer to use Zathura from the terminal or from Vim, especially because it uses Vim key bindings.

```sudo apt-get install zathura```

### Feh

To launch an image viewer from the terminal:

```sudo apt-get install feh```

## 7z

To extract common compression format:

```sudo apt-get install p7zip-full```

## Fine tuning

### Git

Set up git config:

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

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

### Customising Vim

I like the hybrid material colour scheme so let's set that up and a few other things.

Let's install VimPlug:

```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Then create .vimrc file:

```cd```
```vim .vimrc```

**Assuming you are comfortable with Vim**, edit the file using Vim:

```vim .vimrc```

Add the following into the file:

```
" Load Plugins                                                                                                        
call plug#begin()                                                                                                     
                                                                                                                       
        Plug 'flazz/vim-colorschemes'                                                                                 
        Plug 'junegunn/goyo.vim'                                                                                      
        Plug 'junegunn/limelight.vim'                                                                                 
"       Plug 'patstockwell/vim-monokai-tasty'                                                                         

call plug#end()                                                                                                       

" Appearance                                                                                                          
set number                                                                                                            
syntax on                                                                                                             
set background=dark linebreak                                                                                         
colorscheme hybrid_material                                                                                           
"colorscheme vim-monokai-tasty                                                                                        
                                                                                                                       
" Set spelling. Turn off with :setnospell                                                                             
setlocal spell spelllang=en_gb                                                                                        
                                                                                                                       
" Also start and stop Limelight when Goyo starts or stops                                                             
autocmd! User GoyoEnter Limelight                                                                                     
autocmd! User GoyoLeave Limelight!                                                                                    
                                                                                                                       
" Automatically start Goyo for certain filetypes and remove wrap                                                      
au FileType markdown | Goyo | set linebreak                                                                           
au FileType tex | Goyo | set linebreak                                                                                             
```
And then from within Vim type:

```:source %``` and then
```:PlugInstall```

Let Vim do its stuff.

(Alternatively just copy your .vimrc file that you are happy with from elsewhere... remember it is a hidden file)

# Backup container

At the time of writing this is an experimental feature so to turn on type this address into the chrome search bar:

```chrome://flags/#crostini-backup```

Then head to the chrome settings and start the backup from within the Linux (Beta) options.

# Clone git repos

Show contents of public SSH key to copy to add to web git service:

```
cat ~/.ssh/id_rsa.pub
```
Add SSH key to webservice and then clone repo using SSH in the area you want it on your computer:

```
git clone git@gitlab.com:YOURGITUSERNAME/YOURREPO.git
```
