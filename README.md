# devops-netology
В каталоге terraform будут игнорироваться
- файлы состояния с расширением ".tfsatate" или содержащие в имени ".tfstate."
- файл лога crash.log
- все файлы с расширением "tfvars", так как они содержат часную информацию(пароли, приватные ключи и т.д.)
- файлы переонределения, так как они служат для переопределения локальных ресурсов.
- файлы настройки интерфейса командной строки.
