To use Nginx instead of Apache, change the `drupalvm_webserver` variable inside your customized `config.yml`, from `apache` to `nginx`.

Because Nginx server directives behave a little differently than Apache's VirtualHosts, Drupal VM includes a custom Drupal-optimized Nginx server block configuration, and you can control all the servers ('virtual hosts') Nginx will run using the `nginx_hosts` configuration. A few simple examples are shown in `default.config.yml`, but you have some extra flexibility if you need it. See the `nginx-vhost.conf.j2` template for more information.

Also, see the examples included in the [`geerlingguy.nginx` Ansible role's README](https://github.com/geerlingguy/ansible-role-nginx#readme) for more info, as well as many other variables you can override to configure Nginx exactly how you like it.

_Note: if you're using php-fpm, you may want to reflect your use of nginx by setting `php_fpm_pool_user` and `php_fpm_pool_group` in your `config.yml`._

## Enable SSL Support with Nginx

To enable SSL support for you virtual hosts you first need a certificate file. See the same section under the [Apache documentation](webservers-apache.md#enable-ssl-support-with-apache) for how to generate a self-signed certficiate.

Modify your nginx host configuration by adding the following `extra_parameters` to the first entry in `nginx_hosts`:

```yaml
- server_name: "{{ drupal_domain }} www.{{ drupal_domain }}"
  root: "{{ drupal_core_path }}"
  is_php: true
  extra_parameters: |
        listen 443 ssl;
        ssl_certificate     /vagrant/example.crt;
        ssl_certificate_key /vagrant/example.key;
        ssl_protocols       TLSv1.1 TLSv1.2;
        ssl_ciphers         HIGH:!aNULL:!MD5;
```

### Using Ubuntu's snakeoil certificate

If you are using Ubuntu as your base OS and you want to get started quickly with a local development environment you can use the snakeoil certificate that is already generated.

```yaml
- server_name: "{{ drupal_domain }} www.{{ drupal_domain }}"
  root: "{{ drupal_core_path }}"
  is_php: true
  extra_parameters: |
        listen 443 ssl;
        ssl_certificate     /etc/ssl/certs/ssl-cert-snakeoil.pem;
        ssl_certificate_key /etc/ssl/private/ssl-cert-snakeoil.key;
        ssl_protocols       TLSv1.1 TLSv1.2;
        ssl_ciphers         HIGH:!aNULL:!MD5;
```
