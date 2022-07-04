[![CI](https://github.com/de-it-krachten/ansible-role-dovecot/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-dovecot/actions?query=workflow%3ACI)


# ansible-role-dovecot

Installs & configures Dovecot


## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- CentOS 7
- RockyLinux 8
- OracleLinux 8
- AlmaLinux 8
- AlmaLinux 9
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# delivery agent to use
dovecot_lmtp: true
dovecot_lda: false

# defaults settings for dovecot
dovecot_settings:
  protocols:
    path: "{{ dovecot_config }}"
    var: 'protocols'
    value: 'imap lmtp sieve'
  listen:
    path: "{{ dovecot_config }}"
    var: 'listen'
    value: '*'
  disable_plaintext_auth:
    path: "{{ dovecot_confd_dir }}/10-auth.conf"
    var: 'disable_plaintext_auth'
    value: 'no'
  auth_mechanisms:
    path: "{{ dovecot_confd_dir }}/10-auth.conf"
    var: 'auth_mechanisms'
    value: 'plain login'
  mail_location:
    path: "{{ dovecot_confd_dir }}/10-mail.conf"
    var: 'mail_location'
    value: 'maildir:~/Maildir'
  ssl:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl'
    value: 'required'
  ssl_cert:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_cert'
    value: '<{{ dovecot_ssl_chain }}'
  ssl_key:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_key'
    value: '<{{ dovecot_ssl_key }}'
  ssl_protocols:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_protocols'
    state: absent
  ssl_min_protocol:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_min_protocol'
    value: 'TLSv1.2'
  ssl_cipher_list:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_cipher_list'
    value:
      - 'EDH+CAMELLIA'
      - 'EDH+aRSA'
      - 'EECDH+aRSA+AESGCM'
      - 'EECDH+aRSA+SHA384'
      - 'EECDH+aRSA+SHA256'
      - 'EECDH'
      - '+CAMELLIA256'
      - '+AES256'
      - '+CAMELLIA128'
      - '+AES128'
      - '!aNULL'
      - '!eNULL'
      - '!SSLv2'
      - '!LOW'
      - '!DES'
      - '!3DES'
      - '!IDEA'
      - '!MD5'
      - '!EXP'
      - '!PSK'
      - '!DSS'
      - '!RC4'
      - '!SEED'
      - '!ECDSA'
      - 'CAMELLIA256-SHA'
      - 'AES256-SHA'
      - 'CAMELLIA128-SHA'
      - 'AES128-SHA'
  postmaster_address:
    path: "{{ dovecot_confd_dir }}/15-lda.conf"
    var: 'postmaster_address'
    value: 'postmaster@{{ dovecot_domain }}'
  lda_mailbox_autocreate:
    path: "{{ dovecot_confd_dir }}/15-lda.conf"
    var: 'lda_mailbox_autocreate'
    value: 'yes'
  lda_mailbox_autosubscribe:
    path: "{{ dovecot_confd_dir }}/15-lda.conf"
    var: 'lda_mailbox_autosubscribe'
    value: 'yes'

# Diffentiation from defaults
dovecot_settings_overwrite: {}
</pre></code>

### vars/family-Debian.yml
<pre><code>
dovecot_packages:
  - dovecot-imapd
  - dovecot-sieve
  - dovecot-managesieved
  - dovecot-lmtpd
  - mailutils

dovecot_service: dovecot

dovecot_fw_ports:
  - { port: 993, proto: tcp }

dovecot_conf_dir: /etc/dovecot
dovecot_confd_dir: "{{ dovecot_conf_dir }}/conf.d"
dovecot_config: "{{ dovecot_conf_dir }}/dovecot.conf"
</pre></code>

### vars/family-RedHat-9.yml
<pre><code>
dovecot_packages:
  - dovecot
  - dovecot-pigeonhole
  - s-nail

dovecot_service: dovecot

dovecot_fw_ports:
  - { port: 993, proto: tcp }

dovecot_conf_dir: /etc/dovecot
dovecot_confd_dir: "{{ dovecot_conf_dir }}/conf.d"
dovecot_config: "{{ dovecot_conf_dir }}/dovecot.conf"
</pre></code>

### vars/family-RedHat-8.yml
<pre><code>
dovecot_packages:
  - dovecot
  - dovecot-pigeonhole
  - mailx

dovecot_service: dovecot

dovecot_fw_ports:
  - { port: 993, proto: tcp }

dovecot_conf_dir: /etc/dovecot
dovecot_confd_dir: "{{ dovecot_conf_dir }}/conf.d"
dovecot_config: "{{ dovecot_conf_dir }}/dovecot.conf"
</pre></code>

### vars/family-RedHat-7.yml
<pre><code>
dovecot_packages:
  - dovecot
  - dovecot-pigeonhole
  - mailx

dovecot_service: dovecot

dovecot_fw_ports:
  - { port: 993, proto: tcp }

dovecot_conf_dir: /etc/dovecot
dovecot_confd_dir: "{{ dovecot_conf_dir }}/conf.d"
dovecot_config: "{{ dovecot_conf_dir }}/dovecot.conf"

dovecot_settings:
  protocols:
    path: "{{ dovecot_config }}"
    var: 'protocols'
    value: 'imap lmtp sieve'
  listen:
    path: "{{ dovecot_config }}"
    var: 'listen'
    value: '*'
  disable_plaintext_auth:
    path: "{{ dovecot_confd_dir }}/10-auth.conf"
    var: 'disable_plaintext_auth'
    value: 'no'
  auth_mechanisms:
    path: "{{ dovecot_confd_dir }}/10-auth.conf"
    var: 'auth_mechanisms'
    value: 'plain login'
  mail_location:
    path: "{{ dovecot_confd_dir }}/10-mail.conf"
    var: 'mail_location'
    value: 'maildir:~/Maildir'
  ssl:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl'
    value: 'required'
  ssl_cert:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_cert'
    # value: '<{{ ssl_fullchain }}'
    value: '<{{ dovecot_ssl_chain }}'
  ssl_key:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_key'
    # value: '<{{ ssl_key }}'
    value: '<{{ dovecot_ssl_key }}'
  ssl_protocols:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_protocols'
    value: '!SSLv2 !SSLv3 !TLSv1 !TLSv1.1'
  ssl_cipher_list:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_cipher_list'
    # value: 'ALL:!LOW:!SSLv2:!EXP:!aNULL:!RC4::!eNULL:!LOW:!3DES:!MD5:!EXP:!PSK:!SRP:!DSS'
    value:
      - 'EDH+CAMELLIA'
      - 'EDH+aRSA'
      - 'EECDH+aRSA+AESGCM'
      - 'EECDH+aRSA+SHA384'
      - 'EECDH+aRSA+SHA256'
      - 'EECDH'
      - '+CAMELLIA256'
      - '+AES256'
      - '+CAMELLIA128'
      - '+AES128'
      - '!aNULL'
      - '!eNULL'
      - '!SSLv2'
      - '!LOW'
      - '!DES'
      - '!3DES'
      - '!IDEA'
      - '!MD5'
      - '!EXP'
      - '!PSK'
      - '!DSS'
      - '!RC4'
      - '!SEED'
      - '!ECDSA'
      - 'CAMELLIA256-SHA'
      - 'AES256-SHA'
      - 'CAMELLIA128-SHA'
      - 'AES128-SHA'
  ssl_dh_parameters_length:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: ssl_dh_parameters_length
    value: "2048"
  postmaster_address:
    path: "{{ dovecot_confd_dir }}/15-lda.conf"
    var: 'postmaster_address'
    value: 'postmaster@{{ dovecot_domain }}'
  lda_mailbox_autocreate:
    path: "{{ dovecot_confd_dir }}/15-lda.conf"
    var: 'lda_mailbox_autocreate'
    value: 'yes'
  lda_mailbox_autosubscribe:
    path: "{{ dovecot_confd_dir }}/15-lda.conf"
    var: 'lda_mailbox_autosubscribe'
    value: 'yes'
</pre></code>



## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'dovecot'
  hosts: all
  vars:
    dovecot_ssl_key: "{{ openssl_server_key }}"
    dovecot_ssl_chain: "{{ openssl_server_crt }}"
    dovecot_domain: example.com
    postfix_ipv6: False
    postfix_domain: example.com
    postfix_fqdn: host.example.com
    postfix_ssl_key: "{{ openssl_server_key }}"
    postfix_ssl_chain: "{{ openssl_server_crt }}"
  pre_tasks:
    - name: Create 'remote_tmp'
      file:
        path: /root/.ansible/tmp
        state: directory
        mode: "0700"
  roles:
    - cron
    - openssl
    - postfix
  tasks:
    - name: Include role 'dovecot'
      include_role:
        name: dovecot
</pre></code>
