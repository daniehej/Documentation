# Installing Anaconda

We will install a version of Anaconda by downloading a specific version of Anaconda, install and the binaries to the Path

```bash
anacondaType=Anaconda3-2021.11-Linux-x86_64.sh
wget -q https://repo.anaconda.com/archive/$anacondaType
chmod +x $anacondaType

./$anacondaType -b -p
sudo rm -f $anacondaType

export PATH="$HOME/anaconda3/bin:$PATH"

conda init bash
```