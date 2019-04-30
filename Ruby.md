### Installing Ruby
Mac install be default, has a ruby install. This is installed in a folder that is root/admin owned. Need to install a ruby manager to install a client/user copy

* Install ruby version manager `brew install rbenv`
* Install Ruby `rbenv install 2.3.0`
* Initialize the Ruby Version `rbenv init`
    * init may tell you to add something to .bash_profile
        * `sudo vi .bash_profile`
        * add the `eval "$(rbenv init -)"` command
        * save and exit (esc wq enter)
        * May have to restart console at this point
* Check the ruby manager to make sure proper version is running `rbenv local`
    * If needed, change the version 'rbenv local 2.3.0`
* Check ruby version `ruby -v`. double check you are pointing at the correct ruby. `which ruby` can be useful as well
* Check path to where ruby gems are stored `gem env home`. This can be useful if you are having permission issues installing gems. by using rbenv, the directory is most likely in the users home folder


### Rails Console Cheatsheet
Rails console allows you to interact with the ActiveRecord models

Start rails console
```
rails c
```

View contract of the model
```
Article
```

Retrieve First record
```
Article.first
```

Retrieve first 5 records
```
Article.first 5
```

Create a new Record
```
myArticle = Article.new
myArticle.title = 'Hello World'
myArticle.save!
```

Find by Index Key
```
Article.find 123
```

Find 
```
Article.where( title: 'Hello World' )
```

Run last command
```
_
```

### Gems

*awesome_print* - Allows you to call `ap <command>` to print the output in a formatted way.
example
```
Article.find 123
ap _
```


### Resources and Links

https://guides.rubyonrails.org/active_record_basics.html
