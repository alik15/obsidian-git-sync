first set up your freebsd server to have the following enabled
1) static ip on freebsd


3) ssh on your ip to allow remote access(optional)
	- Create  a user
	- add a line to /etc/rc.conf

—-------------------------------

[https://techviewleo.com/install-prometheus-with-node-exporter-grafana-on-freebsd/](https://techviewleo.com/install-prometheus-with-node-exporter-grafana-on-freebsd/)


Set static ip

Allow sshd on root

[https://www.dark-hamster.com/operating-system/how-to-allow-root-account-to-connect-via-ssh-in-freebsd/](https://www.dark-hamster.com/operating-system/how-to-allow-root-account-to-connect-via-ssh-in-freebsd/)

Insatall pkg

Install prometheus, grafana and node_exportery




data is only available when i run prometheus manually using the commmand
prometheus but not when i put it in rc.conf file

deleted 
/var/db/prometheus and it works???




setting up dnsmasq
https://www.server-world.info/en/note?os=FreeBSD_14&p=dnsmasq&f=1