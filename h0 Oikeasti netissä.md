
# H0 Oikeasti netissä

## x. Lue ja tiivistä

[Niemi 2025: H4 Tehtävät](https://github.com/AONCODES/linux-course/blob/main/h4.md)

- Vuokrattiin serveri upcloudilta ja laitettiin se pystyy
- Otettiin ssh yhteys serveriin `ssh root@ip` 
- Lisättiin käyttäjä `adduser username sudo` 
- Lisättiin uudelle käyttäjä ssh avain jotta sillä voi kirjautua
- 
[Partanen 2025: Harjoitus 5: Nimekäs: a) Nimi](https://partanennn.github.io/linux-servers/practices/fifth-week.html)

- Ostettiin domain nimi namecheapilta
- Luotiin dns record domainille

## a. Paikallista

- Ensin latasin [Debian 13](https://www.debian.org/) 

![h0 Oikeasti netissä.png](/Assets/h0%20Oikeasti%20netiss%C3%A4.png)

- Virtuaaliympäristönä käytän virtmanageria (Qemu/KVM) 
- Loin virtuaalikoneen ja säädin kaikki tarvittavat asetukset virtmanagerissa

![h0 Oikeasti netissä-1.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-1.png)

![h0 Oikeasti netissä-2.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-2.png)

![h0 Oikeasti netissä-3.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-3.png)

![h0 Oikeasti netissä-4.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-4.png)

![h0 Oikeasti netissä-5.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-5.png)

- Kun olin luonut koneen asensin itse Debianin

![h0 Oikeasti netissä-6.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-6.png)

![h0 Oikeasti netissä-7.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-7.png)

- Debian 13 asennettu

![h0 Oikeasti netissä-8.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-8.png)

## b. VPS

- Vuokrasin serverin [Upcloudilta](https://upcloud.com/global/) 

![h0 Oikeasti netissä-9.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-9.png)

## c. Alkutoimet

- Otan ssh yhteyden koneelle komenollat 
	- `ssh -i .ssh/id_ed25519 root@94.237.119.72` 
	- Olin tätä ennen luonut ssh avain parin ja kopioinut julkisen avaimen upcloudiin
		- `ssh-keygen` 
	
- Lisään uuden käyttäjä komennolla 
	- `adduser username` 

![h0 Oikeasti netissä-10.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-10.png)

- Lisään uuden käyttäjän ryhmään sudo komenolla 
	- `usermod -aG sudo username` 
- Varmistan että uusi käyttäjä varmasti lisättiin sudo ryhmään komenolla 
	- `groups username` 

![h0 Oikeasti netissä-11.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-11.png)

- Seuraavaksi päivitin järjestelmän komenolla 
	- `apt update && apt upgrade -y` 

- Tämän jälkeen asensin palomuurin valitisn [UFW](https://wiki.archlinux.org/title/Uncomplicated_Firewall) 
- Asensin sen komenolla
	- `apt install ufw` 
![h0 Oikeasti netissä-12.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-12.png)

- Konfiguroin ufw:n sallimaan vain ssh yhteyden koneeseen komennoilla ja laitoin sen päälle

- `ufw allow shh`
- `uf default deny incoming`
- `ufw enable` 

![h0 Oikeasti netissä-13.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-13.png)

## d. Weppipalvelin

- Asensin koneelle [nginx](https://nginx.org/) komenolla 
	- `apt install nginx` 
	
![h0 Oikeasti netissä-14.png](/Assets/h0%20Oikeasti%20netiss%C3%A4-14.png)