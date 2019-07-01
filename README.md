# enclaveos-signer
enclaveos-signer is an independent utility to sign IBM Cloud Data Shield converted containers with your Intel enclave production signing key. Please follow code-signing guidelines/rituals from your organization while using this tool.

# Prerequisite 
Request a production enclave signing key from Intel to sign production enclaves. Follow the instruction below to submit a request to Intel. 
https://software.intel.com/en-us/sgx/request-license

Also, you would have to assign ISVSVN and ISVPRODID parameters for your application. Intel SGX documentation provides more information on these fields.
https://software.intel.com/en-us/blogs/2016/12/20/overview-of-an-intel-software-guard-extensions-enclave-life-cycle

# Installation
The application requires python3 environment (3.6 or older) on your system. You should install pip3 package manager 

`sudo apt-get -y install python3-pip`

Install the dependencies for enclaveos-signer

`pip3  install -r requirements.txt`

# Production signing Flow
enclaveos-signer works with IBM Cloud Data Shield converted container images. you can use the tool to sign a converted container by providing inputs container image and enclave signing keys to cmdline. Please use the tool help to get the details on the flow.

`chmod +x enclaveos-signer`

`./enclaveos-signer -h`

Application can be signed with debug keys keys for running in SGX debug mode. Below more details from Intel on debug and production enclaves:

https://software.intel.com/en-us/blogs/2016/01/07/intel-sgx-debug-production-prelease-whats-the-difference

During the signing process we generate an Enclave Signature of the application. Enclave signature generation is describe the Intel SGX documentation here:

https://software.intel.com/en-us/node/702979

Production enclaves require valid Intel enclave signing keys enforced by CPU and IAS remote attestation. Please use debug enclaves if you need  just testing the enclave-signer flow with your converted applications.

Here is brief on enclave-signer input parameter :
 - The Enclave Author’s Public Key - This can be production or debug key used for enclave signatures.
 - The Security Version Number of the Enclave (ISVSVN) – The enclave author assigns a Security Version Number (SVN) to each version of an enclave. 
 - The Product ID of the Enclave (ISVPRODID) - The enclave author also assigns a Product ID to each enclave.

Sample signing request for debug enclaves:

`./enclaveos-signer --container <registery>/converter-app-sgx-output <registery>/app-sgx-production --isvsvn <version num> --isvprodid <produt id> -key <path to signing key>`

Sample signing request for production enclave:

`./enclaveos-signer --container <registery>/converter-app-sgx-output <registery>/app-sgx-production --isvsvn <version num> --isvprodid <produt id>  --production -key <path to signing key>`
