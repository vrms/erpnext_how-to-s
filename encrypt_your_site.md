### Pre-requisites

- you need to have a domain (www.mysite.org) or subdomain (erp.mysite.org) in order to use let's encrypt. 
- It won't work with just an IP address  
  


---

### 1. setup DNS multitenancy

```
cd /home/frappe/frappe-bench
bench config dns_multitenant on
```
this is needed **even if** you only use a single site

### 2. point your (sub)domain to the IP of your ERPNext server

- this is an "A record" in the DNS settings of your server
- test, wheter your ERPnext instance shows up under that domain (erp.mysite.org)

### 3. move your site

as frappe system user

`mv /home/frappe/frappe-bench/sites/site1.local /home/frappe/frappe-bench/sites/erp.mysite.org`


### 3. apply the let's encrypt certificate

you need to have a user with `sudo` power in order to do this. 
The Frappe system user in general should not be a `sudo` user so you'll have another sudo user for this

```
su - [sudo-user]
cd /home/frappe/frappe-bench
sudo bench setup lets-encrypt erp.mysite.org
```
there will be a couple of question during the installation which you can all agree to.  
One of them is subsribing to a newsletter for let's encrypt. That's the one you can not agree to if you wish.

### 4. update `currentsite.txt`

```
vim ~/frappe/frappe-bench/sites/currensite.txt
```
replace **`site1.local`** with **`erp.mysite.org`**, otherwise i.e. the bench mariadb command will not work anymore because it is looking for `site1.local`.

---

#### Sources

- [Multitenant-Setup](https://github.com/frappe/bench/wiki/Multitenant-Setup)
- [Let's encrypt](https://discuss.erpnext.com/t/issue-setting-up-letsencrypt-ssl/21221/6)
