This is a Microsoft Word Add-in (2013+) that helps with building
templates for [docassemble] guided interviews.

To download and generate for the first time:

```
git clone https://github.com/GBLS/docassemble-template-builder-addin
cd docassemble-template-builder-addin/
npm i
npm run build
```

This will compile the JavaScript and put it into the `build/static/js`
folder.  It will also create `build/index.html`, which is a straight
copy of `public/index.html`.

To deploy the app on GitHub Pages, do:

```
npm run deploy
```

After a minute or so, the new version of the site will be available at
[https://gbls.github.io/docassemble-template-builder-addin/index.html].
Check the "GitHub Pages" section of the [Settings page] for any
warning about a failure to build.  This can happen randomly; if it
does, just make some change to one of the files and deploy again.

The key TypeScript/React/Javascript file is `src/index.tsx`.  The key
HTML file is `public/index.html`.  The key CSS file is
`build/static/css/app.css`.

The `office_addin_manifest.xml` file can be imported into Office as an
add-in.  This file is also [available as a URL].

## Using a local server

The add-in needs to operate over HTTPS.  If you want to run `npm run
start` in order to use a local web server to host the application, you
need to create certificates.  To create these certificates, follow
these steps:

```
mkdir certs
cd certs
openssl req -new -key server.key -days 3650 -out server.csr
```

The last command will ask you for some information.  Put in some
information like the following.  The important line is the "Common
Name," which should match whatever the server name will be (probably
`localhost`, but this can vary by platform).

```
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:MA
Locality Name (eg, city) []:Boston
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Greater Boston Legal Services
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:localhost
Email Address []:qsteenhuis@gbls.org
```

Next generate the signed certificate:

```
openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
```

Double-click on the generated server.crt. Click "Install Certificate", choose "Local Machine" and then 
choose "Place all certificates in the following store:". Browse for the "Trusted Root Certification Authority" store.

This will install the certificate for both IE and Edge, but not for Chrome or Firefox. This is enough to get the 
add-in to work in Microsoft Word.

In the Configuration of your docassemble server, you will need to set:

```
office addin url: https://localhost:8080
```

(By default, the only server that docassemble will communicate with is
https://gbls.github.io.)

A manifest file that is configured to work with https://localhost:8080
can be found at [`build/office_addin_manifest_local.xml`].

Sometimes you may need to clear the local Office cache to uninstall the add-in.
To do so, remove the contents of this folder: `%LOCALAPPDATA%\Microsoft\Office\16.0\Wef\` (change 16 to 15 if you are running Office 2013)


[Settings page]: https://github.com/GBLS/docassemble-template-builder-addin/settings
[https://gbls.github.io/docassemble-template-builder-addin/index.html]: https://gbls.github.io/docassemble-template-builder-addin/index.html
[docassemble]: https://docassemble.org
[available as a URL]: https://gbls.github.io/docassemble-template-builder-addin/office_addin_manifest.xml
[`build/office_addin_manifest_local.xml`]: https://gbls.github.io/docassemble-template-builder-addin/office_addin_manifest_local.xml
[howto]: https://datacenteroverlords.com/2012/03/01/creating-your-own-ssl-certificate-authority/


