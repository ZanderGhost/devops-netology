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
