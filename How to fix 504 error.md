# How to fix 504 error on flutter deployment

go to the deployment folder (typically httpdocs) and add a file called .htaccess with the following content


`RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ /index.html?path=$1 [NC,L,QSA]`
