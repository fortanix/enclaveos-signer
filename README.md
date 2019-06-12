# enclaveos-signer
enclaveos-singer is an independent utitiliy to sign Datashield convertered containers with your production keys. Please follow code-signing guidelines and rituals from your organization while using this tool.

# Prerequisite 
You need to get a production enclave signing key from Intel to use this tool.  Follow the instructon below to submit a request to intel. 
https://software.intel.com/en-us/sgx/request-license

# Installation
The application requires python3 environment on your system.
You can install pip3 package manager 
`sudo apt-get -y install python3-pip`
Install the dependecies for enclaveos-signer
`pip3  install -r requirements.txt
`
# Signing Flow
enclave-os signer works with Fortanix converted container images. you can use the tool to sing a converterd container by providing inputs container image and enclave signing keys to cmdline. Please use the tool help to get the details on the flow.
`chmod +x enclaveos-signer`
`./enclaveos-sginer -h`

