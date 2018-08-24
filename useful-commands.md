# Useful Linux Commands

#### Shell
* delete text in a file
  * `sed "/TEXT/d" < inputfile`
* append date to file
  * `touch "foo.backup.$(date +%F_%R)"`
    * `foo.backup.2013-10-16_19:25`
* `scp` most recent file
  * ````
    $ RECENT=$(ssh someone@example.com ls -lrt /remote/path/ | awk '/.ubx/ { f=$NF }; END { print f }');`
    $ scp someone@example.com:/remote/path/${RECENT} /local/path/${RECENT};
    ````
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

