FROM kubeguide/guestbook-php-frontend
MAINTAINER guestbook-update,troylc <troylc@163.com>

# add new index.html to /var/www/html
ADD index.html /var/www/html/index.html

EXPOSE 80

CMD ["apache2-foreground"]