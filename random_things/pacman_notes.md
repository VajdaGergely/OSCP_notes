# pacman (Arch linux package manager)
## pacman commands
### available packages in the repository (remote packages)
|command|description|
|-------|-----------|
|pacman -Syu|update system|
|pacman -S &lt;package_name&gt;|install package with dependencies from official repo|
|pacman -S "bash>=3.2"|install specific version of package (quotes are needed!!!)|
|pacman -S &lt;package_name&gt; -w|just download a package with dependencies but not install it!!!|
|pacman -S &lt;package_name&gt; -i|just show detailed information of the package but not download and not install it!!|
### installed packages (local packages)
|command|description|
|-------|-----------|
|pacman -Q|list installed packages|
|pacman -Q &lt;package_name&gt; -i|detailed information of a local package|
|pacman -Q &lt;package_name&gt; -c|changelog of package|
|pacman -Q &lt;package_name&gt; -d|dependencies of package|
|pacman -Q &lt;package_name&gt; -l|all files owned by a package|
|pacman -Q &lt;random_file&gt; -o|show the package name that a random file is owned by (which package a random file is belonging to...)|
## default location of downloaded packages
* /etc/pacman.conf -> CacheDir row
  * e.g. /var/cache/pacman/pkg/
