To install sendgrid email API & ActionMailer:

If heroku already has cc verification, run 'heroku addons:create sendgrid:starter'

    -this allows for up to 400 emails a day for free
    
At the heroku dashboard, under the sites resources, click on sendgrid.  This takes
you to the sendgrid account where you may need to verify your email.  

Go to settings/credentials/add new credential and create a new username and password
for *this email account and click on the 'mail' checkbox.

    -bbom
    -K********
    
submit    

Back in the CLI set the credentials by adding:

    heroku config:set SENDGRID_USERNAME=bbom
    
    and
    
    heroku config:set SENDGRID_PASSWORD=K********
    
Then open ~/.profile (linux; .bashrc on mac) to set the env variables:

    export SENDGRID_USERNAME=bbom
    export SENDGRID_PASSWORD=Kaisdad07

These variables are set outside of the workspace, in the root, and therfor are
inaccessible to users but available throuout the application.

Open config/environment.rb to use these variables to configure ActionMailer:

    ActionMailer::Base.smtp_settings = {
        :address => 'smtp.sendgrid.net', 
        :port => '587',
        :authentication => :plain,
        :user_name => ENV['SENDGRID_USERNAME'],    #this is our set ENV variable
        :password => ENV['SENDGRID_PASSWORD'],     #this is our set ENV variable
        :domain => 'heroku.com',
        :enable_starttls_auto => true
    }
    
This setup is in heroku docs.

Configure config/environments/development.rb with:

    config.action_mailer.delivery_method = :test
    config.action_mailer.default_url_options = { :host => 'https://photobom-b-random-sb.c9users.io' }
    
to set up test emails.

Do the same thing for config/environments/production.rb, but with working data:

    config.action_mailer.delivery_options = :smtp
    config.action_mailer.default_url_options = { :host => 'photobom.herokuapp.com', :protocol => 'https' }