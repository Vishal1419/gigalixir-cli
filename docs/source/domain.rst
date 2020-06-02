Domains
~~~~~~~

.. _`custom domains`:

How to Set Up a Custom Domain
=============================

After your first deploy, you can see your app by visiting https://$APP_NAME.gigalixirapp.com/, but if you want, you can point your own domain such as www.example.com to your app. To do this, run the following command and follow the instructions.

.. code-block:: bash

    gigalixir domains:add www.example.com

If you have version 0.27.0 or later of the CLI, you'll be given instructions on what to do next. If not, run :bash:`gigalixir domains` and use the :bash:`cname` value to point your domain at.

This will do a few things. It registers your fully qualified domain name in the load balancer so that it knows to direct traffic to your containers. It also sets up SSL/TLS encryption for you. For more information on how SSL/TLS works, see :ref:`how-tls-works`.

If your DNS provider does not allow CNAME, which is common for naked/root domains, and you are using the gcp v2018-us-central1 region, the default, you can also use an A record. Use the IP address 35.226.132.161. For gcp europe-west1, use 130.211.67.69. For AWS, unfortunately, you have to use a CNAME so the only option is to change DNS providers. While we have no plans to change these ip addresses, we highly recommend you use CNAMEs if at all possible.

Note that if you want both the naked/root domain and a subdomain such as www, be sure to run `gigalixir domains:add` for each one.

If you need a wildcard domain, feel free to :ref:`contact us<help>` and we can help you get set up.

Note that with Phoenix, you may need to change your :elixir:`check_origin` setting in order for websockets to pass the origin check. See https://hexdocs.pm/phoenix/Phoenix.Endpoint.html#module-runtime-configuration

How to Set Up SSL/TLS
=====================

SSL/TLS certificates are set up for you automatically assuming your custom domain is set up properly.  Note that your application will continue to be served on http as well as https.  If you want to force your users to use https by redirecting any http requests, specify that in your `config/prod.exs`:

.. code-block:: elixir

    config :my_app, MyAppWeb.Endpoint,
       force_ssl: [rewrite_on: [:x_forwarded_proto]]

This configures your app to `check the x-forwarded-proto header`_ set by Gigalixir, and redirect to https, if appropriate.

For more information on how this works internally, see :ref:`how-tls-works`.

.. _`check the x-forwarded-proto header`: https://hexdocs.pm/plug/Plug.SSL.html#module-x-forwarded-proto