## x. Lue ja tiivistä

## y. 
## a. Apassi

- Asensin **Apachen** komenolla
	- `sudo apt install apache2`
- Laitoin **Apachen** päälle komenolla
	- `sudo systemctl enable --now apache2`
- Menin selaimella osoitteeseen **localhost**
![](h3%20Demoni.png)

- Jotta kuka tahansa voisi muokata sivua ilman **sudo** oikeuksia muutin tiedoston **/var/www/html/index.html** oikeude niin että ne on `-rw-rw-rw-` komennolla
	- `sudo chmod 666 /var/www/html/index.html`
![](h3%20Demoni-1.png)
## b. Moottorix
- Asensin **Nginx** komenolla
	- `sudo apt install nginx`
- Sitten menin osoitteeseen `localhost` selaimella
![](h3%20Demoni-2.png)

- Jotta kuka tahansa voisi muokata sivua ilman **sudo** oikeuksia muutin tiedoston **/var/www/html/index.html** oikeude niin että ne on `-rwxrwxrwx` komennolla
	- `sudo chmod 777 /var/www/html/index.nginx-debian.html`
	![](h3%20Demoni-3.png)
	
## c. Automoottorix

- Tein ensin `ansible/roles/nginx/tasks/main.yml` tiedoston

```yml
- apt:
    name: nginx
    state: present
```

- Tämä asentaa nginx koneelle
- Sitten tein `ansible/roles/nginx/handlers/main.yml` tiedoston

```yml
- name: restart nginx
  systemd:
    name: nginx
    state: restarted
```

- Sitten kokeilin `ansible-playbook site.yml`
![](h3%20Demoni-4.png)

![](h3%20Demoni-5.png)