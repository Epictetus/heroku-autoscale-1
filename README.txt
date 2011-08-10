Heroku Autoscale

1. INSTALL ON CLEAN UBUNTU INSTANCE
  1.1 Place the autoscale script in desired location
  1.2 Install ruby, rubygems and heroku version 2.4
      $ sudo apt-get install ruby rubygems
      $ sudo gem install heroku -v 2.4
  1.3 Ensure heroku credentials file is configured in ~/.heroku/credentials
      Also, ensure that public/private key pair is setup for the heroku app.
  1.4 Edit autoscale_conf file according to your heroku application configuration
  1.5 Ensure that autoscale script is executable
      chmod +x autoscale


2. HOW TO USE
  2.1 Standalone: run `source autoscale_conf; ./autoscale <HEROKU_APP_NAME>`
      Examples:
      $ source autoscale_conf; ./autoscale viki-staging
      $ source autoscale_conf; ./autoscale viki-production
  2.2 Configure Crontab to run the script every 3 minutes
      $ crontab -e
      Add the following line:
      */3 * * * * `. /autoscale_root_folder/autoscale_conf; /autoscale_root_folder/autoscale app_name >> some_log`
      Example:
      */3 * * * * `. /home/username/heroku_autoscale/autoscale_conf; /home/username/heroku_autoscale/autoscale my_heroku_app >> /home/username/heroku_autoscale/autoscale.log`
       (Note: Backticks are used so that Crontab can run commands that contain white spaces.)


3. CONFIGURATIONS
  Explanation of variables that are customizable within the script
  **The current defaults are optimized already, changed only if MUST**
  
  3.1 interval_minutes - Script will be run every "interval_minutes" minutes
  3.2 max_ratio - Value from 0.00 to 1.00 that determines the maximum allowable (dynos used / dynos set)
  3.3 min_ratio - Value from 0.00 to 1.00 that determines the minimum allowable (dynos used / dynos set)
  3.4 min_dynos - Minimum number of dynos the script will ever set regardless of any settings
  3.5 pessimism - Value from 0.00 to 1.00 to set how much buffer will be given to the number of dynos set so that the actual dyno load will not hit the dynos set. 

4. LICENSE
  This Code is released under the MIT license.

5. AUTHORS
  - Adrian Cheng adrian@viki.com
  - Albert Callarisa Roca albert@viki.com
  - Nia Mutiara nia@viki.com

Explanation for PESSIMISM variable

NewRelic provides capacity percentages based on (dynos+workers). 
The proportion of dynos:workers determines the responsiveness of dyno setting according to dyno load. 
Guide: Higher (num dynos divide by num worker) ==> Higher Pessimism (subjective judgement)
