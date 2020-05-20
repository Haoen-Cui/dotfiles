# Switching between Multiple `git` Configs 

One may have multiple `git` accounts, e.g. for work and open source projects. Hence, switching between them will be necessary at times. 

Here is my workflow using SSH authentication and a few shell commands which allows me to switch between `git` accounts on different hosts. Below is a step-by-step setup walk through and the files in this directory shows an example. 

## Step 1: Setup an SSH key for Each `git` Account 

Reference: [Connecting to GitHub with SSH](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)

We can use the following command to generate a SSH key pair 

```bash
ssh-keygen -t rsa -b 4096 -C "<YOUR_EMAIL_FOR_GIT@example.com>"
```

and save it to `/Users/<YOUR_LAPTOP_USERNAME>/.ssh/id_rsa_<USAGE>`, e.g. `<USAGE> = opensource` for your open-source account. The resulting 

- `/Users/<YOUR_LAPTOP_USERNAME>/.ssh/id_rsa_<USAGE>` file: private key --> never to be shared 
- `/Users/<YOUR_LAPTOP_USERNAME>/.ssh/id_rsa_<USAGE>.pub` file: public key --> put in your GitHub / GitLab / BitBucket profile on their websites 

## Step 2: Edit `~/.ssh/config` 

Create one if the file does not exist 

```bash
touch ~/.ssh/config 
```

Modify the file to include your newly created key pairs

```
# Public GitHub for Open Source 
Host github.com
  Hostname github.com
  User git
  IdentityFile ~/.ssh/id_rsa_opensource 

# Company Internal 
Host git.<YOUR_COMPANY>.com
  Hostname git.<YOUR_COMPANY>.com
  User git
  IdentityFile ~/.ssh/id_rsa_work  
```

## Step 3: Alias a Shell Command 

Put the following in your bash profile (`.bash_profile` or `.zshrc`). Create one alias for each usage you need. 

```bash
alias git_<USAGE>='git config --global user.name "<YOUR_GIT_USERNAME>" && git config --global user.email "<YOUR_EMAIL_FOR_GIT@example.com>"'
```

then, you will be able to call 

```bash
git_<USAGE>
```

to activate your settings for that `<USAGE>` (e.g. `<USAGE> = opensource`). Feel free to rename `git_<USAGE>` to whatever is intuitive to you. 