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
   1. Visit <https://www.python.org/ftp/python/2.7.14/python-2.7.14.amd64.msi> to download 64-bit python 2;
   2.  Run the .exe file you've just download by double clicking, and finish installation follow the instructions;
       (Make sure to tick "add to path" during installation)
   3. Check version after finishing by command: 
     
      \>`py -2 --version`
   4. To find out more about python 2, see <https://docs.python.org/2/>;
2. Install & build virtual environment
   1. Open cmd and type commands below:
     
      \>```cd C:\Users\xxx\``` // xxx is your user name on Windows
      1. Installation
      
         \>```py -2 -m pip install virtualenv```
        
      2. Build Environment
        
         \>```mkdir Envs```
        
         \>`cd Envs`
        
         \>`py -2 -m virtualenv stardust`
      3. Activate Environment
      
         \>`stardust\Script\activate`
        
         Note: Make sure there is a **(stardust)** on the right side of the command prompt looks like 
         
         `(stardust) C:\Users\xxx\Envs\>`
        
        
      4. Deactivate Environment
         
         \> `deactivate`
3. Install GitHub
   1. Visit <https://git-scm.com/download/win> to download git with command prompt only. Alternatively, you can choose GUI or portable version if you like;
   2. Run the .exe file you've just downloaded by double clicking, and finish installation follow the instructions.
   3. Open git.cmd (windows command prompt) or git.bash (linux command prompt) as you like;
   4. Enjoy :) (For more about git, see <https://git-scm.com/book/en/v2/Getting-Started-Installing-Git>)
+
