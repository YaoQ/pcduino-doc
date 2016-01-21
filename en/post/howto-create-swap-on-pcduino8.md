# How to create Swap on pcDuino8 Uno

When I use `pip` command to install `scipy` on pcDuino8 Uno, after about one or two hours to compile, then you get errors seemly like:
> g++: internal compiler error: Killed (program cc1plus)  
Please submit a full bug report,  
with preprocessed source if appropriate.  

Because pcDuino8 Uno has only 1GB ram which is not enough to compile `scipy`.At the same time I use `free -m` to check the ram and swap, it shows that the swap is 0. So if I add a swap partition, this error will be solved. 
```bash
free -m
sudo dd if=/dev/zero of=/var/swap.img bs=1M count=1000
sudo mkswap /var/swap.img
sudo swapon /var/swap.img
free -m
```
This creates a new swap about 1GB.

Then I run `sudo pip install scipy`, after several hours, the installation will be done!

After installation is done, you can run the following commands to remove the swap partition.
```bash
sudo swapoff /var/swap.img
sudo rm /var/swap.img
```
Good luck!


