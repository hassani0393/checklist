# Gitlab Migration to Newer Version
## Steps
1. Take a snapshot of the current and the fresh gitlab machines.
2. Take backup from gitlab in the form of a tar archive.
3. In the new machine, clone the old version of the [Sameersbn project](https://github.com/sameersbn/docker-gitlab/blob/13.8.2/README.md) (the one currently being used.)
4. Copy the contents of the old gitlab’s docker-compose.yml file into the new machine’s docker-compose.yml file.
5. Change the default SSH port to another one. Change the firewall and selinux configuration accordingly and reboot the system. [Reference](https://kifarunix.com/how-to-configure-ssh-to-use-a-different-port-on-centos-7/)
6. Run the docker-compose.yml file to run all three containers.
7. "scp" the tar backup in /srv/docker/gitlab/gitlab/backups
8. Using following command drop in the gitlab's container. <br />
<code> docker exec -it <containername> bash </code>
9. Go to the backups directory in the container and change the owner for the backups' Tar file.<br/>
<code> chown git:git backup.tar </code>
10. Exit the container and scp the contents of the /srv/docker/gitlab/gitlab/ssh , from the old server to the new server.
11. Change to the docker-compose.yml file's directory.
12. Run the backup restore command.<br/>
<code> docker-compose run --rm gitlab app:rake gitlab:backup:restore BACKUP=Backup-file-name</code>
13. Run docker-compose.yml
14. Edit the docker-compose.yml , only change the versions of the 3 old images to new versions.
15. Run the docker-compose.yml file. <br/>
Voila
---

## FAQ
* Q. Why not upgrade Gitlab first and then restore the backup tar file? <br/>
  A. You will get a version mismatch error during restoration.
* Q. During running of the docker-compose.yml file, Gitlab container stays forever in configuration phase. <br/>
  A. Gitlab container can not connect to other two containers. Check docker network configs in .yml file.
* Q. Restoration of the Tar file is stuck. <br/>
  A. It probably isn't. It is just very slow. I suggest you lock your workstation and leave if for a while.
* Q. When running the docker-compose file, I get an error stating 0.0.0.0:22 is already in use.
  A. Refer to step 5. SSH is using the port used by Gitlab.
