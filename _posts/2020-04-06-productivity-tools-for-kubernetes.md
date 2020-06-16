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

3. Aliases and plugins
## Aliases
Defining some aliases is also a good idea and still works with autocompletion. When you use the ohmyzsh plugin kubectl as descriped above, you will get already some of useful aliases like `k` for `kubectl` and `kaf` for `kubectl apply -f` and you can find the full list in this [readme][Kubectl plugin readme]. You can edit thoses aliases by editing file `~/.oh-my-zsh/plugins/kubectl/kubectl.plugin.zsh` and you can add your own aliases. Another good tool that I personally prefer is described in this [post][kubectl aliases post]. The useful thing about this tool that you can modify its [`generate_aliases.py`] script to generate your own aliases sytanx bases on what makes sense for you.

## Plugins
You can write your own kubectl plugin in a couple of minutes simply by starting its name with `kubectl-` and make it visible in your `PATH` and The plugin can be any executable file. If you want to list all available plugins, type `kubectl plugin list` which will search in you `PATh` for any file starting with `kubectl-` that means you can manage your plugins yourself by storing them in one directory, or install them using brew, apt, ..etc. There is also a plugin manager called [Krew][Krew] where you can install/remove/update plugins from a ceneteral place.

The first plugin I recommend to start with is [kubectx][kubectx]. It's actually 2 plugins from the same author; One of them is for changeing the context and the other is for changing the namespace. If you are also using the oh-my-zsh plugin explained abover, you will instantly see the new context and namespace after changing is your terminal prompt.

6. Kubefwd
7. Stern


[Kubernetes]: https://kubernetes.io/
[kubectl]: https://kubernetes.io/docs/reference/kubectl/overview/
[minikube]: https://kubernetes.io/docs/setup/learning-environment/minikube/
[oh-my-zsh]: https://github.com/ohmyzsh/ohmyzsh
[enabling-shell-autocompletion]: https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion
[Kubectl plugin readme]: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/kubectl
[kubectl aliases]: https://ahmet.im/blog/kubectl-aliases/
[kubectl aliases post]: https://ahmet.im/blog/kubectl-aliases/
[generate_aliases.py]: https://github.com/ahmetb/kubectl-aliases/blob/master/generate_aliases.py
[Krew]: https://github.com/kubernetes-sigs/krew/
[kubectx]: https://github.com/ahmetb/kubectx

[kubectl img]: {{ site.baseurl }}/assets/images/2020-04-06-productivity-tools-for-kubernetes/kubctl-plugin.png "kubectl"
[kube_ps1]: {{ site.baseurl }}/assets/images/2020-04-06-productivity-tools-for-kubernetes/kube-ps1-plugin.png "kube_ps1"
[kube_ps1_shell]: {{ site.baseurl }}/assets/images/2020-04-06-productivity-tools-for-kubernetes/ps1-demo.png "kube_ps1_shell"
