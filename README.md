# NAME

App::Nopaste - easy access to any pastebin

# SYNOPSIS

    use App::Nopaste 'nopaste';

    my $url = nopaste(q{
        perl -wle 'print "Prime" if (1 x shift) !~ /^1?$|^(11+?)\1+$/' [number]
    });

    # or on the command line:
    nopaste test.pl
    => http://pastebin.com/fcba51f

# DESCRIPTION

Pastebins (also known as nopaste sites) let you post text, usually code, for
public viewing. They're used a lot in IRC channels to show code that would
normally be too long to give directly in the channel (hence the name nopaste).

Each pastebin is slightly different. When one pastebin goes down (I'm looking
at you, [http://paste.husk.org](http://paste.husk.org)), then you have to find a new one. And if you
usually use a script to publish text, then it's too much hassle.

This module aims to smooth out the differences between pastebins, and provides
redundancy: if one site doesn't work, it just tries a different one.

It's also modular: you only need to put on CPAN a
[App::Nopaste::Service::Foo](https://metacpan.org/pod/App::Nopaste::Service::Foo) module and anyone can begin using it.

# INTERFACE

## CLI

See the documentation in [App::Nopaste::Command](https://metacpan.org/pod/App::Nopaste::Command).

## `nopaste`

    use App::Nopaste 'nopaste';

    my $url = nopaste(
        text => "Full text to paste (the only mandatory argument)",
        desc => "A short description of the paste",
        nick => "Your nickname",
        lang => "perl",
        chan => "#moose",
        private => 1, # default: 0

        # this is the default, but maybe you want to do something different
        error_handler => sub {
            my ($error, $service) = @_;
            warn "$service: $error";
        },

        warn_handler => sub {
            my ($warning, $service) = @_;
            warn "$service: $warning";
        },

        # you may specify the services to use - but you don't have to
        services => ["Shadowcat", "Gist"],
    );

    print $url if $url;

The `nopaste` function will return the URL of the paste on
success, or `undef` on failure.

For each failure, the `error_handler` argument is invoked with the error
message and the service that issued it.

For each warning, the `warn_handler` argument is invoked with the warning
message and the service that issued it.

# SEE ALSO

[WebService::NoPaste](https://metacpan.org/pod/WebService::NoPaste), [WWW::Pastebin::PastebinCom::Create](https://metacpan.org/pod/WWW::Pastebin::PastebinCom::Create), [Devel::REPL::Plugin::Nopaste](https://metacpan.org/pod/Devel::REPL::Plugin::Nopaste)

[http://perladvent.org/2011/2011-12-14.html](http://perladvent.org/2011/2011-12-14.html)

# AUTHOR

Shawn M Moore, `sartak@gmail.com`

# COPYRIGHT AND LICENSE

Copyright 2008-2010 Shawn M Moore.

This program is free software; you can redistribute it and/or modify it
under the same terms as Perl itself.
