# Sudo
Basics of Sudo Configuration.

-> Setup SSH Passwordless Login Using Publickey Authentication :
   
      01. Check for existing SSH key pair :

          Before generating a new SSH key pair first check if you already have an SSH key on your client machine because you don’t want to overwrite your          existing keys.

          Run the following ls command to see if existing SSH keys are present:

          ls -al ~/.ssh/id_*.pub

          If there are existing keys, you can either use those and skip the next step or backup up the old keys and generate a new one.
    
      02. Generate a new SSH key pair :

          The following command will generate a new 4096 bits SSH key pair:

          ssh-keygen -t rsa -b 4096

          To be sure that the SSH keys are generated you can list your new private and public keys with:
          ls ~/.ssh/id_*
       
   03. Copy the public key :
   
       Now that you have generated an SSH key pair, in order to be able to login to your server without a password you need to copy the public key to the        server you want to manage.

       The easiest way to copy your public key to your server is to use a command called ssh-copy-id. On your local machine terminal type:
       
       ssh-copy-id remote_username@server_ip_address
       
       Once the user is authenticated, the public key will be appended to the remote user authorized_keys file and connection will be closed.
         
   04. Login to your server using SSH keys :
    
       After completing the steps above you should be able log in to the remote server without being prompted for a password.

       To test it just try to login to your server via SSH:
       
       ssh remote_username@server_ip_address
       
       If everything went well, you will be logged in immediately.
       
   05. Open the SSH configuration file /etc/ssh/sshd_config, search for the following directives and modify as it follows: 
   
       PermitRootLogin no,
       
       PubkeyAuthentication yes,
       
       PasswordAuthentication no,
       
       ChallengeResponseAuthentication no,
       
       UsePAM no
       
   06. Once you are done save the file and restart the SSH service.

       On Ubuntu or Debian servers, run the following command:
       
       sudo systemctl restart ssh

-> Execute ALL sudo commands without password For particular SSH_USER :

      Type the following command as root user:

      visudo

      Or if you have sudo access and want to grant another user permission, try:

      sudo visudo

      Append the following entry to run ALL command without a password for a SSH_USER :

      SSH_USER ALL=(ALL) NOPASSWD:ALL
