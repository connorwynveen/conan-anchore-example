# conan-anchore-example
This repo has an example of how Syft can generate an SBOM for a C/C++ based code base that uses Conan as the package manager to build software. It will covers how to import the SBOM into Anchore Enterprise. This allows you to manage the SBOMs and do things like identify vulnerabilities within those SBOMs

#How to get this Conan build to work and an SBOM generated into Anchore Enterprise

1. Install Conan:

pip install conan


2. Create a conanfile.txt - this will list your projects dependencies


3. Create a CMakeLists.txt file and copy in this content:


4. <Not sure if this is necessary but doesn't hurt> Create a hello.c file and copy in this content
5. 

6. Run this command to install the dependencies from the conanfile.txt

conan install .


6. Generate an SBOM using Syft with the following command:

syft . -o json > conan-sbom.json


7. Upload the SBOM into Anchore Enterprise using anchorectl

anchorectl source add -a conan@v1 cwynveen/local/build@v1 --from ./conan-sbom.json

