
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

![h1 Hei Ansiblen maailma.png](/Assets/h1%20Hei%20Ansiblen%20maailma.png)

![h1 Hei Ansiblen maailma-1.png](/Assets/h1%20Hei%20Ansiblen%20maailma-1.png)

-  Sitten asensin ssh-demonin
	- `sudo apt install ssh`

![h1 Hei Ansiblen maailma-2.png](/Assets/h1%20Hei%20Ansiblen%20maailma-2.png)

-  Asennuksen jälkeen laitoin sen päälle
	- `sudo systemctl enable --now ssh`
		-  Komento tekee niin että demoni on päällä järjestelmän käynnistyessä
		-  `--now` tekee niin että se menee myös päälle heti

![h1 Hei Ansiblen maailma-3.png](/Assets/h1%20Hei%20Ansiblen%20maailma-3.png)

- Testasin vielä kirjautumista
	- `ssh localhost`

![h1 Hei Ansiblen maailma-4.png](/Assets/h1%20Hei%20Ansiblen%20maailma-4.png)
## c. Pubkey

- Loin ssh avainparin
	- `ssh-keygen`

![h1 Hei Ansiblen maailma-5.png](/Assets/h1%20Hei%20Ansiblen%20maailma-5.png)

- Kopion juliksen avaimen serverille (localhost)
	- `ssh-copy-id localhost`

![h1 Hei Ansiblen maailma-6.png](/Assets/h1%20Hei%20Ansiblen%20maailma-6.png)

- Testasin vielä toimiiko kirjautuminen ilman salasanaa
	- `ssh localhost`

![h1 Hei Ansiblen maailma-7.png](/Assets/h1%20Hei%20Ansiblen%20maailma-7.png)
## d. Hei Ansible

- Ensin asensin ansiblen
	- `sudo apt install ansible`
- Sitten tein oman kansion missä voin tehdä ansible hommia
	- `mkdir ansible`
![h1 Hei Ansiblen maailma-8.png](/Assets/h1%20Hei%20Ansiblen%20maailma-8.png)

- Tein hosts tiedoston minne voin määritellä kaikki tietokoneet mitä haluan hallita ansiblella (tässä tapauksessa vain localhost)
	- `echo localhost >> hosts.ini`

![h1 Hei Ansiblen maailma-9.png](/Assets/h1%20Hei%20Ansiblen%20maailma-9.png)

- Testasin toimiiko se
	- `ansible all -a 'uptime' -i hosts.ini`
		- `all` argumentilla määritellään missä tietokoneissa komento tehdään, eli tässä tapauksessa kaikilla
		-  `-a` argumentilla voidaan määritellä mikä komento kohde koneessa suoritetaan tässä tapauksessa `uptime`
		- `-i` määritellään inventory tiedosto
			- Jotta komennossa ei tarvitse jokakerta määritellä inventory tiedostoa voin määritellä sen suoraan *ansible.cfg* tiedostoon 
			- ![h1 Hei Ansiblen maailma-10.png](/Assets/h1%20Hei%20Ansiblen%20maailma-10.png)![h1 Hei Ansiblen maailma-11.png](/Assets/h1%20Hei%20Ansiblen%20maailma-11.png)


- Tein tarvittavat kansiot
	- `mkdir -p ./roles/hello/tasks`

![h1 Hei Ansiblen maailma-12.png](/Assets/h1%20Hei%20Ansiblen%20maailma-12.png)

- Tein *main.yml* tiedoston *tasks* kansioon johon määritin tehtävän
	 ![h1 Hei Ansiblen maailma-13.png](/Assets/h1%20Hei%20Ansiblen%20maailma-13.png)

- Sitten tein *site.yml* tiedoston
		![h1 Hei Ansiblen maailma-14.png](/Assets/h1%20Hei%20Ansiblen%20maailma-14.png)

- Testasin toimiiko `ansible-playbook site.yml`
	- Jos toimii niin **tmp** kansiossa pitäisi olla **hello** niminen tiedosto jossa lukee **Hello World!!**

![h1 Hei Ansiblen maailma-15.png](/Assets/h1%20Hei%20Ansiblen%20maailma-15.png)
![h1 Hei Ansiblen maailma-16.png](/Assets/h1%20Hei%20Ansiblen%20maailma-16.png)

- Toimi!!

## e. Vapaaehtoinen bonus

- Halusin kopioda **hello** tiedoson **/etc** kansioon, mutta se vaatii sudo oikeudet
	- Lisäsin **site.yml** tiedostoon `become: true`, että voin tehdä komentoja **sudo** oikeuksilla
		![h1 Hei Ansiblen maailma-17.png](/Assets/h1%20Hei%20Ansiblen%20maailma-17.png)

- Muutin **main.yml** tiedostosta kohde kansiota
![h1 Hei Ansiblen maailma-18.png](/Assets/h1%20Hei%20Ansiblen%20maailma-18.png)

- Testasin toimiiko
	- `ansible-playbook site.yml --ask-become-pass`
		- `--ask-become-pass` kysyy sudo kohde koneen sudo salasanaa jotta komento voidaan toteuttaa
![h1 Hei Ansiblen maailma-19.png](/Assets/h1%20Hei%20Ansiblen%20maailma-19.png)
![h1 Hei Ansiblen maailma-20.png](/Assets/h1%20Hei%20Ansiblen%20maailma-20.png)

- Toimi!!!