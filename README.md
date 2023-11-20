# tomcat

FreeBSD & Debian, tomcat 8/9, jdk8, memcache sessions, remoteipvalve support

## variables (default value)

### general config

* `classpath_adds` ('')
  jar's list to be added, column-separated (:)
* `tomcat_java_opts` ('-Djava.awt.headless=true -XX:+UseConcMarkSweepGC -Xms1024m -Xmx2g -Dlog4j2.formatMsgNoLookups=true')
  additional java command args
* `tomcat_lang_opts` ('')
  additional java command args
* `tomcat_http_port` (8080)
  http port
* `tomcat_ajp_port` (8009)
  ajp port
* `tomcat_ajp_address (0.0.0.0)`
  ajp listen address
* `tomcat_keystore` ('')
  path to a keystore file
* `tomcat_storepass` ('')
  keystore password
* `tomcat_catalina_props` ([])
  list of lines to be added/replaced in catalina.properties
  (will replace line matching line before '=')
* `tomcat_apr_port ('')`
  if defined, on which port we should listen with APR connector
* `tomcat_apr_cert ('')`
  If defined to an x509 cert, HTTPS support will be activated with APR support
  cert and key will be copied in tomcat config dir
* `tomcat_apr_key ('')`
  PEM key for above cert. Mandatory if you want TLS with APR
* `jre_dir (depend on distrib)`
  path to jre for tomcat
* `tomcat_env_vars ({})`
  dict of environment variables for tomcat
* `tomcat_env_file ('')`
  a file sourced for environment variables
* see `defaults/main.yml` for exhaustive list

### http(s) reverse-proxy support

* `proxies_ips_regex` ('')
  regex matching proxy(ies) IP (eg: '^(127\.0\.0\.1|::1)$' for localhost IPv4 and IPv6)

* if `proxies_ips_regex` is defined, RemoteIpValve will be configured, supporting:
  * X-Forwarded-For
  * X-Forwarded-Proto (must be 'https' if ssl)

### memcache sessions

if `memcached_nodes` is not empty:

Session are replicated between memcache instances on each tomcat host 
Using https://github.com/magro/memcached-session-manager/wiki

* `memcached_nodes` has to be filled as this:
  <code><pre>memcached_nodes: 'japps3:japps3.serv.int:11211,japps4:japps4.serv.int:11211'</pre></code>
  node names (here japps3/japps4) *MUST* match inventory `inventory_hostname`
* `tomcat_memcached_sticky (True)`
  If set to false, enable non-sticky synchronous sessions

(see templates/context.xml.j2)

### webapps deployment

* `tomcat_webapps ([])` list of dicts describing webapps to deploy
  * `key` is name
  * `war` is war file to deploy in webapps dir
  * `gitsrc` will be cloned in dir `gitdst` if both exists
  * `script` is the command line, relative to `gitdst`, to be launched (as root) user after deploying (if any)

* `myapp ('')` If defined, only deploy this app (must match webapp.key)
