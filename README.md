BASM
====

Bash Advanced Symfony Manager

Simple tool to manage your Symfony Project

Install
=======
Mainly clone the github repository in your desired directory

    mkdir -p ~/work/tools/BASM
    git clone https://github.com/jmeyo/BASM ~/work/tools/BASM

Usage
=====
Launch it like a simple script.

For simple use add it to your path, or better, add a alias (in ~/.bash_aliases for example)

    alias sm="~/work/tools/BASM/symfony_manager.sh"

Then use it like :

	$ sm -h // show help
	Commands : symfony_manager.sh OPTIONS
        WHERE OPTIONS
	   -a : Dump bundles assets resources and generate assets
	   -b : Update composer dependencies
	   -c : Clear and setup cache
	   -e <environment_name> : Set symfony environment
	   -f : Do not ask question mo'fo'
	   -g : Launch Behat tests
	   -i : Install a version of the application
	   -k : Check tools and/or install them
	   -l <sm_config_file> : Load config from a spcific file
	   -p <installation_path> : Set an installation path
	   -s : Drop and ReInstall Database
	   -t : Launch Phpunit tests
	   -u <install|update|none>: Update a version of the application with option for the database : 
		 - install: install database from scratch (drop everything first)
		 - update: update the database
		 - none: do not update the database
	   -u reinstall|update|none: Update a version of the application with option for the database : 
	   -v <svn_version> : Set svn tag/version
	   -w : Generate and watch assets
	   -y : Update database
	   -z : Update Symfony Manager
       You can mix those option together


    
    
	$ sm -casf 
	// clear cache, copy assets, install database with fixtures, and force the question to preferable answer 
    
About configuration
===================

Its possible to load a configuration from several places. 

Either through the -f option, to link with a specific file, or if you just want to work on a specific project, just put a "sm-config-<project_name>" file in your home directory (you can prefix it with a dot to hide it). A sample "sm-config-default" can be found in the default BASM directory

First BASM check for the presence of a configuration file passed as a parameter, then it uses the .sm_config file in the home directory, then it looks in the current directory. If nothing is fine, then it just use default values, which should not feed your needs ;)

Check [sm-config-default](https://github.com/jmeyo/BASM/blob/master/sm-config-default) example file

Best Practice
=============

This simple manager gives you the ability to never lose anymore time with annoying symony commands, as it takes care of rights ;)
A good way of using it for several projects might be to define several alias, with different config file and store them in a common directory (for example /home/user/work/config-tools/sm-config). 

With bash, you could add something like that in the ~/.bash_aliases file to parse that directory and add alias for your projects automatically for both the symony manager (starting with sm_ and the <project_name>) and a shortcut to the home of the project (starting by po_ and the <project_name>)

	# sm conf alias loader
	sm_path=/home/user/work/tools/BASM/symfony_manager.sh
	sm_conf_directory=/home/user/work/config-tools/sm-config-default
	# default shortcut for the calling symfony manager without any conf files
	alias sm_='$sm_path'
	for conffile in `ls $sm_conf_directory`; do
		project_name=${conffile/sm-config-}
		project_name=${conffile/sm-config-} 
		alias sm_$project_name="${sm_path} -l $sm_conf_directory/${conffile}"
		project_path=`cat $sm_conf_directory/$conffile | grep application_install_path | sed 's/application_install_path="//g' | sed 's/"//g'`
		alias po_$project_name="cd $project_path"
	done
	
I tend to prefix my configuraiton files with sm-config-<project_name>, but this is not mandatory, and the alias shortcut will be sm_<project_name> <OPTIONS>


Integrate with Composer
=======================

1)Define a repository, as BASM is not yet in Packagist
```
    "repositories": [
    {
        "type": "package",
        "package": {
            "version": "master",
            "name": "jmeyo/BASM",
            "source": {
                "url": "https://github.com/jmeyo/BASM",
                "type": "git",
                "reference": "master"
            },
            "dist": {
                "url": "https://github.com/jmeyo/BASM/zipball/master",
                "type": "zip"
            }
        }
    }
    ],
```  

2) Add the following line to your Symfony2 composer.json file:

	{
		"require": {
			/* other libs */
			"jmeyo/BASM": "dev-master"
		}
	}

3) Update composer
	php /usr/local/bin/composer.phar update

You can know add an alias to the symfony_manager inside your project. It will then be updated hen you update your project with composer.


Improve the Symfony Manager
===========================

I use this script on many of my symfony projects, thus I tend to update it as much as possible. It's still pure bash script, but it's quite stable, and I would love to have any improvement propositions ;)
