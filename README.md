# small-business
These containers solve the problem of keeping track of goods, services, finances and employees for small businesses.

1. Copy **.env** and **docker-compose.yml** to your server
2. 
   > docker compose up -d
3. Go to **http://your-server-ip:8080//if/flow/initial-setup/** and create the admin IDP account
4. Admin Interface -> System -> Certificates -> Generate 
   Input Grist, grist, 3650 and **Generate**
5. Tap to arrow near the Grist name and download Certificate and PrivateKey.
6. Rename files to **cert.pem** and **private.pem**
7. Go to **Providers** and create new one.
	1. SAML Provider
	2. Name: Grist
	3. Authentication Flow: default-authentication-flow (Welcome to authentik!)
	4. Authorization Flow: default-provider-authorization-implicit-consent (Authorize Application)
	5.  ACS URL: https://grist.domain.com/saml/assert 
	6. Binding: POST
	7. Signing Certificate: Grist
	8. Verification Certificate: Grist
8. Go to Applications and create new one.
	1. Name: Grist
	2. Slig: grist
	3. Provider: Grist
	4. Launch URL: https://grist.domain.com
	5. Icon: Anyone you want
9. Paste files **cert.pem** and **private.pem** to your server config/grist
10. Setting up **Nginx Proxy Manager**
	1. Go to **http://your-server-ip:81** and use **admin@example.com** **changeme**
	2. Go to Hosts and **Add proxy host**
		1. Domain Names: grist.yourdomain.com
		2. Scheme: http
		3. Forward Hostname / IP: your local ip address (for example 192.168.1.55)
		4. Forward Port: 8484
		5. WebSocket Support - YES
		6. SSL
			1. Request a new SSL cert
			2. Force SSL
			3. I agree... 
			
		Add next one:
			PORT 81 - NGINX
			PORT 8080 - IDP

