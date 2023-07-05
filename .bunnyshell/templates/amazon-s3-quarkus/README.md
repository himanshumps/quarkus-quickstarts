## Template Overview

This template showcases how to use the AWS S3 client with Quarkus.

## Testing

Go to `<protocol>://<hostname>/s3.html`, it should show a simple App to manage files on your Bucket.
You can upload files to the bucket via the form.

Alternatively, go to `<protocol>://<hostname>/async-s3.html` with the simple App communicating with Async resources.

## Images Used

- [Redhat UBI8 OpenJDK 11](https://catalog.redhat.com/software/containers/ubi8/openjdk-11/5dd6a4b45a13461646f677f4)

- [S3 via Localstack](https://hub.docker.com/r/localstack/localstack)

## Container images

The images have 2 possible stages(if specified): `dev` and `prod`.

The `dev` stage is suitable for development, as it contains development packages and libraries and has debugging enabled.  
The `prod` stage is meant for production/staging, as it produces an optimized image, as lightweight as possible.

The stage (`dev` or `prod`) can be set from the Environment's configuration (`bunnyshell.yaml`), within each Component's `dockerCompose.build.target` property. The default is `dev`, and it can be changed to `prod` to produce production-like images.

## How to use this Template

You can create Environments from a [Bunnyshell Template](https://documentation.bunnyshell.com/docs/templates-what-are-templates); these Environments can have multiple purposes:


### Staging / Testing

You need to ensure that the `dockerCompose.build.target` is set to `prod` for all the Components, and then [deploy the Environment](https://documentation.bunnyshell.com/docs/environment-workflows-deploy).


### Remote Development

[Remote Development](https://documentation.bunnyshell.com/docs/remote-development) allows you to develop directly in a cloud environment, therefore eliminating all inconsistencies and approximations of traditional local environments.

The code is executed in a container running in Kubernetes, while being synchronized real-time with your local folders.

📖 For more information on how Remote Development works in Bunnyshell, please see the [dedicated documentation](https://documentation.bunnyshell.com/docs/remote-development).

🧱 Remote Development can only be started from the CLI, so you will need to [install the Bunnyshell CLI](https://documentation.bunnyshell.com/docs/bunnyshell-cli-install) installed and to [authenticate in the CLI](https://documentation.bunnyshell.com/docs/bunnyshell-cli-authentication).

You need to ensure that the `dockerCompose.build.target` is set to `dev` for all the Components, and then [deploy the Environment](https://documentation.bunnyshell.com/docs/environment-workflows-deploy).

## Important Note

You must change all passwords and review all parameters to ensure that your Environment is secure.

## Customization of the image using environment variable

The RedHat UBI image and the quarkus application provide a ton of flags to modify the application behaviour using environment variables.

The Java Runtime startup parameters that are available are as follows:

|Variable name|Description|Default value|Example value|
|:----|:----|:----|:----|
|AB_JOLOKIA_CONFIG|If set uses this file (including path) as Jolokia JVM agent properties (as described in the Jolokia reference manual). If not set, the /opt/jolokia/etc/jolokia.properties will be created using the settings as defined in the manual. Otherwise the rest of the settings in this document are ignored.|-|/opt/jolokia/custom.properties|
|AB_JOLOKIA_DISCOVERY_ENABLED|Enable Jolokia discovery.|false|true|
|AB_JOLOKIA_HOST|Host address to bind to.|0.0.0.0|127.0.0.1|
|AB_JOLOKIA_ID|Agent ID to use, which is the container id.|$HOSTNAME|openjdk-app-1-xqlsj|
|AB_JOLOKIA_OFF|If set disables activation of Joloka (that is, echos an empty value).|Jolokia is enabled|true|
|AB_JOLOKIA_OPTS|Additional options to be appended to the agent configuration. They should be specified in the format key=value,key=value,…​.|-|backlog=20|
|AB_JOLOKIA_PASSWORD|Password for basic authentication. By default authentication is switched off.|-|mypassword|
|AB_JOLOKIA_PORT|Port to listen to.|8778|5432|
|AB_JOLOKIA_USER|User for basic authentication.|jolokia|myusername|
|CONTAINER_CORE_LIMIT|A calculated core limit as described in the CFS Bandwidth Control.|-|2|
|CONTAINER_MAX_MEMORY|Memory limit assigned to the container.|-|1024|
|GC_ADAPTIVE_SIZE_POLICY_WEIGHT|The weighting given to the current garbage collector time versus previous garbage collector times.|-|90|
|GC_CONTAINER_OPTIONS|Specify Java GC to use. The value of this variable should contain the necessary JRE command-line options to specify the required GC, which will override the default value.|-XX:+UseParallelOldGC|-XX:+UseG1GC|
|GC_MAX_HEAP_FREE_RATIO|Maximum percentage of heap free after GC to avoid shrinkage.|-|40|
|GC_MAX_METASPACE_SIZE|The maximum metaspace size.|-|100|
|GC_METASPACE_SIZE|The initial metaspace size.|-|20|
|GC_MIN_HEAP_FREE_RATIO|Minimum percentage of heap free after GC to avoid expansion.|-|20|
|GC_TIME_RATIO|Specifies the ratio of the time spent outside the garbage collection (for example, the time spent for application execution) to the time spent in the garbage collection.|-|4|
|HTTPS_PROXY|The location of the HTTPS proxy. This takes precedence over http_proxy and HTTP_PROXY, and will be used for both Maven builds and Java runtime.|-|myuser@127.0.0.1:8080|
|HTTP_PROXY|The location of the HTTP proxy. This will be used for both Maven builds and Java runtime.|-|127.0.0.1:8080|
|JAVA_APP_DIR|The directory where the application resides. All paths in your application are relative to this directory.|-|myapplication/|
|JAVA_ARGS|Arguments passed to the java application.|-|-|
|JAVA_CLASSPATH|The classpath to use. If not given, the startup script checks for a file JAVA_APP_DIR/classpath and uses its content literally as classpath. If this file does not exists all jars in the app dir are added (classes:JAVA_APP_DIR/).|-|-|
|JAVA_DEBUG|If set remote debugging will be switched on.|false|true|
|JAVA_DEBUG_PORT|Port used for remote debugging.|5005|8787|
|JAVA_DIAGNOSTICS|Set this to print some diagnostics information to standard output during the command is running.|false|true|
|JAVA_INITIAL_MEM_RATIO|It is used when no -Xms option is given in JAVA_OPTS. This is used to calculate a default initial heap memory based on the maximum heap memory. If used in a container without any memory constraints for the container then this option has no effect. If there is a memory constraint then -Xms is set to a ratio of the -Xmx memory as set here. The default is 25 which means 25% of the -Xmx is used as the initial heap size. You can skip this mechanism by setting this value to 0 in which case no -Xms option is added.|25|25|
|JAVA_LIB_DIR|Directory holding the Java jar files as well as an optional classpath file which holds the classpath. Either as a single-line classpath (colon separated) or with jar files listed line by line. If not set JAVA_LIB_DIR is set to the value of JAVA_APP_DIR.|JAVA_APP_DIR|-|
|JAVA_MAIN_CLASS|A main class to use as argument for java. When this environment variable is given, all jar files in JAVA_APP_DIR are added to the classpath as well as JAVA_LIB_DIR.|-|com.example.MainClass|
|JAVA_MAX_INITIAL_MEM|It is used when no -Xms option is given in JAVA_OPTS. This is used to calculate the maximum value of the initial heap memory. If used in a container without any memory constraints for the container then this option has no effect. If there is a memory constraint then -Xms is limited to the value set here. The default is 4096 which means the calculated value of -Xms will never be greater than 4096. The value of this variable is expressed in MB.|4096|4096|
|JAVA_MAX_MEM_RATIO|It is used when no -Xmx option is given in JAVA_OPTS. This is used to calculate a default maximum heap memory based on a containers restriction. If used in a container without any memory constraints for the container then this option has no effect. If there is a memory constraint then -Xmx is set to a ratio of the container available memory as set here. The default is 50 which means 50% of the available memory is used as an upper boundary. You can skip this mechanism by setting this value to 0 in which case no -Xmx option is added.|50|-|
|JAVA_OPTS|JVM options passed to the java command.|-|-verbose:class|
|JAVA_OPTS_APPEND|User-specified Java options to be appended to generated options in JAVA_OPTS.|-|-Dsome.property=foo|
|NO_PROXY|A comma-separated lists of hosts, IP addresses or domains that can be accessed directly. This will be used for both Maven builds and Java runtime.|-|foo.example.com,bar.example.com|
|http_proxy|The location of the HTTP proxy. This takes precedence over HTTP_PROXY and is use for both Maven builds and Java runtime.|-|http://127.0.0.1:8080|
|https_proxy|The location of the HTTPS proxy. This takes precedence over HTTPS_PROXY, http_proxy, and HTTP_PROXY, is use for both Maven builds and Java runtime.|-|myuser:mypass@127.0.0.1:8080|
|no_proxy|A comma-separated lists of hosts, IP addresses or domains that can be accessed directly. This takes precedence over NO_PROXY and is use for both Maven builds and Java runtime.|-|*.example.com|<br/>The complete list of Java customization is available [here](https://access.redhat.com/documentation/en-us/openjdk/11/html-single/using_openjdk_11_source-to-image_for_openshift/index#s2i-openshift-config-env).

Quarkus also provides a ton of flags to customize the application behaviour. These are some of the flags that are most commonly used.

| Configuration property | Type| Default|
|--|--|--|
| **quarkus.http.cors**<br/>Enable the CORS filter.<br/>**Environment variable:** QUARKUS_HTTP_CORS| boolean|false|
| **quarkus.http.port**<br/>The HTTP port<br/>**Environment variable:** QUARKUS_HTTP_PORT| int | 8080|
| **quarkus.http.test-port**<br/>The HTTP port used to run tests<br/>**Environment variable:** QUARKUS_HTTP_TEST_PORT| int| 8081|
| **quarkus.http.host**<br/>The HTTP host In dev/test mode this defaults to localhost, in prod mode this defaults to 0.0.0.0 Defaulting to 0.0.0.0 makes it easier to deploy Quarkus to container, however it is not suitable for dev/test mode as other people on the network can connect to your development machine.<br/>**Environment variable:** QUARKUS_HTTP_HOST| string | |
| **quarkus.http.host-enabled**<br/>Enable listening to host:port<br/>**Environment variable:** QUARKUS_HTTP_HOST_ENABLED| boolean| true |
| **quarkus.http.ssl-port**<br/>The HTTPS port<br/>**Environment variable:** QUARKUS_HTTP_SSL_PORT| int | 8443|
| **quarkus.http.test-ssl-port**<br/>The HTTPS port used to run tests<br/>**Environment variable:** QUARKUS_HTTP_TEST_SSL_PORT| int | 8444|
| **quarkus.http.insecure-requests**<br/>If insecure (i.e. http rather than https) requests are allowed. If this is enabled then http works as normal. redirect will still open the http port, but all requests will be redirected to the HTTPS port. disabled will prevent the HTTP port from opening at all.<br/>**Environment variable:** QUARKUS_HTTP_INSECURE_REQUESTS| enabled, redirect, disabled | enabled|
| **quarkus.http.http2**<br/>If this is true (the default) then HTTP/2 will be enabled. Note that for browsers to be able to use it HTTPS must be enabled, and you must be running on JDK11 or above, as JDK8 does not support ALPN.<br/>**Environment variable:** QUARKUS_HTTP_HTTP2| boolean| true|
| **quarkus.http.cors.origins**<br/>Origins allowed for CORS Comma separated list of valid URLs, e.g.: http://www.quarkus.io,http://localhost:3000 In case an entry of the list is surrounded by forward slashes, it is interpreted as a regular expression.<br/>**Environment variable:** QUARKUS_HTTP_CORS_ORIGINS | list of string | |
| **quarkus.http.cors.methods**<br/>HTTP methods allowed for CORS Comma separated list of valid methods. ex: GET,PUT,POST The filter allows any method if this is not set. default: returns any requested method as valid<br/>**Environment variable:** QUARKUS_HTTP_CORS_METHODS | list of string | |
| **quarkus.http.cors.headers**<br/>HTTP headers allowed for CORS Comma separated list of valid headers. ex: X-Custom,Content-Disposition The filter allows any header if this is not set. default: returns any requested header as valid<br/>**Environment variable:** QUARKUS_HTTP_CORS_HEADERS| list of string ||
| **quarkus.http.cors.exposed-headers**<br/>HTTP headers exposed in CORS Comma separated list of valid headers. ex: X-Custom,Content-Disposition default: empty<br/>**Environment variable:** QUARKUS_HTTP_CORS_EXPOSED_HEADERS| list of string | |
| **quarkus.http.cors.access-control-max-age**<br/>The Access-Control-Max-Age response header value indicating how long the results of a pre-flight request can be cached.<br/>**Environment variable:** QUARKUS_HTTP_CORS_ACCESS_CONTROL_MAX_AGE | Duration | |
| **quarkus.http.cors.access-control-allow-credentials**<br/>The Access-Control-Allow-Credentials header is used to tell the browsers to expose the response to front-end JavaScript code when the request’s credentials mode Request.credentials is “include”. The value of this header will default to true if quarkus.http.cors.origins property is set and there is a match with the precise Origin header and that header is not '*'.<br/>**Environment variable:** QUARKUS_HTTP_CORS_ACCESS_CONTROL_ALLOW_CREDENTIALS | boolean| |
| **quarkus.http.ssl.certificate.credentials-provider**<br/>The CredentialsProvider. If this property is configured then a matching 'CredentialsProvider' will be used to get the keystore, keystore key and truststore passwords unless these passwords have already been configured. Please note that using MicroProfile ConfigSource which is directly supported by Quarkus Configuration should be preferred unless using CredentialsProvider provides for some additional security and dynamism.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_CREDENTIALS_PROVIDER| string | |
| **quarkus.http.ssl.certificate.credentials-provider-name**<br/>The credentials provider bean name.<br/>It is the &amp;#64;Named value of the credentials provider bean. It is used to discriminate if multiple CredentialsProvider beans are available. It is recommended to set this property even if there is only one credentials provider currently available to ensure the same provider is always found in deployments where more than one provider may be available.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_CREDENTIALS_PROVIDER_NAME | string | |
| **quarkus.http.ssl.certificate.files**<br/>The list of path to server certificates using the PEM format. Specifying multiple files require SNI to be enabled.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_FILES | list of path | |
| **quarkus.http.ssl.certificate.key-files**<br/>The list of path to server certificates private key file using the PEM format. Specifying multiple files require SNI to be enabled. The order of the key files must match the order of the certificates.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_KEY_FILES | list of path | |
| **quarkus.http.ssl.certificate.key-store-file**<br/>An optional key store which holds the certificate information instead of specifying separate files.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_KEY_STORE_FILE | path| |
| **quarkus.http.ssl.certificate.key-store-file-type**<br/>An optional parameter to specify type of the key store file. If not given, the type is automatically detected based on the file name.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_KEY_STORE_FILE_TYPE| string | |
| **quarkus.http.ssl.certificate.key-store-provider**<br/>An optional parameter to specify a provider of the key store file. If not given, the provider is automatically detected based on the key store file type.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_KEY_STORE_PROVIDER| string | |
| **quarkus.http.ssl.certificate.key-store-password**<br/>A parameter to specify the password of the key store file. If not given, and if it can not be retrieved from CredentialsProvider, then the default ("password") is used.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_KEY_STORE_PASSWORD| string | password |
| **quarkus.http.ssl.certificate.key-store-password-key**<br/>A parameter to specify a CredentialsProvider property key which can be used to get the password of the key store file from CredentialsProvider.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_KEY_STORE_PASSWORD_KEY | string | |
| **quarkus.http.ssl.certificate.key-store-key-alias**<br/>An optional parameter to select a specific key in the key store. When SNI is disabled, if the key store contains multiple keys and no alias is specified, the behavior is undefined.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_KEY_STORE_KEY_ALIAS | string | |
| **quarkus.http.ssl.certificate.key-store-key-password**<br/>An optional parameter to define the password for the key, in case it&#8217;s different from key-store-password If not given then it may be retrieved from CredentialsProvider.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_KEY_STORE_KEY_PASSWORD | string | |
| **quarkus.http.ssl.certificate.key-store-key-password-key**<br/>A parameter to specify a CredentialsProvider property key which can be used to get the password for the key from CredentialsProvider.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_KEY_STORE_KEY_PASSWORD_KEY | string | |
| **quarkus.http.ssl.certificate.trust-store-file**<br/>An optional trust store which holds the certificate information of the certificates to trust.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_TRUST_STORE_FILE | path| |
| **quarkus.http.ssl.certificate.trust-store-file-type**<br/>An optional parameter to specify type of the trust store file. If not given, the type is automatically detected based on the file name.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_TRUST_STORE_FILE_TYPE| string | |
| **quarkus.http.ssl.certificate.trust-store-provider**<br/>An optional parameter to specify a provider of the trust store file. If not given, the provider is automatically detected based on the trust store file type.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_TRUST_STORE_PROVIDER | string | |
| **quarkus.http.ssl.certificate.trust-store-password**<br/>A parameter to specify the password of the trust store file. If not given then it may be retrieved from CredentialsProvider.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_TRUST_STORE_PASSWORD | string | |
| **quarkus.http.ssl.certificate.trust-store-password-key**<br/>A parameter to specify a CredentialsProvider property key which can be used to get the password of the trust store file from CredentialsProvider.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_TRUST_STORE_PASSWORD_KEY | string | |
| **quarkus.http.ssl.certificate.trust-store-cert-alias**<br/>An optional parameter to trust only one specific certificate in the trust store (instead of trusting all certificates in the store).<br/>**Environment variable:** QUARKUS_HTTP_SSL_CERTIFICATE_TRUST_STORE_CERT_ALIAS | string | |
| **quarkus.http.ssl.cipher-suites**<br/>The cipher suites to use. If none is given, a reasonable default is selected.<br/>**Environment variable:** QUARKUS_HTTP_SSL_CIPHER_SUITES | list of string | |
| **quarkus.http.ssl.protocols**<br/>The list of protocols to explicitly enable.<br/>**Environment variable:** QUARKUS_HTTP_SSL_PROTOCOLS | list of string | TLSv1.3,TLSv1.2 |
| **quarkus.http.ssl.sni**<br/>Enables Server Name Indication (SNI), an TLS extension allowing the server to use multiple certificates. The client indicate the server name during the TLS handshake, allowing the server to select the right certificate.<br/>**Environment variable:** QUARKUS_HTTP_SSL_SNI | boolean| false |
| **quarkus.http.static-resources.index-page<**br/>Set the index page when serving static resources.<br/>**Environment variable:** QUARKUS_HTTP_STATIC_RESOURCES_INDEX_PAGE | string | index.html|
| **quarkus.http.static-resources.include-hidden**<br/>Set whether hidden files should be served.<br/>**Environment variable:** QUARKUS_HTTP_STATIC_RESOURCES_INCLUDE_HIDDEN | boolean| true|
| **quarkus.http.static-resources.enable-range-support**<br/>Set whether range requests (resumable downloads; media streaming) should be enabled.<br/>**Environment variable:** QUARKUS_HTTP_STATIC_RESOURCES_ENABLE_RANGE_SUPPORT | boolean| true|
| **quarkus.http.static-resources.caching-enabled**<br/>Set whether cache handling is enabled.<br/>**Environment variable:** QUARKUS_HTTP_STATIC_RESOURCES_CACHING_ENABLED | boolean| true|
| **quarkus.http.static-resources.cache-entry-timeout**<br/>Set the cache entry timeout. The default is 30 seconds.<br/>**Environment variable:** QUARKUS_HTTP_STATIC_RESOURCES_CACHE_ENTRY_TIMEOUT | Duration | 30S |
| **quarkus.http.static-resources.max-age**<br/>Set value for max age in caching headers. The default is 24 hours.<br/>**Environment variable:** QUARKUS_HTTP_STATIC_RESOURCES_MAX_AGE | Duration | 24H |
| **quarkus.http.static-resources.max-cache-size**<br/>Set the max cache size.<br/>**Environment variable:** QUARKUS_HTTP_STATIC_RESOURCES_MAX_CACHE_SIZE | int | 10000 |
| **quarkus.http.handle-100-continue-automatically**<br/>When set to true, the HTTP server automatically sends 100 CONTINUE response when the request expects it (with the Expect: 100-Continue header).<br/>**Environment variable:** QUARKUS_HTTP_HANDLE_100_CONTINUE_AUTOMATICALLY | boolean| false |
| **quarkus.http.io-threads**<br/>The number if IO threads used to perform IO. This will be automatically set to a reasonable value based on the number of CPU cores if it is not provided. If this is set to a higher value than the number of Vert.x event loops then it will be capped at the number of event loops. In general this should be controlled by setting quarkus.vertx.event-loops-pool-size, this setting should only be used if you want to limit the number of HTTP io threads to a smaller number than the total number of IO threads.<br/>**Environment variable:** QUARKUS_HTTP_IO_THREADS | int | |
| **quarkus.http.limits.max-header-size**<br/>The maximum length of all headers.<br/>**Environment variable:** QUARKUS_HTTP_LIMITS_MAX_HEADER_SIZE| MemorySize | 20K |
| **quarkus.http.limits.max-body-size**<br/>The maximum size of a request body.<br/>**Environment variable:** QUARKUS_HTTP_LIMITS_MAX_BODY_SIZE | MemorySize | 10240K |
| **quarkus.http.limits.max-chunk-size**<br/>The max HTTP chunk size<br/>**Environment variable:** QUARKUS_HTTP_LIMITS_MAX_CHUNK_SIZE| MemorySize | 8192|
| **quarkus.http.limits.max-initial-line-length**<br/>The maximum length of the initial line (e.g. "GET / HTTP/1.0").<br/>**Environment variable:** QUARKUS_HTTP_LIMITS_MAX_INITIAL_LINE_LENGTH | int | 4096|
| **quarkus.http.limits.max-form-attribute-size**<br/>The maximum length of a form attribute.<br/>**Environment variable:** QUARKUS_HTTP_LIMITS_MAX_FORM_ATTRIBUTE_SIZE| MemorySize | 2048|
| **quarkus.http.limits.max-connections**<br/>The maximum number of connections that are allowed at any one time. If this is set it is recommended to set a short idle timeout.<br/>**Environment variable:** QUARKUS_HTTP_LIMITS_MAX_CONNECTIONS | int | |
| **quarkus.http.idle-timeout**<br/>Http connection idle timeout<br/>**Environment variable:** QUARKUS_HTTP_IDLE_TIMEOUT| Duration | 30M |
| **quarkus.http.read-timeout**<br/>Http connection read timeout for blocking IO. This is the maximum amount of time a thread will wait for data, before an IOException will be thrown and the connection closed.<br/>**Environment variable:** QUARKUS_HTTP_READ_TIMEOUT| Duration | 60S |
| **quarkus.http.body.handle-file-uploads**<br/>Whether the files sent using multipart/form-data will be stored locally.<br/><br/>If true, they will be stored in quarkus.http.body-handler.uploads-directory and will be made available via io.vertx.ext.web.RoutingContext.fileUploads(). Otherwise, the files sent using multipart/form-data will not be stored locally, and io.vertx.ext.web.RoutingContext.fileUploads() will always return an empty collection. Note that even with this option being set to false, the multipart/form-data requests will be accepted.<br/>**Environment variable:** QUARKUS_HTTP_BODY_HANDLE_FILE_UPLOADS| boolean| true|
| **quarkus.http.body.uploads-directory**<br/>The directory where the files sent using multipart/form-data should be stored.<br/>Either an absolute path or a path relative to the current directory of the application process.<br/>**Environment variable:** QUARKUS_HTTP_BODY_UPLOADS_DIRECTORY| string | ${java.io.tmpdir}/uploads|
| **quarkus.http.body.merge-form-attributes**<br/>Whether the form attributes should be added to the request parameters.<br/>If true, the form attributes will be added to the request parameters; otherwise the form parameters will not be added to the request parameters<br/>**Environment variable:** QUARKUS_HTTP_BODY_MERGE_FORM_ATTRIBUTES| boolean| true|
| **quarkus.http.body.delete-uploaded-files-on-end**<br/>Whether the uploaded files should be removed after serving the request.<br/>If true the uploaded files stored in quarkus.http.body-handler.uploads-directory will be removed after handling the request. Otherwise, the files will be left there forever.<br/>**Environment variable:** QUARKUS_HTTP_BODY_DELETE_UPLOADED_FILES_ON_END| boolean| true|
| **quarkus.http.body.preallocate-body-buffer**<br/>Whether the body buffer should pre-allocated based on the Content-Length header value.<br/>If true the body buffer is pre-allocated according to the size read from the Content-Length header. Otherwise, the body buffer is pre-allocated to 1KB, and is resized dynamically<br/>**Environment variable:** QUARKUS_HTTP_BODY_PREALLOCATE_BODY_BUFFER | boolean| false |
| **quarkus.http.body.multipart.file-content-types**<br/>A comma-separated list of ContentType to indicate whether a given multipart field should be handled as a file part. You can use this setting to force HTTP-based extensions to parse a message part as a file based on its content type. For now, this setting only works when using RESTEasy Reactive.<br/>**Environment variable:** QUARKUS_HTTP_BODY_MULTIPART_FILE_CONTENT_TYPES| list of string | |
| **quarkus.http.auth.session.encryption-key**<br/>The encryption key that is used to store persistent logins (e.g. for form auth). Logins are stored in a persistent cookie that is encrypted with AES-256 using a key derived from a SHA-256 hash of the key that is provided here. If no key is provided then an in-memory one will be generated, this will change on every restart though so it is not suitable for production environments. This must be more than 16 characters long for security reasons<br/>**Environment variable:** QUARKUS_HTTP_AUTH_SESSION_ENCRYPTION_KEY| string | |
| **quarkus.http.so-reuse-port**<br/>Enable socket reuse port (linux/macOs native transport only)<br/>**Environment variable:** QUARKUS_HTTP_SO_REUSE_PORT| boolean| false |
| **quarkus.http.tcp-quick-ack**<br/>Enable tcp quick ack (linux native transport only)<br/>**Environment variable:** QUARKUS_HTTP_TCP_QUICK_ACK| boolean| false |
| **quarkus.http.tcp-cork**<br/>Enable tcp cork (linux native transport only)<br/>**Environment variable:** QUARKUS_HTTP_TCP_CORK| boolean| false |
| **quarkus.http.tcp-fast-open**<br/>Enable tcp fast open (linux native transport only)<br/>**Environment variable:** QUARKUS_HTTP_TCP_FAST_OPEN| boolean| false |
| **quarkus.http.accept-backlog**<br/>The accept backlog, this is how many connections can be waiting to be accepted before connections start being rejected<br/>**Environment variable:** QUARKUS_HTTP_ACCEPT_BACKLOG| int | -1 |
| **quarkus.http.domain-socket**<br/>Path to a unix domain socket<br/>**Environment variable:** QUARKUS_HTTP_DOMAIN_SOCKETc| string | /var/run/io.quarkus.app.socket |
| **quarkus.http.domain-socket-enabled**<br/>Enable listening to host:port<br/>**Environment variable:** QUARKUS_HTTP_DOMAIN_SOCKET_ENABLED | boolean| false |
| **quarkus.http.record-request-start-time**<br/>If this is true then the request start time will be recorded to enable logging of total request time. This has a small performance penalty, so is disabled by default.<br/>**Environment variable:** QUARKUS_HTTP_RECORD_REQUEST_START_TIME| boolean| false |
| **quarkus.http.access-log.enabled**<br/>If access logging is enabled. By default this will log via the standard logging facility<br/>**Environment variable:** QUARKUS_HTTP_ACCESS_LOG_ENABLED | boolean| false |
| **quarkus.http.access-log.exclude-pattern**<br/>A regular expression that can be used to exclude some paths from logging.<br/>**Environment variable:** QUARKUS_HTTP_ACCESS_LOG_EXCLUDE_PATTERN | string | |
| **quarkus.http.access-log.pattern**<br/>The access log pattern.<br/>If this is the string common, combined or long then this will use one of the specified named formats:<br/>common: %h %l %u %t "%r" %s %b<br/>combined: %h %l %u %t "%r" %s %b "%{i,Referer}" "%{i,User-Agent}"<br/>long: %r\n%{ALL_REQUEST_HEADERS}<br/>Otherwise, consult the Quarkus documentation for the full list of variables that can be used.<br/>**Environment variable:** QUARKUS_HTTP_ACCESS_LOG_PATTERN| string | common |
| **quarkus.http.access-log.log-to-file**<br/>If logging should be done to a separate file.<br/>**Environment variable:** QUARKUS_HTTP_ACCESS_LOG_LOG_TO_FILE| boolean| false |
| **quarkus.http.access-log.base-file-name**<br/>The access log file base name, defaults to 'quarkus' which will give a log file name of 'quarkus.log'.<br/>**Environment variable:** QUARKUS_HTTP_ACCESS_LOG_BASE_FILE_NAME| string | quarkus|
| **quarkus.http.access-log.log-directory**<br/>The log directory to use when logging access to a file If this is not set then the current working directory is used.<br/>**Environment variable:** QUARKUS_HTTP_ACCESS_LOG_LOG_DIRECTORY | string | |
| **quarkus.http.access-log.log-suffix**<br/>The log file suffix<br/>**Environment variable:** QUARKUS_HTTP_ACCESS_LOG_LOG_SUFFIX | string | .log|
| **quarkus.http.access-log.category**<br/>The log category to use if logging is being done via the standard log mechanism (i.e. if base-file-name is empty).<br/>**Environment variable:** QUARKUS_HTTP_ACCESS_LOG_CATEGORY | string | io.quarkus.http.access-log |
| **quarkus.http.access-log.rotate**<br/>If the log should be rotated daily<br/>**Environment variable:** QUARKUS_HTTP_ACCESS_LOG_ROTATE| boolean| true|
| **quarkus.http.unhandled-error-content-type-default**<br/>Provides a hint (optional) for the default content type of responses generated for the errors not handled by the application.<br/>If the client requested a supported content-type in request headers (e.g. "Accept: application/json", "Accept: text/html"), Quarkus will use that content type.<br/>Otherwise, it will default to the content type configured here.<br/>**Environment variable:** QUARKUS_HTTP_UNHANDLED_ERROR_CONTENT_TYPE_DEFAULT|json, html| |
| **quarkus.http.proxy.proxy-address-forwarding**<br/>If this is true then the address, scheme etc. will be set from headers forwarded by the proxy server, such as X-Forwarded-For. This should only be set if you are behind a proxy that sets these headers.<br/>**Environment variable:** QUARKUS_HTTP_PROXY_PROXY_ADDRESS_FORWARDING | boolean| false |
| **quarkus.http.proxy.allow-forwarded**<br/>If this is true and proxy address forwarding is enabled then the standard Forwarded header will be used. In case the not standard X-Forwarded-For header is enabled and detected on HTTP requests, the standard header has the precedence. Activating this together with quarkus.http.proxy.allow-x-forwarded has security implications as clients can forge requests with a forwarded header that is not overwritten by the proxy. Therefore, proxies should strip unexpected X-Forwarded or X-Forwarded-* headers from the client.<br/>**Environment variable:** QUARKUS_HTTP_PROXY_ALLOW_FORWARDED| boolean| false |
| **quarkus.http.proxy.allow-x-forwarded**<br/>If either this or allow-forwarded are true and proxy address forwarding is enabled then the not standard Forwarded header will be used. In case the standard Forwarded header is enabled and detected on HTTP requests, the standard header has the precedence. Activating this together with quarkus.http.proxy.allow-x-forwarded has security implications as clients can forge requests with a forwarded header that is not overwritten by the proxy. Therefore, proxies should strip unexpected X-Forwarded or X-Forwarded-* headers from the client.<br/>**Environment variable:** QUARKUS_HTTP_PROXY_ALLOW_X_FORWARDED| boolean| |
| **quarkus.http.proxy.enable-forwarded-host**<br/>Enable override the received request&#8217;s host through a forwarded host header.<br/>**Environment variable:** QUARKUS_HTTP_PROXY_ENABLE_FORWARDED_HOST| boolean| false |
| **quarkus.http.proxy.forwarded-host-header**<br/>Configure the forwarded host header to be used if override enabled.<br/>**Environment variable:** QUARKUS_HTTP_PROXY_FORWARDED_HOST_HEADER| string | X-Forwarded-Host|
| **quarkus.http.proxy.enable-forwarded-prefix**<br/>Enable prefix the received request&#8217;s path with a forwarded prefix header.<br/>**Environment variable:** QUARKUS_HTTP_PROXY_ENABLE_FORWARDED_PREFIX | boolean| false |
| **quarkus.http.proxy.forwarded-prefix-header**<br/>Configure the forwarded prefix header to be used if prefixing enabled.<br/>**Environment variable:** QUARKUS_HTTP_PROXY_FORWARDED_PREFIX_HEADER| string | X-Forwarded-Prefix |
| **quarkus.http.same-site-cookie."same-site-cookie".case-sensitive**<br/>If the cookie pattern is case-sensitive<br/>**Environment variable:** QUARKUS_HTTP_SAME_SITE_COOKIE__SAME_SITE_COOKIE__CASE_SENSITIVE| boolean| false |
| **quarkus.http.same-site-cookie."same-site-cookie".value**<br/>The value to set in the samesite attribute<br/>**Environment variable:** QUARKUS_HTTP_SAME_SITE_COOKIE__SAME_SITE_COOKIE__VALUE | none, strict, lax| |
| **quarkus.http.same-site-cookie."same-site-cookie".enable-client-checker**<br/>Some User Agents break when sent SameSite=None, this will detect them and avoid sending the value<br/>**Environment variable:** QUARKUS_HTTP_SAME_SITE_COOKIE__SAME_SITE_COOKIE__ENABLE_CLIENT_CHECKER| boolean| true|
| **quarkus.http.same-site-cookie."same-site-cookie".add-secure-for-none**<br/>If this is true then the 'secure' attribute will automatically be sent on cookies with a SameSite attribute of None.<br/>**Environment variable:** QUARKUS_HTTP_SAME_SITE_COOKIE__SAME_SITE_COOKIE__ADD_SECURE_FOR_NONE| boolean| true|
| **quarkus.http.header."header".path**<br/>The path this header should be applied<br/>**Environment variable:** QUARKUS_HTTP_HEADER__HEADER__PATH| string | /* |
| **quarkus.http.header."header".value**<br/>The value for this header configuration<br/>**Environment variable:** QUARKUS_HTTP_HEADER__HEADER__VALUE| string | |
| **quarkus.http.header."header".methods**<br/>The HTTP methods for this header configuration<br/>**Environment variable:** QUARKUS_HTTP_HEADER__HEADER__METHODS| list of string |
| **quarkus.http.filter."filter".matches**<br/>A regular expression for the paths matching this configuration<br/>**Environment variable:** QUARKUS_HTTP_FILTER__FILTER__MATCHES| string | |
| **quarkus.http.filter."filter".header**<br/>Additional HTTP Headers always sent in the response<br/>**Environment variable:** QUARKUS_HTTP_FILTER__FILTER__HEADER| Map&lt;String,String&gt; | |
| **quarkus.http.filter."filter".methods**<br/>The HTTP methods for this path configuration<br/>**Environment variable:** QUARKUS_HTTP_FILTER__FILTER__METHODS| list of string | |
| **quarkus.http.filter."filter".order**<br/>**Environment variable:** QUARKUS_HTTP_FILTER__FILTER__ORDER| int | |

The complete list of quarkus flag is available [here](https://quarkus.io/guides/all-config).
