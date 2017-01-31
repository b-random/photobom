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
    config.action_mailer.default_url_options = { :host => 'https://localhost:3000' }

to set up test emails.  **test emails are not actually sent.  look in console for proof.

Do the same thing for config/environments/production.rb, but with working data:

    config.action_mailer.delivery_method = :smtp
    config.action_mailer.default_url_options = { :host => 'photobom.herokuapp.com', :protocol => 'https' }




**Now when a user signs up in development an email isnt actually sent- but if you look
in the server window you will see

    Sent mail to brandon.brigance@gmail.com (11.4ms)
    Date: Sat, 03 Dec 2016 05:44:23 +0000
    From: please-change-me-at-config-initializers-devise@example.com
    Reply-To: please-change-me-at-config-initializers-devise@example.com
    To: brandon.brigance@gmail.com
    Message-ID: <58425bb71c6ec_1f783fddf884516488630@b-random-sb-photobom-4014139.mail>
    Subject: Confirmation instructions
    Mime-Version: 1.0
    Content-Type: text/html;
     charset=UTF-8
    Content-Transfer-Encoding: 7bit

    <p>Welcome brandon.brigance@gmail.com!</p>

    <p>You can confirm your account email through the link below:</p>

    <p><a href="https://photobom-b-random-sb.c9users.io/users/confirmation?confirmation_token=vodHxPtzYBePxyHgN15e&amp;locale=en">Confirm my account</a></p>

    Redirected to https://photobom-b-random-sb.c9users.io/
    Completed 302 Found in 483ms (ActiveRecord: 8.9ms)

if we paste just the URL in the localhost address bar

    "https://photobom-b-random-sb.c9users.io/users/confirmation?confirmation_token=vodHxPtzYBePxyHgN15e&amp;locale=en

You will see a success message.
