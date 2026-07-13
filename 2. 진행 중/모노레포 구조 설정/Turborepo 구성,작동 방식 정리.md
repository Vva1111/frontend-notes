
/root [build 명령 적용]
	- app1
		- packages.json 
			- dev: "vite --host",
			- build : "run-p type-check \"build-only {@}\" --"
			- ....
	- app2
		- packages.json 
			- dev: "vite --host",
			- build : "run-p type-check \"build-only {@}\" --"