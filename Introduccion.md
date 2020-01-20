# Seguridad de sistemas
### Introducción
Permmisos tradicionales UNIX sobre ficheros:
- Esquema ugoa. **umask** máscara de los permisos. 
- Permisos especiales (SUID (el usuario tiene permisos, pero como propietario, en octal es el 2), SGID (el usuario tiene permisos, pero como grupo propietario, en octal es el 4), sticky bit(se puede escribir pero no se puede borrar lo que no es nuestro, en octal es el 1))

Es una implementación de un sistema de DAC (Discretionary Access Control) ya que los ususario pueden modificar permisos (chmod)

~~~
root@coatlicue:/home/paloma/DISCO2/CICLO II/ADMINISTRACION_DE_SISTEMAS_OPERATIVOS/Seguridad_de_sistemas# find / -perm /4000
find: ‘/run/user/1000/keybase/kbfs’: Permiso denegado
find: ‘/run/user/1000/gvfs’: Permiso denegado
/usr/share/code/chrome-sandbox
/usr/sbin/exim4
/usr/sbin/mount.nfs
/usr/sbin/pppd
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/ntfs-3g
/usr/bin/sudo
/usr/bin/newgrp
/usr/bin/fusermount
/usr/bin/chfn
/usr/bin/mount
/usr/bin/umount
/usr/bin/keybase-redirector
/usr/bin/bwrap
/usr/bin/su
/usr/bin/pkexec
/usr/bin/chsh
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/xorg/Xorg.wrap
/usr/lib/eject/dmcrypt-get-device
/usr/lib/virtualbox/VBoxSDL
/usr/lib/virtualbox/VBoxNetDHCP
/usr/lib/virtualbox/VBoxVolInfo
/usr/lib/virtualbox/VBoxHeadless
/usr/lib/virtualbox/VirtualBoxVM
/usr/lib/virtualbox/VBoxNetNAT
/usr/lib/virtualbox/VBoxNetAdpCtl
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/spice-gtk/spice-client-glib-usb-acl-helper
~~~

### Linux kernel POSIX capabilities
Tradicionalmente hay dos privilegios en el sistema:
- Procesos privilegiados: Se saltan las comprobaciones de permisos.
- Procesos no privilegiados: Comprobación estricta de permisos.

**Kernel Capabilities**: mecanismo de seguridad basado en le principio de mínimo privilegio, agrupando ciertos privilegios en una "capacidad".

Lista de capacidades:
- cap_chown: un proceso puede cambiar los usuarios.
- cap_kill: capacidad de matar un proceso del que no sea propietario.
- cap_net_admin: por ejemplo, se necesita que network manager modifique las interfaces, pues se le da este permiso, pero no ejecuta como root.
- cap_net_bind_service
- cap_net_raw
- cap_sys_admin
- cap_sys_module
- cap_sys_rawio: acceso a dispositivos de entrada/salida.
- cap_sys_time

Para gestional las capabilities hay que instalar **libcap2-bin**:
- setcap: define las capacidades de un fichero.
- getcap: obtiene las capacidades de un fichero.
- getpcaps: lista las capacidades de un proceso.

> Practiquita:
Se cambian los permisos de passwd para que no tenga 2755:
~~~
vagrant@servidor:~$ sudo chmod 0755 /bin/passwd 
~~~

Se añade la capacidad:
~~~
vagrant@servidor:~$ sudo setcap cap_dac_override,cap_chown,cap_fowner+ep /bin/passwd
~~~

### Atributos de ficheros
Estos atributos se hicieron para ext, pero en otros sistemas de ficheros no tienen que estar soportados, o no todos. 
- a: append only
- i: inmutable
- c: compressed
- d:no dump
- e: extent format
- j: data journalling
- t: no tail-merging
- u: undelete
- A: no atime updates
- C: no copy on write
...

Cambiar atributos: **chattr**, listar **lsattr**.

