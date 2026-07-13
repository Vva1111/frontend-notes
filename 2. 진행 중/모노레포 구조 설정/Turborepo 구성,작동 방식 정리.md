
/root
- turbo.json 
	- build: "dependsOn": ["^build"], "outputs": ["dist/**"]
	
		- **app1**
			- packages.json    **<-- root 에서 참조**
				- dev: "vite --host",
				- build : "run-p type-check \"build-only {@}\" --" 
				- ....

		- **app2**
			- packages.json    **<-- root 에서 참조**
				- dev: "vite --host",
				- build : "run-p type-check \"build-only {@}\" --"
				- ...