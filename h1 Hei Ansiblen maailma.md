
# h1 Hei Ansiblen maailma
## x. Lue ja tiivistä

[SSH public key - Login without password](https://terokarvinen.com/ssh-public-key-login-without-password/)

-  Asennetaan ssh `sudo apt-get -y install ssh`
-  Laitetaan ssh daemon päälle `sudo systemctl enable --now ssh`
-  Testataan ssh:ta `ssh localhost`
-  Luodaan ssh avainpari `ssh-keygen`
-  Kopiodaan julkinen avain laitteelle jotta voidaan kirjautua ilman salasanaa `ssh-copy-id localhost`

[Hello Ansible](https://terokarvinen.com/hello-ansible/)

- Luodaan hosts.ini tiedosto
- Kerrotaan miten ansiblea käytetään
	- `ansible all -a 'uptime' -i hosts.ini`
- Kerrotaan miten päästää **python virheestä eroon**
```
[all:vars]
ansible_python_interpreter=/usr/bin/python3
```
- Kerrotaan miten määritellään **hosts.ini** tiedosto inventory tiedostoksi
	- **ansible.cfg**
- **site.yml** määritys
- Oikeat kansiot ja tiedostot oikeisiin paikkoihin
- `ansible-playbook` komento
- Miten käytetään **sudoa** ansiblen kanssa
	- `become: true`
## a. [Paikallista](h0%20Oikeasti%20netissä.md#a.%20Paikallista)

## b. Sshecrets

- Ensin päivitin järjestelmän paketit
	- `sudo apt update`
	- `sudo apt upgrade`

![Pasted image 20261104120711.png](<Pasted image 20261104120711.png>)

![Pasted image 20261104120751.png](<Pasted image 20261104120751.png>)

-  Sitten asensin ssh-demonin
	- `sudo apt install ssh`

![Pasted image 20261104120936.png](<Pasted image 20261104120936.png>)

-  Asennuksen jälkeen laitoin sen päälle
	- `sudo systemctl enable --now ssh`
		-  Komento tekee niin että demoni on päällä järjestelmän käynnistyessä
		-  `--now` tekee niin että se menee myös päälle heti

![Pasted image 20261104121137.png](<Pasted image 20261104121137.png>)

- Testasin vielä kirjautumista
	- `ssh localhost`

![Pasted image 20261104121230.png](<Pasted image 20261104121230.png>)
## c. Pubkey

- Loin ssh avainparin
	- `ssh-keygen`

![Pasted image 20261104121352.png](<Pasted image 20261104121352.png>)

- Kopion juliksen avaimen serverille (localhost)
	- `ssh-copy-id localhost`

![Pasted image 20261104121524.png](<Pasted image 20261104121524.png>)

- Testasin vielä toimiiko kirjautuminen ilman salasanaa
	- `ssh localhost`

![Pasted image 20261104121633.png](<Pasted image 20261104121633.png>)
## d. Hei Ansible

- Ensin asensin ansiblen
	- `sudo apt install ansible`
- Sitten tein oman kansion missä voin tehdä ansible hommia
	- `mkdir ansible`
![Pasted image 20261104121958.png](<Pasted image 20261104121958.png>)

- Tein hosts tiedoston minne voin määritellä kaikki tietokoneet mitä haluan hallita ansiblella (tässä tapauksessa vain localhost)
	- `echo localhost >> hosts.ini`

![Pasted image 20261104122250.png](<Pasted image 20261104122250.png>)

- Testasin toimiiko se
	- `ansible all -a 'uptime' -i hosts.ini`
		- `all` argumentilla määritellään missä tietokoneissa komento tehdään, eli tässä tapauksessa kaikilla
		-  `-a` argumentilla voidaan määritellä mikä komento kohde koneessa suoritetaan tässä tapauksessa `uptime`
		- `-i` määritellään inventory tiedosto
			- Jotta komennossa ei tarvitse jokakerta määritellä inventory tiedostoa voin määritellä sen suoraan *ansible.cfg* tiedostoon 
			- ![Pasted image 20261104123009.png](<Pasted image 20261104123009.png>)![Pasted image 20261104123046.png](<Pasted image 20261104123046.png>)


- Tein tarvittavat kansiot
	- `mkdir -p ./roles/hello/tasks`

![Pasted image 20261104124821.png](<Pasted image 20261104124821.png>)

- Tein *main.yml* tiedoston *tasks* kansioon johon määritin tehtävän
	 ![Pasted image 20261104125151.png](<Pasted image 20261104125151.png>)

- Sitten tein *site.yml* tiedoston
		![Pasted image 20261104125337.png](<Pasted image 20261104125337.png>)

- Testasin toimiiko `ansible-playbook site.yml`
	- Jos toimii niin **tmp** kansiossa pitäisi olla **hello** niminen tiedosto jossa lukee **Hello World!!**

![Pasted image 20261104125532.png](<Pasted image 20261104125532.png>)
![Pasted image 20261104125637.png](<Pasted image 20261104125637.png>)

- Toimi!!

## e. Vapaaehtoinen bonus

- Halusin kopioda **hello** tiedoson **/etc** kansioon, mutta se vaatii sudo oikeudet
	- Lisäsin **site.yml** tiedostoon `become: true`, että voin tehdä komentoja **sudo** oikeuksilla
		![Pasted image 20261104130251.png](<Pasted image 20261104130251.png>)

- Muutin **main.yml** tiedostosta kohde kansiota
![Pasted image 20261104130612.png](<Pasted image 20261104130612.png>)

- Testasin toimiiko
	- `ansible-playbook site.yml --ask-become-pass`
		- `--ask-become-pass` kysyy sudo kohde koneen sudo salasanaa jotta komento voidaan toteuttaa
![Pasted image 20261104130910.png](<Pasted image 20261104130910.png>)
![Pasted image 20261104130955.png](<Pasted image 20261104130955.png>)

- Toimi!!!