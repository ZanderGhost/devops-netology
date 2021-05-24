# devops-netology
1. Полный хеш - aefead2207ef7e2aa5dc81a34aedf0cad4c32545

   Комментарий: "Update CHANGELOG.md"

   $ git show -q aefea

2. tag: v0.12.23

   $ git show -q 85024d3

3. У коммита b8d720 два родителя

   56cd7859e05c36c06b56d013b55a252d0bb7e158

   9ea88f22fc6269854151c571162c5bcf958bee2b

   
   $ git show -q b8d720^@

4. 33ff1c03b (tag: v0.12.24) v0.12.24

   b14b74c49 [Website] vmc provider links

   3f235065b Update CHANGELOG.md

   6ae64e247 registry: Fix panic when server is unreachable

   5c619ca1b website: Remove links to the getting started guide's old location

   06275647e Update CHANGELOG.md

   d5f9411f5 command: Fix bug when using terraform login on Windows

   4b6d06cc5 Update CHANGELOG.md

   dd01a3507 Update CHANGELOG.md

   225466bc3 Cleanup after v0.12.23 release
   
   $ git log --oneline v0.12.23..v0.12.24

5. Функция добавлена в коммите 8c928e835

   $ git log -S'func providerSource' --oneline

   $ git show 8c928e835 #проверим, что нашли нужный коммит

6. Найдем файл в котором создана функция:

   $ git grep -n 'func globalPluginDirs'
   
   Смотрим все изменения в теле функции:

   git log -q -L:globalPluginDirs:plugins.go

   commit 78b12205587fe839f10d946ea3fdc06719decb05

   Author: Pam Selle <204372+pselle@users.noreply.github.com>

   Date:   Mon Jan 13 16:50:05 2020 -0500

    Remove config.go and update things using its aliases

   
   commit 52dbf94834cb970b510f2fba853a5b49ad9b1a46

   Author: James Bardin <j.bardin@gmail.com>

   Date:   Wed Aug 9 17:46:49 2017 -0400

    keep .terraform.d/plugins for discovery


   commit 41ab0aef7a0fe030e84018973a64135b11abcd70

   Author: James Bardin <j.bardin@gmail.com>

   Date:   Wed Aug 9 10:34:11 2017 -0400

    Add missing OS_ARCH dir to global plugin paths
    
    When the global directory was added, the discovery system still
    attempted to search for OS_ARCH subdirectories. It has since been
    changed only search explicit paths.

 
   commit 66ebff90cdfaa6938f26f908c7ebad8d547fea17

   Author: James Bardin <j.bardin@gmail.com>

   Date:   Wed May 3 22:24:51 2017 -0400

    move some more plugin search path logic to command
    
    Make less to change when we remove the old search path

 
   commit 8364383c359a6b738a436d1b7745ccdce178df47

   Author: Martin Atkins <mart@degeneration.co.uk>
 
    Date:   Thu Apr 13 18:05:58 2017 -0700

    Push plugin discovery down into command package
    
    Previously we did plugin discovery in the main package, but as we move
    towards versioned plugins we need more information available in order to
    resolve plugins, so we move this responsibility into the command package
    itself.
    
    For the moment this is just preserving the existing behavior as long as
    there are only internal and unversioned plugins present. This is the
    final state for provisioners in 0.10, since we don't want to support
    versioned provisioners yet. For providers this is just a checkpoint along
    the way, since further work is required to apply version constraints from
    configuration and support additional plugin search directories.
    
    The automatic plugin discovery behavior is not desirable for tests because
    we want to mock the plugins there, so we add a new backdoor for the tests
    to use to skip the plugin discovery and just provide their own mock
    implementations. Most of this diff is thus noisy rework of the tests to
    use this new mechanism.


7. Автор функции synchronizedWriters Martin Atkins <mart@degeneration.co.uk>
   
   git log -S'func synchronizedWriters'
