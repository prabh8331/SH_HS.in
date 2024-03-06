my google domain - prabh.de

add custome name server in google domain

to check if it is working - 

sudo apt install whois

whois prabh.de

this will show the name servers in this 


--created a cloudflare account 
-- and this will give name server 

add these name server in google domain


----------------create tunnel 

Step 1. buy a domain name using godady 




docker run -d  --restart always cloudflare/cloudflared:latest tunnel --no-autoupdate run --token <token key>