# Сброс Windows пароля из Linux

Частенько бывает что особо продвинутые "пользователи Windows" ставят пароли, а потом не могут войти в систему. Сегодня я покажу как сбрасывать пароли Windows.

Иногда пароли забывают, иногда они просто не подходят. Был например случай, когда чувак поставил русский пароли, символов 8 наверное, и все, ввести его снова не получилось. Потом выяснилось что русские пароли в windows лучше вообще не ставить.

## Немного теории
Пароли в Windows NT/XP/Vista хранятся в специальных файлах `SAM`. Этот файл - кусок реестра, в котором записаны эти пароли. Очень удобно, стер `SAM` с паролем, положил на его место пустой, и все, пароля как не бывало. Но при этом теряется информация о пользователях.  
Файл лежит обычно в `C:\WINDOWS\system32\config\SAM`

## А пользоваться ей очень просто.
Для начала монтируем раздел с windows (если еще не смонтировался автоматически), например так
```sh
mount /dev/XXX /media/win_c
```

А потом собственно изменяем пароли (в интерактивном режиме)
```sh
chntpw -i /media/win_c/WINDOWS/system32/config/SAM
```

Программа будет спрашивать имя пользователя, пароль которого нужно изменить, если пользователь всего один, то предложит именно его (просто нажать `enter`). С русскими именами пользователей могут возникнуть проблемы.  
Потом программа спросит пароль, или `*` что бы был вход без пароля. Вот как примерно выглядит интерактивный сеанс работы. Конечно очень некрасиво, но кое-как понятно. Как видно русское имя пользователя программа показывает коряво.
```
chntpw version 0.99.3 040818, (c) Petter N Hagen
Hive's name (from header): <\SystemRoot\System32\Config\SAM>
ROOT KEY at offset: 0x001020 * Subkey indexing type is: 666c 
Page at 0x7000 is not 'hbin', assuming file contains garbage at end
File size 262144 [40000] bytes, containing 6 pages (+ 1 headerpage)
Used for data: 237/18920 blocks/bytes, unused: 7/5464 blocks/bytes.

* SAM policy limits:
Failed logins before lockout is: 0
Minimum password length : 0
Password history count : 0


<>========<> chntpw Main Interactive Menu <>========<>

Loaded hives: 

1 - Edit user data and passwords
2 - Syskey status & change
3 - RecoveryConsole settings
- - -
9 - Registry editor, now with full write support!
q - Quit (you will be asked if there is something to save)


What to do? [1] -> 


===== chntpw Edit User Info & Passwords ====

RID: 03e8, Username: , *disabled or locked*
RID: 03ea, Username: , *disabled or locked*
RID: 03ef, Username: <__vmware_user__>
RID: 01f4, Username: <>
RID: 01f5, Username: <>, *BLANK password*

Select: ! - quit, . - list users, 0x - User with RID (hex)
or simply enter the username to change: [] 
RID : 0500 [01f4]
Username: 4<8=8AB@0B>@
fullname: 
comment : AB@>5==0O CG5B=0O 70?8AL 04<8=8AB@0B>@0 :><5=0
homedir : 

Account bits: 0x0210 =
[ ] Disabled | [ ] Homedir req. | [ ] Passwd not req. | 
[ ] Temp. duplicate | [X] Normal account | [ ] NMS account | 
[ ] Domain trust ac | [ ] Wks trust act. | [ ] Srv trust act | 
[X] Pwd don't expir | [ ] Auto lockout | [ ] (unknown 0x08) | 
[ ] (unknown 0x10) | [ ] (unknown 0x20) | [ ] (unknown 0x40) | 

Failed login count: 0, while max tries is: 0
Total login count: 1312
** LANMAN password not set. User MAY have a blank password.
** Usually safe to continue

* = blank the password (This may work better than setting a new password!)
Enter nothing to leave it unchanged
Please enter new password: *
Blanking password!

Do you really wish to change it? (y/n) [n] y
Changed!


Select: ! - quit, . - list users, 0x - User with RID (hex)
or simply enter the username to change: [48=8AB@0B>@] !


<>========<> chntpw Main Interactive Menu <>========<>

Loaded hives: 

1 - Edit user data and passwords
2 - Syskey status & change
3 - RecoveryConsole settings
- - -
9 - Registry editor, now with full write support!
q - Quit (you will be asked if there is something to save)


What to do? [1] -> q

Hives that have changed:
# Name
0 
Write hive files? (y/n) [n] : y
0 - OK
```

Вот и все.
