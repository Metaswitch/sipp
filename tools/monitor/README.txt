/*
 *  This program is free software; you can redistribute it and/or modify
 *  it under the terms of the GNU General Public License as published by
 *  the Free Software Foundation; either version 2 of the License, or
 *  (at your option) any later version.
 *
 *  This program is distributed in the hope that it will be useful,
 *  but WITHOUT ANY WARRANTY; without even the implied warranty of
 *  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *  GNU General Public License for more details.
 *
 *  You should have received a copy of the GNU General Public License
 *  along with this program; if not, write to the Free Software
 *  Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307, USA
 *
 *  Author : Juan Antonio Alvarez <jualvarez@gmail.com>
 */
README

These scripts are intended to gather information from a SIP server under test.
An entry in SIPp wiki is available for more details:
http://sipp.sourceforge.net/wiki/index.php/Getting_feedback_from_the_server


The script auto_test runs sipp with a series of parameters configured as BASH 
variables. snmparser goes trough the files generated by snmp runs and builds a 
comma separated values file with the relevant information (i.e. those fields 
that changed over the different runs). fullcsv runs snmparser for all the snmp*
files in the directory and pastes them together with sipp stats output.

REQUIREMENTS

First of all, your SIP server must be running some snmp server.

auto_script needs
- bash
- awk
- netcat
- net-snmp

snmparser is a php script, so it needs php-cli (chmod +x if you want to use it
with fullcsv).

fullcsv is a simple bash script, it uses sed and paste, but that should be
available in most linux boxes.

HOW TO RUN THEM

Make sure your system meets the requirements. Edit auto_script. Modify all the
variables so that they fit your taste, everything is commented so it shouldn't
be a problem. The variable test_name is some custom name for this particular
run. A directory will be created with that name and every stats will be dumped
there.

To start the test, simply run bash auto_script. Note that the script only takes
care of the UAC part. If you intend sipp to run the UAS part as well, you should
run it for yourself. Something like
sipp -sn uas -p <sip_listen_port> -mp <media_port>
should be enough.

If you wish to convert what you've got into something easier to analyze, cd to
test_name directory, and run

bash ../fullcsv

A file named full.csv will be created. It should be really easy to import that
file in any spreadsheet, and even SPSS or R for further analysis.

PLEASE NOTE

All three scripts should be in the same directory. Be sure to edit the path to
sipp in auto_script.

When running fullcsv, take into account that it expects the files from only one
run to reside in the directory. You can know they are all from the same run,
because they have the same ending, that is the unix timestamp of the time the
test was launched.

