---
layout: post
title: Salt and pepper - How to encrypt database passwords
date: '2013-12-18'
comments: true
categories: perl

---

To explain how salt and pepper work in encryption, I will walk through a few
scenarios.

# No salt

**Summary for the impatient:**  *Using no salt means an attacker doesn't need
to generate a rainbow table because they can reuse an existing one.*

If an attacker obtains my database of encrypted passwords it will be very time
consuming to brute force them.  However there exist pre-computed tables of
encrypted values of thousands of commonly used passwords.  These tables are called
rainbow tables.  It is computationally inexpensive to match the encrypted
values in a rainbow table with the encrypted values in my database.


# One salt

**Summary for the impatient:**  *Using the same salt for all your passwords
means an attacker must generate 1 rainbow table.*

Salt is data used as an additional input to the algorthim that encrypts a
password.  If I use a salt when I encrypt a password the resulting output will
be different from someone who did not use the same salt.  That means my
attacker cannot reuse an existing rainbow table.  They must generate a new one
using the same salt I used.  

Note that salt is usually stored as plain text in the database with the
encrypted passwords.  So the attacker usually has access to the salt.  Even so,
I have successfully made the attack more expensive.


# A random salt per password

**Summary for the impatient:**  *Using a random salt for each password means an
attacker must generate 1 rainbow table per password.*

Instead of using a single salt for the entire database I can use a different
(random) salt for each user's password.  This means the attacker must generate
a set of rainbow tables for each password which is even more expensive.  

Note that I also need to store the salt for each password I have generated.
This is a pain to do manually.  Happily some clever person came up with RFC
2307 which suggests a much simpler solution.

Instead of storing the just the encrypted password in the password column, store
a string which concatenates the salt and the encrypted password.  This may not
sound easier.  It implies the need to parse and concatenate strings.  However
this is handled for me by the encryption libraries so its 100% pain free.  Lets
see an example.

To encrypt the plaintext string 'pie' use the following Perl code

    my $blowfish = Authen::Passphrase::BlowfishCrypt->new(
       passphrase  => 'pie',
       salt_random => 1,
       cost        => 16,
    );

    say $blowfish->as_rfc2307; 
    # the output will look like this:
    # {CRYPT}$2a$14$sS80d1JlF3oR6Q4UHT.9w.DIXnV0/dLQMoVBsOp2gMRT65bWvP0P2

That crazy *{CRYPT}$2a$blarblar* mumbo jumbo is what we will save to the db in the password
column.  However if I know what to look for, I can see the mumbo jumbo is actually
several things smushed together:

    {CRYPT} $ 2a $ 16 $ sS80d1JlF3oR6Q4UHT.9w.DIXnV0/dLQMoVBsOp2gMRT65bWvP0P2

 - **{CRYPT}** - This is the scheme identifier.  It indicates which scheme is being used
   so I know how to parse the rest of the string.
 - **$** - These are field separators
 - **2a** - A version number for this scheme
 - **16** - The cost
 - Then there is the **salt** (22 base 64 digits -- plain text)
 - Followed by the **encrypted password** (31 base 64 digits)

To check if a user has submitted a valid `$password`  use the following code

    my $secret   = ''{CRYPT}$2a$16$sS80d1JlF3oR6Q4UHT.9w.DIXnV0/dLQMoVBsOp2gMRT65bWvP0P2';
    my $blowfish = Authen::Passphrase->from_rfc2307($secret);

    if ($blowfish->match($password)) {
        say "You may enter";
    }
    else {
        say "You did not say the magic word";
    }

Of course we want to build this into our ORM so the Authen::Passphrase objects
are inflated and deflated for us.  Here is what that looks like in a DBIx
Result class:

    __PACKAGE__->load_components(qw/FilterColumn/);
    __PACKAGE__->filter_column( password => {
        filter_to_storage   => sub { $_[1]->as_rfc2307() },                      # deflate
        filter_from_storage => sub { Authen::Passphrase->from_rfc2307($_[1]) },  # inflate
    });

But I only showed you that so you would understand what
DBIx::Class::InflateColumn::Authen::Passphrase does under the covers.  I use
that because it makes my code simpler:

    __PACKAGE__->load_components(qw/InflateColumn::Authen::Passphrase/);
    __PACKAGE__->add_columns(
        ...,
        password => {
            data_type          => 'text',
            inflate_passphrase => 'rfc2307',
        },
        ...,
    );

This is how I encrypt passwords on [networthify.com](https://networthify.com)
and [iijo.org](http://iijo.org).


# Adding pepper

**Summary for the impatient:**  *Using pepper means an attacker must generate
many rainbow tables per password. But few people use pepper and its
controversial.*

Pepper is the same as salt except that I don't save the value anywhere.  Lets
say I choose an 8 bit value for my pepper.  That means there are 256
possible values.  If I don't save that value anywhere then when a user logs
in I will need to try up to 256 values to see if the user has the right
password.  However it means my attacker will need to generate up to 256
rainbow tables for each password.

One big problem is that trying 256 possible values is going to take me about 4
minutes on average hardware.

Even if I ignore that issue, this option is controversial and my understanding
is that few people do it.  It is generally accepted that messing about with
salt and pepper should be left to the professionals who are writing the
encryption libraries.  Pepper is not supported by Authen::Passphrase. 


# Caveats 

Salting is done to make rainbow tables inneffective.  For various reasons
crackers rarely use rainbow tables anymore.  Instead they use sophisticated
brute force algorithms which combine dictionary attacks with databases of known
or commonly used passwords.  These kinds of brute force attacks can often crack
battery-horse-staple [XKCD](http://xkcd.com/936/) style passwords even if they
are very long.  Password are not secure unless they are very long *and*
very random.  

And there is more bad news.  While salting makes it harder to crack all the
passwords in the database, cracking a single targeted password is often not
computationally hard.  A single completely random 8 character password can be
cracked with brute force in 10 days.  

The only way to protect your users is to require very long and very random
passwords.  Make sure your website requires a minimum password length of 8
characters or more.

#### Sources 

 * [Anatomy of a hack: even your 'complicated' password is easy to crack](http://www.wired.co.uk/news/archive/2013-05/28/password-cracking/viewall) (Wired magazine)
 * [Authen::Passphrase](https://metacpan.org/pod/Authen::Passphrase)
 * [Authen::Passphrase::BlowfishCrypt](https://metacpan.org/pod/Authen::Passphrase::BlowfishCrypt)
 * [DBIx::Class::InflateColumn::Authen::Passphrase](https://metacpan.org/pod/DBIx::Class::InflateColumn::Authen::Passphrase)
 * [frew](http://blog.afoolishmanifesto.com/archives/1910)
 * [crypto.stackexchange.com](http://crypto.stackexchange.com/questions/1776/can-you-help-me-understand-what-a-cryptographic-salt-is)
 * [security.stackexchange.com](http://security.stackexchange.com/questions/3272/password-hashing-add-salt-pepper-or-is-salt-enough)


