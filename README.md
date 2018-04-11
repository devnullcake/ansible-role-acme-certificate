ACME Certificate Generation with DNS Challenge Automation 
=========================================================
An opinionated role using ansible [letsencrypt](http://docs.ansible.com/ansible/latest/modules/letsencrypt_module.html) module and [crypto modules](http://docs.ansible.com/ansible/latest/modules/list_of_crypto_modules.html) to allow for easy account registration, certificate generation and certificate signing against a server implementing [ACME](https://github.com/ietf-wg-acme/acme).

This implementation also provides automation for DNS challenges against cloud provider hosted DNS zones. This support currently extends to the following providers (more to be added).

* Azure

This role has been tested to work against the [Let's Encrypt](https://letsencrypt.org/) v2 server.

Note that this role has been developed for convenience in a majority simple use case. And is not intended to be a catch-all role for certificate automation.

Known Limitations
-----------------
### Certificate Revocation
The letsencrypt module provided by ansible does not support certificate revocation. Once this is implemented or an alternative module used for ACME support, this will me made available in this role.

Requirements
------------

No additional requirements are enforced other than an an Ansbile version greater than 2.5. Development was done on Ansible 2.5+.

If any of private key, signing request or account key is not provided, the role will attempt to use the Ansible [crypto modules](http://docs.ansible.com/ansible/latest/modules/list_of_crypto_modules.html). This expects the execution environment to support OpenSSL and have any requirements related to it met.

Role Variables
--------------

A list of available configuration variables can be found in the [defaults/main.yml](defaults/main.yml) file. Only a limited subset of the variables need to specified, unless further control is required.

Dependencies
------------

No Dependencies required other than Ansible itself. However, depending on the cloud provider you are using, you will have to ensure all the dependencies required for it has been met and any configuration requirements done.

For provider specific dependencies, refer to the Ansible provided guides.
* [Microsoft Azure](http://docs.ansible.com/ansible/devel/scenario_guides/guide_azure.html)
* [Amazong Web Services](http://docs.ansible.com/ansible/devel/scenario_guides/guide_aws.html)
* [Google Cloud Platform](http://docs.ansible.com/ansible/devel/scenario_guides/guide_gce.html)

Example Playbook
----------------

    ---
    - name: prepare certificate for foo.bar.example.com
      hosts: localhost
      tasks:
        - include_role:
             name: devnullcake.acme-certificate
          vars:
            acme_destination: /tmp
            acme_csr_common_name: "foo.bar.example.com"
            dns_zone: example.com
            dns_cloud_provider: azure
            dns_cloud_provider_config:
              azure:
                resource_group: organisation
            acme_account_email: contact@example.com

Testing
-------
There are no automated tests defined for this role at the moment. Molecule tests will be added at a later date. Current `tox` execution validates source using [ansible-lint](https://github.com/willthames/ansible-lint).

### Tox
This project has [tox](http://tox.readthedocs.io/en/latest/) configured to run against a clean environment. This can simply be run using tox.

```sh
tox
```

License
-------

Apache License 2.0
