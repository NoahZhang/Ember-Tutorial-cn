Deploying to Heroku
==========

全世界的人应该看到我们的杰作,如果你是用Git和Heroku,那么你可以很快的部署你的应用.

首先添加Heroku希望你有的`rails_12factor`gem.Bundle并且commit.然后运行这些命令:

    heroku create <whatever-name-you-want>
    git push heroku master
    heroku run rake db:migrate db:populate
    heroku open
    
就是这样!
