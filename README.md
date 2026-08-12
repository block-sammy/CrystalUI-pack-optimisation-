ULTRA-REALISTIC COMPUTER PHYSICS SIMULATOR

Tu es un ingénieur logiciel senior spécialisé en :

* émulation CPU ;
* virtualisation ;
* architecture des ordinateurs ;
* systèmes d’exploitation ;
* électronique numérique ;
* simulation physique ;
* systèmes embarqués ;
* filesystems ;
* réseaux ;
* hyperviseurs ;
* simulation déterministe ;
* interfaces terminal/ASCII.

Ta mission est de CONSTRUIRE un simulateur informatique extrêmement avancé appelé :

PHYSICORE VM

Le but est de créer une machine virtuelle simulée dont le fonctionnement interne se rapproche autant que possible d’une véritable machine informatique.

Il ne faut PAS simplement générer des réponses qui ressemblent à celles d’un ordinateur.

Il faut créer un modèle d’état interne cohérent, évolutif et déterministe.

Chaque composant doit interagir avec les autres.

⸻

1. PRINCIPE FONDAMENTAL

Construis le simulateur comme si tu concevais une véritable plateforme matérielle.

Architecture :

                         PHYSICORE VM
                              |
                 +------------+------------+
                 |                         |
             HOST LAYER               VM LAYER
                 |                         |
                 |              +----------+----------+
                 |              |                     |
                 |           HARDWARE              DEVICES
                 |              |                     |
                 |      +-------+-------+       +-----+-----+
                 |      |       |       |       |     |     |
                 |     CPU     RAM     BUS     DISK  NET   VGA
                 |      |
                 |   CACHE
                 |      |
                 |   MMU/TLB
                 |
             CLOCK / SCHEDULER

Le système doit fonctionner selon une boucle d’exécution déterministe :

FETCH
 ↓
DECODE
 ↓
EXECUTE
 ↓
MEMORY ACCESS
 ↓
DEVICE ACCESS
 ↓
INTERRUPTS
 ↓
CLOCK UPDATE
 ↓
SCHEDULER
 ↓
NEXT CYCLE

⸻

2. TEMPS SIMULÉ

Ne base pas toute la simulation sur des délais arbitraires.

Crée une horloge virtuelle :

SIMULATION TIME
├── cycles CPU
├── nanoseconds
├── microseconds
├── milliseconds
├── seconds
└── uptime

Exemple :

SIM TIME
---------------------------
CPU cycles : 18,493,921
Time       : 00:00:04.283
Frequency  : 3.20 GHz

Le moteur doit pouvoir fonctionner selon plusieurs modes :

REALTIME
FAST
SLOW
STEP
PAUSED
DETERMINISTIC

⸻

3. CPU

Construis un CPU virtuel structuré.

Le CPU doit posséder :

Registers
Program Counter
Stack Pointer
Flags
Instruction Decoder
ALU
Control Unit
Interrupt Controller
MMU
TLB
Caches
Pipeline

Architecture initiale :

x86_64

mais prépare le système pour :

ARM64
RISC-V
x86

⸻

4. REGISTRES CPU

Maintiens réellement :

RAX
RBX
RCX
RDX
RSI
RDI
RBP
RSP
R8
R9
R10
R11
R12
R13
R14
R15
RIP
RFLAGS

Exemple :

CPU STATE
RAX = 0x0000000000000042
RBX = 0x0000000000000000
RCX = 0x0000000000000010
RDX = 0x0000000000000001
RSP = 0x00007FFFFFFFE000
RBP = 0x00007FFFFFFFE100
RIP = 0x000000000040102A
RFLAGS = 0x00000202

Les valeurs doivent être modifiées par les opérations du CPU.

⸻

5. ALU

Implémente une ALU virtuelle.

Elle doit gérer :

ADD
SUB
MUL
DIV
AND
OR
XOR
NOT
SHL
SHR
CMP
TEST

Les flags doivent être mis à jour :

ZERO
CARRY
SIGN
OVERFLOW
PARITY

⸻

6. INSTRUCTIONS

Crée un moteur d’instructions extensible.

Pipeline :

FETCH
 ↓
DECODE
 ↓
OPERANDS
 ↓
EXECUTE
 ↓
MEMORY
 ↓
WRITEBACK

Chaque instruction doit pouvoir modifier :

REGISTERS
FLAGS
MEMORY
PROGRAM COUNTER
STACK

Ne simule pas seulement le texte affiché.

Simule réellement les changements d’état nécessaires.

⸻

7. CACHE CPU

Ajoute :

L1 Instruction
L1 Data
L2
L3

Chaque cache possède :

SIZE
LINE SIZE
ASSOCIATIVITY
HIT
MISS
LATENCY

Exemple :

CPU CACHE
L1I : 32 KB
L1D : 32 KB
L2  : 512 KB
L3  : 16 MB
CACHE STATISTICS
L1 hits  : 981239
L1 miss  : 12839
L2 hits  : 10923
L2 miss  : 1916

Ajoute des latences simulées.

⸻

8. RAM

La mémoire virtuelle doit être adressable.

Exemple :

RAM = 8 GB
PAGE SIZE = 4096 bytes

Implémente conceptuellement :

Physical Memory
Virtual Memory
Page Tables
Pages
Frames
MMU
TLB
Page Fault

Exemple :

VIRTUAL ADDRESS
0x7FFFFFFFE123
       |
       v
      MMU
       |
       +---- TLB HIT
       |
       v
PHYSICAL ADDRESS
0x00042A123

⸻

9. PAGE FAULT

Si une page n’est pas présente :

PAGE FAULT
Virtual address:
0x00007FFFFA120000
Page:
NOT PRESENT
Action:
ALLOCATE FRAME
Frame:
0x000001A4
Mapping:
CREATED

Le système doit modifier réellement les tables mémoire.

⸻

10. BUS

Ajoute un bus système simulé.

CPU
 |
 +---- MEMORY BUS
 |
 +---- PCI BUS
 |
 +---- STORAGE BUS
 |
 +---- USB BUS
 |
 +---- NETWORK BUS

Chaque périphérique doit avoir :

ADDRESS
IRQ
DMA
STATUS

⸻

11. INTERRUPTIONS

Implémente un système d’interruptions.

Exemples :

TIMER INTERRUPT
KEYBOARD INTERRUPT
DISK INTERRUPT
NETWORK INTERRUPT
PAGE FAULT
SYSTEM CALL

Exemple :

[INTERRUPT]
IRQ: 32
SOURCE: TIMER
Saving CPU state...
Loading interrupt vector...
Executing handler...
Returning to previous context...

⸻

12. DMA

Les périphériques de stockage et réseau doivent pouvoir utiliser un modèle DMA simulé.

DEVICE
   |
   | DMA REQUEST
   v
DMA CONTROLLER
   |
   v
RAM

Cela doit affecter l’état mémoire sans faire passer artificiellement chaque octet par le CPU.

⸻

13. STOCKAGE

Crée un contrôleur de disque virtuel.

Support logique :

SATA
NVMe
IDE

Le disque possède :

capacity
sectors
blocks
latency
queue
controller
cache

Exemple :

NVMe0
Capacity : 128 GB
Queue    : 4
Pending  : 2
Read     : 183 MB/s
Write    : 97 MB/s

Les performances doivent être simulées à partir des opérations effectuées.

⸻

14. FILESYSTEM

Crée un filesystem interne.

Supporte au minimum :

EXT-like
FAT-like
NTFS-like

Chaque filesystem doit avoir :

superblock
inode/file record
directory
metadata
permissions
timestamps
blocks
free-space map

Une commande :

write file.txt "hello"

doit provoquer :

filesystem
 ↓
block allocation
 ↓
disk request
 ↓
storage queue
 ↓
controller
 ↓
virtual disk

⸻

15. CONTRÔLEUR DISQUE

Ne fais pas apparaître instantanément les données.

Une opération doit passer par :

APPLICATION
 ↓
KERNEL
 ↓
FILESYSTEM
 ↓
BLOCK DEVICE
 ↓
CONTROLLER
 ↓
VIRTUAL DISK

Le simulateur peut alors afficher :

DISK REQUEST #1832
READ
LBA: 18392
SIZE: 4096 bytes
QUEUE: 2
LATENCY: 0.42 ms
STATUS: COMPLETED

⸻

16. GPU / DISPLAY

Ajoute une carte graphique virtuelle.

Elle possède :

VRAM
FRAMEBUFFER
DISPLAY MODE
RESOLUTION
REFRESH RATE

Mode texte :

80x25

Mode avancé :

640x480
800x600
1024x768

Si l’environnement ne permet pas de véritable affichage graphique, convertis le framebuffer en ASCII.

Exemple :

████████████████████████████████
██                              ██
██        PHYSICORE VM          ██
██                              ██
████████████████████████████████

⸻

17. CLAVIER

Le clavier doit être un périphérique virtuel.

Flux :

KEYBOARD
 ↓
SCAN CODE
 ↓
IRQ
 ↓
KERNEL
 ↓
INPUT DRIVER
 ↓
TERMINAL

Une touche pressée doit réellement traverser cette chaîne simulée.

⸻

18. RÉSEAU

Crée une stack réseau virtuelle.

Architecture :

APPLICATION
 ↓
SOCKET
 ↓
TCP/UDP
 ↓
IP
 ↓
ETHERNET
 ↓
VIRTUAL NIC

Support logique :

Ethernet
ARP
IPv4
IPv6
ICMP
UDP
TCP
DNS
DHCP

La connexion réelle à Internet doit être désactivée par défaut.

Si un backend réel est disponible, il doit être explicitement signalé.

⸻

19. SYSTÈME D’EXPLOITATION

Permets de simuler un kernel.

Architecture :

USER SPACE
-------------------------
Shell
Programs
Libraries
-------------------------
KERNEL SPACE
-------------------------
Scheduler
Memory Manager
Filesystem
Drivers
Network Stack
IPC
Syscalls
-------------------------
HARDWARE

⸻

20. SYSCALLS

Implémente un modèle de syscall.

Exemples :

open()
read()
write()
close()
fork()
exec()
exit()
mmap()
brk()
socket()
connect()

Exemple :

PROGRAM
 |
 | write()
 v
SYSCALL
 |
 v
KERNEL
 |
 v
FILESYSTEM
 |
 v
DISK

⸻

21. PROCESSUS

Chaque processus possède :

PID
PPID
UID
GID
STATE
REGISTERS
MEMORY MAP
OPEN FILES
CPU TIME
PRIORITY
THREADS

États :

NEW
READY
RUNNING
SLEEPING
WAITING
STOPPED
ZOMBIE
TERMINATED

⸻

22. SCHEDULER

Implémente un scheduler simulé.

Supporte :

Round Robin
Priority
Multilevel Queue

À chaque changement :

PROCESS 42
RUNNING
        ↓
TIMER INTERRUPT
        ↓
SCHEDULER
        ↓
PROCESS 87
RUNNING

⸻

23. THREADS

Chaque processus peut posséder plusieurs threads.

PROCESS 42
THREAD 1
THREAD 2
THREAD 3
THREAD 4

Chaque thread possède son propre :

RIP
RSP
REGISTERS
STATE

⸻

24. HORLOGE MATÉRIELLE

Simule :

RTC
PIT
HPET
TSC

L’OS doit pouvoir obtenir l’heure via le matériel virtuel.

⸻

25. TEMPÉRATURE ET ÉNERGIE

Ajoute un modèle de consommation de ressources.

Chaque composant possède une consommation virtuelle :

CPU
GPU
RAM
DISK
NETWORK

Exemple :

POWER / THERMAL
CPU : 42 W
GPU : 18 W
RAM : 7 W
SSD : 3 W
NIC : 1 W
TOTAL : 71 W
Temperature:
CPU 58°C
GPU 46°C

La température doit évoluer selon la charge.

Utilise un modèle physique simplifié et clairement présenté comme une approximation.

⸻

26. REFROIDISSEMENT

Simule :

FAN
HEATSINK
THERMAL LIMIT
THROTTLING

Si la température dépasse une limite :

THERMAL WARNING
CPU temperature: 96°C
Thermal throttling activated.
Frequency:
3.20 GHz
   ↓
2.10 GHz

⸻

27. ERREURS MATÉRIELLES

Ajoute un système de panne contrôlée.

Exemples :

RAM ERROR
DISK ERROR
NETWORK FAILURE
DEVICE DISCONNECT
CPU FAULT
POWER LOSS
THERMAL SHUTDOWN

Commande :

fault inject disk

doit injecter une panne dans la simulation.

Le système d’exploitation doit éventuellement réagir.

⸻

28. MODE OSCILLOSCOPE / DIAGNOSTIC

Commande :

monitor

affiche :

SYSTEM MONITOR
CPU LOAD
████████████████░░░░ 78%
MEMORY
██████████░░░░░░░░░░ 52%
DISK I/O
██████░░░░░░░░░░░░░░ 31%
NETWORK
███░░░░░░░░░░░░░░░░░ 15%
TEMPERATURE
███████████░░░░░░░░░ 58°C

⸻

29. TRACE HARDWARE

Commande :

trace hardware

Exemple :

CPU FETCH
  ↓
L1 CACHE HIT
  ↓
REGISTER READ
  ↓
ALU
  ↓
FLAGS UPDATE
  ↓
RAM WRITE
  ↓
INTERRUPT CHECK
  ↓
NEXT INSTRUCTION

⸻

30. MODE ÉLECTRONIQUE

Ajoute un mode permettant d’inspecter les signaux logiques.

BUS DATA
CLK : ─┐ ┌─┐ ┌─┐ ┌─┐ ┌─
       └─┘ └─┘ └─┘ └─┘
DATA:
1011010010110101
ADDR:
0000110011010010
IRQ:
00010000

Ce mode reste une abstraction numérique : ne prétends pas simuler électroniquement chaque transistor.

⸻

31. TRANSISTORS — MODE EXPÉRIMENTAL

Ajoute éventuellement un niveau extrêmement détaillé.

LOGIC GATE
   |
   +-- AND
   +-- OR
   +-- NOT
   +-- XOR

Puis :

ALU
 ↓
CONTROL UNIT
 ↓
CPU

Ce mode ne doit être activé que si l’utilisateur le demande car il serait beaucoup trop coûteux pour une simulation normale.

⸻

32. SNAPSHOTS PHYSIQUES

Un snapshot doit sauvegarder absolument tout :

CPU REGISTERS
RAM
CACHE
TLB
PAGE TABLES
DISK
FILESYSTEM
PROCESSES
THREADS
DEVICES
NETWORK
CLOCK
TEMPERATURE
POWER STATE
KERNEL STATE

Ainsi :

snapshot create checkpoint

puis :

snapshot restore checkpoint

doit restaurer la machine dans un état quasiment identique.

⸻

33. DÉTERMINISME

Ajoute un seed :

simulation seed 123456

Avec le même seed et les mêmes entrées :

INPUT A
+
SEED A
=
STATE A

doit produire le même résultat.

Cela permet :

replay
debug
testing
rollback

⸻

34. REPLAY

Commande :

replay start
replay stop
replay save test.replay
replay load test.replay

Le système doit pouvoir rejouer les événements :

BOOT
KEYBOARD
CPU
DISK
NETWORK
INTERRUPTS
PROCESS EVENTS

⸻

35. MODE MACHINE PHYSIQUE

Commande :

physical

affiche :

╔══════════════════════════════════════════════════════════════╗
║                 PHYSICORE HARDWARE                          ║
╠══════════════════════════════════════════════════════════════╣
║ CPU                                                         ║
║   Cores        : 8                                          ║
║   Frequency    : 3.20 GHz                                  ║
║   Temperature  : 57°C                                      ║
║   Power        : 42 W                                      ║
║                                                              ║
║ MEMORY                                                       ║
║   RAM          : 16 GB                                     ║
║   Used         : 5.2 GB                                    ║
║                                                              ║
║ STORAGE                                                      ║
║   NVMe         : 512 GB                                    ║
║   Temperature  : 38°C                                      ║
║                                                              ║
║ NETWORK                                                      ║
║   NIC          : UP                                         ║
╚══════════════════════════════════════════════════════════════╝

⸻

36. MODE VM

Commande :

vm

affiche :

HOST
  |
  +--- PHYSICORE HYPERVISOR
          |
          +--- VM-01
          |     CPU
          |     RAM
          |     DISK
          |     NET
          |
          +--- VM-02
          |     CPU
          |     RAM
          |     DISK
          |
          +--- VM-03

Chaque VM doit être indépendante.

⸻

37. MIGRATION

Ajoute :

vm save
vm load
vm export
vm import

Si techniquement possible :

vm migrate VM-01 VM-02

simule une migration de l’état.

⸻

38. CONSOLE ASCII

L’interface par défaut doit ressembler à une vraie console de diagnostic.

╔════════════════════════════════════════════════════════════╗
║ PHYSICORE VM v1.0                                         ║
╠════════════════════════════════════════════════════════════╣
║ VM: TEST-01                                               ║
║ STATE: RUNNING                                            ║
╠════════════════════════════════════════════════════════════╣
║ CPU  [##############------] 72%                           ║
║ RAM  [########----------] 41%                             ║
║ DISK [###---------------] 16%                             ║
║ NET  [##----------------] 11%                             ║
║ TEMP [##########--------] 54C                             ║
╠════════════════════════════════════════════════════════════╣
║ user@physicore:~$                                         ║
╚════════════════════════════════════════════════════════════╝

⸻

39. COMMANDES PRINCIPALES

Implémente :

vm
machine
cpu
ram
memory
cache
disk
storage
network
devices
process
threads
interrupts
kernel
filesystem
temperature
power
monitor
trace
logs
snapshot
replay
fault
debug
physical
hardware
help
shutdown
reboot

⸻

40. ARCHITECTURE DU CODE

Sépare clairement :

/core
    clock
    scheduler
    events
    state
/cpu
    registers
    alu
    decoder
    pipeline
    cache
    mmu
    tlb
/memory
    ram
    pages
    page_tables
/devices
    keyboard
    display
    disk
    network
    timer
    pci
    usb
/storage
    controller
    filesystem
    virtual_disk
/network
    ethernet
    ipv4
    ipv6
    tcp
    udp
    dns
/kernel
    scheduler
    syscalls
    processes
    threads
    drivers
/vm
    machine
    snapshots
    cloning
    configuration
/ui
    ascii
    terminal
    dashboard
    monitor

⸻

41. RÈGLE DE RÉALISME

Ne simule jamais un événement uniquement parce qu’il serait joli à afficher.

Chaque événement affiché doit avoir une cause dans le modèle interne.

Exemple interdit :

CPU: 73%

si aucune charge CPU n’existe.

Exemple correct :

PROCESS 42 exécutant 180000 instructions
        ↓
CPU utilization calculated
        ↓
CPU: 73%

Même principe pour :

RAM
DISK
NETWORK
TEMPERATURE
POWER
PROCESS
CACHE
INTERRUPTS

⸻

42. RÈGLE DE TRANSPARENCE

Le système doit toujours distinguer :

[REAL]

pour une opération réellement exécutée par l’environnement hôte,

et :

[SIMULATED]

pour une opération effectuée par le moteur de simulation.

Exemple :

[SIMULATED] CPU instruction executed
[SIMULATED] NVMe latency applied
[SIMULATED] RAM page allocated

Si QEMU/KVM est réellement utilisé :

[REAL BACKEND] QEMU process started

Ne jamais prétendre qu’un transistor, une instruction native ou un périphérique physique a réellement fonctionné lorsque seul le modèle logiciel l’a simulé.

⸻

43. OBJECTIF FINAL

Le résultat doit ressembler à ceci :

USER
 |
 v
PHYSICORE UI
 |
 v
HYPERVISOR
 |
 v
VIRTUAL MACHINE
 |
 +-------------------------------+
 | CPU                           |
 |  ↓                            |
 | CACHE                         |
 |  ↓                            |
 | MMU                           |
 |  ↓                            |
 | RAM                           |
 |                               |
 | BUS                           |
 |  ↓          ↓          ↓      |
 | DISK       NIC        GPU     |
 |                               |
 | KERNEL                        |
 |  ↓                            |
 | PROCESSES                     |
 |  ↓                            |
 | APPLICATIONS                  |
 +-------------------------------+

Le logiciel doit donner une impression de machine informatique vivante, avec un état qui évolue continuellement.

Il doit être possible de démarrer une VM, laisser tourner des processus, observer le CPU, remplir la RAM, écrire sur le disque, générer des interruptions, faire évoluer la température, arrêter un périphérique, provoquer une panne, prendre un snapshot et restaurer la machine.

IMPORTANT :

Ce projet doit rester honnête sur son niveau de simulation.

Le niveau « transistor » est expérimental et optionnel.

Le niveau principal doit être une simulation architecturale cohérente et suffisamment détaillée pour reproduire le comportement observable d’une vraie machine sans prétendre reproduire physiquement chaque électron ou transistor.

Si un backend de virtualisation réel est disponible, utilise-le pour les opérations réellement exécutables et conserve le moteur PHYSICORE pour la visualisation, l’inspection et la simulation avancée.
