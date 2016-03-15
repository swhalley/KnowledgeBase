See first the [ruby article](https://github.com/swhalley/KnowledgeBase/blob/master/Ruby_Install.md). Permission errors and other dependency problems may arise if a user copy of Ruby isn't installed. 

### Jekyll Install 
* If Ruby is installed ok, it should be as simple as running `gem install jekyll` from a command line

Most recent updates here https://jekyllrb.com/docs/quickstart/

Creating a new project or running from command line, see the Jekyll documentation

### Webstorm integration - Add Jekyll Launcher
* Preferences (command + ,)
* External Tools
* Add (+)
    * Give it any name (i.e. Jekyll Launcher) and description
    * For Program field use `jekyll`
    * For Parameters use `serve --watch` which auto updates the server with changes
    * For Working Directory there are a few choices
        * Add your projects path
        * Use a variable `insert macro` and choose ProjectFileDir, Projectpath, or another choice which points to your jekyll project
        
At this point, launch the application normall. use `run` `edit configuration` `new` pick a javascript runner/debug and in the URL field put in `http://127.0.0.1:4000` which is the URL that jekyll will serve from by default
