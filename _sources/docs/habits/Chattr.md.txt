# Chattr

Write protect configuration files.

After configuration is done, protect those configuration files, even from yourself. Nobody, including root, can update a file protected with chattr:

    # chattr +i filename

For example:

    # chattr +i /etc/php5/apache2/php.ini

To disable write protection:

    # chattr -i filename

