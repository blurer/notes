# Useful Linux Commands

* check status of mdadm RAID
  * `cat /proc/mdstat`
* mount smb share on Linux
  * `mount -t cifs -o username=USERNAME,password=PASSWORD,uid=UID,gid=GID //HOST/SHARE /local/mount/point`

#### Docker
* Start container on a port and link with another
  * `docker run --name mediawiki -p 8080:80 --link mediawiki-mysql:mysql -d mediawiki`
* List running containers
  * `docker ps`
* Start a conainter on a port
  * `docker run --name mediawiki -p 8080:80 -d mediawiki`
* Kill a container
  * `docker kill 233fce5bcd21`
* Copy files to a container
  * `docker cp LocalSettings.php mediawiki:/var/www/html`
* List resource usage of running containers
  * `docker cp LocalSettings.php mediawiki:/var/www/html`

