# Gitlab Migration to Newer Version
1. Take a snapshot of the current and the fresh gitlab machines.
2. Take backup from gitlab in the form of a tar archive.
3. In the new machine, clone the old version of the [Sameersbn project](https://github.com/sameersbn/docker-gitlab/blob/13.8.2/README.md) (the one currently being used.
4. Copy the contents of the old gitlab’s docker-compose.yml file into the new machine’s docker-compose.yml file.
5. Change the default ssh port to another port. Change the firewall and selinux configuration accordingly. [Reference](https://kifarunix.com/how-to-configure-ssh-to-use-a-different-port-on-centos-7/).
6. Run the docker-compose.yml file to run all three containers.
7. "scp" the tar backup in /srv/docker/gitlab/gitlab/backups
8. Using following command drop in the gitlab's container. <br />
<code> docker exec -it <containername> bash </code>
9. Go to the backups directory in the container and change the owner for the backups' Tar file.<br/>
<code> chown git:git <backup.tar> </code>
10. Exit the container and change to the docker-compose.yml file's directory.
11. Run the backup restore command.<br/>
<code> docker-compose run --rm gitlab app:rake gitlab:backup:restore BACKUP=<Backup-file-name></code>
