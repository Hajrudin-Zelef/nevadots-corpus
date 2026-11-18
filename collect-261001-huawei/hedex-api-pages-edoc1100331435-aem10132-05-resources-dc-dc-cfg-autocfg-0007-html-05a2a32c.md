If the FTP server is used, the FTP server IP address must be the same as the value of Option 143 configured on the DHCP server. If the TFTP server is used, the TFTP server IP address must be the same as the value of Option 150 configured on the DHCP server. If the SFTP server is used, the SFTP server IP address must be the same as the value of Option 149 configured on the DHCP server.
The SFTP server is recommended.
The file server can be the router or a PC. In the following example, a router functions as an SFTP server.
Currently, the device supports only password authentication for file access through SFTP.
The interface view is displayed.
The IP address of the SFTP server is configured.
After the file server is configured, place the intermediate file (optional), system software (optional), patch file (optional), and configuration file (mandatory) to the working directory of the file server.
When uploading files, ensure that there is sufficient space in the directory.
If a PC functions as the file server, copy files to the working directory of the PC (working directory of the file server needs to be specified).
If the router functions as the file server, upload files to the working directory of the file server using a file client program.