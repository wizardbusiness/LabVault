##### ***Use To*** Create a Tailscale login, which is a prerequisite for using Tailscale

#### Prerequisites
- [[<Internal Link Here>]]
#### More Resources
1. [Tailscale Quickstart][1]
2. 

[1]: https://tailscale.com/docs/how-to/quickstart
#### Related Docs
- [[<Related Doc>]]
### *Info*
##### **Q:** Why do i need to create a Tailscale login?  

****

### 1. Go to [tailscale.com/start](https://tailscale.com/start)- *Before clicking anything, proceed to **Step 2** of this guide.* 
```ad-img
![[image-1.png]]
```

### 2. Choose a supported native identity provider.
***Note: You can also choose a custom identity provider but that's out of the scope of this guide***

```ad-important
**Tailscale does not support sign-up with email addresses. By design, Tailscale is not an identity provider—there are no Tailscale passwords.**
```

##### Tailscale supports the following identity providers
- [Apple](https://tailscale.com/docs/integrations/identity/apple-sso)
- [Google](https://tailscale.com/docs/integrations/identity/google-sso) *(including Gmail and Google Workspace/G Suite)*
- [Microsoft](https://tailscale.com/docs/integrations/identity/entra), *(including Microsoft Accounts, Office365, Active Directory, and Microsoft Entra ID)*
- [Okta](https://tailscale.com/docs/integrations/identity/okta)
- [Okta](https://tailscale.com/docs/integrations/identity/okta)

```ad-tip
title: #### **Important Considerations When Choosing an identity provider**

---
#### ***>> Switching providers later is a pain in the ass***
- ***Not all providers allow you to change your mind later***
  - **If you want to change your provider you've got to actually call Tailscale Support and request the change** 
	- **If you choose Github or Apple, you can't migrate your provider later at all.**
---  
#### **>> Your Tailscale network's security is only as a good as your provider account security***
- ***Set up MFA, a strong password, and recovery options.*** 
- ***Use a password manager to secure your account credentials***
- ***Make sure your provider account login credentials are accessible - if you can't login to that account, you'll lose access to your tailscale network too*** 

#### **>> Home use VS Enterprise - Whether you plan to scale your deployment should inform your choice.**
-  **The downside to choosing scaleability is friction. Choosing an enterprise-ready deployment path will add a certain amount of complexity even with a low device count.** 
  - **If you have tons of user in an enterprise environment who all need identity management that complexity and friction will be less than if you choose an option that's easy to set up but scales poorly.**
  - ** If you just want to have access to your pc when travelling, that complexity is going to be a huge pain in the ass for no reason.**

##### **> Home and Casual use**
- **Choose an easy option without much overhead** *Ie personal gmail*

##### **> Entrerprise use**
- ***If you have existing identity management infrastructure that uses a supported provider, use that for Tailscaler***
- **If you're building greenfield infrastructure and tailscale is being used as a core part of your core ifnrastructure management architecture, think about how tailscale will integrate into your automation and identity management strategies.**
- ***Choose a provider that supports `SCIM` (System for Cross-domain Identity Management). Google Workspace or Microsoft Entra ID both support this which means user/group provisioning can be automated.***

```
##### For more info see the tailscale docs page -> [Supported SSO identity Providers](https://tailscale.com/docs/integrations/identity)

### 3. Select the identity provder you want to use for Tailscale. 
#### 1. Click your provider and sign in. Read the Provider data access disclosure and click 'Continue' if you still want this to be your Tailscale provider. 

#### 2. Fill out the onboarding form. 
1. **Fill out 'Product Needs and Usecases'. Then click 'Next: Add your first device'**
   ```ad-img
   ![[image-4.png]]
   ```
   2. ****