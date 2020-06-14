---
layout: post
title:  "Productivity Tools for Kubernetes"
author: mou
categories: [ devops ]
image: assets/images/2020-04-06-productivity-tools-for-kubernetes.jpg
featured: flase
hidden: false
sitemap: true
---
If you are using [Kubernetes][Kubernetes] and you still have to write all [kubectl][[kubectl] commands like this: `kubectl --context my-ctx --namespace my-namespace ...`, then you are probably wasting so much time in typing repeated things and memorizing commands or keeping too many notes. There are a lot of useful tools that can make working with kubectl much more productive. I will list in this post some of those tools that saved time for me and made me more confident while working on Kubernetes.

# Before you begin

You need to have [kubectl][kubectl] installed and some K8s cluster to play with. Maybe you have a test cluster already. If not, you can just make it simple and create a Minikube cluster for learning like in [here][minikube].

This post is a bit openionated because I chose only tools that worked well with me after trying many others. I'm also using [oh-my-zsh][oh-my-zsh], so some those tools could be different on other shells like bash or fish.


1. Auto-complete

Open `~/.zsrch` and find the plugins section and add `kubectl` to the list. That's it nothing more. This will give the tab-complettion for almost everything related to `kubectl`. It will auto-complete commands, switches and even resources names.

![kubectl][kubectl img]

If you are not using oh-my-zsh, you can find how to set up autocompletion for your environment in Kubernetes docs [here][enabling-shell-autocompletion]

2. ZSH PROMPT

To avoid deleting resources on production by mistake (especially if the devops trust you and give all permissions!), I like to always see the current context/namespace in my shell prompt. To do so in oh-my-zsh is very easy. Just open `~/.zshrc` and add `kube-ps1` to your plugins. Then add this line:

```
PROMPT='$(kube_ps1) '$PROMPT
```

That will override `PROMPT` variable to include `$(kube_ps1)`. You can put `$(kube_ps1)` before or after `'$PROMPT`.

![kube_ps1][kube_ps1]

This will make you shell prompt something like that:

![kube_ps1_shell][kube_ps1_shell]

3. 'k' Alias
Defining some aliases is also a good idea and still works with autocompletion. 

4. Context Switcher
5. Namespace Switcher
6. Kubefwd
7. Stern


[Kubernetes]: https://kubernetes.io/
[kubectl]: https://kubernetes.io/docs/reference/kubectl/overview/
[minikube]: https://kubernetes.io/docs/setup/learning-environment/minikube/
[oh-my-zsh]: https://github.com/ohmyzsh/ohmyzsh
[enabling-shell-autocompletion]: https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion

[kubectl img]: https://lh3.googleusercontent.com/9cNlg9f4nUU2qNfe9n8EHpHHEafrjzL6jyy9dRKhLPchC65VaTuaHPqU2m-lTWFrEKF-rmU26s3c_ie6xnTOT8PYgAtiaVUu1R7fNsC3adSesSZhi9zDy3IhGoJc6w7WssZnnAGZ1dfm4cGmvylPtjLr6bz2gS68fx3zcUwLnN9r776crzvaOnmYBcCpQIPriynLMmgtV9uFHy-2gA66orCOmorEIySBD2hYX7vDTE3SYmwxXL5ZevEdZPI853ei-mA69Z7VQa_YXhcHXaK_n7eausG4_0KCgLT79-4Adj4gjmwjSwwpLHEo3yMeFE1_xdRNox6yqWGBsKVZVep3nmM_q3jhvU_yk7399g3msFV-QE1VQvJfs_nVp9ltKHBm1P4YQM5AV1Mwft2FxRFCHNQiNZjOYwWQOebwAS4eUcaBb0CrPgAQIlsUmTgePppdPSIVl_rzdmbcXYNfke6N3lAh4YABCPAc5SrnWCmIQMUNJbSqVL3Wgos0k-gJud5qXHCpYDnjI8vKfuDK8kSUCLv4VgBBxVxPCsxRZwQh9Vgp9teWNRchKQWyi2qew5Llexme6FW7HLgNtTFsd2v6OAx2eOTmiwKXFoE6o-ICgkpsSk1TeAJS8xdk-fv1srV9yFQ4cCeM2565aftztASzdk-7OH75JTlMl2NRZZFMbkjO860ACw34cBtMYh_5UJg=w223-h255-no?authuser=0 "kubectl"

[kube_ps1]: https://lh3.googleusercontent.com/TP1kdX_9pOx0mJ64jvACaaub8lmhxo8_-Rkk1kyXMvkvVSRJ4iNVFEBUfDvpvv-Q-qQVwjQxIskbKIvqONd3Y35CA3fqjvcjXqmt1Q5kQlktNydkgF2oRmWkL8hQ0rWt2O-hmlLiK4YZYUFaarGWDgT0oo66jFe7XGu4o51eEhRXqRQqGFk6YzlxFLnw3_Z3o3YFG_ZqQmtPhCr3H8hUZGsT0yZrJx6bHWZPZlzmVAut36h6KR_tcJpJLU1B7w4nKVElrlZDMWZTgpZFFHtrhn1fEehD0XdNZPo3KcU4OPBBQUx58pL71qmus1OqEAW2mqf0Xcyo70G_f6RKCSqElTnjmQNFsl96GnZ-6s2AtEtWxLPTOKyd1Qhc-9Wmd08AKrVt0Ttdr3U6siUQp-zGBQExAy09tnmYMKunPU0223ecBZ7iFXY4Qx-1AS1sVU6GL-m3r8jD6jfCUJl4ZzqZBMy39YmUbJ8iv1hedf6GhOk5zTS_UUrbf7ymPXU3iRUyvJikZZ0Kul6Cmn66Eoem0LEDS0v73vDqn8h4KPow9iPGMcV_byDaQOrI2jPVZBOuILHbQKus3SHFn43LXl4YtvHlLnWfT4PXKW97NXJ55SD_QBkRv0q6MpBve9LFbFxJl8RLs_UQpVxW2TJKjhI04UQ8bLAbHUCCcG5PFxysMpbswzDO4ldnbUxWlGI9ETk=w405-h285-no?authuser=0 "kube_ps1"

[kube_ps1_shell]: https://lh3.googleusercontent.com/8o27ghXJqjQ_YzjPLHEiWbMcbGCRfHWV2AW05TcqzUwRnX58PPrk1DVDcEY6CR8H7_Xd0noZPi53nRMMyDvTsF1idnhK1HYzkWzgungjbryzNW8NXCJsAtjuJwpbH_BSah3K4ng2MsFkOs5pEQAgQx9L9sMYyvU4ayOwZADQ-lSHWn2V9dESpG9-wntuIAqZC1KXFpHxMcgZTEkBH6skD9Xkw3dofKCw9gKeH366JtB3JpfP-r8lZPMxMG2OhLvQNzjHwbppyg5Q1owwQEq5ACw8ExhBYMYPwvOb8kTzKz3d5iiFs9DKr7aAVDaQ9-3F04mrIGntDWCC5eyqtBX86nMCo0bLDKDCmbtF4P6IDl72eLRlMG9TtYA-hFamuKPMiysW2741PpPnQ4EmmoLMcyxVCYY2OWXH9eOl3JEAQOAUL-yi9jbFIU5DIbnlrKCFIRcBohn_-3-Uh6bfYGZmjLVpkAdgDReh1JNlXI17pFCcO13rbPH1AqygfmAd6X6uRvQq0sAO03FH2f4ZC6e1IdJ_qE7FjE8OtouPp_68XpyjlUq7FeJJFStINyeKuXx62IRi1yckLiJaXWYp99PNex2utkkvypsSVCghy3h-ANQSXmRWonlnKqdM5NqR-CxAB18VbDOYI3Lh0da5LM52Q9b-ZyM0v2IlVFUlmDgNkldoXg8cypB_gKGFdvj3v9w=w581-h83-no?authuser=0 "kube_ps1_shell"
