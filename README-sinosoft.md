## slapd for 中科软

A basic configuration of the OpenLDAP server, slapd, with support for data
volumes.

This image will initialize a basic configuration of slapd. Most common schemas
are preloaded (all the schemas that come preloaded with the default Ubuntu
Precise install of slapd), but the only record added to the directory will be
the root organisational unit.

You can (and should) configure the following by providing environment variables
to `docker run`:

- `LDAP_DOMAIN` sinosoft.com.cn
- `LDAP_ORGANISATION` Sinosoft Co. ,Ltd.
- `LDAP_ROOTPASS` `cn=admin,dc=sinosoft,dc=com,dc=cn`的密码`admin`

For example, to start a container running slapd for the `sinosoft.com.cn` domain,
with data stored in `/data/ldap` on the host, use the following:

    docker run -v /data/ldap:/var/lib/ldap \
               -d nickstenning/slapd

You can find out which port the LDAP server is bound to on the host by running
`docker ps` (or `docker port <container_id> 389`). You could then load an LDIF
file (to set up your directory) like so:

    ldapadd -h localhost -p <host_port> -c -x -D cn=admin,dc=sinosoft,dc=com,dc=cn -W -f data.ldif

**NB**: Please be aware that by default docker will make the LDAP port
accessible from anywhere if the host firewall is unconfigured.
