Purpose
------------
We use this version of pistolero's zbx-statsd to send data to zabbix server for monitoring activity in our Django app,
such as page load time, query response time, amount of data updates etc.
Based on (https://github.com/pistolero/zbx-statsd)

Requirements
------------
1. Python
2. Simplejson (http://pypi.python.org/pypi/simplejson/) configured for use with current Python version
3. Zabbix agent, configured on this machine 

Usage
-------------
Instruction for Debian:
Put code into wherever directory you like. (Default is /usr/local/zbxstatsd)
Check that first row in server.py leads to corresponding Python executable. (Default is /usr/local/bin/python)  
Recheck installation directories and change corresponding paths in zabbixstatsd script. 
Put zabbixstatsd file in /etc/init.d/ directory. 
	Register with update-rc.d zabbixstatsd default. 
		Use just as usual init script.
   
Embed in code
-------------
Client:
	from path.to.client import Client

	sc = Client('zabbix-host', 8126, 'zabbix_name_of_this_machine')

	sc.timing('some_meaningful_name.time',500)
	sc.increment('some_meaningful_name.inc_float')
	sc.decrement('some_meaningful_name.decr_float')

#Server: #Seriously, you should consider using a daemon
#	from zbxstatsd import Server
#	srvr = Server(debug=True)
#	srvr.serve()

Zabbix side
------------
If you already have configured host for a machine on which you are doing all this stuff, 
you should add one item for each unique metric name, used in your code. E.g.
	 some_meaningful_name.time
	 some_meaningful_name.inc_float
	 some_meaningful_name.decr_float
Use Type = Zabbix agent (active)(necessary) and Type of information Numeric (float)(recommended). 
Any other configurations are at your discretion.  