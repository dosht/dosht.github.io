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
If you are using [Kubernetes]() and you still have to write all [kubectl]() commands like this: `kubectl --context my-ctx --namespace my-namespace ...`, then you are probably wasting so much time in typing repeated things and memorizing commands or keeping too many notes. There are a lot of useful tools that can make working with kubectl much more productive. I will list in this post some of those tools that saved time for me and made me more confident while working on Kubernetes.

[image]: https://lh3.googleusercontent.com/DiI__pBSBd5EkZawmRaORMIG3QkcKxfV8Fuf19DMlvLG6Pr6foGdkfqjhzioAKcUAo2nmihDsPz4dMhnotp6DM6yvtFsNVSO8TdBSdI6JcrhDidfyTf3Mm_2opH6pv8YC3ANhEpO7AAnigxBoslV5jSHZ6Wk99ZNL-Ff1UfM1hK_SpcpRhRmPooQk2O20l6TRdBymrQ-JOA7Z7Noy-o4_18nVBa-6W4x3Cb8ACVJdZ2ddA9EY9XbMCqwEWX6hR61sqLWt9TbheyHjzVqImyqTnl9nwBG5EnTYm7Ecsc29nTFoDvFE1XhrS6n6GroFHfw4IvWejDmpq3O6OYhVtvM2JyPEzVk_zkKv1PTj6xs3nIpA9qu0x6hqXgKIAiLVvTKSIGLNzfnvEjCuRXCcyFi45IHfrIZlmIOnvAF7V1pDBMxrjQLTUEwgOZBxnR62LAmndABjt84ZIjNhrrhTRD9UhcFWX5I81UdN3x3-BgQg_DLCPfOyfpT9yBVvQ4Q1XGZXlDMlG7iKcdeYBf3t9NlwsFG8gF2UrpVvEstYRfi5lxacpEKqlkrCIXkNi-R5PjXmUuWXyH5sOObmUR-KERPrnJm3D7tcXpiGYZSYA3RJizwQkO5I4D4MPMg7oV3LW38udTk2JxpWNMWXttbNNmR9up7ElKaKMwW22Tkh7gXDnNevaUogv04e7iJTmvycNI=w1254-h941-no?authuser=0 image1

# Before you begin

![text][image]

You need to have [kubectl]() installed and some K8s cluster to play with. Maybe you have a test cluster already. If not, you can just make it simple and create a Minikube cluster for learning like in [here](https://kubernetes.io/docs/setup/learning-environment/minikube/).

This post is a bit openionated because I chose only tools that worked well with me after trying many others. I'm also using [oh-my-zsh](), so some those tools could be different on other shells like bash or fish.


1. Auto-complete

Open `~/.zsrch` and find the plugins section and add `kubectl` to the list. That's it nothing more. This will give the tab-complettion for almost everything related to `kubectl`. It will auto-complete commands, switches and even resources names.

IMG

If you are not using oh-my-zsh, you can find how to set up autocompletion for your environment in Kubernetes docs [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion)

2. ZSH PROMPT

To avoid deleting resources on production by mistake (especially if the devops trust you and give all permissions!), I like to always see the current context/namespace in my shell prompt. To do so in oh-my-zsh is very easy. Just open `~/.zshrc` and add `kube-ps1` to your plugins. Then add this line:

```
PROMPT='$(kube_ps1) '$PROMPT
```

That will override `PROMPT` variable to include `$(kube_ps1)`. You can put `$(kube_ps1)` before or after `'$PROMPT`.

IMG

This will make you shell prompt something like that:

IMG

3. 'k' Alias
Defining some aliases is also a good idea and still works with autocompletion. 

4. Context Switcher
5. Namespace Switcher
6. Kubefwd
7. Stern
