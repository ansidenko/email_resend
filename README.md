
# IBM App Connect Enterprice Resend Email IMAP/POP3 to SMTP via SSL simple application

How to connect to pop3s imaps server (like Gmail) to recieve and resend email with body and attachements wit IBM App Connect Enterprise (formely IBM Integration Bus)
## Pre Installation instructions


1. Import IMAP / POP3 server certifacates in BASE64 format and put files in /distr/certs/root.cer and /distr/certs/root_immediate.cer
2.  Report Java ComIbmJVMManager settings 
```bash
  mqsireportproperties NODEname -у ISname -o ComIbmJVMManager -a
```
"truststoreFile" is a path to cacerts with default trusted certificates

3. Add root and intermediate certificate to cacerts
```bash
  sudo /opt/ibm/appconnect/ace-12.0.0.5/common/jdk/jre/bin/keytool -import -alias root.cer -file /distr/cert/root.cer -keystore "/opt/ibm/appconnect/ace-12.0.0.5/common/jdk/jre/lib/security/cacerts" -storepass changeit
  sudo /opt/ibm/appconnect/ace-12.0.0.5/common/jdk/jre/bin/keytool -import -alias root_immediate.cer -file /distr/cert/root_immediate.cer -keystore "/opt/ibm/appconnect/ace-12.0.0.5/common/jdk/jre/lib/security/cacerts" -storepass changeit

```   
4. Change default truststoreFile settings
```bash
  mqsireportproperties NODEname -у ISname -o ComIbmJVMManager -n truststoreFile -v "/opt/ibm/appconnect/ace-12.0.0.5/common/jdk/jre/lib/security/cacerts"
```
5. Report changed settings
```bash
  mqsireportproperties NODEname -у ISname -o ComIbmJVMManager -a
```
6. Create security user profile for SMTP and IMAP 
```bash
  mqsisetdbparms NODEname email::email_user -u email_user
  mqsisetdbparms NODEname smtp::smtp_user -u smtp_user
```
7. Restart ACE 
```bash
  mqsistop NODEname
  mqsistart NODEname
```
## Import code to ACE

In App Connect Enterprise create new project and inport ZIP source code from Github

Create security policy and security profiles for IMAP and SMTP
```bash
  email_user and smtp_user
```

Deploy project and security policy to Integration server
## Debug

Start user trace for Integration Server and look for errors in file NODEname*Trace.0.txt
based at:

Windows
```bash
  C:\IBM\ACE\workspace\config\common\log
```
or linux
```bash
  /var/mqsi/common/log
```

