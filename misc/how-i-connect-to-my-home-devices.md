# How I Connect To My Home Devices

Nowadays, more and more inteligent devices become common, maybe you want to check some files on your computer which is still in your home but you're in office, **how**?

It's my experience, and I hope this is helpful to you.  
## Prerequisites
1. A router which supports custom scripts
2. A domain

## Preparation
### 1. Manage your domain
Make sure the service provider of you domain supports updating DNS records through API. If it doesn't, you can manage your domain on other providers, such as Cloudflare. I put my domain on Cloudflare, so in the following parts, I will take this as the example.
### 2. Get the API and tokens
Usually, you can find the APIs and usage from the [official documentations](https://api.cloudflare.com/).
#### 2.1 Get Zone ID
1. Log in Cloudflare with your account and password;
2. Select the domain you want to use;
3. And you can find the zone ID at the right pane, also you can find your account ID nearby.

#### 2.2 Get the 
