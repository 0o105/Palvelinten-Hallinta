## x. Lue ja tiivistä

[Sudo without password](https://terokarvinen.com/passwordless-sudo/)
- Luodaan käyttäjä, sudoless ryhmä ja lisätään käyttäjä sudoless ryhmään
	- `sudo add user antero`
	- `sudo groupadd sudoless`
	- `sidp adduser antero sudoless`
- Tehdään uusi sudoers sääntö jonka avulla voidaan tehdä sudo komentoja ilman salasanaa
	- `sudo visudo /ets/sudoers.d/sudoless`
	- `%sudoless ALL = (ALL) NO PASSWD: ALL`

[xkcd 149: Sandwitch](https://xkcd.com/149/)
- Kuva joka selittää miten **sudo** toimii
- Ukko käskee "Make me a sandwich" mutta saa kieltävän vastauksen
- Ukko lisää "sudo Make me a sandwich" ja saa myöntävän vastauksen

[Passwordless Sudo with Ansible](https://terokarvinen.com/passwordless-sudo-with-ansible/)
- Kerrotaan miten voidaan tehdä käyttäjä joka voi kirjautua sisään tietokoneelle ja käyttää sudoa ilman salasanaa
- Main.yml
``` yml
- group: #Tehdään uusi ryhmä
    name: "sudoless" # Ryhmän nimi
    state: present # Ryhmän pitää olla olemassa, jos ei ole se luodaan
- user: # Tehdään uusi käyttäjä
    name: "antero" # Käyttäjän nimi
    state: present # Käyttäjän pitää olla olemassa, jos ei ole se luodaan
    groups: ["sudoless", "sudo", "adm"] # Mihin ryhmiin käyttäjä lisätään
- authorized_key: # Määritellään ssh avaimella kirjautuminen
    user: "antero" # Käyttäjän nimi
    key: "ssh-ed25519 U2VlIHlvdSBhdCBUZXJvS2FydmluZW4uY29tIQ== tero@example.com" #Julkinen ssh avain
- copy: # Tehdään uusi tiedosto
    dest: "/etc/sudoers.d/sudoless" # Tiedoston paikka
    content: "%sudoless ALL = (ALL) NOPASSWD: ALL\n" # Tiedoston sisältö
    owner: "root" # Tiedoson omistaja
    group: "root" # tiedoston ryhmä
    mode: "0644" # Tiedoston luvat
```

### Ansible komennot:

#### copy:
ESIMERKKI:
```yml
	- name: Copy file with owner and permissions
	  ansible.builtin.copy:
	    src: /srv/myfiles/foo.conf
		dest: /etc/foo.conf
		owner: foo
		group: foo
		mode: '0644'	
```
- Kopio tiedoston tai kansion kohde koneelle
	- Content:
		- Voi määritellä tiedoston sisällön
	 - dest:
		 - Määritellään minne tiedosto kopiodaan
	- group
		- Ryhmän nimi joka omistaa tiedoston
	 - Mode:
		 - Määritellään tiedoston luvat
	 - Owner:
		 - Määritellään tiedoston omistaja
	 - src:
		 - Määritellään sijainti paikalliselle tiedostolle joka kopiodaan kohde koneeseen

#### apt:
ESIMERKKI:
```yml
	- name: Install apache httpd (state=present is optional)
	  ansible.builtin.apt:
	    name: apache2
	    state: present
```
- Hallittaan apt paketteja
	- name:
		- Määritellään paketin nimi
	 - state:
		 - Määritellään paketin tila esim. latest = uusin paketti
	- update_cache
		- Hakee uusimmat paketit

#### file:
ESIMERKKI:
```yml
	- name: Change file ownership, group and permissions
	  ansible.builtin.file:
	    path: etc/foo.conf
	    owner: foo
	    group: foo
	    mode: '0644'
```
- Hallittaan tiedostoja ja/tai kansioita
	- path:
		- Määritellään mitä tiedostoa tai kansiota hallitaan
	 - recurse:
		 - Voidaan määritellä että atribuutit menevet recursiivisesti kaikkiin kansiossa oleviin tiedostoihin
	- src:
		- Määritellään mihin tiedostoon objekti linkataan
	- state:
		- Jos ei ole kansiont poistetaan rekursiivisesti
	- owner:
		- Määritellään objektin omistaja
	 - group:
		 - Määritellään objektin ryhmä
	 - mode:
		 - Määritellään objektin luvat

#### user:
ESIMERKKI:
```yml
- name: Add the user 'johnd' with a specific uid and a primary group of 'admin'
	  ansible.builtin.user:
	    name: johnd
	    comment: John Doe
	    uid: 1040
	    group: admin
```
- Hallittaan käyttäjiä ja niiden atribuutteja
	- name:
		- Määritellään käyttäjän nimi
	 - create_home:
		 - Määritellään luodaanko käyttäjälle koti kansiota
	- comment:
		- Voidaan määritellä kuvaus käyttäjästä
	- groups:
		- Määritellään mihin ryhmiin käyttäjä kuuluu
	- shell:
		- Voidaan määritellä käyttäjän shell esim. bash, fish jne.
	 - state:
		 - Määritellään kuulko käyttäjän olla olemassa vai ei
	 - system:
		 - Määritellään onko käyttäjä järjestelmä käyttäjä

#### authorized_key
ESIMERKKI:
```yml
- name: Set authorized key taken from file
	  ansible.posix.authorized_key:
	    user: charlie
	    state: present
	    key: "{{ lookup('file', '/homecharlie/.ssh/id_rsa.pub') }}"
```
- Hallittaan käyttäjän ssh avaimia
	- user:
		- Käyttäjä nimi joiden avaimia hallitaan
	 - key:
		 - Määritellään ssh julkinen avain
## a. Sudoless

Tein ensiksi `/etc/sudoers.d/sudoless` tiedoston johon voin lisätä käyttäjiä, jotka voivat käyttää sudoa ilman salasanaa
- `sudo visudo /etc/sudoers.d/sudoless`
- `%sudoless ALL = (ALL) NOPASSWD: ALL`

Tämän jälkeen loin uuden ryhmän
- `sudo groupadd sudoless`

Sitten tein uuden käyttäjän ansiblea varten ja lisäsin sen **sudoless** ryhmään
- `sudo adduser ansible`
- `sudo adduser ansible sudoless`
 ![](h2%20Voileipä.png)

Tarkistin vielä että käyttäjä on varmasti ryhmässä
- `sudo groups ansible`
![](h2%20Voileipä-1.png)

Jotta pääsin kirjautumaan ansible käyttäjälle siirsin julkisen avaimen ansiblen .ssh kansioon
- `sudo mkdir /home/ansible/.ssh`
- `sudo cp authorized_keys /home/ansible/.ssh`
- `sudo chown -R [](ansible:ansible) /home/ansible/.ssh`

Sitten kirjauduin ansible käyttäjällä sisään ja testasin toimiiko sudo ilman salasanaa
- `ssh ansible@80.47.224.23`
- `sudo ls -la`
![](h2%20Voileipä-2.png)

## b. Antero

Tein uuden roolin ja siihen oikeat kansiot
- `mkdir -p ansible/roles/sudoless/tasks`
![](h2%20Voileipä-3.png)

Sitten tein **tasks** kansioon main.yml tiedoston

```yml
- group:
    name: "sudoless"
    state: present
- user:
    name: "anatude"
    state: present
    groups: ["sudoless", "sudo", "adm"]
- authorized_key:
    user: "anatude"
    key: "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMrbRnwUrO1GUif+L+hqltcthayHkBLjERpEsNGQvVnT debian@debian"
- copy:
    dest: "/etc/sudoers.d/sudoless"
    content: "%sudoless ALL = (ALL) NOPASSWD: ALL\n"
    owner: "root"
    group: "root"
    mode: "0644"
````
Muutin myös **site.yml** tiedostoa, jotta sudoless roolin taski tehdään

```yml
- hosts: all
  become: true
  roles:
    - sudoless
```

Sitten kokeilin toimiiko
- `ansible-playbook site.yml --ask-become-password`
![](h2%20Voileipä-4.png)

Komento näytti siltä että se toimi mutta kokeilin vielä
- koitin kirjautua sisään anatude käyttäjällä
	- `ssh anatude@localhost`
	![](h2%20Voileipä-5.png)
- Koitin jotain sudo komentoa kun olin kirjautunut sisään anatude käyttäjänä
	- `sudo ls -la`
	![](h2%20Voileipä-6.png)

## c. package

Ensiksi tein uuden roolin
- `mkdir roles/packages/tasks`
Sitten tein sinne **main.yml** tiedoston
- `micro roles/packages/tasks/main.yml`
```yml
- apt:
    name:
      - cowsay
      - fastfetch
    state: latest
```

Sitten muutin **site.yml** tiedostoa
``` yml
- hosts: all
  remote_user: anatude 
  become: true
  roles:
    - packages

```
- Lisäsin sinne `remote_user: anatude` jotta ansible käyttää uuttaa käyttäjää joka ei tarvitse sudo salasanaa

Sitten kokeilin toimiiko
- `ansible-playbook site.yml`
![](h2%20Voileipä-7.png)

Katsoin vielä ovatko ohjelmat oikeasti asennettu

- `cowsay hello`
 ![](h2%20Voileipä-8.png)
- `fastfetch`
![](h2%20Voileipä-9.png)

## d. File

Tein uuden ryhmän ja sille **main.yml** tiedoston
- `mkdir -p roles/file/tasks`
- `micro roles/file/tasks/main.yml`

```yml
- copy:
    dest: "/home/anatude/hello"
    content: "Hello\nThis is me\nyes\n"
    owner: "debian"
    group: "root"
    mode: "667"

```

Tämän pitäisi tehdä tiedot **hello** kansioon **/home/anatude** tiedoston omistaja käyttäjä on **debian** ja omistaja ryhmä on **root** tiedoston oikeudet ovat `-rw-rw-rwx`

- ensimmäinen `-` merkitsee että kyseessä on tiedosto eikä kansio jos se olisi `drw-rw-rw-` se tarkottaisi että kyseessä on kansio
- ensimmäinen `rw-` tarkoittaa että tiedoston omistaja käyttäjällä (**debian**) on luku ja kirjoitus oikeudet tiedostoon, mutta ei suoritus oikeuksia
- toinen `rw-` tarkoittaa että tiedoston omistaja ryhmällä (**root**) on luku ja kirjoitus oikeudet tiedostoon, mutta ei suoritus oikeuksia
- viimeinen `rwx` tarkoittaa että kaikilla muilla käyttäjillä jotka eivä ole **debian** ja eivätkä kuulu **root** ryhmään on luku, kirjoitus ja suoritus oikeudet

Kokeilin toimiiko
- `ansible-playbook site.yml`
![](h2%20Voileipä-10.png)

![](h2%20Voileipä-11.png)

![](h2%20Voileipä-12.png)

## e. Jotain muuta

Halusin kokeilla komentoa `get_url`

Tämän komennon avulla latasin **debian 13** asennus tiedoston

Tein taas uuden roolin ja tarvittavat kansiot
- `mkdir -p roles/something/tasks`

Sitten tein sinne **main.yml** tiedoston

```yml
- get_url:
    url: "https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-13.7.0-amd64-netinst.iso"
    dest: "/home/anatude"
    checksum: "sha256:https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/SHA256SUMS"
    owner: "anatude"
    group: "anatude"

```

- `url:` kohdassa määritellään tiedoso joka ladataan
- `dest:` määritellään kohde kansio tiedostolle
- `checksum:` tarkistaa tiedoston **hash** arvon, jotta voin olla varma ettei se ole korruptoitunut tms. ja määrittelin komentoon että se **hash** arvo ladataan myös niin sitä ei tarvitse manuaalisesti kirjoittaa komentoon
- `owner:` tiedoston omistaja käyttäjä
- `group:` tiedoston omistaja ryhmä

![](h2%20Voileipä-13.png)

![](h2%20Voileipä-14.png)