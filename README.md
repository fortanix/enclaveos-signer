# enclaveos-signer
enclaveos-singer is an independent utitiliy to sign IBM Cloud Data Shield convertered containers with your Intel enclave production signing key. Please follow code-signing guidelines/rituals from your organization while using this tool.

# Prerequisite 
You need to get a production enclave signing key from Intel to use this tool. Follow the instructon below to submit a request to intel. 
https://software.intel.com/en-us/sgx/request-license
Also, you would have to assing ISVSVN and ISVPRODID paramaters for your applicaiton. Intel SGX documentation provides more information on these fields.
https://software.intel.com/en-us/blogs/2016/12/20/overview-of-an-intel-software-guard-extensions-enclave-life-cycle

# Installation
The application requires python3 environment on your system. You should install pip3 package manager 

`sudo apt-get -y install python3-pip`

Install the dependecies for enclaveos-signer

`pip3  install -r requirements.txt`

# Production signing Flow
enclaveos-signer works with IBM Cloud Data Shield converted container images. you can use the tool to sign a converterd container by providing inputs container image and enclave signing keys to cmdline. Please use the tool help to get the details on the flow.

`chmod +x enclaveos-signer`
`./enclaveos-sginer -h`

Below is a sample production signing request:

`./enclaveos-signer --container <registery>/converter-app-sgx-output <registery>/app-sgx-production --isvsvn <version num> --isvprodid <produt id>  --production -key <path to signing key>`
