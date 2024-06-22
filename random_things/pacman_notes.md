# pacman (Arch linux package manager)
|command|description|
|-------|-----------|
|pacman -S &lt;package_name&gt;|install package with dependencies from official repo|
|pacman -S &lt;package_name&gt; -w|jut download a package with dependencies but not install it!!!|
|pacman -S "bash>=3.2"|install specific version of package (quotes are needed!!!)|
|pacman -Syu|update system|
|pacman -Q|list installed packages|
|pacman -Q &lt;package_name&gt; -c|changelog of package|
|pacman -Q &lt;package_name&gt; -d|dependencies of package|
|pacman -Q &lt;package_name&gt; -i|detailed information of package|
|pacman -Q &lt;package_name&gt; -l|all files owned by a package|
|pacman -Q &lt;random_file&gt; -o|show the package name that a random file is owned by (which package a random file is belonging to...)|
|pacman -Q &lt;package_name&gt; -| of package|
