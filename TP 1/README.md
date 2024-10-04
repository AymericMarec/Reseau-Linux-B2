# TP1 : Maîtrise réseau du votre poste

## I. Basics

### ☀️ Carte réseau WiFi

```
PS C:\Users\aymer> ipconfig /all

Carte réseau sans fil Wi-Fi :
Adresse physique . . . . . . . . . . . : 58-06-5D-E0-CA-2F
Adresse IPv4. . . . . . . . . . . . . .: 10.33.77.14
Masque de sous-réseau. . . . . . . . . : 255.255.240.0

en notation CIDR : /20
```

### ☀️ Déso pas déso

- l'adresse de réseau du LAN auquel vous êtes connectés en WiFi :

10.33.77.14

- l'adresse de broadcast :

10.33.77.255

- le nombre d'adresses IP disponibles dans ce réseau :

4096

### ☀️ Hostname

- déterminer le hostname de votre PC :

```
PS C:\Users\aymer> hostname
Aymeric
```


### ☀️ Passerelle du réseau

- Déterminer l'adresse IP de la passerelle du réseau

```
PS C:\Users\aymer> ipconfig /all

Carte réseau sans fil Wi-Fi :
Passerelle par défaut. . . . . . . . . : 10.33.79.254 (l'adresse IP de la passerelle du réseau )
```
- Déterminer l'adresse MAC de la passerelle du réseau

```
PS C:\Users\aymer> arp -a

Interface : 10.33.77.14 --- 0x4
  Adresse Internet      Adresse physique      Type
  10.33.69.24           b8-1e-a4-8f-79-47     dynamique
  10.33.73.77           98-8d-46-c4-fa-e5     dynamique
  10.33.77.160          c8-94-02-f8-ab-97     dynamique
  ☀️ 10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

```

### ☀️ Serveur DHCP et DNS

```
PS C:\Users\aymer> ipconfig /all

Carte réseau sans fil Wi-Fi :
Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
```

### ☀️ Table de routage

```
PS C:\Users\aymer> netstat -r
===========================================================================
Itinéraires actifs :
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
☀️         0.0.0.0          0.0.0.0     10.33.79.254      10.33.77.14     30
```

## II. Go further

### ☀️ Hosts ?

```
PS C:\Users\aymer> ping b2.hello.vous

Envoi d’une requête 'ping' sur b2.hello.vous [1.1.1.1] avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=15 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=16 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=18 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=16 ms TTL=55

Statistiques Ping pour 1.1.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 15ms, Maximum = 18ms, Moyenne = 16ms
```

### ☀️ Go mater une vidéo youtube et déterminer, pendant qu'elle tourne...

- l'adresse IP du serveur auquel vous êtes connectés pour regarder la vidéo

- Apres avoir trouvé l'adresse IP grace a wireshark :

```
PS C:\windows\system32> netstat -a -n -b | Select-String 91.68.245.76: -Context 1,0

   [svchost.exe]
>   UDP    0.0.0.0:57619          ☀️91.68.245.76:443
```

- le port du serveur auquel vous êtes connectés:
```
PS C:\windows\system32> netstat -a -n -b | Select-String 91.68.245.76: -Context 1,0

   [svchost.exe]
>   UDP    0.0.0.0:57619          91.68.245.76:☀️443
```

- le port que votre PC a ouvert en local pour se connecter au port du serveur distant
```
PS C:\windows\system32> netstat -a -n -b | Select-String 91.68.245.76: -Context 1,0

   [svchost.exe]
>   UDP    0.0.0.0:☀️57619          91.68.245.76:443
```

### ☀️ Requêtes DNS

```
PS C:\Users\aymer> nslookup www.thinkerview.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    www.thinkerview.com
Addresses:  2a06:98c1:3121::7
          2a06:98c1:3120::7
          ☀️188.114.97.7
          188.114.96.7
```

### ☀️ Hop hop hop

```
PS C:\Users\aymer> tracert -d www.ynov.com

Détermination de l’itinéraire vers www.ynov.com [172.67.74.226]
avec un maximum de 30 sauts :

  1     5 ms     2 ms     2 ms  10.33.79.254
  2     5 ms     3 ms     2 ms  195.7.117.145
  3     4 ms     3 ms     2 ms  86.79.195.237
  4     5 ms     3 ms     3 ms  86.65.224.196
  5    12 ms    11 ms    11 ms  194.6.147.164
  6     *       56 ms     *     162.158.20.24
  7    17 ms    17 ms    15 ms  162.158.20.240
  8    16 ms    15 ms    15 ms  172.67.74.226

Itinéraire déterminé.
```

### ☀️ IP publique
```
PS C:\Users\aymer> curl ifconfig.me


StatusCode        : 200
StatusDescription : OK
Content           : 195.7.117.146
```

## III. Le requin

### ☀️ Capture ARP

[Lien vers capture ARP](./captures/arp.pcapng)

### ☀️ Capture DNS

[Lien vers capture ARP](./captures/dns.pcapng)

### ☀️ Capture TCP

[Lien vers capture ARP](./captures/tcp.pcapng)