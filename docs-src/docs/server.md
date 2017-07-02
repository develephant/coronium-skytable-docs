# System Services

When your __Coronium SkyTable__ instance starts, its monitored by a utility called __Monit__, which makes sure that your instance keeps running. In the event that the server runs into an issue or crashes, it will be restarted shortly.

In the rare case where you need to manually stop or start the instance services, log in using the `skytable` user.

`ssh skytable@<your-instance-ip>`

__redis__

|Action|Command|
|------|-------|
|stop|`sudo monit stop redis`|
|start|`sudo monit start redis`|
|restart|`sudo monit restart redis`|

__nginx__

|Action|Command|
|------|-------|
|stop|`sudo monit stop nginx`|
|start|`sudo monit start nginx`|
|restart|`sudo monit restart nginx`|

_You will be prompted for your password. The default is __coroniumadmin__._

!!! caution
    You should rarely need to manually control the instance services.

---

## Configuration

### Server Key

---

## Viewing Logs

When developing your application, there may be times when you'd like to see what's happening on the server side. You can view the log file to acheive this.

First, log into your instance using the `skytable` user:

`ssh skytable@<your-instance-ip>`

To follow the __redis__ log file, run the following on the command line:

`tail -f /home/skytable/logs/redis.log`

To follow the __nginx__ log file, run the following on the command line:

`tail -f /home/skytable/logs/nginx.log`

!!! note
    The log files are managed automatically, and will be "rotated" once they exceed a certain size limit.
