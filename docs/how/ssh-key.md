# How to generate an SSH key pair

## Step 0: Check if you have an existing SSH key pair

To check if you already have an SSH key pair on your local machine (macOS or Linux), you can follow these steps:

1. Open a terminal or command prompt on your local machine.

2. Navigate to your SSH directory by entering the following command:

```shell
cd ~/.ssh
ls
```

The above command will display the files and directories within the `.ssh` directory.

If you already have an SSH key pair, you should see one or more files with names similar to the following:

- `id_rsa` (private key)

- `id_rsa.pub` (public key)

- `id_dsa` (private key)

- `id_dsa.pub` (public key)

The `.pub` files represent the public keys, while the files without the .pub extension are the private keys.

If you see the key files listed, it means you already have an SSH key pair on your local machine. If the files are not
present or the directory doesn't exist, you don't have an SSH key pair yet and can proceed with generating one using the
ssh-keygen command.

## Step 1: Generate SSH key pair

If you do not have an SSH key pair, on your local machine, open a terminal or command prompt and run the following
command to generate an SSH key pair:

```shell
ssh-keygen -t rsa -b 2048 -C "your_email@example.com"
```

OR:

```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

!!! warning
    If you’re using the latest macOS, it’s highly recommended to use the ed25519 algorithm to generate your SSH keys;
    otherwise, you might need to update the SSH configuration file to support the rsa option.

Copy the Public Key on macOS: Use the pbcopy command to copy the contents of the public key to your clipboard, suppose
the name of your public key is `id_ed25519.pub`:

```shell
pbcopy < ~/.ssh/id_ed25519.pub
```