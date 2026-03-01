# Cloudflare Gateway Block Ads
A GitHub Actions script to automatically create and update Cloudflare Zero Trust Gateway ad blocking lists and policy.

The script works by periodically downloading the [OISD](https://oisd.nl/) small blocklist, splitting it into smaller chunks, uploading it to Cloudflare as multiple Domains lists, and then creating a policy that blocks all traffic to the domains in the lists.

It does not use the Cloudflare API unnecessarily as it only updates the Domains lists if the OSID blocklist has changed since the last run.

## Setup

### Cloudflare
First, you should ensure you have a Cloudflare account with a Zero Trust subscription. The free plan is sufficient for this script. You can sign up at [https://dash.cloudflare.com/sign-up/teams](https://dash.cloudflare.com/sign-up/teams).

You will then need to create a Cloudflare API token. You can do this at [https://dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens). There click on create token --> Create Custom Token --> set permission `Account: Zero Trust: Edit` and `Account: Firewall Access Rules: Edit` then continue to summary --> create token.

<img width="957" alt="Screenshot 2025-05-26 203059 (1)" src="https://github.com/user-attachments/assets/3df73a91-30ac-491a-9138-2af2fdc7482c" />

You will also need to find your Cloudflare account ID. You can find this by going to the [Account Home](https://dash.cloudflare.com/) page in the Cloudflare dashboard and copying the account ID from the URL. It should be a 32 character string of letters and numbers.

<img width="1279" alt="Screenshot 2025-05-26 203736" src="https://github.com/user-attachments/assets/c092603e-8ef7-4893-b2b2-282a99b9fa1f" />

Please take note of the API token and account ID for use in the next step.

### GitHub
Next you should make a fork of this project on GitHub. You can do this by clicking the "Fork" button in the top right of the page.

Next you must input the Cloudflare API token into your GitHub fork as a secret. You can do this by going to the "Settings" tab of your fork, then under the "Secrets and variables" dropdown in the sidebar, click "Actions". Click the "New repository secret" button and add a secret with the name `API_TOKEN` and the value of your Cloudflare API key. Create another secret with the name `ACCOUNT_ID` and the value of your Cloudflare account ID. 

you should ensure the GitHub actions are enabled for your fork. You can do this by going to the "Settings" tab of your fork, then under the "Actions" dropdown in the sidebar, click "General". Ensure the all actions and reusable workflows are permitted, and that workflow has read and write permissions.

Finally, go to Actions Accept the shown warning then go to "Block Ads Update" in the left side and enable the github action then click on run work flow and run it. afterwards github will run the action once every day.

<img width="1280" alt="Screenshot 2025-05-26 205121" src="https://github.com/user-attachments/assets/ed57ba2c-c2f2-44a9-bf43-b044ffbce105" />

## How to use on ios

First go to [Cloudflare One Dashboard](https://one.dash.cloudflare.com/) then to Gateway --> DNS locations --> Add location now in Add DNS locations enable the following settings

<img width="1292" alt="Screenshot 2025-05-26 205949" src="https://github.com/user-attachments/assets/281b8a02-2ea6-468f-a7db-2db58eaffcfd" />

Then protect your endpoint keep everything as default 

<img width="1070" alt="Screenshot 2025-05-26 210028" src="https://github.com/user-attachments/assets/16577f84-69e1-485d-8958-c72268427f0d" />

Then continue to "review the setup" page and to "Confirmation" page, here copy the "DNS over HTTPS" address

<img width="1057" alt="Screenshot 2025-05-26 211418 (1) (1)" src="https://github.com/user-attachments/assets/458e785a-0a30-4e09-9e28-ea582f3f8a29" />

Open terminal/Powershell and execute the following command:

Note: only type the `subdomain` in the command `https://6yd73hgb51.cloudflare-gateway.com/dns-query` (`https://<your-subdomain>.cloudflare-gateway.com/dns-query`) --> `6yd73hgb51.cloudflare-gateway.com` 

- for windows

  - IPV4 Address
    ```bash
    nslookup -type=A <your-subdomain>.cloudflare-gateway.com
    ```
  - IPV6 Address
    ```bash
    nslookup -type=AAAA <your-subdomain>.cloudflare-gateway.com
    ```
- for linux/mac
  - IPV4 Address
    ```bash
    dig +short A <your-subdomain>.cloudflare-gateway.com
    ```
  - IPV6 Address
    ```bash
    dig +short AAAA <your-subdomain>.cloudflare-gateway.com
    ```
    
<img width="861" alt="Screenshot 2025-05-26 213049" src="https://github.com/user-attachments/assets/83dc8c16-ef13-475c-b22e-35c3568454da" />

Save the IPV4 and IPV6 address along with the "DNS over HTTPS" address

Finally go to [https://dns.notjakob.com/tool.html](https://dns.notjakob.com/tool.html) and fill the form using the data we got from previous steps and hit add profile

<img width="1255" alt="Screenshot 2025-05-26 215326" src="https://github.com/user-attachments/assets/378e4a09-f061-43e2-bf5e-472d3b0b1e36" />

Now download the profile and send it to your ios device using mail or other apps 

<img width="1112" alt="Screenshot 2025-05-26 215342" src="https://github.com/user-attachments/assets/85149ad1-c4d8-481f-b9a1-90931909c4bd" />

Once downloaded in your ios device, go to files then to the downloaded folder and click on it now follow [this to install the profile](https://support.apple.com/en-in/102400), once done change the dns to the newly installed DNS Profile.

Enjoy!
