## F5 BIG-IP Runtime Init Schema and Examples

### runtime_parameters: Schema

_Runtime parameters used to render Automation Toolchain declarations._

Type: `array`

<i id="#">path: #</i>

 - **_Items_**
 - Type: `object`
 - <i id="#/items">path: #/items</i>
 - This schema accepts additional properties.
 - **_Properties_**
	 - <b id="#/items/properties/name">name</b> `required`
		 - Type: `string`
		 - <i id="#/items/properties/name">path: #/items/properties/name</i>
		 - Example values: 
			 1. _"ADMIN_PASSWORD"_
			 2. _"HOST_NAME"_
	 - <b id="#/items/properties/type">type</b> `required`
		 - Type: `string`
		 - <i id="#/items/properties/type">path: #/items/properties/type</i>
		 - The value is restricted to the following: 
			 1. _"static"_
			 2. _"secret"_
			 3. _"metadata"_
	 - <b id="#/items/properties/value">value</b>
		 - Type: `string`
		 - <i id="#/items/properties/value">path: #/items/properties/value</i>
		 - Example values: 
			 1. _"myValue"_
	 - <b id="#/items/properties/secretProvider">secretProvider</b>
		 - Type: `object`
		 - <i id="#/items/properties/secretProvider">path: #/items/properties/secretProvider</i>
		 - This schema accepts additional properties.
		 - **_Properties_**
			 - <b id="#/items/properties/secretProvider/properties/environment">environment</b> `required`
				 - Type: `string`
				 - <i id="#/items/properties/secretProvider/properties/environment">path: #/items/properties/secretProvider/properties/environment</i>
				 - The value is restricted to the following: 
					 1. _"gcp"_
					 2. _"aws"_
					 3. _"azure"_
			 - <b id="#/items/properties/secretProvider/properties/type">type</b> `required`
				 - Type: `string`
				 - <i id="#/items/properties/secretProvider/properties/type">path: #/items/properties/secretProvider/properties/type</i>
				 - The value is restricted to the following: 
					 1. _"SecretsManager"_
					 2. _"SecretManager"_
					 3. _"default"_
					 4. _"KeyVault"_
			 - <b id="#/items/properties/secretProvider/properties/secretId">secretId</b> `required`
				 - _ID or name of the secret in the secret manager of the specified cloud_
				 - Type: `string`
				 - <i id="#/items/properties/secretProvider/properties/secretId">path: #/items/properties/secretProvider/properties/secretId</i>
				 - Example values: 
					 1. _"mySecretId"_
					 2. _"test-document-01"_
			 - <b id="#/items/properties/secretProvider/properties/version">version</b>
				 - _Version identifier for the secret to be retrieved_
				 - Type: `string`
				 - <i id="#/items/properties/secretProvider/properties/version">path: #/items/properties/secretProvider/properties/version</i>
				 - Example values: 
					 1. _"AWSCURRENT"_
					 2. _"1.0"_
			 - <b id="#/items/properties/secretProvider/properties/vaultUrl">vaultUrl</b>
				 - _URL of the Azure Key Vault_
				 - Type: `string`
				 - <i id="#/items/properties/secretProvider/properties/vaultUrl">path: #/items/properties/secretProvider/properties/vaultUrl</i>
				 - Example values: 
					 1. _"https://my-keyvault.vault.azure.net"_
					 2. _"https://my-keyvault.vault.usgovcloudapi.net"_
				 - The value must match this pattern: `(https?://(.+?\.)?vault\.(azure|usgovcloudapi)\.net(/[A-Za-z0-9\-\._~:/\?#\[\]@!$&'\(\)\*\+,;\=]*)?)`
	 - <b id="#/items/properties/metadataProvider">metadataProvider</b>
		 - Type: `object`
		 - <i id="#/items/properties/metadataProvider">path: #/items/properties/metadataProvider</i>
		 - This schema accepts additional properties.
		 - **_Properties_**
			 - <b id="#/items/properties/metadataProvider/properties/environment">environment</b> `required`
				 - Type: `string`
				 - <i id="#/items/properties/metadataProvider/properties/environment">path: #/items/properties/metadataProvider/properties/environment</i>
				 - The value is restricted to the following: 
					 1. _"gcp"_
					 2. _"aws"_
					 3. _"azure"_
			 - <b id="#/items/properties/metadataProvider/properties/type">type</b> `required`
				 - Type: `string`
				 - <i id="#/items/properties/metadataProvider/properties/type">path: #/items/properties/metadataProvider/properties/type</i>
				 - The value is restricted to the following: 
					 1. _"network"_
					 2. _"compute"_
			 - <b id="#/items/properties/metadataProvider/properties/field">field</b>
				 - Type: `string`
				 - <i id="#/items/properties/metadataProvider/properties/field">path: #/items/properties/metadataProvider/properties/field</i>
				 - Example values: 
					 1. _"name"_
					 2. _"hostname"_
					 3. _"ipv4"_
					 4. _"local-ipv4s"_
					 5. _"subnet-ipv4-cidr-block"_
			 - <b id="#/items/properties/metadataProvider/properties/index">index</b>
				 - Type: `integer`
				 - <i id="#/items/properties/metadataProvider/properties/index">path: #/items/properties/metadataProvider/properties/index</i>
				 - Example values: 
					 1. _"0"_
					 2. _"1"_
					 3. _"2"_


### runtime_parameters: Configuration Examples

```yaml
aws:
  description: AWS Example
  runtime_parameters:
    - name: ADMIN_PASS
      type: secret
      secretProvider:
        type: SecretManager
        environment: aws
        version: AWSCURRENT
        secretId: test-document-01
    - name: HOST_NAME
      type: metadata
      metadataProvider:
        environment: aws
        type: compute
        field: hostname
    - name: SELF_IP_EXTERNAL
      type: metadata
      metadataProvider:
        environment: aws
        type: network
        field: local-ipv4s
        index: 1
    - name: SELF_IP_INTERNAL
      type: metadata
      metadataProvider:
        environment: aws
        type: network
        field: local-ipv4s
        index: 2
    - name: DEFAULT_ROUTE
      type: metadata
      metadataProvider:
        environment: aws
        type: network
        field: subnet-ipv4-cidr-block
        index: 1
azure:
  description: Azure Example
  runtime_parameters:
    - name: AZURE_SERVICE_PRINCIPAL
      type: secret
      secretProvider:
        type: KeyVault
        environment: azure
        vaultUrl: 'https://my-keyvault.vault.azure.net'
        secretId: my_azure_secret
    - name: HOST_NAME
      type: metadata
      metadataProvider:
        environment: azure
        type: compute
        field: name
    - name: SELF_IP_INTERNAL
      type: metadata
      metadataProvider:
        environment: azure
        type: network
        field: ipv4
        index: 1
    - name: SELF_IP_EXTERNAL
      type: metadata
      metadataProvider:
        environment: azure
        type: network
        field: ipv4
        index: 2
gcp:
  description: Google Example
  runtime_parameters:
    - name: ADMIN_PASS
      type: secret
      secretProvider:
        type: SecretsManager
        environment: gcp
        version: latest
        secretId: my-secret-id-01
    - name: ROOT_PASS
      type: secret
      secretProvider:
        type: SecretsManager
        environment: gcp
        version: latest
        secretId: my-secret-id-02
    - name: HOST_NAME
      type: metadata
      metadataProvider:
        environment: gcp
        type: compute
        field: name

```
***
### post_onboard_enabled: Schema

_Used to specify commands which will be executed following extension services operations._

Type: `array`

<i id="#">path: #</i>

 - **_Items_**
 - Type: `object`
 - <i id="#/items">path: #/items</i>
 - **_Properties_**
	 - <b id="#/items/properties/name">name</b>
		 - Type: `string`
		 - <i id="#/items/properties/name">path: #/items/properties/name</i>
		 - Example values: 
			 1. _"my_postonboard_command"_
			 2. _"example_local_exec"_
	 - <b id="#/items/properties/type">type</b>
		 - Type: `string`
		 - <i id="#/items/properties/type">path: #/items/properties/type</i>
		 - The value is restricted to the following: 
			 1. _"inline"_
			 2. _"file"_
			 3. _"url"_
	 - <b id="#/items/properties/command">command</b>
		 - Type: `array`
		 - <i id="#/items/properties/command">path: #/items/properties/command</i>
			 - **_Items_**
			 - Type: `string`
			 - <i id="#/items/properties/command/items">path: #/items/properties/command/items</i>
			 - Example values: 
				 1. _"/tmp/post_onboard_script.sh"_
				 2. _"https://the-delivery-location.com/remote_post_onboard.sh"_
	 - <b id="#/items/properties/verifyTls">verifyTls</b>
		 - _For enabling secure site verification_
		 - Type: `boolean`
		 - <i id="#/items/properties/verifyTls">path: #/items/properties/verifyTls</i>
		 - Example values: 
			 1. _true_
			 2. _false_


### post_onboard_enabled: Configuration Examples

```yaml
inline:
  description: Runs commands specified inline
  post_onboard_enabled:
    - name: example_inline_command
      type: inline
      commands:
        - touch /tmp/post_onboard_script.sh
        - chmod 777 /tmp/post_onboard_script.sh
        - >-
          echo "touch /tmp/create_by_autogenerated_post_local" >
          /tmp/post_onboard_script.sh
local_exec:
  description: Runs commands from a local file
  post_onboard_enabled:
    - name: example_local_exec
      type: file
      commands:
        - /tmp/post_onboard_script.sh
remote_exec:
  description: Runs commands from a URL
  post_onboard_enabled:
    - name: example_remote_exec
      type: url
      commands:
        - 'https://the-delivery-location.com/remote_post_onboard.sh'

```
***
### pre_onboard_enabled: Schema

_Used to specify commands which will be executed before extension package operations._

Type: `array`

<i id="#">path: #</i>

 - **_Items_**
 - Type: `object`
 - <i id="#/items">path: #/items</i>
 - **_Properties_**
	 - <b id="#/items/properties/name">name</b>
		 - Type: `string`
		 - <i id="#/items/properties/name">path: #/items/properties/name</i>
		 - Example values: 
			 1. _"my_preonboard_command"_
			 2. _"example_local_exec"_
			 3. _"provision_rest"_
	 - <b id="#/items/properties/type">type</b>
		 - Type: `string`
		 - <i id="#/items/properties/type">path: #/items/properties/type</i>
		 - The value is restricted to the following: 
			 1. _"inline"_
			 2. _"file"_
			 3. _"url"_
	 - <b id="#/items/properties/command">command</b>
		 - Type: `array`
		 - <i id="#/items/properties/command">path: #/items/properties/command</i>
			 - **_Items_**
			 - Type: `string`
			 - <i id="#/items/properties/command/items">path: #/items/properties/command/items</i>
			 - Example values: 
				 1. _"/usr/bin/setdb provision.extramb 500"_
				 2. _"/usr/bin/setdb restjavad.useextramb true"_
				 3. _"/tmp/pre_onboard_script.sh"_
	 - <b id="#/items/properties/verifyTls">verifyTls</b>
		 - _For enabling secure site verification_
		 - Type: `boolean`
		 - <i id="#/items/properties/verifyTls">path: #/items/properties/verifyTls</i>
		 - Example values: 
			 1. _true_
			 2. _false_


### pre_onboard_enabled: Configuration Examples

```yaml
inline:
  description: >-
    Runs commands specified inline. For improved performance, F5 recommends
    including pre_onboard commands to increase provisioning of the REST
    framework, and to pre-provision the ASM module when deploying WAF.
  pre_onboard_enabled:
    - name: example_inline_command
      type: inline
      commands:
        - touch /tmp/pre_onboard_script.sh
        - chmod 777 /tmp/pre_onboard_script.sh
        - >-
          echo "touch /tmp/create_by_autogenerated_pre_local" >
          /tmp/pre_onboard_script.sh
        - /usr/bin/setdb provision.extramb 500
        - /usr/bin/setdb restjavad.useextramb true
        - 'echo ''sys provision asm { level nominal }'' >> bigip_base.conf'
local_exec:
  description: Runs commands from a local file
  pre_onboard_enabled:
    - name: example_local_exec
      type: file
      commands:
        - /tmp/pre_onboard_script.sh
remote_exec:
  description: Runs commands from a URL
  pre_onboard_enabled:
    - name: example_remote_exec
      type: url
      commands:
        - 'https://the-delivery-location.com/remote_pre_onboard.sh'

```
***
### extension_packages: Schema

_Used to specify Automation Toolchain packages to be installed on device._

Type: `object`

<i id="#">path: #</i>

This schema accepts additional properties.

**_Properties_**

 - <b id="#/properties/install_operations">install_operations</b> `required`
	 - _Specify the type, version, location, and endpoint of packages to install_
	 - Type: `array`
	 - <i id="#/properties/install_operations">path: #/properties/install_operations</i>
		 - **_Items_**
		 - Type: `object`
		 - <i id="#/properties/install_operations/items">path: #/properties/install_operations/items</i>
		 - This schema accepts additional properties.
		 - **_Properties_**
			 - <b id="#/properties/install_operations/items/properties/extensionType">extensionType</b> `required`
				 - Type: `string`
				 - <i id="#/properties/install_operations/items/properties/extensionType">path: #/properties/install_operations/items/properties/extensionType</i>
				 - The value is restricted to the following: 
					 1. _"do"_
					 2. _"as3"_
					 3. _"ts"_
					 4. _"cf"_
					 5. _"ilx"_
			 - <b id="#/properties/install_operations/items/properties/extensionVersion">extensionVersion</b>
				 - Type: `string`
				 - <i id="#/properties/install_operations/items/properties/extensionVersion">path: #/properties/install_operations/items/properties/extensionVersion</i>
				 - Example values: 
					 1. _"1.12.0"_
					 2. _"3.19.1"_
			 - <b id="#/properties/install_operations/items/properties/extensionHash">extensionHash</b>
				 - Type: `string`
				 - <i id="#/properties/install_operations/items/properties/extensionHash">path: #/properties/install_operations/items/properties/extensionHash</i>
				 - Example values: 
					 1. _"ba2db6e1c57d2ce6f0ca20876c820555ffc38dd0a714952b4266c4daf959d987"_
					 2. _"95c2b76fb598bbc36fb93a2808f2e90e6c50f7723d27504f3eb2c2850de1f9e1"_
			 - <b id="#/properties/install_operations/items/properties/verifyTls">verifyTls</b>
				 - _For enabling secure site verification_
				 - Type: `boolean`
				 - <i id="#/properties/install_operations/items/properties/verifyTls">path: #/properties/install_operations/items/properties/verifyTls</i>
				 - Example values: 
					 1. _true_
					 2. _false_
			 - <b id="#/properties/install_operations/items/properties/extensionUrl">extensionUrl</b>
				 - Type: `string`
				 - <i id="#/properties/install_operations/items/properties/extensionUrl">path: #/properties/install_operations/items/properties/extensionUrl</i>
				 - Example values: 
					 1. _"https://github.com/F5Networks/f5-appsvcs-extension/releases/download/v3.20.0/f5-appsvcs-3.20.0-3.noarch.rpm"_
					 2. _"file:///var/lib/cloud/icontrollx_installs/f5-declarative-onboarding-1.10.0-2.noarch.rpm"_
				 - The value must match this pattern: `^(https?|file)://[^\s$.?#].[^\s]+\.(rpm)$`
			 - <b id="#/properties/install_operations/items/properties/extensionVerificationEndpoint">extensionVerificationEndpoint</b>
				 - Type: `string`
				 - <i id="#/properties/install_operations/items/properties/extensionVerificationEndpoint">path: #/properties/install_operations/items/properties/extensionVerificationEndpoint</i>
				 - Example values: 
					 1. _"/mgmt/shared/myIlxApp/info"_


### extension_packages: Configuration Examples

```yaml
default:
  description: Installs packages from metadata using latest versions
  extension_packages:
    install_operations:
      - extensionType: do
      - extensionType: as3
versioned:
  description: Installs packages using specific versions
  extension_packages:
    install_operations:
      - extensionType: do
        extensionVersion: 1.12.0
      - extensionType: as3
        extensionVersion: 3.19.1
hashed:
  description: Verifies and installs packages using specified hashes
  extension_packages:
    install_operations:
      - extensionType: do
        extensionVersion: 1.12.0
        extensionHash: 95c2b76fb598bbc36fb93a2808f2e90e6c50f7723d27504f3eb2c2850de1f9e1
      - extensionType: as3
        extensionVersion: 3.19.1
        extensionHash: 4477f84d0be2fa8fb551109a237768c365c4d17b44b2665e4eb096f2cfd3c4f1
url:
  description: Installs packages from custom locations
  extension_packages:
    install_operations:
      - extensionType: do
        extensionVersion: 1.10.0
        extensionUrl: >-
          https://github.com/F5Networks/f5-declarative-onboarding/releases/download/v1.10.0/f5-declarative-onboarding-1.10.0-1.noarch.rpm
      - extensionType: as3
        extensionVersion: 3.20.0
        extensionUrl: >-
          file:///var/lib/cloud/icontrollx_installs/f5-appsvcs-3.20.0-3.noarch.rpm
ilx:
  description: Installs a custom iLX package
  extension_packages:
    install_operations:
      - extensionType: do
        extensionVersion: 1.10.0
      - extensionType: as3
        extensionVersion: 3.20.0
      - extensionType: ilx
        extensionUrl: >-
          file:///var/lib/cloud/icontrollx_installs/f5-appsvcs-templates-1.1.0-1.noarch.rpm
        extensionVersion: 1.1.0
        extensionVerificationEndpoint: /mgmt/shared/fast/info

```
***
### extension_services: Schema

_Used to specify configuration operations to be performed against specific extensions on device._

Type: `object`

<i id="#">path: #</i>

This schema accepts additional properties.

**_Properties_**

 - <b id="#/properties/service_operations">service_operations</b> `required`
	 - _Specify the operations to be performed against the specified services_
	 - Type: `array`
	 - <i id="#/properties/service_operations">path: #/properties/service_operations</i>
		 - **_Items_**
		 - Type: `object`
		 - <i id="#/properties/service_operations/items">path: #/properties/service_operations/items</i>
		 - This schema accepts additional properties.
		 - **_Properties_**
			 - <b id="#/properties/service_operations/items/properties/extensionType">extensionType</b>
				 - Type: `string`
				 - <i id="#/properties/service_operations/items/properties/extensionType">path: #/properties/service_operations/items/properties/extensionType</i>
				 - The value is restricted to the following: 
					 1. _"do"_
					 2. _"as3"_
					 3. _"ts"_
					 4. _"cf"_
			 - <b id="#/properties/service_operations/items/properties/type">type</b>
				 - Type: `string`
				 - <i id="#/properties/service_operations/items/properties/type">path: #/properties/service_operations/items/properties/type</i>
				 - **Comment**<br/>_url in case of local file (file:) or remote (https: or http:) and inline in case the declaration is part of the config file_
				 - The value is restricted to the following: 
					 1. _"url"_
					 2. _"inline"_
			 - <b id="#/properties/service_operations/items/properties/value">value</b>
				 - _URL of local or remote file containing the declarations to be applied, or the entire declaration inline as an object_
				 - <i id="#/properties/service_operations/items/properties/value">path: #/properties/service_operations/items/properties/value</i>
				 - Example values: 
					 1. _"https://cdn.f5.com/product/cloudsolutions/declarations/autoscale-waf/autoscale_do_payg.json"_
					 2. _"file:///examples/declarations/as3.json"_
					 3. _"class: AS3 action: deploy persist: true declaration: class: ADC schemaVersion: 3.0.0 id: urn:uuid:33045210-3ab8-4636-9b2a-c98d22ab915d label: Sample 1 remark: Simple HTTP Service with Round-Robin Load Balancing Sample_01: class: Tenant A1: class: Application template: http serviceMain: class: Service_HTTP virtualAddresses: - 10.0.1.10 pool: web_pool web_pool: class: Pool monitors: - http members: - servicePort: 80 serverAddresses: - 192.0.1.10 - 192.0.1.11"_
			 - <b id="#/properties/service_operations/items/properties/verifyTls">verifyTls</b>
				 - _For enabling secure site verification_
				 - Type: `boolean`
				 - <i id="#/properties/service_operations/items/properties/verifyTls">path: #/properties/service_operations/items/properties/verifyTls</i>
				 - Example values: 
					 1. _true_
					 2. _false_


### extension_services: Configuration Examples

```yaml
url:
  description: Configures services from a URL declaration
  extension_services:
    service_operations:
      - extensionType: do
        type: url
        value: >-
          https://cdn.f5.com/product/cloudsolutions/declarations/autoscale-waf/autoscale_do_payg.json
        verifyTls: false
      - extensionType: as3
        type: url
        value: >-
          https://cdn.f5.com/product/cloudsolutions/templates/f5-azure-arm-templates/examples/modules/bigip/autoscale_as3.json
file:
  description: Configures services from a file declaration
  extension_services:
    service_operations:
      - extensionType: as3
        type: url
        value: 'file:///examples/declarations/as3.json'
inline:
  description: Configures services from an inline declaration
  extension_services:
    service_operations:
      - extensionType: as3
        type: inline
        value:
          class: AS3
          action: deploy
          persist: true
          declaration:
            class: ADC
            schemaVersion: 3.0.0
            id: 'urn:uuid:33045210-3ab8-4636-9b2a-c98d22ab915d'
            label: Sample 1
            remark: Simple HTTP Service with Round-Robin Load Balancing
            Sample_01:
              class: Tenant
              A1:
                class: Application
                template: http
                serviceMain:
                  class: Service_HTTP
                  virtualAddresses:
                    - 10.0.1.10
                  pool: web_pool
                web_pool:
                  class: Pool
                  monitors:
                    - http
                  members:
                    - servicePort: 80
                      serverAddresses:
                        - 192.0.1.10
                        - 192.0.1.11

```
***
### post_hook: Schema

_Details of an HTTP request to send when deployment is finished._

Type: `array`

<i id="#">path: #</i>



### post_hook: Configuration Examples

```yaml
webhook:
  description: Sends webhook payload to specified URL
  post_hook:
    - name: example_webhook
      type: webhook
      url: 'https://webhook.site'
custom_properties:
  description: Sends webhook payload with user-specified custom properties
  post_hook:
    - name: example_webhook
      type: webhook
      url: 'https://webhook.site'
      properties:
        optionalKey1: optional_value1
        optionalKey2: optional_value2

```
***
## Additional Examples

### Automated Toolchain declarations referenced here are available in the examples/declarations folder.

```yaml
example_1:
  description: >-
    Verifies and installs Automation Toolchain components (DO, AS3) on a local
    BIG-IP and then configures AS3 from a local declaration file.
  runtime_config:
    runtime_parameters: []
    pre_onboard_enabled:
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.12.0
          extensionHash: 95c2b76fb598bbc36fb93a2808f2e90e6c50f7723d27504f3eb2c2850de1f9e1
        - extensionType: as3
          extensionVersion: 3.19.1
          extensionHash: 4477f84d0be2fa8fb551109a237768c365c4d17b44b2665e4eb096f2cfd3c4f1
    extension_services:
      service_operations:
        - extensionType: as3
          type: url
          value: 'file:///examples/declarations/as3.json'
example_2:
  description: >-
    Verifies and installs DO and myIlxApp RPMs from local directories and
    configures DO from a local declaration file. Install operations with an
    extensionUrl value that points to a local file may only be installed from
    the /var/lib/cloud or /var/lib/cloud/icontrollx_installs directories.
  runtime_config:
    runtime_parameters: []
    pre_onboard_enabled:
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    extension_packages:
      install_operations:
        - extensionType: do
          extensionUrl: >-
            file:///var/lib/cloud/icontrollx_installs/f5-declarative-onboarding-1.10.0-2.noarch.rpm
          extensionHash: 95c2b76fb598bbc36fb93a2808f2e90e6c50f7723d27504f3eb2c2850de1f9e1
        - extensionType: ilx
          extensionUrl: 'file:///var/lib/cloud/myIlxApp.rpm'
          extensionVersion: 1.1.0
          extensionVerificationEndpoint: /mgmt/shared/myIlxApp/info
          extensionHash: 4477f84d0be2fa8fb551109a237768c365c4d17b44b2665e4eb096f2cfd3c4f1
    extension_services:
      service_operations:
        - extensionType: do
          type: url
          value: 'file:///var/lib/cloud/do.json'
example_3:
  description: >-
    Installs DO and AS3 on a local BIG-IP and renders the Azure service
    principal secret into an AS3 declaration downloaded from a URL.
  runtime_config:
    runtime_parameters:
      - name: AZURE_SERVICE_PRINCIPAL
        type: secret
        secretProvider:
          type: KeyVault
          environment: azure
          vaultUrl: 'https://my-keyvault.vault.azure.net'
          secretId: my_azure_secret
    pre_onboard_enabled:
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.12.0
        - extensionType: as3
          extensionVersion: 3.19.1
    extension_services:
      service_operations:
        - extensionType: do
          type: url
          value: >-
            https://cdn.f5.com/product/cloudsolutions/templates/f5-azure-arm-templates/examples/modules/bigip/autoscale_do.json
        - extensionType: as3
          type: url
          value: 'file:///examples/declarations/example_3_as3.json'
example_4:
  description: >-
    Renders secret referenced within DO declaration to configure the admin
    password on a BIG-IP device in AWS.
  runtime_config:
    runtime_parameters:
      - name: ADMIN_PASS
        type: secret
        secretProvider:
          type: SecretManager
          environment: aws
          version: AWSCURRENT
          secretId: test-document-01
      - name: ROOT_PASS
        type: secret
        secretProvider:
          type: SecretManager
          environment: aws
          version: AWSCURRENT
          secretId: test-document-02
    pre_onboard_enabled:
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.12.0
        - extensionType: as3
          extensionVersion: 3.19.1
    extension_services:
      service_operations:
        - extensionType: do
          type: url
          value: 'file:///examples/declarations/example_4_do.json'
example_5:
  description: >-
    Renders secret referenced within DO declaration to configure the admin
    password on a BIG-IP device in GCP.
  runtime_config:
    runtime_parameters:
      - name: ADMIN_PASS
        type: secret
        secretProvider:
          type: SecretsManager
          environment: gcp
          version: latest
          secretId: my-secret-id-01
      - name: ROOT_PASS
        type: secret
        secretProvider:
          type: SecretsManager
          environment: gcp
          version: latest
          secretId: my-secret-id-02
    pre_onboard_enabled:
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.12.0
        - extensionType: as3
          extensionVersion: 3.19.1
    extension_services:
      service_operations:
        - extensionType: do
          type: url
          value: 'file:///examples/declarations/example_5_do.json'
example_6:
  description: >-
    Replaces variables used within DO declaration with properties from instance
    metadata to configure hostname and self IP addresses on BIGIP device.
  runtime_config:
    runtime_parameters:
      - name: HOST_NAME
        type: metadata
        metadataProvider:
          environment: aws
          type: compute
          field: hostname
      - name: SELF_IP_EXTERNAL
        type: metadata
        metadataProvider:
          environment: aws
          type: network
          field: local-ipv4s
          index: 1
      - name: SELF_IP_INTERNAL
        type: metadata
        metadataProvider:
          environment: aws
          type: network
          field: local-ipv4s
          index: 2
      - name: DEFAULT_ROUTE
        type: metadata
        metadataProvider:
          environment: aws
          type: network
          field: subnet-ipv4-cidr-block
          index: 1
    pre_onboard_enabled:
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.12.0
        - extensionType: as3
          extensionVersion: 3.19.1
    extension_services:
      service_operations:
        - extensionType: do
          type: url
          value: 'file:///examples/declarations/example_6_do.json'
example_7:
  description: Installs AS3 and DO and uses an inline AS3 declaration to setup the BIG-IP.
  runtime_config:
    runtime_parameters: []
    pre_onboard_enabled:
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.5.0
        - extensionType: as3
          extensionVersion: 3.13.0
    extension_services:
      service_operations:
        - extensionType: as3
          type: inline
          value:
            class: AS3
            action: deploy
            persist: true
            declaration:
              class: ADC
              schemaVersion: 3.0.0
              id: 'urn:uuid:33045210-3ab8-4636-9b2a-c98d22ab915d'
              label: Sample 1
              remark: Simple HTTP Service with Round-Robin Load Balancing
              Sample_01:
                class: Tenant
                A1:
                  class: Application
                  template: http
                  serviceMain:
                    class: Service_HTTP
                    virtualAddresses:
                      - 10.0.1.10
                    pool: web_pool
                  web_pool:
                    class: Pool
                    monitors:
                      - http
                    members:
                      - servicePort: 80
                        serverAddresses:
                          - 192.0.1.10
                          - 192.0.1.11
example_8:
  description: Using runtime parameters with inline Automation Toolchain declarations.
  runtime_config:
    runtime_parameters:
      - name: SCHEMA_VERSION
        type: static
        value: 3.0.0
      - name: HOST_NAME
        type: static
        value: bigip1.example.com
    pre_onboard_enabled:
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.5.0
        - extensionType: as3
          extensionVersion: 3.13.0
    extension_services:
      service_operations:
        - extensionType: do
          type: inline
          value:
            schemaVersion: '{{{ SCHEMA_VERSION }}}'
            class: Device
            async: true
            label: my BIG-IP declaration for declarative onboarding
            Common:
              class: Tenant
              hostname: '{{{ HOST_NAME }}}'
              myDns:
                class: DNS
                nameServers:
                  - 8.8.8.8
              myNtp:
                class: NTP
                servers:
                  - 0.pool.ntp.org
                timezone: UTC
              myProvisioning:
                class: Provision
                ltm: nominal
                asm: nominal
              dbvars:
                class: DbVariables
                provision.extramb: 500
                restjavad.useextramb: true
        - extensionType: as3
          type: inline
          value:
            class: AS3
            action: deploy
            persist: true
            declaration:
              class: ADC
              schemaVersion: '{{{ SCHEMA_VERSION }}}'
              label: Sample 1
              remark: Simple HTTP Service with Round-Robin Load Balancing
              Sample_01:
                class: Tenant
                A1:
                  class: Application
                  template: http
                  serviceMain:
                    class: Service_HTTP
                    virtualAddresses:
                      - 10.0.1.10
                    pool: web_pool
                  web_pool:
                    class: Pool
                    monitors:
                      - http
                    members:
                      - servicePort: 80
                        serverAddresses:
                          - 192.0.1.10
                          - 192.0.1.11
example_9:
  description: Using custom pre-onboard and post-onboard commands.
  runtime_config:
    runtime_parameters: []
    pre_onboard_enabled:
      - name: example_inline_command
        type: inline
        commands:
          - touch /tmp/pre_onboard_script.sh
          - chmod 777 /tmp/pre_onboard_script.sh
          - >-
            echo "touch /tmp/create_by_autogenerated_pre_local" >
            /tmp/pre_onboard_script.sh
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
      - name: example_local_exec
        type: file
        commands:
          - /tmp/pre_onboard_script.sh
      - name: example_remote_exec
        type: url
        commands:
          - 'https://the-delivery-location.com/remote_pre_onboard.sh'
    post_onboard_enabled:
      - name: example_inline_command
        type: inline
        commands:
          - touch /tmp/post_onboard_script.sh
          - chmod 777 /tmp/post_onboard_script.sh
          - >-
            echo "touch /tmp/create_by_autogenerated_post_local" >
            /tmp/post_onboard_script.sh
      - name: example_local_exec
        type: file
        commands:
          - /tmp/post_onboard_script.sh
      - name: example_remote_exec
        type: url
        commands:
          - 'https://the-delivery-location.com/remote_post_onboard.sh'
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.12.0
        - extensionType: as3
          extensionVersion: 3.19.1
    extension_services:
      service_operations: []
example_10:
  description: Sending a customized webhook on completion.
  runtime_config:
    runtime_parameters: []
    pre_onboard_enabled:
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.12.0
        - extensionType: as3
          extensionVersion: 3.19.1
    extension_services:
      service_operations: []
    post_onboard_enabled: []
    post_hook:
      - name: example_webhook
        type: webhook
        url: 'https://webhook.site'
        verifyTls: true
        properties:
          optionalKey1: optional_value1
          optionalKey2: optional_value2
example_11:
  description: >-
    Overrides default certificate validation/verification using the verifyTls
    parameter. The following attributes support verifyTls: pre_onboard_enabled,
    post_onboard_enabled, extension_packages.install_operations,
    extension_services.service_operations, and post_hook.
  runtime_config:
    runtime_parameters: []
    pre_onboard_enabled:
      - name: example_remote_exec
        type: url
        commands:
          - >-
            https://ak-metadata-package-poc.s3.amazonaws.com/remote_pre_onboard.sh
      - name: provision_rest
        type: inline
        commands:
          - /usr/bin/setdb provision.extramb 500
          - /usr/bin/setdb restjavad.useextramb true
    post_onboard_enabled:
      - name: example_inline_command
        type: inline
        commands:
          - touch /tmp/post_onboard_script.sh
          - chmod 777 /tmp/post_onboard_script.sh
          - >-
            echo "touch /tmp/created_by_autogenerated_post_local" >
            /tmp/post_onboard_script.sh
      - name: example_local_exec
        type: file
        commands:
          - /tmp/post_onboard_script.sh
      - name: example_remote_exec
        type: url
        commands:
          - >-
            https://ak-metadata-package-poc.s3.amazonaws.com/remote_post_onboard.sh
        verifyTls: false
      - name: example_remote_exec
        type: url
        commands:
          - >-
            https://ak-metadata-package-poc.s3.amazonaws.com/remote_post_onboard.sh
    extension_packages:
      install_operations:
        - extensionType: do
          extensionVersion: 1.10.0
        - extensionType: as3
          extensionVersion: 3.20.0
          verifyTls: false
          extensionUrl: >-
            https://github.com/F5Networks/f5-appsvcs-extension/releases/download/v3.20.0/f5-appsvcs-3.20.0-3.noarch.rpm
          extensionHash: ba2db6e1c57d2ce6f0ca20876c820555ffc38dd0a714952b4266c4daf959d987
        - extensionType: ilx
          extensionUrl: >-
            file:///var/lib/cloud/icontrollx_installs/f5-appsvcs-templates-1.1.0-1.noarch.rpm
          extensionVersion: 1.1.0
          extensionVerificationEndpoint: /mgmt/shared/fast/info
    extension_services:
      service_operations:
        - extensionType: do
          type: url
          value: >-
            https://cdn.f5.com/product/cloudsolutions/declarations/autoscale-waf/autoscale_do_payg.json
          verifyTls: false
        - extensionType: as3
          type: url
          value: >-
            https://cdn.f5.com/product/cloudsolutions/templates/f5-azure-arm-templates/examples/modules/bigip/autoscale_as3.json
    post_hook:
      - name: example_webhook
        type: webhook
        verifyTls: false
        url: 'https://postman-echo.com/post'
        properties:
          optionalKey1: optional_value1
          optionalKey2: optional_value2

```