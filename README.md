Bacula
======

Configurações do Bacula

Bacula Server

#!/usr/bin/perl -w
#
# Install Bacula Automatically - Parte1
# Tested With Centos 5
# Cleuson de O. Alves
# Current version is bacula 5.0.3

# Depois de instalado: chmod +x <script>

use strict;
use Cwd;
use File::Copy;

###### Bacula Preparation ##########

my $current_path = getcwd;
chdir "/root" unless $current_path =~ ! /\/root$/;
system "yum -y update";
system "yum -y install mysql-server mysql-devel gcc gcc-c++ mtx openssl openssl-devel httpd mod_ssl ncurses ncurses-devel libtermcap-devel libxml2-devel zlib-devel pango-devel atk-devel gtk2-devel libgnomeui-devel ORBit2-devel libart_lgpl-devel libbonobo-devel libbonoboui-devel GConf2-devel freetype-devel";

--------------------------------------------------------------------------------------------------------

#!/usr/bin/perl -w
#
# Install Bacula Automatically
# Tested With Centos 5
# Cleuson de O. Alves
# Current version is bacula 5.0.3
#

use strict;
use Cwd;
use File::Copy;

###### Bacula Preparation ##########
# Depois de instalado: chmod +x <script>

my $current_path = getcwd;
chdir "/root" unless $current_path =~ ! /\/root$/;
system "wget http://sourceforge.net/projects/bacula/files/bacula/5.0.3/bacula-5.0.3.tar.gz/download";
system "tar -zxvf bacula-5.0.3.tar.gz";
rename "/root/bacula-5.0.3" , "/root/bacula";


#### Configure Bacula ######

chdir "/root/bacula" or die "cant chdir to bacula";
system "./configure --with-mysql";
system "make";
system "make install";
system "make install-autostart-fd start";


#### Configure MYSQL ########
chdir "/etc/bacula" or die "cant chdir to bacula";
system "service mysqld start";

print "\n############ Enter Root mysql password if prompted ###########\n";
system "./grant_mysql_privileges -u root -p";
system "./create_mysql_database -u root -p";
system "./make_mysql_tables -u root -p";
system "/etc/bacula/bacula start";

