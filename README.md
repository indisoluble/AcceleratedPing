# AcceleratedPing

## Use Case

You want to connect to a game server far from your region, for example you need to connect to a server located in UK from Australia (or vice versa).

## Proposal

A direct connection over long distances usually routes your TCP packages over many different networks which produces a high ping and increased lag. However, if there is an AWS region in the country where the server is located, you can use [AWS Global Accelerator](https://aws.amazon.com/global-accelerator) to get rid off most of the intermediate networks.

## What

You can use the Cloudformation file in this repository:
* To create a private TCP proxy in the destination region that will only accept connections from your computer and direct your traffic to the game server
* And to enable [AWS Global Accelerator](https://aws.amazon.com/global-accelerator) to speed up your traffic to the proxy

### Design

![Design](/Images/Design.jpg?raw=true)

### Usage

7 mandatory values to inform:
* Your **public IP** - The TCP proxy will only accept connections coming from this IP
* **Server domain** (or IP) - The domain (or IP) of the server
* **Server port** - If you do not know it, probably it is the default port for the game
* **Proxy port** - As the server, the proxy also has a port. To keep things simple, you can specify here the same port used in the server
* Proxy stats - The TCP proxy is actually [HAProxy](http://www.haproxy.org/) and, out of the box, it provides some usage stats, that is why you also have to inform:
  * Proxy stats port - Port where the webpage with the stats will be presented
  * Proxy stats username - Username authorized to check the stats
  * Proxy stats password - Password of the user authorized to check the stats

1 optional value:
* Instance type - Set to `t2.micro` given that it is usually part of AWS **free tier** but you can use any **64-bit x86** instance

### Outputs

Once the Cloudformation stack is deployed, in the `Output` tab you will find the following values:
* **Proxy domain:port** - To say in another way, the URL you have to use now to connect to the game server
* **Proxy stats domain:port** - The URL you have to copy in your browser to check the usage stats
* **Proxy stats username** - You informed this values when you created the stack, this info is here so all the relevant information related to the stats is in one place
* **Proxy stats password** - You informed this values when you created the stack, this info is here so all the relevant information related to the stats is in one place
* **Proxy instance ID** - The ID of the EC2 instance executing the proxy. You might need this information in case you want to have a look inside the EC2 instance. Notice that you can easily connect to the EC2 instance with Session Manager

## WARNING WARNING WARNING

This infrastructure is not free, even if you are **free tier** eligible. **Keep an eye in you AWS bill**.
