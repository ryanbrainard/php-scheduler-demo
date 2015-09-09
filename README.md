A simple demo of running a scheduled PHP process on Heroku.

1. Clone this repo

        $ git clone https://github.com/ryanbrainard/php-scheduler-demo.git

2. Go in the dir

        $ cd php-scheduler-demo

3. Create the Heroku app with the buildpack set to a fork of the PHP buildpack that support the PHP CLI

        $ heroku create --buildpack https://github.com/ryanbrainard/heroku-buildpack-php.git

4. Push the code to Heroku

        $ git push heroku master

5. Test the script by running it in a one-off worker dyno just to see it work before scheduling it

        $ heroku run php www/script.php
        Running `php www/script.php` attached to terminal... up, run.6599
        Current time: 1359943709 

**Note:** The file structure on Heroku seems to have changed recently. Your script might be located simply in the root directory so if the code above isn't working try 'heroku run php script.php' and you should be alright. 

6. Add the [Scheduler Add-on](https://devcenter.heroku.com/articles/scheduler)

        $ heroku addons:add scheduler:standard 

7. Open the scheduler configuration page

        $ heroku addons:open scheduler

8. Add the script as a job. Same as one-off step above, but without the `heroku run`
       - Add job...
       - `$` `bin/php www/script.php`

9. View the scheduled job running in the logs

        $ heroku logs
        ...
        2013-02-04T01:55:13+00:00 heroku[api]: Starting process with command `bin/php www/script.php` by scheduler@addons.heroku.com
        2013-02-04T01:55:16+00:00 heroku[scheduler.4997]: Starting process with command `bin/php www/script.php`
        2013-02-04T01:55:16+00:00 app[scheduler.4997]: Current time: 1359942916
        2013-02-04T01:55:18+00:00 heroku[scheduler.4997]: Process exited with status 0
        2013-02-04T01:55:18+00:00 heroku[scheduler.4997]: State changed from starting to complete
        ...
