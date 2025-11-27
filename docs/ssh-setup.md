# SSH to Github

SSH or Secure Shell is a way of connecting or logging in to our resources i.e. Github.com.

SSH is a more secure way of connecting to our resources and we use a GitBash terminal to connect to our GitHub Repo's and to push/pull changes to/from our local machines/remote hosts.


1. Generate SSH key via CLI using `ssh-keygen`
2. Register Publick key on github
3. Add Private key to SSH register on local machine

## Generating SSH Keys
1. Open a GitBash Terminal navigate to the .ssh directoy
   - `cd ~/.ssh`

2. Run the following command to generate a new key pair. 
 - `ssh-keygen -t rsa -b 4096 -C "craig.woodcock89@hotmail.co.uk"`
   - `-t rsa` = type rsa
   - `-b 4096` = bytes 4096
   - `-C "Email address` = Email to use

3. Enter a name for your Key files.
   - This should be named after it's use case e.g.
   - `craig-github-ssh-key` 

4. Press Enter through the prompts and you should end up with this:<br>
![RSA KEY](<../SSH Screenshots/Screenshot 2024-01-08 145102.png>)
   
5. Confirm the files have been created.
   - `ls` will show a list of files in the directory
   - we should see 2 files.
   - `.pub` extension is the public key!!!

6. We need to print the content of the public key to the screen.
   - `cat craig-github-ssh-key.pub`
   - And then copy the public key:<br>
![Public key](<../SSH Screenshots/Screenshot 2024-01-08 150705.png>)

7. Now we need to Register on Github.
   - Head to github,
   - click on your profile picture,
   - Go to Settings,
   - 'SSH and GPG KEYS'
   - 'ADD NEW KEY'.
   - Name your key the same as the key file.
   - And paste the key from the following step.<br>
 ![SSH Github](<../SSH Screenshots/Screenshot 2024-01-08 151034.png>)

8. Start the SSH Agent.
   - `eval $(ssh-agent -s)`

9. Add Key to SSH Register:
   - `ssh-add craig-github-ssh-key`

10. Test Connection:
   - `cd ..` to move out .ssh folder
   - `ssh -T git@github.com`

11. Now we can create a test folder in the directory where our github files go and then create a file to push to github..
    - `mkdir test-ssh-repo`

12. cd into the repo and make a new file with a new line of text
    - `echo this is a test line > testFile.txt`
  
13. now add, commit and push,
    - `git remote add origin <url to repo>`
    - `git add .`
    - `git commit testFile.txt -m "message"`
    - `git push origin main`

14. when launching a new bash terminal, we need to re-add the key and start the agent:
   - Start the SSH Agent.
    - ``eval `ssh-agent -s``

   - Add Key to SSH Register:
    - `ssh-add craig-github-ssh-key`
  
15. To Make the Key persist, add it to the .bashrc file:<br>
    - The following lines will start the agent and add the key each time the terminal is launched without printing confirmation to the terminal.
```
 # Start SSH agent in silent mode
eval "$(ssh-agent -s)" > /dev/null 2>&1

# Add SSH key silently
ssh-add ~/.ssh/craig-github-ssh-key > /dev/null 2>&1

```

 
