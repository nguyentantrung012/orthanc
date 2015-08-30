

# Contributing to Orthanc #

If you find Orthanc useful and wish to contribute to its development, here are some tasks you can take in charge that would greatly help us:
  * Use Orthanc in the real life. ;)
  * Report possible problems together with sample DICOM images on the issue tracker.
  * Answer questions posted to the [mailing list](https://groups.google.com/forum/#!forum/orthanc-users).
  * Index external contributions on the ["Orthanc Contributed" GitHub repository](https://github.com/jodogne/OrthancContributed), via pull requests.
  * Provide documentation and use cases (e.g. on GitHub).
  * Share maintenance scripts (e.g. on GitHub).
  * Advertise about Orthanc, and [answer the survey](http://www.orthanc-server.com/static.php?page=blog#survey).
  * Package Orthanc and its associated plugins for more UNIX or GNU/Linux distributions (e.g. RHEL, CentOS, SUSE...). In particular, help would be welcome on upgrading the [Fedora package](https://admin.fedoraproject.org/updates/orthanc).
  * Improve the [Wikipedia page](https://en.wikipedia.org/wiki/Orthanc_(software)) about Orthanc.
  * Interface Orthanc with other software (e.g. 3D Slicer, Matlab/Octave, Python, Horos, dicompyler...). Check the [already supported frameworks](http://www.orthanc-server.com/static.php?page=resources).
  * Develop [C/C++ plugins](https://github.com/jodogne/OrthancContributed/tree/master/Plugins), some ideas of which can be found [in the official roadmap](https://trello.com/b/cjA9X1wM/orthanc-roadmap).

Currently, the Orthanc core is under heavy refactoring until 1.0.0 release. As a consequence, we cannot accept external code contributions for the moment. If the current plugin SDK is insufficient for you to develop some feature as a plugin, do not hesitate to ask an extension of the SDK. To put it in other words, **the recommended way of contributing to the Orthanc source code is by creating C/C++ plugins**.

# Troubleshooting #

  * Always make sure you use the [most recent version](http://www.orthanc-server.com/download.php) of Orthanc.
  * **I cannot login to Orthanc Explorer**: For security reasons, access to Orthanc from remote hosts is disabled by default. Only the localhost is allowed to access Orthanc. You have to set the `RemoteAccessAllowed` option in the [configuration file](OrthancConfiguration.md) to `true`. It is then strongly advised to set `AuthenticationEnabled` to `true` and to add a user to the `RegisteredUsers` option, also in the configuration file.
  * **Orthanc Explorer is slow under Windows on the localhost**: You have to disable the IPv6 support. This is a Windows-specific problem that is discussed [here](http://superuser.com/questions/43823/google-chrome-is-slow-to-localhost) and [here](http://stackoverflow.com/questions/1726585/firefox-and-chrome-slow-on-localhost-known-fix-doesnt-work-on-windows-7).
  * Under Windows, Orthanc creates the "`OrthancStorage`" folder, and crashes with the error "**SQLite: Unable to open the database**": Your directory name is either too long, or it contains special characters. Please try and run Orthanc in a folder with a simple name such as "`C:\Orthanc`".
  * Orthanc does not compile with **Microsoft Visual Studio 2012**. Aaron Boxer has fixed the build: Apply [patch 1](https://groups.google.com/forum/?hl=fr#!topic/orthanc-users/M7sLKLitp7E) and [patch 2](https://groups.google.com/forum/?hl=fr#!topic/orthanc-users/kbYj_o-8gp4).

# Troubleshooting DICOM communications #

In general, communication problems between two DICOM modalities over a computer network are related to the configuration of these modalities. As preliminary debugging actions, you should:

  * Make sure you use the [most recent version](http://www.orthanc-server.com/download.php) of Orthanc.
  * Make sure the two computers can "ping" each other.
  * Turn off all the firewalls on the two computers (especially on Microsoft Windows).
  * Write down on a paper the following information about each modality:
    * its IP address (avoid using symbolic names if possible to troubleshot any DNS problem),
    * its TCP port for DICOM communications (for Orthanc, cf. the `DicomPort` option), and
    * its AET (Application Entity Title - for Orthanc, cf. the `DicomAet` option).
  * Carefully re-read all your configuration files. As far as Orthanc is concerned, the most important section is `DicomModalities`: Make sure its content matches what you wrote on the paper at the step above.
  * In the `DicomModalities` configuration section of Orthanc, have a look at the fourth parameter that can activate some patches for specific vendors.
  * Have a look at the following options of Orthanc to enable the more fault-tolerant DICOM support:
    * `DicomServerEnabled` must be set to `true`.
    * `DicomCheckCalledAet` should be set to `false`.
    * All the transfer syntaxes should be set to `true` (see the options with a `TransferSyntaxAccepted` suffix).
    * Temporarily disable any Lua script and any plugin, i.e. set the options `LuaScripts` and `Plugins` both to the empty list.
  * Restart Orthanc with the `--verbose` option at command line, and carefully inspect the log. This might provide immediate debugging information.
  * Issue a DICOM C-Echo from each modality to make sure the DICOM protocol is properly configured (sending a C-Echo from Orthanc Explorer is possible starting with Orthanc 0.9.3, in the "Query/Retrieve" page).

If the two modalities succeed with C-Echo, but the transfers do not succeed, please contact the [mailing list](https://groups.google.com/forum/#!forum/orthanc-users) by sending a detailed description of your problem, notably:

  * What fails: The sending of a file (aka. C-Store SCU), the searching of a patient/study (aka. C-Find SCU), or the retrieve of a file (aka. C-Move SCU)? Is Orthanc acting as a client or as a server?
  * Describe your network topology, as written above on your paper (IP address, port number, and AET for both modalities).
  * Specify the operating system, the vendor, the DICOM software, and the version of each modality.
  * Attach sample DICOM files, possibly anonymized.
  * Attach the log of the two modalities. The log must be generated with the "`--trace`" command-line option as far as Orthanc is concerned.
  * Attach any screenshot that is useful to understand the problem.

# Please explain the version numbering scheme of Orthanc #

Each release of Orthanc is identified by a version that is made of three parts: `API.MAJOR.MINOR` (e.g. 0.5.1).

  * `API` (currently, 0) changes when the REST API is refactored.
  * `MAJOR` changes when a new major feature is introduced (either in the REST API or in the DICOM support), when an incompatibility in the database schema is introduced, or when an important refactoring is achieved.
  * `MINOR` changes after each introduction of a minor feature, after a bugfix, or after a speed or GUI improvement. It also changes when an experimental feature is introduced.

# I use the Linux distribution XXX, how can I build Orthanc? #

  * **Orthanc >= 0.7.1**: See the instructions [inside the source package](https://bitbucket.org/sjodogne/orthanc/src/default/LinuxCompilation.txt).
  * **Orthanc <= 0.7.0**: See [this Wiki page](LinuxCompilationUpTo070.md).

# I use Mac OS X, how can I build Orthanc? #

The mainline of Orthanc can compile under Apple OS X, with the XCode compiler, since June 24th, 2014. Build instructions [are available](https://bitbucket.org/sjodogne/orthanc/src/default/DarwinCompilation.txt).

_Older answers:_
  * Cían Hughes has created a [fork of the Orthanc mainline](https://code.google.com/r/cian-orthanc-mac/) that is tested on Mavericks (10.9), using the [Homebrew](http://brew.sh/) package manager.
  * Ryan Walklin has created a [fork of Orthanc 0.5.2](https://code.google.com/r/ryanwalklin-mac/) that compiles and runs under Mac OS X.

# What are the Recycling/Protection/Compression Features? #

Because of its focus on low-end computers, Orthanc implements **disk space recycling**: The oldest series of images can be automatically deleted when the available disk space drops below a threshold, or when the number of stored series grows above a threshold. This enables the automated control of the disk space.

Recycling is controlled by the `MaximumStorageSize` and the `MaximumPatientCount` options in the [Orthanc configuration file](OrthancConfiguration.md).

It is possible to prevent important data from being automatically recycled. This mechanism is called **protection**. Each patient, each study and each series can be individually protected against recycling by using the `Unprotected/Protected` switch that is available in Orthanc Explorer.

If your disk space is limited, you should also consider using **disk space compression**. When compression is enabled, Orthanc compresses the incoming DICOM instances on-the-fly before writing them to the filesystem, using [zlib](http://en.wikipedia.org/wiki/Zlib). This is useful, because the images are often stored as raw,
uncompressed arrays in DICOM instances: The size of a typical DICOM
instance can hopefully be divided by a factor 2 through lossless
compression. This compression process is transparent to the user, as
Orthanc automatically decompresses the file before sending it back to
the external world.

Compression is controlled by the `StorageCompression` option in the [Orthanc configuration file](OrthancConfiguration.md).


# How can I upgrade the database version? #

Different versions of Orthanc might use a different database version, as explained on [this page](DatabaseVersioning.md). To **upgrade an existing database** for a newer version of Orthanc, you can use this [sample script](https://bitbucket.org/sjodogne/orthanc/src/default/Resources/Samples/ImportDicomFiles/ImportDicomFiles.py). This script will import all the DICOM files of the existing database through the REST API of Orthanc. You just have to apply this script on the place where the older database is stored (presumably in the `OrthancStorage` folder, but this depends on your configuration). For instance:

```
# ImportDicomFiles.py localhost 8042 OrthancStorage
```


# How can I install Orthanc as a Debian/Ubuntu daemon? #

To install Orthanc as a Linux daemon on a Debian/Ubuntu system, you can:
  1. [Download this service script](http://anonscm.debian.org/viewvc/debian-med/trunk/packages/orthanc/trunk/debian/orthanc.init?view=markup) (this file is part of the official [Debian package of Orthanc](http://debian-med.alioth.debian.org/tasks/imaging#orthanc)),
  1. Adapt some of its variables to reflect the configuration of your system,
  1. Copy it in `/etc/init.d` as root (the filename cannot contain dot, otherwise it is not executed), make it belong to root, and tag it as executable:
```
# sudo mv orthanc.init /etc/init.d/orthanc
# sudo chown root:root /etc/init.d/orthanc
# sudo chmod 755 /etc/init.d/orthanc
```
  1. If you wish the daemon to be automatically launched at boot time and stopped at shutdown:
```
# sudo update-rc.d orthanc defaults
```
  1. If you wish to remove the automatic launching at boot time later on:
```
# sudo update-rc.d -f orthanc remove
```

_Note:_ You can use `rcconf` to easily monitor the services that are run at startup (`sudo apt-get install rcconf`).


# How can I access an Orthanc instance that is running inside a Fedora/RHEL/CentOS box? #

For remote access to Orthanc, you will have to open the 4242 and the 8042 ports on `iptables`:

```
# sudo iptables -I INPUT -p tcp --dport 8042 -j ACCEPT
# sudo iptables -I INPUT -p tcp --dport 4242 -j ACCEPT
# sudo iptables-save 
```


# How can I run Orthanc behind Apache? #

It is possible to make Orthanc run behind Apache through a [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy). To map the REST API of an Orthanc server listening on the port 8000 on the URI `/Orthanc`, paste the following code in your `/etc/apache2/httpd.conf`:

```
LoadModule proxy_module /usr/lib/apache2/modules/mod_proxy.so
LoadModule proxy_http_module /usr/lib/apache2/modules/mod_proxy_http.so
ProxyRequests On
ProxyPass /Orthanc/ http://localhost:8000/ retry=0
```

_Note:_ These instructions are for Ubuntu 11.10. You perhaps have to adapt the absolute paths above to your distribution.

# How can I run Orthanc behind nginx? #

Similarly to Apache, [Orthanc is reported](https://groups.google.com/d/msg/orthanc-users/oTMCM6kElfw/uj0r062mptoJ) to be able to run as a [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy) behind nginx. Here is the configuration snippet for nginx:

```
server{
       listen  80  default_server;
       ...
       location  /orthanc/  {
                proxy_pass http://localhost:8042;
		proxy_set_header HOST $host;
                proxy_set_header X-Real-IP $remote_addr;
                rewrite /orthanc(.*) $1 break;
        }
        ...
}
```

_Note:_ Thanks to Qaler for submitting this information!


# How can I enable CORS with nginx? #

This question extends the previous answer. Here is the configuration snippet:

```
server{
       listen  80  default_server;
       ...
       location  /orthanc/  {
                proxy_pass http://localhost:8042;
                proxy_set_header HOST $host;
                proxy_set_header X-Real-IP $remote_addr;
                rewrite /orthanc(.*) $1 break;
                add_header 'Access-Control-Allow-Credentials' 'true';
                add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
                add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
                add_header 'Access-Control-Allow-Origin' '*';
        }
        ...
}
```

_Note:_ Thanks to Fernando for [submitting this information](https://groups.google.com/d/msg/orthanc-users/LH-ej_fB-dw/CmWP4jM3BgAJ)!


# How can I use HTTPS (SSL encryption) with Orthanc? #

You need to obtain a [X.509 certificate](http://en.wikipedia.org/wiki/X.509) in the [PEM format](http://en.wikipedia.org/wiki/X.509#Certificate_filename_extensions), and prepend its content with your private key. You can then modify the `SslEnabled` and `SslCertificate` variables in the [Orthanc configuration file](OrthancConfiguration.md).

Here are simple instructions to create a self-signed SSL certificate that is suitable for test environments with the [OpenSSL](http://en.wikipedia.org/wiki/Openssl) command-line tools :

```
# openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
# cat private.key certificate.crt > certificate.pem
```

Some interesting references about this topic can be found [here](http://devsec.org/info/ssl-cert.html), [here](http://www.akadia.com/services/ssh_test_certificate.html), and [here](http://stackoverflow.com/questions/991758/how-to-get-an-openssl-pem-file-from-key-and-crt-files).

# Orthanc Fails to Store a DICOM File, Complaining About an "Inexistent Tag" #

If you receive the `"Exception while storing DICOM: Inexistent tag"` error while [storing a DICOM instance into Orthanc](StoringInstances.md), please make sure that all the 4 following tags do exist in the DICOM file:

  * `PatientID` (0010,0020),
  * `StudyInstanceUID` (0020,000d),
  * `SeriesInstanceUID` (0020,000e),
  * `SOPInstanceUID` (0008,0018).

These tags are all used to index the incoming DICOM instances. In Orthanc, each patient, study, series and instance is indeed assigned with an unique identifier that is computed as the [SHA-1 hash](http://en.wikipedia.org/wiki/Sha-1) of a combination of these four tags.

The error message is more explicit starting with Orthanc 0.7.4.

_Note:_ The actual implementation of the hashing is carried on by the [DicomInstanceHasher class](https://bitbucket.org/sjodogne/orthanc/src/default/Core/DicomFormat/DicomInstanceHasher.cpp).

# Orthanc Does not Compile, Complaining About Missing `uuid-dev` Package #

This problem seems to occur when fist building Orthanc without the `uuid-dev` package installed, then installing `uuid-dev`, then rebuilding Orthanc. It seems that the build scripts do not update the cached variable about the presence of `uuid-dev`.

To solve this problem, as reported by [Peter Somlo](https://groups.google.com/d/msg/orthanc-users/hQYulBBvJvs/S1Pm125o59gJ), it is necessary to entirely remove the build directory (e.g. with `rm -rf Build`) and start again the build from a fresh directory.


# How Can I Run a C-Find SCU Query (experimental)? #

An _experimental_ C-Find SCU support is implemented in Orthanc. This is a three-step API:

  1. Retrieve the PatientID:
```
# curl http://localhost:8042/modalities/pacs/find-patient -X POST -d '{"PatientName":"JOD*","PatientSex":"M"}'
```
  1. Retrieve the studies of this patient (using the "PatientID" returned from Step 1):
```
# curl http://localhost:8042/modalities/pacs/find-study -X POST -d '{"PatientID":"0555643F"}'
```
  1. Retrieve the series of one study (using the "PatientID" from Step 1, and the "StudyInstanceUID" from Step 2):
```
# curl http://localhost:8042/modalities/pacs/find-series -X POST -d '{"PatientID":"0555643F","StudyInstanceUID":"1.2.840.113704.1.111.2768.1239195678.57"}' 
```

You will have to define the modality "pacs" in the [configuration file](OrthancConfiguration.md) of Orthanc (under the section `DicomModalities`).

Please however note that this unstable API will most likely change in
future versions of Orthanc.

# Using Orthanc to Ease WADO Querying #

As of Orthanc 0.6.1, it will be possible to use Orthanc to easily gather the three identifiers that are required to run a [WADO query](ftp://medical.nema.org/medical/dicom/2006/06_18pu.pdf) against a remote modality (without storing the files inside Orthanc). These identifiers are:

  * `StudyInstanceUID` (0020,000d),
  * `SeriesInstanceUID` (0020,000e),
  * `ObjectUID`, that exactly corresponds to the `SOPInstanceUID` tag (0008,0018) (cf. the [WADO specification](ftp://medical.nema.org/medical/dicom/2006/06_18pu.pdf), Section 8.1.4).

The trick consists in using the experimental C-Find SCU API, going down to the instance level:

```
# curl http://localhost:8042/modalities/pacs/find-patient -X POST -d '{"PatientName":"JOD*","PatientSex":"M"}'
# curl http://localhost:8042/modalities/pacs/find-study -X POST -d '{"PatientID":"0555643F"}'
# curl http://localhost:8042/modalities/pacs/find-series -X POST -d '{"PatientID":"0555643F","StudyInstanceUID":"1.2.840.113704.1.111.2768.1239195678.57"}' 
# curl http://localhost:8042/modalities/pacs/find-instance -X POST -d '{"PatientID":"0555643F","StudyInstanceUID":"1.2.840.113704.1.111.2768.1239195678.57","SeriesInstanceUID":"1.3.46.670589.28.2.7.2200939417.2.13493.0.1239199523"}'
```

The first three steps are described in [this FAQ entry](https://code.google.com/p/orthanc/wiki/FAQ#How_Can_I_Run_a_C-Find_SCU_Query_(experimental)?). The fourth step retrieves the list of the instances of the series. The latter query was not possible until Orthanc 0.6.1.

As a result of this sequence of four commands, the `StudyInstanceUID`, `SeriesInstanceUID` and `SOPInstanceUID` are readily available for each instance of the series.

_Warning:_ The C-Find SCU API is still experimental and might change in future versions of Orthanc.

# Configuring DICOM Query/Retrieve #

Starting with release 0.7.0, Orthanc supports DICOM Query/Retrieve (i.e. Orthanc can act as C-Find SCP and C-Move SCP). To configure this feature, follow these instructions:

  * Get the AET (Application Entity Title), the IP address and the port number of your DICOM client.
  * Add an entry in the `DicomModalities` section of your Orthanc [configuration file](OrthancConfiguration.md) to reflect these settings.
  * Restart Orthanc with the updated configuration file.
  * Symmetrically, in your DICOM client, configure an additional DICOM node corresponding to your Orthanc AET (see the `DicomAet` parameter of your Orthanc configuration, by default, `ORTHANC`), IP address et port number (cf. `DicomPort`, by default 4242).

# How to Backup Orthanc #

To backup an instance of Orthanc, you need to get a copy of 3 elements:
  1. Your configuration file.
  1. The DICOM files (by default, the `OrthancStorage` subdirectories).
  1. The SQLite index (by default, the `OrthancStorage/index*` files).

Karsten Hilbert provided us with [a sample backup script](https://github.com/jodogne/OrthancContributed/blob/master/Scripts/Backup/2014-01-31-KarstenHilbert.sh) for the official Debian package of Orthanc.

In this script, the call to the SQLite command-line tool is used to force the [WAL replay](http://www.sqlite.org/wal.html). This manual replay will not be necessary for Orthanc >= 0.7.3.

# Supported DICOM images #

Even though Orthanc can receive/store/send any kind of standard DICOM files, its core engine is not able to render all of them as PNG images. An image that Orthanc cannot decode is displayed as "Unsupported" by Orthanc Explorer.

Currently, the core engine of Orthanc can decode uncompressed (raw) DICOM files, JPEG, and JPEG-LS, with either RGB or Grayscale photometric interpretation, and up to 16bpp depth. Multiframe (notably cine), uncompressed DICOM instances can also be displayed from Orthanc Explorer.

Other type of encodings are available in the Orthanc [Web viewer plugin](http://www.orthanc-server.com/static.php?page=web-viewer), that mostly supports whatever is supported by the well-known [GDCM toolkit](http://sourceforge.net/projects/gdcm/) by Mathieu Malaterre. Note that multiframe (notably cine) DICOM instances are currently not supported by the Web viewer plugin. It is also planned to create a [plugin to extend the image formats](https://trello.com/c/MAh6vIXF) that are supported by the Orthanc core.

# What is an Orthanc peer? #

A "peer" is another instance of Orthanc, possibly running on a remote computer.

Contrarily to a "modality", it is possible to communicate with a peer through the HTTP/HTTPS protocol (i.e. through the REST API of Orthanc), and not through the DICOM protocol. This implies a much easier configuration: It is only required to know the URL, the username and the password to communicate with an Orthanc peer. This contrasts with DICOM query/retrieve, that is quite complex and that involves a lot of pitfalls (cf. the FAQ entry about troubleshooting DICOM communications above) that can be bypassed if using HTTP.

Furthermore, as HTTP(S) communications are generally not blocked by firewalls (contrarily to the DICOM protocol that is inherently an Intranet protocol and that often requires the setup of VPN tunnels), it is much easier to setup communications of medical images through the Internet with Orthanc peers.


# Why "Orthanc"? #

The spelling "[Orthanc](http://en.wikipedia.org/wiki/Orthanc#Orthanc)" originates from J.R.R. Tolkien’s work. Orthanc is the black tower of Isengard that houses one of the **palantíri**. A [palantír](http://en.wikipedia.org/wiki/Palant%C3%ADr) is a spherical stone whose name literally means "One that Sees from Afar". When one looks into a palantír, one can communicate with other such stones and anyone who might be looking into them. This is quite similar to the Orthanc server, which is designed for a simple, transparent, programmatic access to medical images in the entire hospital DICOM topology.

"Orthanc" also contains the trigram "RTH", standing for **Radiotherapy**. This is a reference to the fact that Orthanc originates from the research of the [Radiotherapy Service](http://www.chu.ulg.ac.be/jcms/c_13318/radiotherapie) of the [CHU of Liège](http://www.chu.ulg.ac.be/).