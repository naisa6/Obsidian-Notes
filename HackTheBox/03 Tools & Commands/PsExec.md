PsExec is a portable tool from Microsoft that lets you run processes remotely using any user's credentials. It’s a bit like a remote access program but instead of controlling the computer with a mouse, commands are sent via Command Prompt, without having to manually install client software..

[[Impacket]] creates a remote service by uploading a randomly-named executable on the ADMIN$ share on the remote system and then register it as a Windows service. This will result in having an interactive shell available on the remote Windows system via TCP port 445. Psexec requires credentials for a user with local administrator privileges or higher since reading/writing to the ADMIN$ share is required. Once you successfully authenticate, it will drop you into a NT AUTHORITY\SYSTEM shell.

git clone https://github.com/SecureAuthCorp/impacket.git
cd impacket
pip3 install .
OR:
sudo python3 setup.py install

In case you are missing some modules:
pip3 install -r requirements.txt

psexec.py administrator@\[target host IP]
whoami
