# Openscap

Openscap reminder for ubuntu

http://static.open-scap.org/
Liste des profils de scan disponible :
https://static.open-scap.org/ssg-guides/ssg-ubuntu2204-guide-index.html
Manuel utilisateur :
https://static.open-scap.org/openscap-1.2/oscap_user_manual.html 

# Install Base

https://www.open-scap.org/tools/openscap-base/

apt-get install libopenscap8

# Install security guide 

http://www.open-scap.org/security-policies/scap-security-guide/#install

# Official version (ubuntu 20.04 and before)

apt install ssg-base ssg-debderived ssg-debian ssg-nondebian ssg-applications

# Installation from official repository (ubuntu 22.04 and later / also available for other)

Download latest release from repo https://github.com/ComplianceAsCode/content

cd /tmp

wget https://github.com/ComplianceAsCode/content/releases/download/v0.1.67/scap-security-guide-0.1.67.zip

unzip scap-security-guide-0.1.67.zip

#List vailable profiles
oscap info "/usr/share/xml/scap/ssg/content/ssg-rhel6-xccdf.xml"


# Pass the Bench – Generate a report

oscap xccdf eval --report report.html --profile xccdf_org.ssgproject.content_profile_cis_level1_server scap-security-guide-0.1.67/ssg-ubuntu2204-ds.xml --results scan-xccdf-results.xml

# Remediate after reporting

oscap xccdf remediate --results scan-xccdf-results.xml scan-xccdf-results.xml

# Generate a script for later remediation 

oscap xccdf generate fix --template urn:xccdf:fix:script:sh --profile xccdf_org.ssgproject.content_profile_cis_level1_server --output my-remediation-script.sh scap-security-guide-0.1.67/ssg-ubuntu2204-ds.xml

# Vulnerabilities Scan (OVAL)

https://ubuntu.com/security/oval

wget https://security-metadata.canonical.com/oval/com.ubuntu.$(lsb_release -cs).usn.oval.xml.bz2

bunzip2 com.ubuntu.$(lsb_release -cs).usn.oval.xml.bz2

oscap oval eval --report report.html com.ubuntu.$(lsb_release -cs).usn.oval.xml

# Docker Image Scanning

#Add extra scanner

sudo apt install openscap-utils

sudo apt install python3-openscap

sudo apt install openscap-scanner

# Doc : 
https://manpages.ubuntu.com/manpages/lunar/man8/oscap-docker.8.html

oscap-docker image-cve IMAGE_NAME --results oval-results-file.xml --report report.html

oscap-docker container-cve CONTAINER_NAME --results oval-results-file.xml --report report.html

# Dirty Debian 11 Version 

wget http://ftp.us.debian.org/debian/pool/main/o/openscap/libopenscap8_1.3.4-1_amd64.deb
wget http://ftp.us.debian.org/debian/pool/main/s/scap-security-guide/ssg-base_0.1.62-2_all.deb
wget http://ftp.us.debian.org/debian/pool/main/s/scap-security-guide/ssg-debian_0.1.62-2_all.deb

# Test for missing dependecies
apt -s install ./libopenscap8_1.3.4-1_amd64.deb
apt install -s ./ssg-base_0.1.62-2_all.deb
apt install -s ./ssg-debian_0.1.62-2_all.deb

# Install if everythin is ok
apt install ./libopenscap8_1.3.4-1_amd64.deb
apt install ./ssg-base_0.1.62-2_all.deb
apt install ./ssg-debian_0.1.62-2_all.deb


