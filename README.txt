Heroku Autoscale

1. INSTALL ON CLEAN UBUNTU INSTANCE
	1.1 Place the autoscale script in desired location
	1.2 Install ruby, rubygems and heroku version 2.4
 	    $ sudo apt-get install ruby rubygems
	    $ sudo gem install heroku -v 2.4
	1.3 Export credentials for email notifications. The emails can only be sent for the same domain.
 	    $ export AUTOSCALE_EMAIL_DOMAIN="domain.com"
 	    $ export AUTOSCALE_SENDER_EMAIL="email"
 	    $ export AUTOSCALE_SENDER_PASS="somepass"
 	    $ export AUTOSCALE_EMAIL_RECIPIENTS="me,you,we"
	1.4 Set your heroku app id and newrelic api key in the script
 	    look for the line that starts with heroku_app_id and replace 0 with your app id
 	    look for the line that starts with newrelic_api_key and replace 'xxxx' with your api key


2. HOW TO USE
	2.1 run `./autoscale [APP_NAME]`
	    Examples:
 	    $ ./autoscale viki-staging
 	    $ ./autoscale viki-production
	2.2 Configure Crontab to run the script every 3 minutes
 	    $ crontab -e
 	    */3 * * * * `/autoscale_root_folder/autoscale app_name >> some_log`
 	    (Note: Backticks are used so that Crontab can run commands that contain white spaces.)


3. CONFIGURATIONS
	Explanation of variables that are customizable within the script
	**The current defaults are optimized already, changed only if MUST**
  
	3.1 interval_minutes - Script will be run every "interval_minutes" minutes
  3.2 max_ratio - Value from 0.00 to 1.00 that determines the maximum allowable (dynos used / dynos set)
  3.3 min_ratio - Value from 0.00 to 1.00 that determines the minimum allowable (dynos used / dynos set)
  3.4 min_dynos - Minimum number of dynos the script will ever set regardless of any settings
  3.5 pessimism - Value from 0.00 to 1.00 to set how much buffer will be given to the number of dynos set so that the actual dyno load will not hit the dynos set. 



Explanation for PESSIMISM variable

NewRelic provides capacity percentages based on (dynos+workers). 
The proportion of dynos:workers determines the responsiveness of dyno setting according to dyno load. 
Guide: Higher (num dynos divide by num worker) ==> Higher Pessimism (subjective judgement)
