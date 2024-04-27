

sudo cp velero /usr/local/bin


backup dir 


/home/ali_khalid/backups

velero snapshot-location create default --provider /home/ali_khalid/backups



velero backup create chkk-nginx-backup --selector run=chkk-test-nginx

