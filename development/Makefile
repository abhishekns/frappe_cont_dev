# Default setting for the project

PYENV_VERSION:=3.10.5
NVM_DEFAULT:=v14
FRAPPE_BRANCH_VERSION:=version-14
SKIP_REDIS_CONFIG_GEN=--skip-redis-config-generation
SITE:=dev.localhost
MARIADB_PASSWD=123
ADMIN_PASSWD=admin
NVM:=. ${HOME}/.nvm/nvm.sh && nvm 



setup:
	git config --global --add safe.directory /workspace
	$(NVM) use $(NVM_DEFAULT)
	bench init $(SKIP_REDIS_CONFIG_GEN) --frappe-branch $(FRAPPE_BRANCH_VERSION) frappe-bench 
	cd frappe-bench
	bench set-mariadb-host mariadb
	bench set-redis-cache-host redis://redis-cache:6379
	bench set-redis-queue-host redis://redis-queue:6379
	bench set-redis-socketio-host redis://redis-socketio:6379
	./env/bin/pip install honcho
	sed -i '/redis/d' ./Procfile

create-site:
	bench new-site $(SITE) --mariadb-root-password $(MARIADB_PASSWD) --admin-password $(ADMIN_PASSWD) --no-mariadb-socket
	bench --site $(SITE) set-config developer_mode 1
	bench --site $(SITE) clear-cache

install-app:
	# --branch is optional, use it to point to branch on custom app repository
	bench get-app --branch version-12 https://github.com/myusername/myapp
	bench --site development.localhost install-app myapp

