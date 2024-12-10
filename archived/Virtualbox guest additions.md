


Update Machine
```bash
sudo apt get update
sudo apt get upgrade
```

Download dependencies
```bash
sudo apt install build-essential dkms
```

Install virtualbox guest addition package
```bash
sudo apt-get install virtualbox-guest-additions-iso
```



open your file manager and press CTRL + L
this will take you to your search bar where you can enter a path
enter the following path

`/usr/share/virtualbox/`

now you can mount the `VBoxGuestAdditions.iso` file
it will show up on the right dock
open it, open terminal at that location and finally you can run the following

```bash
sudo ./VBoxLinuxAdditions.run
```


once it completes simply restart

dont forget to turn on shared clipboard and bidirectional copy and paste