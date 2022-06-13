# Distier
> 13th June 2022

A [ZeroTier](https://www.zerotier.com/) management discord bot. 

## What is ZeroTier?

ZeroTier is a simple to use SD-WAN solution for creation a virtual LANs, VPNs 
and other SDN management. It's free for 25 clients/nodes and has pricing plans
for users that need more.

## What can Distier do?

As mentioned above, Distier is designed to be a management tool for discord 
servers. It will allow the following:

1. Creation of `networks` per channel
2. Authorisation of `nodes` (users) in a channel
3. Deauthorisation of `nodes` in a channel
4. Status of `nodes` in the channel

Upon using these bot commands, it will give users instuctions of what to do
following on.

## Why use Distier?

Are you playing a multiplayer game where one person hosts a server and requires
to port forward their device open onto the internet so their friends can join?
Well ZeroTier allows LAN connections without requiring the user to expose their
router/firewall to the internet.

When setting up a network on a discord channel, users that join that network
will be given a new local IP within the range of `10.69.x.1/24` where `x` is the
channels with a network.

> **Note**: The use of the bot across multiple servers with the same users is 
> subject to having IP collisions. This will need to be looked at.

## Usage

To use the bot/set the bot up. You will have to do a couple of things:

1. Make an free account on [ZeroTier](https://www.zerotier.com/)
2. After making the account, go to the Account section and create an API Access
   Token by pressing the `New Token` button. The name can be what every you 
   want.
3. Have a machine or VPS that you are going to run the bot on.
4. From the `Releases` tab in the Github repo, download the binary for your 
   machines OS/Arch type.
5. Set an environment variable called `DISTIER_ZT_ACCESS_TOKEN` to be the value
   of *API Access Token* you generated in `step 2`.
6. Run the binary.
7. Create a **Discord Role** called `DisTier Admin` and give it to the people
   you want to be able to run admin level commands.

For clients to use the bot, they will have to download the 
[ZeroTier](https://www.zerotier.com/) client. **They don't have to make an 
account.**

## What are the Bot Commands

### Create a network in a Discord Channel

This command will create a new ZeroTier network named the same as the channel
the command is ran in. The DHCP auto IPv4 Assignment will be the `10.68.x.1/24`
where `x` is the number of channel/networks the channel is in the server.

```
/register-channel
```

### Destroy a network in a Discord Channel

This command will delete a ZeroTier network that has been previously registered.
If the channel doesn't have an associated network attached, then the command 
will do nothing.

```
/unregister-channel
```

### Authorise an user to join a Channel Network.

This command will authorise the user who initiated the command with their given
`ZeroTier Address` (given by the user's client), into the channel's network. It
will then prompt the user to join the network using their ZeroTier client by
showing the Channel Networks ZeroTier ID.

```
/authorise-me <zerotier_address>
```

A user can only have a single device authorised to a channel network at a time.
If the user has a second computer/laptop they are using at the time, they can
re-run the `authorise-me` command with their new `zerotier_address` of their 
current device and it will deauthorise the previous device and authorise the new
one. This can then be done again when the user goes back to their original 
device. This is so the ZeroTier account owner doesn't use up all their avaliable
nodes from users having multiple devices.

### Deauthorise an user from a Channel Network

There are two commands that can be ran for this, depending on the authorisation
access of the user calling the command. Any user can deauthorise themselves 
using the `deauthorise-me` command below.

```
/deauthorise-me
```

If the user running the command has the `DisTier Admin` role, they can run the
following command to remotely deauthorise a user in a channel, *which will also
blacklist the user from being able to authorise themselves in the channel 
again*. To un-blacklist the user, use the 
**[Reauthorise Command](#reauthorise-a-blacklisted-user)**

```
/deauthorise-user <user_discord_tag>
```

### Reauthorise a blacklisted user

If a user has been deauthorised from a channel network by and **DisTier Admin**,
they can be reauthorised with the following command to regain access to the 
channels network.

```
/reauthorise-user <user_discord_tag>
```

### Channel Network Audit

A user running this command will require the `DisTier Admin` role. The command
will make the bot check everyone in the focused channel, and delete any nodes on
the ZeroTier network that are no longer in the channel.

```
/audit-channel
```

### See the Channel Status

Anyone can run this can get a status of all the users/nodes in the channel's
network. The information included will be the:

- Users Discord Tag
- Their ZeroTier IP Address for the Channel
- If their device is Active

```
/status-channel
```
