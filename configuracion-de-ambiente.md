## Configuración de Ambiente![](/assets/ebc1.png)En el SO Red Hat se instalará lo siguiente:

* zsh y oh-my-zsh
* tmux
* htop
* nmap

### ZSH

> sudo yum install zsh
>
> sudo sh -c "$\(curl -fsSL [https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh](https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh%29%29%29%29\)\)"

Cambio de shell por defecto

> sudo chsh -s /bin/zsh

```
[sudo] password for TAWS01:
Complementos cargados:langpacks, product-id, search-disabled-repos, subscription-manager
Resolviendo dependencias
--> Ejecutando prueba de transacción
---> Paquete zsh.x86_64 0:5.0.2-28.el7 debe ser instalado
--> Resolución de dependencias finalizada

Dependencias resueltas

==================================================================================================================================================
 Package                     Arquitectura                   Versión                              Repositorio                                Tamaño
==================================================================================================================================================
Instalando:
 zsh                         x86_64                         5.0.2-28.el7                         rhel-7-server-rpms                         2.4 M

Resumen de la transacción
==================================================================================================================================================
Instalar  1 Paquete

Tamaño total de la descarga: 2.4 M
Tamaño instalado: 5.6 M
Is this ok [y/d/N]: y
Downloading packages:
zsh-5.0.2-28.el7.x86_64.rpm                                                                                                | 2.4 MB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Instalando    : zsh-5.0.2-28.el7.x86_64                                                                                                     1/1
  Comprobando   : zsh-5.0.2-28.el7.x86_64                                                                                                     1/1

Instalado:
  zsh.x86_64 0:5.0.2-28.el7

¡Listo!
```

    Cloning Oh My Zsh...
    Cloning into '/u01/.oh-my-zsh'...
    remote: Counting objects: 864, done.
    remote: Compressing objects: 100% (727/727), done.
    remote: Total 864 (delta 16), reused 776 (delta 10), pack-reused 0
    Receiving objects: 100% (864/864), 576.33 KiB | 0 bytes/s, done.
    Resolving deltas: 100% (16/16), done.
    Looking for an existing zsh config...
    Using the Oh My Zsh template file and adding it to ~/.zshrc
    Time to change your default shell to zsh!
    Cambiando intérprete de órdenes para TAWS01.
    Contraseña:
    Se ha cambiado el intérprete de órdenes.
             __                                     __
      ____  / /_     ____ ___  __  __   ____  _____/ /_
     / __ \/ __ \   / __ `__ \/ / / /  /_  / / ___/ __ \
    / /_/ / / / /  / / / / / / /_/ /    / /_(__  ) / / /
    \____/_/ /_/  /_/ /_/ /_/\__, /    /___/____/_/ /_/
                            /____/                       ....is now installed!


    Please look over the ~/.zshrc file to select plugins, themes, and options.

    p.s. Follow us at https://twitter.com/ohmyzsh.

    p.p.s. Get stickers and t-shirts at https://shop.planetargon.com.

### TMUX

> sudo yum install tmux -y

```
Complementos cargados:langpacks, product-id, search-disabled-repos, subscription-manager
Resolviendo dependencias
--> Ejecutando prueba de transacción
---> Paquete tmux.x86_64 0:1.8-4.el7 debe ser instalado
--> Procesando dependencias: libevent-2.0.so.5()(64bit) para el paquete: tmux-1.8-4.el7.x86_64
--> Ejecutando prueba de transacción
---> Paquete libevent.x86_64 0:2.0.21-4.el7 debe ser instalado
--> Resolución de dependencias finalizada

Dependencias resueltas

==================================================================================================================================================
 Package                        Arquitectura                 Versión                               Repositorio                              Tamaño
==================================================================================================================================================
Instalando:
 tmux                           x86_64                       1.8-4.el7                             rhel-7-server-rpms                       243 k
Instalando para las dependencias:
 libevent                       x86_64                       2.0.21-4.el7                          rhel-7-server-rpms                       214 k

Resumen de la transacción
==================================================================================================================================================
Instalar  1 Paquete (+1 Paquete dependiente)

Tamaño total de la descarga: 457 k
Tamaño instalado: 1.3 M
Downloading packages:
(1/2): libevent-2.0.21-4.el7.x86_64.rpm                                                                                    | 214 kB  00:00:00
(2/2): tmux-1.8-4.el7.x86_64.rpm                                                                                           | 243 kB  00:00:00
--------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                             584 kB/s | 457 kB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Instalando    : libevent-2.0.21-4.el7.x86_64                                                                                                1/2
  Instalando    : tmux-1.8-4.el7.x86_64                                                                                                       2/2
  Comprobando   : tmux-1.8-4.el7.x86_64                                                                                                       1/2
  Comprobando   : libevent-2.0.21-4.el7.x86_64                                                                                                2/2

Instalado:
  tmux.x86_64 0:1.8-4.el7

Dependencia(s) instalada(s):
  libevent.x86_64 0:2.0.21-4.el7

¡Listo!
```

### Tmux Configuration

En el **home** del usuario crear el archivo **.tmux.conf** con la siguiente configuración:

```
set -g prefix C-a
unbind C-b
set -sg escape-time 1
set -g base-index 1
setw -g pane-base-index 1

bind r source-file ~/.tmux.conf \; display "Configuración recargada"
bind C-a send-prefix
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
bind c new-window -c "#{pane_current_path}"
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
bind -r C-h select-window -t :-
bind -r C-l select-window -t :+
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 6
bind -r L resize-pane -R 5
setw -g mode-mouse off
set -g mouse-select-pane off
set -g mouse-resize-pane off
#set mouse-select-window off
set -g default-terminal "screen-256color"
set -g status-fg white
set -g status-bg black
setw -g window-status-fg cyan
setw -g window-status-bg default
setw -g window-status-attr dim
setw -g window-status-current-fg white
setw -g window-status-current-bg red
setw -g window-status-current-attr bright
set -g pane-border-fg green
set -g pane-border-bg black
set -g pane-active-border-fg white
set -g pane-active-border-bg yellow
set -g message-fg white
set -g message-bg black
set -g message-attr bright
set -g status-left-length 40
set -g status-left "#[fg=green]Session: #S #[fg=yellow]#I #[fg=cyan]#P"
set -g status-right "#[fg=cyan]%d %b %R"
setw -g utf8 on
set -g status-utf8 on
set -g status-interval 60
set -g status-justify centre
setw -g monitor-activity on
set -g visual-activity on
setw -g mode-keys vi
unbind [
bind Escape copy-mode
unbind p
bind p paste-buffer
bind -t vi-copy 'v' begin-selection
bind -t vi-copy 'y' copy-selection
bind -n C-k send-keys -R \; clear-history
```

## HTop

> wget [https://rpmfind.net/linux/epel/7/x86\_64/Packages/h/htop-2.1.0-1.el7.x86\_64.rpm](https://rpmfind.net/linux/epel/7/x86_64/Packages/h/htop-2.1.0-1.el7.x86_64.rpm)
>
> sudo rpm -ihv htop-2.1.0-1.el7.x86\_64.rpm

## NMap

> sudo yum install nmap

Inspeccionar puertos habilitados con nmap

> nmap -sT -O localhost

```
Starting Nmap 6.40 ( http://nmap.org ) at 2018-05-29 12:44 CDT                                                                                                               
Nmap scan report for localhost (127.0.0.1)                                                                                                                                   
Host is up (0.00014s latency).                                                                                                                                               
Other addresses for localhost (not scanned): 127.0.0.1                                                                                                                       
Not shown: 995 closed ports                                                                                                                                                  
PORT      STATE SERVICE                                                                                                                                                      
22/tcp    open  ssh                                                                                                                                                          
25/tcp    open  smtp                                                                                                                                                         
111/tcp   open  rpcbind                                                                                                                                                      
8080/tcp  open  http-proxy                                                                                                                                                   
50000/tcp open  ibm-db2                                                                                                                                                      
Device type: general purpose                                                                                                                                                 
Running: Linux 3.X                                                                                                                                                           
OS CPE: cpe:/o:linux:linux_kernel:3                                                                                                                                          
OS details: Linux 3.7 - 3.9                                                                                                                                                  
Network Distance: 0 hops                                                                                                                                                                                                                                                                                                                    
OS detection performed. Please report any incorrect results at http://nmap.org/submit/ .                                                                                     
Nmap done: 1 IP address (1 host up) scanned in 1.66 seconds
```



