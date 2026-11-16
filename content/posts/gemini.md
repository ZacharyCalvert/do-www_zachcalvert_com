+++
date = '2026-11-16T08:08:00-05:00'
draft = false
title = 'Planned Wallet'
tags = ["architecture", "engineering", "DevSecOps", "AI"]
weight = 179
+++

# Pet Project with Gemini

I wrote about my impression with Gemini and Antigravity on [Medium](https://medium.com/@zachdoesdevops/week-of-gemini-antigravity-2d36a0a97f09?sharedUserId=zachdoesdevops).  This post is more about my pet project / possible startup idea.  At a minimum, this is a tool I'd like to have completed for myself.  More on that in a moment.

First, a bit of an overview of the project and what I want to do / why I want to do it.  I purchased the plannedwallet.com domain name on September 23rd, 2008 - over 18 years ago.  I had to go pull my email records to find out when I bought it and am blown away I sat on this idea for 18 years.  I've always been a saver (even as a kid), and I've always, ALWAYS, been completely unwilling to link any application or third party system to my bank, brokerage/trading, or crypto accounts.  I don't like the idea of a breach of some central service exposing a ton of my financial accounts and personal wealth to bad actors.  

Next, for more history, I don't keep all of my savings in a single bank.  I had a family member who had the IRS accuse them and their business of fraud and tax evasion.  Without trial, the bank that had their business and personal checking account froze assets at the request of the IRS.  It took two weeks for this person to sort everything out with the IRS, but in the meantime, they were asking for friends to purchase things and using cash where possible.  It was a very, very stressful time and incredibly eye opening as to how paying for things like food becomes impossible without a functioning bank account.  I keep my business, checking, and savings accounts in different banks for this reason.  This event happened over twenty years ago, but I still remember how stressed they were begging the family for help.

So, what's the plannedwallet.com idea?  I want to build a service where I can track my checking, savings, and trading accounts, assets including crypto, precious metals I keep in a safe deposit box, and collectable items such as MTG and Pokemon cards.  I want to be able to enter values of items, such as the worth of the trading card on a given day, and trend it over time.  I want to be able to track all of this and show graphs of combined account and asset perspectives.  Finally, I want to be able to track and trend liquid assets vs retirement assets, as well as trend average spending per day over a year to provide a trajectory of financial ruin ($0 in liquid assets).

I've traditionally done this in a spreadsheet.  I've done it this way for 25 years because every single time I go look at some financial service offering, it:
- Wants to link to my accounts / banks
- Doesn't offer customized asset trending (such as playing cards)
- Doesn't offer financial trajectories based on zero income or daily income
- Doesn't trend what I'm saving / how much I should be saving

So, that's what I'm currently building using Gemini as my coder, with me as the architect and product owner.  

For a bit of a taste as to what I've observed with Gemini, I was able to draft the following prompt and get a fully implemented hosting infrastructure on Azure cloud:
> I have a golang 12 factor microservice built into a docker scratch container as well as static code NextJS for the front-end.  The back-end needs to persist data to an Azure managed postgres instance.  My cloud provider is Azure and I want services geofenced to the US only.  I own an Azure subscription, and can generate Enterprise Applications for logging in and automating the IaC.  I want to use Terraform for the IaC.  Azure for the cloud.  Terraform state is to be stored in an Azure blob container.
> 
> The service is for the domain "plannedwallet.com" which I have purchased in a separate DNS registrar.  I will need to instruct Azure to use this domain.
> 
> The initial release is for stage.plannedwallet.com which will be used for testing prior to production.  The second phase and release is for www.plannedwallet.com.
> 
> Back-end API services are mounted on /api, so I am hoping we can serve static content from / and API content from /api via the Azure Front Door.
> 
> I need Azure Front Door with WAF behaviors, including geofencing to the US.  
> 
> I need to be able to configure environment variables for the containers being launched in Azure to describe the environment including database location, CORS support, and environment run mode.
> 
> Secrets need to be stored in Azure Key Vault, with the secrets treated as environment variables for the container, including a JWT secret 256 bit base64 encoded value.
> 
> I need:
> - Terraform configuring the Azure Front Door, Azure Key Vault, Azure Container Apps
> - Instructions for wiring up my registrar DNS to Azure for the hosting, including TLS configuration.  I will need to wire up configuration for www.plannedwallet.com (production) as well as stage.plannedwallet.com (pre-production testing).  Place DNS instructions in DNS_CONFIG.md
> - Terraform definition of networking from Azure Front Door to the container hosting service.  Kubernetes is too expensive, so I want to use Azure Container Apps (ACS)
> - Terraform for the managed postgres instance, and configure ACA for launching my container with access to postgres
> - A GitHub Action workflow defined to support terraform plan and terraform apply jobs, manually executed with workflow dispatch.  State should go to the same blob storage, but 2 different state file definitions in the blog storage, 1 for prod, 1 for stage.
> - All configuration details required, including secrets / app registration for running this job need to go to GITHUB_CONF.md
> - I want to use GitHub environments (stage, prod) for running the github action workflow, which means the terraform needs to be parameterized in such a way that I can run the stage environment workflow or the prod environment workflow, and all changes come from the GitHub Action environment variables.  
> - Ask me questions regarding anything unclear.  I have admin control of Azure, and admin control of my DNS registrar.  Provide any pitfalls or concerns or questions in the chat.  Anything dangerous or considered risky in terms of massive costs or risks of hacking exposures needs to go to WARNINGS.md regarding the Azure environment we are to provision and manage through Terraform.

Produced:

```
      33 text files.
      30 unique files.
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
HCL                             24            242             52           1343
Markdown                         4            140              0            521
make                             1             24             28            138
YAML                             1             15              2             90
-------------------------------------------------------------------------------
SUM:                            30            421             82           2092
-------------------------------------------------------------------------------
```

The code worked flawlessly once wiring up the Azure IAM federated access on the repo and properly targeted the centralus (eastus and westu3 zones did not offer the PostgreSQL skus I wanted).

So far, so good.  The service is up and running with the intent that I frequently tear down and destroy data at https://stage.plannedwallet.com.  Feel free to check it out if you're curious.  The plan is heavy use features, such as accunts with over 1000 records in a year, or 10,000 records lifetime will need a paid subscription.  For now, I'm just focused on delivery of the features that I want for the tool that I intend to replace my spreadsheets with.