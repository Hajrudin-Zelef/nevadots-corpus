An MSDP peer relationship is identified by the addresses of the local and remote MSDP peers. You must create an MSDP peer relationship on both the local and remote ends.
The system view is displayed.
The MSDP view is displayed.
MSDP peers are created.
peer-address: specifies the address of the remote MSDP peer.
interface-type interface-number: specifies the local interface connected to the remote MSDP peer.
The description of a remote MSDP peer is added.
This configuration helps to differentiate remote MSDP peers and manage the connections to the remote MSDP peers.
The interval at which MSDP peers retry to set up a connection with each other is set.
A TCP connection needs to be quickly established between MSDP peers in one of the following situations:
You can run this command to adjust the interval at which MSDP peers retry to set up a connection.
To improve the security of a TCP connection, MSDP supports two authentication modes: message digest algorithm 5 (MD5) and keychain. MD5 authentication and keychain authentication are mutually exclusive on an MSDP peer. You must configure the same password on both ends in MD5 authentication or configure the same encryption algorithm and password on both ends in keychain authentication. Otherwise, the TCP connection cannot be set up.
MD5 is not a secure authentication algorithm. To ensure security, you are advised to use the more secure Keychain algorithm for MSDP authentication.
Run peer peer-address password { cipher cipher-password | simple simple-password }
MSDP MD5 authentication is configured.
If simple is selected, the password is saved in the configuration file in plain text. This brings security risks. It is recommended that you select cipher to save the password in cipher text.
Run peer peer-address keychain keychain-name
MSDP keychain authentication is configured.
keychain-name in this command is defined in the keychain command. For details, see "Keychain Configuration" in the NetEngine AR600, AR6100, AR6200, and AR6300 Configuration Guide-Security.
The session with the remote MSDP peer is closed.
After the session with the remote MSDP peer is closed, SA messages are not exchanged between the MSDP peers. The configuration, however, is saved. You can run the undo shutdown peer-address command to set up a session with the remote MSDP peer and to reestablish a TCP connection.