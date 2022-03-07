Introduction
============

The kernel is written mostly in C, with some architecture-dependent
parts written in assembly.

The kernel is written using GNU C and the GNU toolchain.  While it
adheres to the ISO C89 standard, it uses a number of extensions that are
not featured in the standard.  The kernel is a freestanding C
environment, with no reliance on the standard C library, so some
portions of the C standard are not supported.  Arbitrary long long
divisions and floating point are not allowed.  It can sometimes be
difficult to understand the assumptions the kernel has on the toolchain
and the extensions that it uses, and unfortunately there is no
definitive reference for them.

The existing development community is a diverse group of people, with
high standards for coding, style and procedure.  These standards have
been created over time based on what they have found to work best for
such a large and geographically dispersed team.  People will not adapt
to you or your company's way of doing things.


Legal Issues
------------

The Linux kernel source code is released under the GPL.

Common questions and answers about the GPL...

        https://www.gnu.org/licenses/gpl-faq.html


Documentation
-------------

The Linux kernel source tree has a large range of documents that are
invaluable for learning how to interact with the kernel community.  When
new features are added to the kernel, it is recommended that new
documentation files are also added which explain how to use the feature.
When a kernel change causes the interface that the kernel exposes to
userspace to change, it is recommended that you send the information or
a patch to the manual pages explaining the change to the manual pages
maintainer at mtk.manpages@gmail.com, and CC the list
linux-api@vger.kernel.org.

Here is a list of files that are in the kernel source tree that are
required reading:

- Documentation/admin-guide/README.rst
    This file gives a short background on the Linux kernel and describes
    what is necessary to do to configure and build the kernel.  People
    who are new to the kernel should start here.

- Documentation/process/changes.rst
    This file gives a list of the minimum levels of various software
    packages that are necessary to build and run the kernel
    successfully.

- Documentation/process/coding-style.rst
    This describes the Linux kernel coding style, and some of the
    rationale behind it. All new code is expected to follow the
    guidelines in this document. Most maintainers will only accept
    patches if these rules are followed, and many people will only
    review code if it is in the proper style.

- Documentation/process/submitting-patches.rst
- Documentation/process/submitting-drivers.rst
    These files describe in explicit detail how to successfully create
    and send a patch, including (but not limited to):

       - Email contents
       - Email format
       - Who to send it to

    Following these rules will not guarantee success (as all patches are
    subject to scrutiny for content and style), but not following them
    will almost always prevent it.

    Other excellent descriptions of how to create patches properly are:

        "The Perfect Patch"
                https://www.ozlabs.org/~akpm/stuff/tpp.txt

        "Linux kernel patch submission format"
                https://web.archive.org/web/20180829112450/http://linux.yyz.us/patch-format.html

- Documentation/process/stable-api-nonsense.rst
    This file describes the rationale behind the conscious decision to
    not have a stable API within the kernel, including things like:

      - Subsystem shim-layers (for compatibility?)
      - Driver portability between Operating Systems.
      - Mitigating rapid change within the kernel source tree (or
        preventing rapid change)

    This document is crucial for understanding the Linux development
    philosophy and is very important for people moving to Linux from
    development on other Operating Systems.

- Documentation/admin-guide/security-bugs.rst
    If you feel you have found a security problem in the Linux kernel,
    please follow the steps in this document to help notify the kernel
    developers, and help solve the issue.

- Documentation/process/management-style.rst
    This document describes how Linux kernel maintainers operate and the
    shared ethos behind their methodologies.  This is important reading
    for anyone new to kernel development (or anyone simply curious about
    it), as it resolves a lot of common misconceptions and confusion
    about the unique behavior of kernel maintainers.

- Documentation/process/stable-kernel-rules.rst
    This file describes the rules on how the stable kernel releases
    happen, and what to do if you want to get a change into one of these
    releases.

- Documentation/process/kernel-docs.rst
    A list of external documentation that pertains to kernel
    development.  Please consult this list if you do not find what you
    are looking for within the in-kernel documentation.

- Documentation/process/applying-patches.rst
    A good introduction describing exactly what a patch is and how to
    apply it to the different development branches of the kernel.

The kernel also has a large number of documents that can be
automatically generated from the source code itself or from
ReStructuredText markups (ReST), like this one. This includes a
full description of the in-kernel API, and rules on how to handle
locking properly.

All such documents can be generated as PDF or HTML by running::

        make pdfdocs
        make htmldocs

respectively from the main kernel source directory.

The documents that uses ReST markup will be generated at Documentation/output.
They can also be generated on LaTeX and ePub formats with::

        make latexdocs
        make epubdocs

Becoming A Kernel Developer
---------------------------

If you do not know anything about Linux kernel development, you should
look at the Linux KernelNewbies project:

        https://kernelnewbies.org

It consists of a helpful mailing list where you can ask almost any type
of basic kernel development question (make sure to search the archives
first, before asking something that has already been answered in the
past.)  It also has an IRC channel that you can use to ask questions in
real-time, and a lot of helpful documentation that is useful for
learning about Linux kernel development.

The website has basic information about code organization, subsystems,
and current projects (both in-tree and out-of-tree). It also describes
some basic logistical information, like how to compile a kernel and
apply a patch.

If you do not know where you want to start, but you want to look for
some task to start doing to join into the kernel development community,
go to the Linux Kernel Janitor's project:

        https://kernelnewbies.org/KernelJanitors

It is a great place to start.  It describes a list of relatively simple
problems that need to be cleaned up and fixed within the Linux kernel
source tree.  Working with the developers in charge of this project, you
will learn the basics of getting your patch into the Linux kernel tree,
and possibly be pointed in the direction of what to go work on next, if
you do not already have an idea.

Before making any actual modifications to the Linux kernel code, it is
imperative to understand how the code in question works.  For this
purpose, nothing is better than reading through it directly (most tricky
bits are commented well), perhaps even with the help of specialized
tools.  One such tool that is particularly recommended is the Linux
Cross-Reference project, which is able to present source code in a
self-referential, indexed webpage format. An excellent up-to-date
repository of the kernel code may be found at:

        https://elixir.bootlin.com/


The development process
-----------------------

Linux kernel development process currently consists of a few different
main kernel "branches" and lots of different subsystem-specific kernel
branches.  These different branches are:

  - Linus's mainline tree
  - Various stable trees with multiple major numbers
  - Subsystem-specific trees
  - linux-next integration testing tree

Mainline tree
~~~~~~~~~~~~~

The mainline tree is maintained by Linus Torvalds, and can be found at
https://kernel.org or in the repo.  Its development process is as follows:

  - As soon as a new kernel is released a two week window is open,
    during this period of time maintainers can submit big diffs to
    Linus, usually the patches that have already been included in the
    linux-next for a few weeks.  The preferred way to submit big changes
    is using git (the kernel's source management tool, more information
    can be found at https://git-scm.com/) but plain patches are also just
    fine.
  - After two weeks a -rc1 kernel is released and the focus is on making the
    new kernel as rock solid as possible.  Most of the patches at this point
    should fix a regression.  Bugs that have always existed are not
    regressions, so only push these kinds of fixes if they are important.
    Please note that a whole new driver (or filesystem) might be accepted
    after -rc1 because there is no risk of causing regressions with such a
    change as long as the change is self-contained and does not affect areas
    outside of the code that is being added.  git can be used to send
    patches to Linus after -rc1 is released, but the patches need to also be
    sent to a public mailing list for review.
  - A new -rc is released whenever Linus deems the current git tree to
    be in a reasonably sane state adequate for testing.  The goal is to
    release a new -rc kernel every week.
  - Process continues until the kernel is considered "ready", the
    process should last around 6 weeks.

It is worth mentioning what Andrew Morton wrote on the linux-kernel
mailing list about kernel releases:

        *"Nobody knows when a kernel will be released, because it's
        released according to perceived bug status, not according to a
        preconceived timeline."*

Various stable trees with multiple major numbers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Kernels with 3-part versions are -stable kernels. They contain
relatively small and critical fixes for security problems or significant
regressions discovered in a given major mainline release. Each release
in a major stable series increments the third part of the version
number, keeping the first two parts the same.

This is the recommended branch for users who want the most recent stable
kernel and are not interested in helping test development/experimental
versions.

Stable trees are maintained by the "stable" team <stable@vger.kernel.org>, and
are released as needs dictate.  The normal release period is approximately
two weeks, but it can be longer if there are no pressing problems.  A
security-related problem, instead, can cause a release to happen almost
instantly.

The file 'Documentation/process/stable-kernel-rules.rst'
in the kernel tree documents what kinds of changes are acceptable for
the -stable tree, and how the release process works.

Subsystem-specific trees
~~~~~~~~~~~~~~~~~~~~~~~~

The maintainers of the various kernel subsystems --- and also many
kernel subsystem developers --- expose their current state of
development in source repositories.  That way, others can see what is
happening in the different areas of the kernel.  In areas where
development is rapid, a developer may be asked to base his submissions
onto such a subsystem kernel tree so that conflicts between the
submission and other already ongoing work are avoided.

Most of these repositories are git trees, but there are also other SCMs
in use, or patch queues being published as quilt series.  Addresses of
these subsystem repositories are listed in the MAINTAINERS file.  Many
of them can be browsed at https://git.kernel.org/.

Before a proposed patch is committed to such a subsystem tree, it is
subject to review which primarily happens on mailing lists (see the
respective section below).  For several kernel subsystems, this review
process is tracked with the tool patchwork.  Patchwork offers a web
interface which shows patch postings, any comments on a patch or
revisions to it, and maintainers can mark patches as under review,
accepted, or rejected.  Most of these patchwork sites are listed at
https://patchwork.kernel.org/.

linux-next integration testing tree
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before updates from subsystem trees are merged into the mainline tree,
they need to be integration-tested.  For this purpose, a special
testing repository exists into which virtually all subsystem trees are
pulled on an almost daily basis:

        https://git.kernel.org/?p=linux/kernel/git/next/linux-next.git

This way, the linux-next gives a summary outlook onto what will be
expected to go into the mainline kernel at the next merge period.
Adventurous testers are very welcome to runtime-test the linux-next.


Bug Reporting
-------------

The file 'Documentation/admin-guide/reporting-issues.rst' in the main kernel
source directory describes how to report a possible kernel bug, and details
what kind of information is needed by the kernel developers to help track
down the problem.


Managing bug reports
--------------------

One of the best ways to put into practice your hacking skills is by fixing
bugs reported by other people. Not only you will help to make the kernel
more stable, but you'll also learn to fix real world problems and you will
improve your skills, and other developers will be aware of your presence.
Fixing bugs is one of the best ways to get merits among other developers,
because not many people like wasting time fixing other people's bugs.

To work on already reported bug reports, find a subsystem you are interested in.
Check the MAINTAINERS file where bugs for that subsystem get reported to; often
it will be a mailing list, rarely a bugtracker. Search the archives of said
place for recent reports and help where you see fit. You may also want to check
https://bugzilla.kernel.org for bug reports; only a handful of kernel subsystems
use it actively for reporting or tracking, nevertheless bugs for the whole
kernel get filed there.


Mailing lists
-------------

As some of the above documents describe, the majority of the core kernel
developers participate on the Linux Kernel Mailing list.  Details on how
to subscribe and unsubscribe from the list can be found at:

        http://vger.kernel.org/vger-lists.html#linux-kernel

There are archives of the mailing list on the web in many different
places.  Use a search engine to find these archives.  For example:

        http://dir.gmane.org/gmane.linux.kernel

It is highly recommended that you search the archives about the topic
you want to bring up, before you post it to the list. A lot of things
already discussed in detail are only recorded at the mailing list
archives.

Most of the individual kernel subsystems also have their own separate
mailing list where they do their development efforts.  See the
MAINTAINERS file for a list of what these lists are for the different
groups.

Many of the lists are hosted on kernel.org. Information on them can be
found at:

        http://vger.kernel.org/vger-lists.html

Please remember to follow good behavioral habits when using the lists.
Though a bit cheesy, the following URL has some simple guidelines for
interacting with the list (or any list):

        http://www.albion.com/netiquette/

If multiple people respond to your mail, the CC: list of recipients may
get pretty large. Don't remove anybody from the CC: list without a good
reason, or don't reply only to the list address. Get used to receiving the
mail twice, one from the sender and the one from the list, and don't try
to tune that by adding fancy mail-headers, people will not like it.

Remember to keep the context and the attribution of your replies intact,
keep the "John Kernelhacker wrote ...:" lines at the top of your reply, and
add your statements between the individual quoted sections instead of
writing at the top of the mail.

If you add patches to your mail, make sure they are plain readable text
as stated in 'Documentation/process/submitting-patches.rst'.

Kernel developers don't want to deal with
attachments or compressed patches; they may want to comment on
individual lines of your patch, which works only that way. Make sure you
use a mail program that does not mangle spaces and tab characters. A
good first test is to send the mail to yourself and try to apply your
own patch by yourself. If that doesn't work, get your mail program fixed
or change it until it works.

Above all, please remember to show respect to other subscribers.


Working with the community
--------------------------

The goal of the kernel community is to provide the best possible kernel
there is.  When you submit a patch for acceptance, it will be reviewed
on its technical merits and those alone.  So, what should you be
expecting?

  - criticism
  - comments
  - requests for change
  - requests for justification
  - silence

Remember, this is part of getting your patch into the kernel.  You have
to be able to take criticism and comments about your patches, evaluate
them at a technical level and either rework your patches or provide
clear and concise reasoning as to why those changes should not be made.
If there are no responses to your posting, wait a few days and try
again, sometimes things get lost in the huge volume.

What should you not do?

  - expect your patch to be accepted without question
  - become defensive
  - ignore comments
  - resubmit the patch without making any of the requested changes

In a community that is looking for the best technical solution possible,
there will always be differing opinions on how beneficial a patch is.
You have to be cooperative, and willing to adapt your idea to fit within
the kernel.  Or at least be willing to prove your idea is worth it.
Remember, being wrong is acceptable as long as you are willing to work
toward a solution that is right.

It is normal that the answers to your first patch might simply be a list
of a dozen things you should correct.  This does **not** imply that your
patch will not be accepted, and it is **not** meant against you
personally.  Simply correct all issues raised against your patch and
resend it.


Differences between the kernel community and corporate structures
-----------------------------------------------------------------

The kernel community works differently than most traditional corporate
development environments.  Here are a list of things that you can try to
do to avoid problems:

  Good things to say regarding your proposed changes:

    - "This solves multiple problems."
    - "This deletes 2000 lines of code."
    - "Here is a patch that explains what I am trying to describe."
    - "I tested it on 5 different architectures..."
    - "Here is a series of small patches that..."
    - "This increases performance on typical machines..."

  Bad things you should avoid saying:

    - "We did it this way in AIX/ptx/Solaris, so therefore it must be
      good..."
    - "I've being doing this for 20 years, so..."
    - "This is required for my company to make money"
    - "This is for our Enterprise product line."
    - "Here is my 1000 page design document that describes my idea"
    - "I've been working on this for 6 months..."
    - "Here's a 5000 line patch that..."
    - "I rewrote all of the current mess, and here it is..."
    - "I have a deadline, and this patch needs to be applied now."

Another way the kernel community is different than most traditional
software engineering work environments is the faceless nature of
interaction.  One benefit of using email and irc as the primary forms of
communication is the lack of discrimination based on gender or race.
The Linux kernel work environment is accepting of women and minorities
because all you are is an email address.  The international aspect also
helps to level the playing field because you can't guess gender based on
a person's name. A man may be named Andrea and a woman may be named Pat.
Most women who have worked in the Linux kernel and have expressed an
opinion have had positive experiences.

The language barrier can cause problems for some people who are not
comfortable with English.  A good grasp of the language can be needed in
order to get ideas across properly on mailing lists, so it is
recommended that you check your emails to make sure they make sense in
English before sending them.


Break up your changes
---------------------

The Linux kernel community does not gladly accept large chunks of code
dropped on it all at once.  The changes need to be properly introduced,
discussed, and broken up into tiny, individual portions.  This is almost
the exact opposite of what companies are used to doing.  Your proposal
should also be introduced very early in the development process, so that
you can receive feedback on what you are doing.  It also lets the
community feel that you are working with them, and not simply using them
as a dumping ground for your feature.  However, don't send 50 emails at
one time to a mailing list, your patch series should be smaller than
that almost all of the time.

The reasons for breaking things up are the following:

1) Small patches increase the likelihood that your patches will be
   applied, since they don't take much time or effort to verify for
   correctness.  A 5 line patch can be applied by a maintainer with
   barely a second glance. However, a 500 line patch may take hours to
   review for correctness (the time it takes is exponentially
   proportional to the size of the patch, or something).

   Small patches also make it very easy to debug when something goes
   wrong.  It's much easier to back out patches one by one than it is
   to dissect a very large patch after it's been applied (and broken
   something).

2) It's important not only to send small patches, but also to rewrite
   and simplify (or simply re-order) patches before submitting them.

Here is an analogy from kernel developer Al Viro:

        *"Think of a teacher grading homework from a math student.  The
        teacher does not want to see the student's trials and errors
        before they came up with the solution. They want to see the
        cleanest, most elegant answer.  A good student knows this, and
        would never submit her intermediate work before the final
        solution.*

        *The same is true of kernel development. The maintainers and
        reviewers do not want to see the thought process behind the
        solution to the problem one is solving. They want to see a
        simple and elegant solution."*

It may be challenging to keep the balance between presenting an elegant
solution and working together with the community and discussing your
unfinished work. Therefore it is good to get early in the process to
get feedback to improve your work, but also keep your changes in small
chunks that they may get already accepted, even when your whole task is
not ready for inclusion now.

Also realize that it is not acceptable to send patches for inclusion
that are unfinished and will be "fixed up later."


Justify your change
-------------------

Along with breaking up your patches, it is very important for you to let
the Linux community know why they should add this change.  New features
must be justified as being needed and useful.


Document your change
--------------------

When sending in your patches, pay special attention to what you say in
the text in your email.  This information will become the ChangeLog
information for the patch, and will be preserved for everyone to see for
all time.  It should describe the patch completely, containing:

  - why the change is necessary
  - the overall design approach in the patch
  - implementation details
  - testing results

For more details on what this should all look like, please see the
ChangeLog section of the document:

  "The Perfect Patch"
      https://www.ozlabs.org/~akpm/stuff/tpp.txt


All of these things are sometimes very hard to do. It can take years to
perfect these practices (if at all). It's a continuous process of
improvement that requires a lot of patience and determination. But
don't give up, it's possible. Many have done it before, and each had to
start exactly where you are now.

Executive summary
-----------------

The rest of this section covers the scope of the kernel development process
and the kinds of frustrations that developers and their employers can
encounter there.  There are a great many reasons why kernel code should be
merged into the official ("mainline") kernel, including automatic
availability to users, community support in many forms, and the ability to
influence the direction of kernel development.  Code contributed to the
Linux kernel must be made available under a GPL-compatible license.

- Development Process

introduces the development process, the kernel
release cycle, and the mechanics of the merge window.  The various phases in
the patch development, review, and merging cycle are covered.  There is some
discussion of tools and mailing lists.  Developers wanting to get started
with kernel development are encouraged to track down and fix bugs as an
initial exercise.

- Early-stage

covers early-stage project planning, with an
emphasis on involving the development community as soon as possible.

- Coding

is about the coding process; several pitfalls which
have been encountered by other developers are discussed.  Some requirements for
patches are covered, and there is an introduction to some of the tools
which can help to ensure that kernel patches are correct.

- Posting

talks about the process of posting patches for
review. To be taken seriously by the development community, patches must be
properly formatted and described, and they must be sent to the right place.
Following the advice in this section should help to ensure the best
possible reception for your work.

- Followthrough

covers what happens after posting patches; the
job is far from done at that point.  Working with reviewers is a crucial part
of the development process; this section offers a number of tips on how to
avoid problems at this important stage.  Developers are cautioned against
assuming that the job is done when a patch is merged into the mainline.

- Advanced Topics

introduces a couple of "advanced" topics:
managing patches with git and reviewing patches posted by others.

- Conclusion

concludes the document with pointers to sources
for more information on kernel development.

What this document is about
---------------------------

The Linux kernel, at over 8 million lines of code and well over 1000
contributors to each release, is one of the largest and most active free
software projects in existence.  Since its humble beginning in 1991, this
kernel has evolved into a best-of-breed operating system component which
runs on pocket-sized digital music players, desktop PCs, the largest
supercomputers in existence, and all types of systems in between.  It is a
robust, efficient, and scalable solution for almost any situation.

With the growth of Linux has come an increase in the number of developers
(and companies) wishing to participate in its development.  Hardware
vendors want to ensure that Linux supports their products well, making
those products attractive to Linux users.  Embedded systems vendors, who
use Linux as a component in an integrated product, want Linux to be as
capable and well-suited to the task at hand as possible.  Distributors and
other software vendors who base their products on Linux have a clear
interest in the capabilities, performance, and reliability of the Linux
kernel.  And end users, too, will often wish to change Linux to make it
better suit their needs.

One of the most compelling features of Linux is that it is accessible to
these developers; anybody with the requisite skills can improve Linux and
influence the direction of its development.  Proprietary products cannot
offer this kind of openness, which is a characteristic of the free software
process.  But, if anything, the kernel is even more open than most other
free software projects.  A typical three-month kernel development cycle can
involve over 1000 developers working for more than 100 different companies
(or for no company at all).

Working with the kernel development community is not especially hard.  But,
that notwithstanding, many potential contributors have experienced
difficulties when trying to do kernel work.  The kernel community has
evolved its own distinct ways of operating which allow it to function
smoothly (and produce a high-quality product) in an environment where
thousands of lines of code are being changed every day.  So it is not
surprising that Linux kernel development process differs greatly from
proprietary development methods.

The kernel's development process may come across as strange and
intimidating to new developers, but there are good reasons and solid
experience behind it.  A developer who does not understand the kernel
community's ways (or, worse, who tries to flout or circumvent them) will
have a frustrating experience in store.  The development community, while
being helpful to those who are trying to learn, has little time for those
who will not listen or who do not care about the development process.

It is hoped that those who read this document will be able to avoid that
frustrating experience.  There is a lot of material here, but the effort
involved in reading it will be repaid in short order.  The development
community is always in need of developers who will help to make the kernel
better; the following text should help you - or those who work for you -
join our community.

Credits
-------

This document was written by Jonathan Corbet, corbet@lwn.net.  It has been
improved by comments from Johannes Berg, James Berry, Alex Chiang, Roland
Dreier, Randy Dunlap, Jake Edge, Jiri Kosina, Matt Mackall, Arthur Marsh,
Amanda McPherson, Andrew Morton, Andrew Price, Tsugikazu Shibata, and
Jochen VoÃŸ.

This work was supported by the Linux Foundation; thanks especially to
Amanda McPherson, who saw the value of this effort and made it all happen.

The importance of getting code into the mainline
------------------------------------------------

Some companies and developers occasionally wonder why they should bother
learning how to work with the kernel community and get their code into the
mainline kernel (the "mainline" being the kernel maintained by Linus
Torvalds and used as a base by Linux distributors).  In the short term,
contributing code can look like an avoidable expense; it seems easier to
just keep the code separate and support users directly.  The truth of the
matter is that keeping code separate ("out of tree") is a false economy.

As a way of illustrating the costs of out-of-tree code, here are a few
relevant aspects of the kernel development process; most of these will be
discussed in greater detail later in this document.  Consider:

- Code which has been merged into the mainline kernel is available to all
  Linux users.  It will automatically be present on all distributions which
  enable it.  There is no need for driver disks, downloads, or the hassles
  of supporting multiple versions of multiple distributions; it all just
  works, for the developer and for the user.  Incorporation into the
  mainline solves a large number of distribution and support problems.

- While kernel developers strive to maintain a stable interface to user
  space, the internal kernel API is in constant flux.  The lack of a stable
  internal interface is a deliberate design decision; it allows fundamental
  improvements to be made at any time and results in higher-quality code.
  But one result of that policy is that any out-of-tree code requires
  constant upkeep if it is to work with new kernels.  Maintaining
  out-of-tree code requires significant amounts of work just to keep that
  code working.

  Code which is in the mainline, instead, does not require this work as the
  result of a simple rule requiring any developer who makes an API change
  to also fix any code that breaks as the result of that change.  So code
  which has been merged into the mainline has significantly lower
  maintenance costs.

- Beyond that, code which is in the kernel will often be improved by other
  developers.  Surprising results can come from empowering your user
  community and customers to improve your product.

- Kernel code is subjected to review, both before and after merging into
  the mainline.  No matter how strong the original developer's skills are,
  this review process invariably finds ways in which the code can be
  improved.  Often review finds severe bugs and security problems.  This is
  especially true for code which has been developed in a closed
  environment; such code benefits strongly from review by outside
  developers.  Out-of-tree code is lower-quality code.

- Participation in the development process is your way to influence the
  direction of kernel development.  Users who complain from the sidelines
  are heard, but active developers have a stronger voice - and the ability
  to implement changes which make the kernel work better for their needs.

- When code is maintained separately, the possibility that a third party
  will contribute a different implementation of a similar feature always
  exists.  Should that happen, getting your code merged will become much
  harder - to the point of impossibility.  Then you will be faced with the
  unpleasant alternatives of either (1) maintaining a nonstandard feature
  out of tree indefinitely, or (2) abandoning your code and migrating your
  users over to the in-tree version.

- Contribution of code is the fundamental action which makes the whole
  process work.  By contributing your code you can add new functionality to
  the kernel and provide capabilities and examples which are of use to
  other kernel developers.  If you have developed code for Linux (or are
  thinking about doing so), you clearly have an interest in the continued
  success of this platform; contributing code is one of the best ways to
  help ensure that success.

All of the reasoning above applies to any out-of-tree kernel code,
including code which is distributed in proprietary, binary-only form.
There are, however, additional factors which should be taken into account
before considering any sort of binary-only kernel code distribution.  These
include:

- The legal issues around the distribution of proprietary kernel modules
  are cloudy at best; quite a few kernel copyright holders believe that
  most binary-only modules are derived products of the kernel and that, as
  a result, their distribution is a violation of the GNU General Public
  license (about which more will be said below).  Your author is not a
  lawyer, and nothing in this document can possibly be considered to be
  legal advice.  The true legal status of closed-source modules can only be
  determined by the courts.  But the uncertainty which haunts those modules
  is there regardless.

- Binary modules greatly increase the difficulty of debugging kernel
  problems, to the point that most kernel developers will not even try.  So
  the distribution of binary-only modules will make it harder for your
  users to get support from the community.

- Support is also harder for distributors of binary-only modules, who must
  provide a version of the module for every distribution and every kernel
  version they wish to support.  Dozens of builds of a single module can
  be required to provide reasonably comprehensive coverage, and your users
  will have to upgrade your module separately every time they upgrade their
  kernel.

- Everything that was said above about code review applies doubly to
  closed-source code.  Since this code is not available at all, it cannot
  have been reviewed by the community and will, beyond doubt, have serious
  problems.

Makers of embedded systems, in particular, may be tempted to disregard much
of what has been said in this section in the belief that they are shipping
a self-contained product which uses a frozen kernel version and requires no
more development after its release.  This argument misses the value of
widespread code review and the value of allowing your users to add
capabilities to your product.  But these products, too, have a limited
commercial life, after which a new version must be released.  At that
point, vendors whose code is in the mainline and well maintained will be
much better positioned to get the new product ready for market quickly.

Licensing
---------

Code is contributed to the Linux kernel under a number of licenses, but all
code must be compatible with version 2 of the GNU General Public License
(GPLv2), which is the license covering the kernel distribution as a whole.
In practice, that means that all code contributions are covered either by
GPLv2 (with, optionally, language allowing distribution under later
versions of the GPL) or the three-clause BSD license.  Any contributions
which are not covered by a compatible license will not be accepted into the
kernel.

Copyright assignments are not required (or requested) for code contributed
to the kernel.  All code merged into the mainline kernel retains its
original ownership; as a result, the kernel now has thousands of owners.

One implication of this ownership structure is that any attempt to change
the licensing of the kernel is doomed to almost certain failure.  There are
few practical scenarios where the agreement of all copyright holders could
be obtained (or their code removed from the kernel).  So, in particular,
there is no prospect of a migration to version 3 of the GPL in the
foreseeable future.

It is imperative that all code contributed to the kernel be legitimately
free software.  For that reason, code from anonymous (or pseudonymous)
contributors will not be accepted.  All contributors are required to "sign
off" on their code, stating that the code can be distributed with the
kernel under the GPL.  Code which has not been licensed as free software by
its owner, or which risks creating copyright-related problems for the
kernel (such as code which derives from reverse-engineering efforts lacking
proper safeguards) cannot be contributed.

Questions about copyright-related issues are common on Linux development
mailing lists.  Such questions will normally receive no shortage of
answers, but one should bear in mind that the people answering those
questions are not lawyers and cannot provide legal advice.  If you have
legal questions relating to Linux source code, there is no substitute for
talking with a lawyer who understands this field.  Relying on answers
obtained on technical mailing lists is a risky affair.

