# Useful Linux Commands

#### Misc

* check status of mdadm RAID
  * `cat /proc/mdstat`
* mount smb share on Linux
  * `mount -t cifs -o username=USERNAME,password=PASSWORD,uid=UID,gid=GID //HOST/SHARE /local/mount/point`
* backup mysql db
  * `mysqldump -h localhost -u rt -p --default-character-set=binary wiki > backup.sql`
* restore mysql db
  * `mysql -u root -p my_wiki < backup.sql`

#### Docker

* start container on a port and link with another
  * `docker run --name mediawiki -p 8080:80 --link mediawiki-mysql:mysql -d mediawiki`
* list running containers
  * `docker ps`
* start a conainter on a port
  * `docker run --name mediawiki -p 8080:80 -d mediawiki`
* kill a container
  * `docker kill 233fce5bcd21`
* copy files to a container
  * `docker cp LocalSettings.php mediawiki:/var/www/html`
* list resource usage of running containers
  * `docker cp LocalSettings.php mediawiki:/var/www/html`
* launch a bash shell in a conatiner
  * `docker exec -it mediawiki-mysql bash`

