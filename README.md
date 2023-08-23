# conan-anchore-example
This repo has an example of how Syft can generate an SBOM for a C/C++ based code base that uses Conan as the package manager to build software. The code in this repo produces a simple Conan build executable that prints out "Hello world" when run. We will covers how to generate an SBOM for this Conan Hello World app and how to import that SBOM into Anchore Enterprise. Once the SBOM is in Anchore Enterprise, we can do things like manage the SBOMs (e.g., group them into an Application/versions of an Application) and do things like identify vulnerabilities within those SBOMs

#How to get this Conan build to work and an SBOM generated into Anchore Enterprise
Note this example only has 3 [requires] packages listed: json-c/0.17, openssl/3.0.5, libcurl/7.85.0. You can modify the conanfile.txt to include additional Conan packages in the build

1. Install Conan:
   pip install conan

2. Clone this repo locally
   git clone: https://github.com/connorwynveen/conan-anchore-example.git

3. cd into the directory
   cd conan-anchore-example

4. Run this command to install the dependencies from the conanfile.txt
   conan install .

5. Create a conan.lock file. NOTE: you can skip this step because the repo already has the conan.lock file in it. When I tried to generate this file manually I discovered that v0.5 of the conan.lock file was not compatible with Syft's detection, so I generated a v0.4 version of the lockfile, which is what is in this repo
   conan lock create conanfile.txt --lockfile-out=conan.lock

6. Generate an SBOM using Syft with the following command. NOTE: make sure you are still at the parent folder conan-anchore-example:
   syft . -o json > conan-sbom.json

7. Upload the SBOM into Anchore Enterprise using anchorectl. For information on how to configure anchorectl if you have not done so already, see here: https://docs.anchore.com/current/docs/deployment/anchorectl/
   anchorectl source add -a conan@v1 cwynveen/local/build@v1 --from ./conan-sbom.json

8. Now you can view the SBOM in the Applications tab of the Anchore Enterprise UI. If you click on the source code SBOM digest and navigate to the Vulnerabilities tab you can see if any of our Hello World Conan app have known vulnerabilities. NOTE: Vulnerability matches will be related to CVE's in the NVD that match with the CPE's created for the packages in the SBOM we created
