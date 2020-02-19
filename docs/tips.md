## Connect to an FTP server hosted inside XQEMU
Writing files to your Xbox's HDD is currently a pain. It will be easy once the
fatx driver has write capabilities, but until then you have the option of
connecting over FTP to a server hosted inside XQEMU.

Run XQEMU as normal, but add option to forward to port 21 inside VM as follows:

	-net nic,model=nvnet -net user,hostfwd=tcp:127.0.0.1:1021-:21

It's assumed you're using Filezilla. If not, look for the respective settings in
your client. Fire up Filezilla and set the following options:

* Filezilla > Settings > Connection > FTP > Active mode
  * Active mode IP, "Use the following IP address:" 10.0.2.2
  * Make sure "Don't use external IP address on local connections." is NOT checked

Then create your new "Site" with IP address 127.0.0.1 and port 1021. Under Transfer Settings, check Active.

### Details for the curious
FTP has two modes, passive and active.

Passive mode involves the client first connecting to the server for control and
then again on another port other for data. The client first connects, then the
server gives it another address and port to connect to for data. The first
problem with this is that the server reports to the client the only IP address
that it knows (10.0.2.15) which is not correct for our needs--we need 127.0.0.1.
That's alright because we can override this in Filezilla. The second problem is
that we don't know which port the server is going to choose, so we can't forward
it ahead of time.

In active mode, the client first connects to the server, then the server
connects to the client! The client needs to give the IP address for the server
to connect to. By default, it will give your computer's IP address, but the
guest cannot connect to using that IP. So instead we need to override this
setting and provide the IP address that the guest should connect to to actually
connect to the host, which is 10.0.2.2.
