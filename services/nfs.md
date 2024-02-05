# NFS file system
## info
* WebNFS
  * common port: TCP/UDP 2049
* sok fele fajtaja van az NFS-nek
* elvileg Windows-on es Linuxon is futhat NFS server
* mehet HTTP-n is vagy mas modon is
## source of information
* HTB academy - Footprinting module
  * van hozza feladat is, szoval lab-ban le is lehet tesztelni a mukodeset
## scans, enumeration, exploitation
### IMPORTANT !!!!
* nmap scripts are buggy!!!!! Tobbszor meg kell probalni lefuttatni oket!!!!
* es nem eleg sC-vel hanem egyesevel a konkret nevevel, 3-4szer is probaljuk meg
### scanning
* $ sudo nmap -Pn -v 10.10.10.3 -p111,2049 -sV -sC
  * rpcinfo ha le tud futni, akkor hasznos dolgokat ir
  * The rpcinfo NSE script retrieves a list of all currently running RPC services, their names and descriptions, and the ports they use. This lets us check whether the target share is connected to the network on all required ports.
* $ sudo nmap -Pn -v 10.10.10.3 -p111,2049 -sV --script=nfs*
  * Also, for NFS, Nmap has some NSE scripts that can be used for the scans. These can then show us, for example, the contents of the share and its stats.
    * nfs-showmount, nfs-ls, nfs-statfs
  * Once we have discovered such an NFS service, we can mount it on our local machine. For this, we can create a new empty folder to which the NFS share will be mounted. Once mounted, we can navigate it and view the contents just like our local system.
### mounting nfs share to our local file system
* $ showmount -e 10.10.10.3
* $ sudo su -
* $ cd /mnt
* $ mkdir target-NFS
* $ sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
* $ cd target-NFS
* $ tree .
### list contents
* $ ls -l mnt/nfs/ (list contents with usernames and group names)
* $ ls -n mnt/nfs/ (list contents with UIDs and GUIDs)
### unmount nfs share
* $ cd ..
* $ sudo umount ./target-NFS
### further things
* It is important to note that if the root_squash option is set, we cannot edit the files with write permission even as root.
* We can also use NFS for further escalation. For example, if we have access to the system via SSH and want to read files from another folder that a specific user can read, we would need to upload a shell to the NFS share that has the SUID of that user and then run the shell via the SSH user.
* After we have done all the necessary steps and obtained the information we need, we can unmount the NFS share.
