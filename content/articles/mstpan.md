---
layout: post
title: mstpan quick reference
date: '2015-09-08'
comments: true
categories:
  - perl

---

Last Christmas, the inimitable Matt Trout (mst) created an opinionated tour of
CPAN where he recommended modules for some common problems.  I think its
brilliant, useful, and entertaining and I've decided to compile a high level
overview all on one page in order to create a quick reference.

I've tried to distill his wisdom and wit down to a few words.  But I recommend
clicking the section headers and following the links to Matt's original posts
to see his actual real opinions in their full complexity.  This single page
can't replace 16 great posts and he usually includes advice on the best way
to use the module.  You won't want to miss that.

# Table of Contents

 - [Web Frameworks](#webframeworks)
 - [Web Deployment](#webdeployment)
 - [XML](#xml)
 - [HTML](#html)
 - [Files](#files)
 - [Databases](#databases)
 - [JSON](#json)
 - [Async](#async)
 - [Library Deployment](#librarydeployment)
 - [Perl VM Deployment](#perlvmdeployment)
 - [Distribution building](#distributionbuilding)
 - [Object orientation](#objectorientation)
 - [SOAP](#soap)
 - [Exporting](#exporting)
 - [Email](#email)
 - [Logging](#logging)

<h2><a name="webframeworks" href="http://shadow.cat/blog/matt-s-trout/mstpan-1">Web Frameworks</a></h2>

 - **CGI.pm (the CPAN module)** - Run away
 - **Catalyst** - Venerable
 - **Dancer** - Solid and lightweight
 - **Mojolicious** - Shiny
 - **Web::Simple** - Low level
 - **Plack** - Awesome (but not a web framework)

<h2><a name="webdeployment" href="http://shadow.cat/blog/matt-s-trout/mstpan-2">Web Deployment</a></h2>

 - **mod_perl** - Run away
 - **CGI (the protocol)** - Ok for some trivial things
 - **FastCGI + unix sockets for zero downtime deploys** - Yes
 - **Apache + FastCgiExternalServer** - Good
 - **Nginx + Starman + Unix sockets** - Good
 - **Mojolicious + Hypnotoad** - Good
 - **PSGI Async**
    - **Net::Async::HTTP::Server::PSGI** if you like IO::Async
    - **Twiggy** if you like AnyEvent

<h2><a name="xml" href="http://shadow.cat/blog/matt-s-trout/mstpan-3">XML</a></h2>

 - **XML::Simple** - No
 - **XML::Twig** - Excellent whipuptitude
 - **XPath** - Not a CPAN module but you should learn it
 - **XML::LibXML** - Good
 - **Template::Semantic** - Good
 - **XML::Toolkit** - Good
 - **XML::Rabbit** - Nice
 - **xmllint** - Not a Perl module. But useful if you're XML file is made of "old man wee and fail"

<h2><a name="html" href="http://shadow.cat/blog/matt-s-trout/mstpan-4">HTML</a></h2>

 - Parsing HTML
   - **Regular expressions** - Don't
   - **HTML::TreeBuilder** - Venerable
   - **Mojo::DOM** - Pleasant
   - **XML::LibXML** or **XML::Twig** - Ok but why
 - Generating HTML
   - **CGI.pm** - Please don't
   - **Template Toolkit** - Venerable
   - **Text::Xslate** - Brilliant
   - **HTML::Mason** - Yes but no because embedded Perl
   - **Mojo::Template** - Yes but no because embedded Perl
   - **HTML::Zoom** - mst wrote it, mst doesn't hate it

<h2><a name="files" href="http://shadow.cat/blog/matt-s-trout/mstpan-5">Files</a></h2>

 - **Files::Spec** - Core, standard
 - **Files::Spec::Functions** - Use this instead of File::Spec
 - **File::stat** - Yes
 - **autodie** - Core, but "a giant bag of crack balanced precariously atop .. an even bigger bag of tainted crack"
 - **File::Open** - Better than autodie
 - **File::Slurp** - Avoid
 - **Path::Tiny** - Excellent
 - **IO::All** - Good if you want to be procedural and don't want OO

<h2><a name="databases" href="http://shadow.cat/blog/matt-s-trout/mstpan-6">Databases</a></h2>

 - **DBI** - 99% of the time, just use these 2 methods and nothing else

<pre><code>
    $dbh->do($sql, {}, @args);
    my @array_of_hashrefs = @{$dbh->selectall_arrayref($self, { Slice => {} }, @args)};
</code></pre>

 - **DBIx::Connector** - You want it
 - **Mojo::PG** - Yes
 - **DBIx::Class** - Yes
 - **DBIx::Class::Candy** - Shiny
 - **DBIx::Class::DeploymentHandler** - Yes
 - **DBIx::Class::Fixtures** - Useful for testing
 - **DBIx::Class::PassphraseColum** - Yes please

<h2><a name="json" href="http://shadow.cat/blog/matt-s-trout/mstpan-7">JSON</a></h2>

 - **JSON** - Yes but there are alternatives
 - **JSON::PP** - Pure perl, core, fatpacks
 - **JSON::XS** - Fast
 - **Cpanel::JSON::XS** - Faster
 - **JSON::MaybeXS** - Recommended
 - **JSON::Diffable** - Useful
 - **Mojo::JSON** - Really nice

<h2><a name="async" href="http://shadow.cat/blog/matt-s-trout/mstpan-8">Async</a></h2>

 - Don't
 - **threads.pm** - Don't.  Its slow.
 - **POE** - Good but weird UI.
 - **MooseX::POE** - Better
 - **Reflex** - Interesting
 - **AnyEvent** - Ok, but maintainer is difficult
 - **IO::Async** - Nice
 - **Mojo::IOLoop** - Nice
 - **Promises** - Neat but mst likes Future
 - **curry** - Useful

<h2><a name="librarydeployment" href="http://shadow.cat/blog/matt-s-trout/mstpan-9">Library deployment</a></h2>

 - **cpan as root** - No
 - **Vendor Packages** - Ok
 - **CPANPLUS** - Out of favor
 - **CPAN** - Standard
 - **cpanminus** - Use this
 - **FindBin** - Good for git deploys, bad for dist deploys
 - **local::lib** - Yes
 - **Carton** - Yes
 - **App::FatPacker** - Yes
 - **CPAN::Mini** - Maximum underkill
 - **Pinto** - Maximum overkill

<h2><a name="perlvmdeployment" href="http://shadow.cat/blog/matt-s-trout/mstpan-10">Perl VM deployment</a></h2>

 - **System perl** - Ok with local::lib
 - **Manual compilation** - Yes
 - **perlbrew** - Yes usually all the shims are more annoying than necessary
 - **Perl::Build** - Nice
 - **plenv** - Nice
 - **Windows** - Active State or Strawberry Perl

<h2><a name="distributionbuilding" href="http://shadow.cat/blog/matt-s-trout/mstpan-11">Distribution building</a></h2>
 
 - **ExtUtils::MakeMaker** - Hated by everyone except people who like Makefiles.
 - **Module::Build** - No
 - **Module::Install** - "a giant tower of crack"
 - **Module::Build::Tiny** - Nice.  See also App::ModuleBuildTiny.
 - **Dist::Zilla** - Power.  Maximum overkill.
 - **Dist::Milla** - Sensible
 - **Minilla** - Great.  Maximum underkill.

<h2><a name="objectorientation" href="http://shadow.cat/blog/matt-s-trout/mstpan-12">Object orientation</a></h2>

 - **Moose** - Awesome
 - **Moo** - Shiny
 - **Mouse** - Niche
 - **Type::Tiny** - Yes
 - **Moops** - Good
 - **Throwable** - Sensible
 - **Safe::Isa** - Might be useful

<h2><a name="soap" href="http://shadow.cat/blog/matt-s-trout/mstpan-13">SOAP</a></h2>

 - **SOAP::WSDL** - Avoid
 - **XML::Compile::SOAP** - Insane and brilliant
 - **Catalyst::Controller::SOAP** - Least worst option
 - **SOAP::Lite** - Ancient and insane

<h2><a name="exporting" href="http://shadow.cat/blog/matt-s-trout/mstpan-14">Exporting</a></h2>

 - **Exporter** - Sufficient
 - **Sub::Exporter** - Worth a look
 - **Sub::Exporter::Progressive** - Light
 - **Moose::Exporter** - Yes
 - **Exporter::Tiny** - Recommended
 - **Exporter::Declare** - Maximum overkill
 - **namespace::(auto)clean** - Useful
 - **Import::Into** - or Import::Base

<h2><a name="email" href="http://shadow.cat/blog/matt-s-trout/mstpan-15">Email</a></h2>

 - **Net::SMTP** - Too low level
 - **Email::Send** - Usable
 - **Email::Sender** - Recommended
 - **Email::Stuffer** - mst favorite
 - **Emailesque** - mst favorite with sugar on top
 - **Email::Mime** - Standard
 - **Email::Mime::Kit** - Best answer for templating
 - **Mail::Box** - Brilliant and insane
 - **Courriel** - Elegant

<h2><a name="logging" href="http://shadow.cat/blog/matt-s-trout/mstpan-16">Logging</a></h2>

 - **warn()** - Perfectly fine
 - **Log::Dispatch** - Nice
 - **Log::Log4Pperl** - Maximum overkill
 - **Log::Any** - Good
 - **Log::Contextual** - Nice
 - **Message::Passing** - Worth a look. Not quite logging.


