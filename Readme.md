# FREE EPG for EyeTv with perl XMLTV : French TV and free Sat UK TV sample case

## XMLTV require perl and some developper tools and skill, lets go !
### Download and install macport from https://www.macports.org
### Download and install Xcode AND Xcode command line tool from https://developer.apple.com/downloads/ (needs a free developper connection account from apple)

### In a terminal try :
 which make  
### it shall output this :  
 /usr/bin/make  
### configure perl update system to use CPAN
sudo perl -MCPAN -e shell  
 perl> o conf init  
 perl> exit  
sudo perl -MCPAN -e 'install Bundle::CPAN'  

### Install main perl xmltv dependencies :
 sudo perl -MCPAN -e shell  
  perl> install Tk Tk::TableMatrix XML::Parser XML::Twig XML::Writer Date::Manip LWP Memoize Storable HTML::Parser HTML::TreeBuilder SOAP::Lite CGI Term::ProgressBar PerlIO::gzip Compress::Zlib Lingua::Preferred Unicode::String Lingua::EN::Numbers::Ordinate Log::TraceMessages   
  perl> exit  

## Tricky part : the WWW::Mechanize workarround
### perl bundle "WWW::Mechanize" have a bug with Mac os X ipv6 alias.
### in file /etc/hosts, comment IPV6 alias :
 sudo vi /etc/hosts  
#### type ‘i’ to enter vi insert mode and
#### comment ipv6 localhost alias
#### type Escape and ":wq" command to save your change

### Then install WWW::Mechanize perl bundle :
 sudo perl -MCPAN -e shell  
  perl> install WWW::Mechanize   
  perl> exit  

### Finaly revert /etc/hosts to its original content
 sudo vi /etc/hosts   
### uncomment ipv6 localhost alias

### Now continue installing main perl xmltv dependencies :
 sudo perl -MCPAN -e shell  
  perl> install HTML::TableExtract Archive::Zip IO::Scalar   
  perl> exit  

### Now install xmltv grabber specifics dependencies
 sudo perl -MCPAN -e shell  
\# These dependencies are required for tv_grab_uk_rt:  
  perl> install DateTime DateTime::Duration DateTime::TimeZone HTTP::Cache::Transparent  
\# These dependencies are missing for tv_grab_is:  
  perl> install XML::DOM  
\# These dependencies are missing for tv_grab_it_dvb:  
  perl> install Data::Dump Linux::DVB  
\# These dependencies are missing for tv_grab_dk_dr:  
  perl> install JSON  
\# These dependencies are missing for tv_grab_pt:  
  perl> install Unicode::UTF8simple  
\# These dependencies are missing for tv_grab_eu_epgdata:  
  perl> install DateTime::Format::Strptime  
\# These dependencies are missing for tv_grab_fr_kazer:  
  perl> install XML::LibXML File::Slurp  
  perl> exit  

## The developper part, download and build xmltv :  
### install wget tool :  
 sudo port install wget  

### get xmltv using wget, (go to a location where you can compile things) :  
 cd /path/to/where/you/want  
 wget http://downloads.sourceforge.net/project/xmltv/xmltv/0.5.66/xmltv-0.5.66.tar.bz2  
 tar xvjf xmltv-0.5.66.tar.bz2  
 cd xmltv-0.5.66  
 sudo perl Makefile.PL PREFIX=/opt/local  
 make  
 make test  

### make test could fail due to deprecated api usage or stderr unexpected output result from some unit test predicate (this is due to poor designed unit tests, IGNORE IT!)
### install perl xmltv  
 sudo make install  

### shall output things like that :

Installing /opt/local/lib/perl5/site_perl/XMLTV.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Ask.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Capabilities.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Clumps.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Config_file.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Configure.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Date.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Description.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/DST.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Get_nice.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Grab_XML.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Grep.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/GUI.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Gunzip.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/IMDB.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Memoize.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Mode.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Options.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/PreferredMethod.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/ProgressBar.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Summarize.pm   
Installing /opt/local/lib/perl5/site_perl/XMLTV/Supplement.pm   
Installing /opt/local/lib/perl5/site_perl/XMLTV/TZ.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Usage.pm   
Installing /opt/local/lib/perl5/site_perl/XMLTV/ValidateFile.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/ValidateGrabber.pm   
Installing /opt/local/lib/perl5/site_perl/XMLTV/Version.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Ask/Term.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Ask/Tk.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Configure/Writer.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/Data/Recursive/Encode.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/ProgressBar/None.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/ProgressBar/Term.pm  
Installing /opt/local/lib/perl5/site_perl/XMLTV/ProgressBar/Tk.pm  
Installing /opt/local/share/man/man1/tv_augment_tz.1  
Installing /opt/local/share/man/man1/tv_cat.1  
Installing /opt/local/share/man/man1/tv_check.1  
Installing /opt/local/share/man/man1/tv_count.1  
Installing /opt/local/share/man/man1/tv_extractinfo_ar.1  
Installing /opt/local/share/man/man1/tv_extractinfo_en.1  
Installing /opt/local/share/man/man1/tv_find_grabbers.1  
Installing /opt/local/share/man/man1/tv_grab_ar.1  
Installing /opt/local/share/man/man1/tv_grab_ch_search.1 
Installing /opt/local/share/man/man1/tv_grab_combiner.1  
Installing /opt/local/share/man/man1/tv_grab_dtv_la.1  
Installing /opt/local/share/man/man1/tv_grab_es_laguiatv.1  
Installing /opt/local/share/man/man1/tv_grab_eu_egon.1  
Installing /opt/local/share/man/man1/tv_grab_eu_epgdata.1  
Installing /opt/local/share/man/man1/tv_grab_fi.1  
Installing /opt/local/share/man/man1/tv_grab_fi_sv.1  
Installing /opt/local/share/man/man1/tv_grab_fr.1  
Installing /opt/local/share/man/man1/tv_grab_fr_kazer.1  
Installing /opt/local/share/man/man1/tv_grab_hr.1  
Installing /opt/local/share/man/man1/tv_grab_huro.1  
Installing /opt/local/share/man/man1/tv_grab_il.1  
Installing /opt/local/share/man/man1/tv_grab_is.1  
Installing /opt/local/share/man/man1/tv_grab_it.1  
Installing /opt/local/share/man/man1/tv_grab_na_dd.1  
Installing /opt/local/share/man/man1/tv_grab_na_dtv.1  
Installing /opt/local/share/man/man1/tv_grab_na_tvmedia.1  
Installing /opt/local/share/man/man1/tv_grab_nl.1  
Installing /opt/local/share/man/man1/tv_grab_no_gfeed.1  
Installing /opt/local/share/man/man1/tv_grab_pt.1  
Installing /opt/local/share/man/man1/tv_grab_pt_meo.1  
Installing /opt/local/share/man/man1/tv_grab_se_swedb.1  
Installing /opt/local/share/man/man1/tv_grab_se_tvzon.1  
Installing /opt/local/share/man/man1/tv_grab_tr.1  
Installing /opt/local/share/man/man1/tv_grab_uk_atlas.1  
Installing /opt/local/share/man/man1/tv_grab_uk_bleb.1  
Installing /opt/local/share/man/man1/tv_grab_uk_rt.1  
Installing /opt/local/share/man/man1/tv_grab_uk_tvguide.1  
Installing /opt/local/share/man/man1/tv_grab_za.1 
Installing /opt/local/share/man/man1/tv_grep.1  
Installing /opt/local/share/man/man1/tv_imdb.1  
Installing /opt/local/share/man/man1/tv_merge.1  
Installing /opt/local/share/man/man1/tv_remove_some_overlapping.1  
Installing /opt/local/share/man/man1/tv_sort.1  
Installing /opt/local/share/man/man1/tv_split.1  
Installing /opt/local/share/man/man1/tv_to_latex.1  
Installing /opt/local/share/man/man1/tv_to_potatoe.1  
Installing /opt/local/share/man/man1/tv_to_text.1  
Installing /opt/local/share/man/man1/tv_validate_file.1  
Installing /opt/local/share/man/man1/tv_validate_grabber.1  
Installing /opt/local/share/man/man3/Configure.3  
Installing /opt/local/share/man/man3/Configure::Writer.3  
Installing /opt/local/share/man/man3/Data::Recursive::Encode.3  
Installing /opt/local/share/man/man3/Options.3  
Installing /opt/local/share/man/man3/PreferredMethod.3  
Installing /opt/local/share/man/man3/ValidateFile.3  
Installing /opt/local/share/man/man3/ValidateGrabber.3  
Installing /opt/local/share/man/man3/Version.3  
Installing /opt/local/share/man/man3/XMLTV.3  
Installing /opt/local/share/man/man3/XMLTV::Date.3  
Installing /opt/local/share/man/man3/XMLTV::Grab_XML.3  
Installing /opt/local/share/man/man3/XMLTV::GUI.3  
Installing /opt/local/share/man/man3/XMLTV::Gunzip.3  
Installing /opt/local/share/man/man3/XMLTV::Summarize.3  
Installing /opt/local/share/man/man3/XMLTV::Supplement.3  
Installing /opt/local/bin/tv_augment_tz  
Installing /opt/local/bin/tv_cat  
Installing /opt/local/bin/tv_check  
Installing /opt/local/bin/tv_count  
Installing /opt/local/bin/tv_extractinfo_ar  
Installing /opt/local/bin/tv_extractinfo_en  
Installing /opt/local/bin/tv_find_grabbers  
Installing /opt/local/bin/tv_grab_ar  
Installing /opt/local/bin/tv_grab_ch_search   
Installing /opt/local/bin/tv_grab_combiner  
Installing /opt/local/bin/tv_grab_dk_dr  
Installing /opt/local/bin/tv_grab_dtv_la  
Installing /opt/local/bin/tv_grab_es_laguiatv  
Installing /opt/local/bin/tv_grab_eu_egon  
Installing /opt/local/bin/tv_grab_eu_epgdata  
Installing /opt/local/bin/tv_grab_fi  
Installing /opt/local/bin/tv_grab_fi_sv  
Installing /opt/local/bin/tv_grab_fr  
Installing /opt/local/bin/tv_grab_fr_kazer  
Installing /opt/local/bin/tv_grab_hr  
Installing /opt/local/bin/tv_grab_huro  
Installing /opt/local/bin/tv_grab_il  
Installing /opt/local/bin/tv_grab_is  
Installing /opt/local/bin/tv_grab_it  
Installing /opt/local/bin/tv_grab_na_dd  
Installing /opt/local/bin/tv_grab_na_dtv  
Installing /opt/local/bin/tv_grab_na_tvmedia  
Installing /opt/local/bin/tv_grab_nl  
Installing /opt/local/bin/tv_grab_no_gfeed  
Installing /opt/local/bin/tv_grab_pt  
Installing /opt/local/bin/tv_grab_pt_meo   
Installing /opt/local/bin/tv_grab_se_swedb  
Installing /opt/local/bin/tv_grab_se_tvzon  
Installing /opt/local/bin/tv_grab_tr  
Installing /opt/local/bin/tv_grab_uk_atlas  
Installing /opt/local/bin/tv_grab_uk_bleb  
Installing /opt/local/bin/tv_grab_uk_rt  
Installing /opt/local/bin/tv_grab_uk_tvguide  
Installing /opt/local/bin/tv_grab_za  
Installing /opt/local/bin/tv_grep  
Installing /opt/local/bin/tv_imdb  
Installing /opt/local/bin/tv_merge  
Installing /opt/local/bin/tv_remove_some_overlapping  
Installing /opt/local/bin/tv_sort  
Installing /opt/local/bin/tv_split  
Installing /opt/local/bin/tv_to_latex  
Installing /opt/local/bin/tv_to_potatoe  
Installing /opt/local/bin/tv_to_text  
Installing /opt/local/bin/tv_validate_file  
Installing /opt/local/bin/tv_validate_grabber  
Appending installation info to /opt/local/Library/Perl/Updates/5.12.3/darwin-thread-multi-2level/perllocal.pod  
Installing contents of blib/doc into /.//opt/local/share/doc/xmltv-0.5.66  
Installing /opt/local/share/doc/xmltv-0.5.66/COPYING  
Installing /opt/local/share/doc/xmltv-0.5.66/QuickStart  
Installing /opt/local/share/doc/xmltv-0.5.66/README  
Installing /opt/local/share/doc/xmltv-0.5.66/README.tv_check  
Installing /opt/local/share/doc/xmltv-0.5.66/README.win32  
Installing /opt/local/share/doc/xmltv-0.5.66/tv_check_doc.html  
Installing /opt/local/share/doc/xmltv-0.5.66/tv_check_doc.jpg  
Installing contents of blib/share into /.//opt/local/share/xmltv  
Installing /opt/local/share/xmltv/xmltv-lineups.xsd  
Installing /opt/local/share/xmltv/xmltv.dtd  
Installing /opt/local/share/xmltv/tv_grab_eu_epgdata/channel_ids  
Installing /opt/local/share/xmltv/tv_grab_huro/catmap.cz  
Installing /opt/local/share/xmltv/tv_grab_huro/catmap.hu  
Installing /opt/local/share/xmltv/tv_grab_huro/catmap.ro  
Installing /opt/local/share/xmltv/tv_grab_huro/catmap.sk  
Installing /opt/local/share/xmltv/tv_grab_huro/jobmap  
Installing /opt/local/share/xmltv/tv_grab_is/category_map  
Installing /opt/local/share/xmltv/tv_grab_it/channel_ids  
Installing /opt/local/share/xmltv/tv_grab_uk_atlas/tv_grab_uk_atlas.map.channels.conf  
Installing /opt/local/share/xmltv/tv_grab_uk_atlas/tv_grab_uk_atlas.map.genres.conf  
Installing /opt/local/share/xmltv/tv_grab_uk_atlas/tv_grab_uk_atlas.pa.genres.conf  
Installing /opt/local/share/xmltv/tv_grab_uk_atlas/tv_grab_uk_atlas.user.map.conf  
Installing /opt/local/share/xmltv/tv_grab_uk_atlas/cgi-bin/getatlas.pl  
Installing /opt/local/share/xmltv/tv_grab_uk_bleb/icon_urls  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/channel_icons  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/channel_ids  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/channels_platforms  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/prog_titles_to_process  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/regional_channels_by_postcode  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/utf8_fixups  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/freesat.map  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/freesat.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/freesatfromsky.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/freesathd.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/freeview.map  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/freeview.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/freeviewhd.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/lineups.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/saorview.map  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/saorview.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/sky.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/skyhd.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/upcireland.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/upcirelandhd.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/virgin.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/virginhd.xml  
Installing /opt/local/share/xmltv/tv_grab_uk_rt/lineups/xmltv-lineups.xsl  
Installing /opt/local/share/xmltv/tv_grab_uk_tvguide/tv_grab_uk_tvguide.map.conf  

## French TV case
### use tv_grab_fr_kazer script provided by xmltv 
### tv_grab_fr_kazer is located at /opt/local/bin/tv_grab_fr_kazer (see logs before)
### tv_grab_fr_kazer need a free account (and it hash access key) from http://www.kazer.org 
### create an account at http://www.kazer.org and select your channels at http://www.kazer.org/my-channels.html

## Uk TV case
### download xmltv sky network EPG grabber
 wget https://raw.githubusercontent.com/franckbonin/tv_grab_uk_sky/master/tv_grab_uk_sky  
### you may change the first line of this script to use the right perl interpretor
### install xmltv sky network EPG grabber where all other grabber where installed with XMLTV, see logs before : (/opt/local/bin/)
 sudo mv tv_grab_uk_sky /opt/local/bin/  
 sudo chmod 755 /opt/local/bin/tv_grab_uk_sky  
### JSON::XS dependency is required for tv_grab_uk_sky :
 sudo perl -MCPAN -e shell  
  perl> install JSON::XS  
  perl> exit  

## Configure, test and automate it all !
### following instructions are for both tv_grab_uk_sky or tv_grab_fr_kazer, (just replace tv_grab_uk_sky by tv_grab_fr_kazer)
### configure xmltv sky network EPG grabber :
### you may require to define PERL5LIB env var if perl script fail to found its dependencies. It shall be setted to /opt/local/lib/perl5/site_perl/ or something like that
 PERL5LIB=/opt/local/lib/perl5/site_perl/ /opt/local/bin/tv_grab_uk_sky --configure  
### test it (it could be long, be patient):
 PERL5LIB=/opt/local/lib/perl5/site_perl/ /opt/local/bin/tv_grab_uk_sky > xmltv.xml  

### Automate and integrate it with EyeTv  
 mkdir -p /Library/WebServer/Documents/eyetv  
 sudo vi /Library/WebServer/Documents/eyetv/update.sh  
### Paste in the following text

\#!/bin/sh  
cd /Library/WebServer/Documents/eyetv  
\# with perl xmltv  
PERL5LIB=/opt/local/lib/perl5/site_perl/ /opt/local/bin/tv_grab_uk_sky > /Library/WebServer/Documents/eyetv/xmltv.xml  
\# open EyeTV with xmltv file  
open -a EyeTV /Library/WebServer/Documents/eyetv/xmltv.xml  

### Make your script executable
 sudo chmod 755 /Library/WebServer/Documents/eyetv/update.sh  
### And make your script automatically called every day. To do so edit your crontab :
 crontab -e  
### type ‘i’ to enter insert mode and enter the following:  
 15 07 * * * /Library/WebServer/Documents/eyetv/update.sh  

### This should import your data every morning at 7:15 AM. Test it out:
 /Library/WebServer/Documents/eyetv/update.sh  
### Eye tv should be lanched at the and of the process
### go to channels list and choose xmltv as program provider for every channel.
### it should find programs within xml epg provided or ask you for corresponding channel names 

## Enjoy !






