# enclaveos-signer
enclaveos-signer is an independent utility to sign IBM Cloud Data Shield converted containers with your Intel enclave production signing key. Please follow code-signing guidelines/rituals from your organization while using this tool.

# Prerequisite 
Request a production enclave signing key from Intel to sign production enclaves. Follow the instruction below to submit a request to Intel. 

https://software.intel.com/en-us/sgx/request-license

Also, you would have to assign ISVSVN and ISVPRODID parameters for your application. Intel SGX documentation provides more information on these fields.

https://software.intel.com/en-us/blogs/2016/12/20/overview-of-an-intel-software-guard-extensions-enclave-life-cycle

# Installation
The application requires a python3 environment 'on your system. You should install pip3 package manager 

`sudo apt-get -y install python3-pip`

Install the dependencies for enclaveos-signer

`pip3  install -r requirements.txt`

# Production signing Flow
enclaveos-signer works with IBM Cloud Data Shield converted container images. you can use the tool to sign a converted container by providing inputs container image and enclave signing keys to cmdline. Please use the tool help to get the details on the flow.

`chmod +x enclaveos-signer`

`./enclaveos-signer -h`

Application can be signed with debug keys keys for running in SGX debug mode. Below more details from Intel on debug and production enclaves:

https://software.intel.com/en-us/blogs/2016/01/07/intel-sgx-debug-production-prelease-whats-the-difference

During the signing process we generate Enclave Signatures of the application. Enclave signature generation is describe the Intel SGX documentation here:

https://software.intel.com/en-us/node/702979

Production enclaves require valid Intel enclave signing keys enforced by CPU and IAS remote attestation. Please use debug enclaves if you need  just testing the enclave-signer flow with your converted applications.

Here is brief on enclave-signer input parameter :
 - The Enclave Author’s Public Key - This can be production or debug key used for enclave signatures.
 - The Security Version Number of the Enclave (ISVSVN) – The enclave author assigns a Security Version Number (SVN) to each version of an enclave. 
 - The Product ID of the Enclave (ISVPRODID) - The enclave author also assigns a Product ID to each enclave.

Sample signing request for debug enclaves:

You can generate a debug signig keys (RSA private key size 3072-bit)

`openssl genrsa -3 -out private_rsa_key.pem 3072`

`./enclaveos-signer --container <registery>/converter-app-sgx-output <registery>/app-sgx-production --isvsvn <version num> --isvprodid <produt id> -key <path to signing key>`

Sample signing request for production enclave:

`./enclaveos-signer --container <registery>/converter-app-sgx-output <registery>/app-sgx-production --isvsvn <version num> --isvprodid <produt id>  --production -key <path to signing key>`

# Signing for SGX1 and SGX2 layouts

SGX2 introduces new features to SGX that permit more flexible management of memory assigned to SGX enclaves. One of the things made possible by SGX2 is reducing the initial amount of memory assigned to the enclave while running applications with EnclaveOS.

https://software.intel.com/content/www/us/en/develop/download/intel-sgx-support-for-dynamic-memory-management-inside-an-enclave.html

To take advantage of memory savings with SGX2, on SGX2 systems, the enclave will be constructed with a smaller amount of initial memory. Additional memory will be added to the enclave as needed as the application runs. SGX1 systems will not be able to run enclaves constructed with the SGX2 memory layout, because they will not have adequate memory to run. Each of these memory layouts requires a separate SGX enclave signature. When applications are converted, two signatures are generated, one for the SGX1 enclave layout and one for the SGX2 enclave layout. SGX2 systems are capable of running enclaves with the SGX1 memory layout, but will not get the benefit of decreased memory usage.

When re-signing an enclave with your own signing keys, you will have the choice of signing for just the SGX1 memory layout, just the SGX2 memory layout, or both. Signing both layouts allows the best layout to be used based on what hardware is available, but does require generating two different signatures. If you only have SGX1-only hardware, or only have SGX2 hardware, then you may sign for just the type of hardware that you have. Containers with just an SGX1 signature will run on SGX2 hardware, but won't take advantage of SGX2 features. Containers signed only with the SGX2 layout will not run on SGX1-only hardware.

The --memory-model option to enclaveos-signer controls which types of signatures are generated. The possible options are:
--memory-model=sgx1 generate only the signature for the SGX1 memory layout
--memory-model=sgx2 generate only the signature for the SGX2 memory layout
--memory-model=all  generate signatures for both SGX1 and SGX2 memory layouts

When only the SGX1 or only the SGX2 signature is produced, the other signature will be removed from the container. Once the signature has been removed, it cannot later be regenerated.

The default behavior is currently --memory-model=sgx1, for compatibility with older versions of the enclaveos-signer tool, which only produced signatures for the SGX1-compatible memory layout. It is recommended that you add an explicit --memory-model options to your workflow with the desired memory layout, as the default will likely change in a future release.