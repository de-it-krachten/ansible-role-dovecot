[![CI](https://github.com/de-it-krachten/ansible-role-dovecot/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-dovecot/actions?query=workflow%3ACI)


# ansible-role-dovecot

Installs & configures Dovecot



## Dependencies

#### Roles
None

#### Collections
None

## Platforms

Supported platforms

- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- Red Hat Enterprise Linux 10<sup>1</sup>
- RockyLinux 8
- RockyLinux 9
- RockyLinux 10
- OracleLinux 8
- OracleLinux 9
- OracleLinux 10
- AlmaLinux 8
- AlmaLinux 9
- AlmaLinux 10
- SUSE Linux Enterprise 15<sup>1</sup>
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Debian 13 (Trixie)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Default config locations
dovecot_conf_dir: /etc/dovecot
dovecot_confd_dir: "{{ dovecot_conf_dir }}/conf.d"
dovecot_config: "{{ dovecot_conf_dir }}/dovecot.conf"

# Dovecot services
dovecot_service: dovecot

# Firewall ports
dovecot_fw_ports:
  - { port: 993, proto: tcp }

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
      - 'EDH+ aRSA'
      - 'EECDH+ aRSA+AESGCM'
      - 'EECDH+ aRSA+SHA384'
      - 'EECDH+ aRSA+SHA256'
      - 'EECDH'
      - ' +CAMELLIA256'
      - ' +AES256'
      - ' +CAMELLIA128'
      - ' +AES128'
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

# Diffentiation from defaults
dovecot_settings_os_specific: {}
dovecot_settings_overwrite: {}
</pre></code>

### defaults/family-Debian.yml
<pre><code>
dovecot_packages:
  - dovecot-imapd
  - dovecot-sieve
  - dovecot-managesieved
  - dovecot-lmtpd
  - mailutils
</pre></code>

### defaults/family-RedHat-7.yml
<pre><code>
dovecot_packages:
  - dovecot
  - dovecot-pigeonhole
  - mailx

dovecot_settings_os_specific:
  ssl_protocols:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_protocols'
    value: '!SSLv2 !SSLv3 !TLSv1 !TLSv1.1'
  ssl_min_protocol:
    path: "{{ dovecot_confd_dir }}/10-ssl.conf"
    var: 'ssl_min_protocol'
    state: absent
</pre></code>

### defaults/family-RedHat-8.yml
<pre><code>
dovecot_packages:
  - dovecot
  - dovecot-pigeonhole
  - mailx
</pre></code>

### defaults/family-RedHat-9.yml
<pre><code>
dovecot_packages:
  - dovecot
  - dovecot-pigeonhole
  - s-nail
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'dovecot'
  hosts: all
  become: 'yes'
  vars:
    dovecot_ssl_key: '{{ openssl_server_key }}'
    dovecot_ssl_chain: '{{ openssl_server_crt }}'
    dovecot_domain: example.com
    postfix_ipv6: false
    postfix_domain: example.com
    postfix_fqdn: host.example.com
    postfix_ssl_key: '{{ openssl_server_key }}'
    postfix_ssl_chain: '{{ openssl_server_crt }}'
  roles:
    - deitkrachten.cron
    - deitkrachten.openssl
    - deitkrachten.postfix
  tasks:
    - name: Include role 'dovecot'
      ansible.builtin.include_role:
        name: dovecot
</pre></code>
