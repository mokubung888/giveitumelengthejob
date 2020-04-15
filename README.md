FROM ubuntu:18.04

# Set noninteractive installation
ARG DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update && apt-get install apache2 php libapache2-mod-php -y   

#Create php info file and paste configuration
RUN touch cp ./phpinfo.php /var/www/html/ && echo '<?php phpinfo(); ?>' > /var/www/html/phpinfo.php

#Make phpinfo the default home page
RUN sed '/Indexes FollowSymLink/ a DirectoryIndex phpinfo.php' /etc/apache2/apache2.conf

# Configure apache
RUN echo '. /etc/apache2/envvars' > /root/run_apache.sh && \
 echo 'mkdir -p /var/run/apache2' >> /root/run_apache.sh && \
 echo 'mkdir -p /var/lock/apache2' >> /root/run_apache.sh && \
 echo '/usr/sbin/apache2 -D FOREGROUND' >> /root/run_apache.sh && \
 chmod 755 /root/run_apache.sh

EXPOSE 80

CMD /root/run_apache.sh
