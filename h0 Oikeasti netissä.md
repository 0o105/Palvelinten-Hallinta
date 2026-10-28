
# H0 Oikeasti netissä

## x) Lue ja tiivistä

[Niemi 2025: H4 Tehtävät](https://github.com/AONCODES/linux-course/blob/main/h4.md)

- Vuokrattiin serveri upcloudilta ja laitettiin se pystyy
- Otettiin ssh yhteys serveriin `ssh root@ip` 
- Lisättiin käyttäjä `adduser username sudo` 
- Lisättiin uudelle käyttäjä ssh avain jotta sillä voi kirjautua
  
[Partanen 2025: Harjoitus 5: Nimekäs: a) Nimi](https://partanennn.github.io/linux-servers/practices/fifth-week.html)

- Ostettiin domain nimi namecheapilta
- Luotiin dns record domainille

## a) Paikallista

- Ensin latasin [Debian 13](https://www.debian.org/) 

<img width="648" height="893" alt="Pasted image 20261028122205" src="https://github.com/user-attachments/assets/b2b46c26-6074-44dd-b38a-9716e236305d" />

- Virtuaaliympäristönä käytän virtmanageria (Qemu/KVM) 
- Loin virtuaalikoneen ja säädin kaikki tarvittavat asetukset virtmanagerissa

<img width="648" height="753" alt="Pasted image 20261028122337" src="https://github.com/user-attachments/assets/2f4a5a85-7314-4bdc-b1a6-66eb96da2d7f" />

<img width="648" height="753" alt="Pasted image 20261028122356" src="https://github.com/user-attachments/assets/a9aaba8c-80e2-444c-bb69-19bfd1f389b1" />

<img width="648" height="753" alt="Pasted image 20261028122412" src="https://github.com/user-attachments/assets/d52d042b-2647-40e3-97f3-4594851e1b17" />

<img width="648" height="753" alt="Pasted image 20261028122430" src="https://github.com/user-attachments/assets/8ac8d3af-fee5-4a4c-b1f3-93737b721f34" />

<img width="648" height="753" alt="Pasted image 20261028122447" src="https://github.com/user-attachments/assets/82f8ba41-483b-4946-9483-8d8bd202dc9a" />

- Kun olin luonut koneen asensin itse Debianin

<img width="1275" height="896" alt="Pasted image 20261028122511" src="https://github.com/user-attachments/assets/3d333ee8-3d05-432f-998f-421deba4179b" />

<img width="1275" height="896" alt="Pasted image 20261028122528" src="https://github.com/user-attachments/assets/789f449c-5e81-4bb0-85a7-06bdebb59043" />

- Debian 13 asennettu

<img width="1280" height="903" alt="Pasted image 20261028130321" src="https://github.com/user-attachments/assets/6b1c5fa8-217d-44ee-b0d2-09f81187083f" />

## b) VPS

- Vuokrasin serverin [Upcloudilta](https://upcloud.com/global/) 

<img width="1081" height="265" alt="Pasted image 20261028123237" src="https://github.com/user-attachments/assets/42a0e1eb-8006-4184-beb8-15b002e6688d" />

## c) Alkutoimet

- Otan ssh yhteyden koneelle komenolla
	- `ssh -i .ssh/id_ed25519 root@ip` 
	- Olin tätä ennen luonut ssh avain parin ja kopioinut julkisen avaimen upcloudiin
		- `ssh-keygen` 
	
- Lisään uuden käyttäjä komennolla 
	- `adduser username` 

<img width="485" height="237" alt="Pasted image 20261028124203" src="https://github.com/user-attachments/assets/e17f0a31-1769-4a90-813c-067b8379b6a7" />

- Lisään uuden käyttäjän ryhmään sudo komenolla 
	- `usermod -aG sudo username` 
- Varmistan että uusi käyttäjä varmasti lisättiin sudo ryhmään komenolla 
	- `groups username` 

<img width="542" height="96" alt="Pasted image 20261028124358" src="https://github.com/user-attachments/assets/b9c4fc91-eb87-4987-b069-7e1a7e25f2e7" />

- Seuraavaksi päivitin järjestelmän komenolla 
	- `apt update && apt upgrade -y` 

- Tämän jälkeen asensin palomuurin valitisn [UFW](https://wiki.archlinux.org/title/Uncomplicated_Firewall) 
- Asensin sen komenolla
	- `apt install ufw`
 
<img width="658" height="324" alt="Pasted image 20261028124733" src="https://github.com/user-attachments/assets/a3b178e0-5f74-4367-b665-0537293db0aa" />

- Konfiguroin ufw:n sallimaan vain ssh yhteyden koneeseen komennoilla ja laitoin sen päälle

- `ufw allow shh`
- `uf default deny incoming`
- `ufw enable` 

<img width="702" height="202" alt="Pasted image 20261028125400" src="https://github.com/user-attachments/assets/ea6dec0c-3ce3-4f1b-8f3c-4e1b39707e33" />

## d) Weppipalvelin

- Asensin koneelle [nginx](https://nginx.org/) komenolla 
	- `apt install nginx` 
	
<img width="1280" height="676" alt="Pasted image 20261028132233" src="https://github.com/user-attachments/assets/ea7a46cd-9752-421a-b0ba-9f7f1fc0e731" />
