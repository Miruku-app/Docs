A curated collection of custom commands for the [Miruku Discord Bot](https://miruku.app) based on the custom commands from the [YAGPDB Discord Bot](https://yagpdb.xyz).

- **Well tested:** We review all commands added and many commands have widespread usage, meaning that it's unlikely — [though not impossible](#disclaimer) — for there to be serious bugs in them.
- **Well documented:** We take pride in our high quality of documentation, which makes it easy for anyone to figure out how to use a given command.

## Disclaimer

The Miruku developer, YAGPDB Dev, YAGPDB staff, and/or support are not responsible for any issues with or caused by these custom commands. Furthermore, though we thoroughly review all commands added, they are not guaranteed to be working; use them at your own risk.

## Need help?

If you are experiencing issues with a custom command from this collection, check to see if it's solved on the [YAGPDB Custom Command Repo](https://github.com/yagpdb-cc/yagpdb-cc)


## Contributors

**[yagpdb-cc](https://github.com/yagpdb-cc/yagpdb-cc)** is maintained by a group of YAGPDB staff members: [@jo3-l](https://github.com/jo3-l), [@Satty9361](https://github.com/Satty9361), [@l-zeuch](https://github.com/l-zeuch), [@Pedro-Pessoa](https://github.com/Pedro-Pessoa), and [@DaviiD1337](https://github.com/DaviiD1337). They made this available under the MIT License and so I'm rehosting it here for backup and to make changes for what is added to Miruku

We would also like to acknowledge [the community contributors](https://github.com/yagpdb-cc/yagpdb-cc/graphs/contributors) to the yagpdb-cc github that contributed much of the work here. Thank you!


# Adding Custom commands
This collection of custom commands is separated into different categories.
For example, fun commands can be found [here](fun/overview), while the giveaway system is located [here](giveaway/overview).

Generally speaking, there are two kinds of custom commands. These are as follows:

- A **standalone** command, which you can add by itself. It should "just work" after you set it up.
- A **system** of commands (sometimes called a **package**). As the name suggests, it is comprised of several custom commands.
  One command from the set may not work on its own: you may need to add several other commands from the set or even all of them for it to work.

Now, let's explain how you would install a command from this collection. We will separate this guide into two sections; one for standalone commands and one for systems of commands. Depending on what you need, feel free to skip to the corresponding section:

- [For adding standalone commands](adding-ccs/#adding-standalone-custom-commands)
- [For adding systems of commands](adding-ccs/#adding-systems-of-commands)

## Adding standalone custom commands

Once you've found a command you like, you first need to add it to your server.

:::caution

To add custom commands, you will need to be able to access your server's control panel.

:::

Start by going to the [YAGPDB control panel](https://yagpdb.xyz/manage/), logging in, and selecting your server.

Now, navigate to `Core -> Custom Commands`. You should see a green button labelled `Create a new Custom Command`. Click on that. You should be directed to a new page that allows you to configure your new command.

At this point, you'll need some more information about the command. To find what you need, go back to the page for the custom command.

First, you need to find the **trigger**. Typically, there is a section called `Trigger` dedicated to this on the page for the command. Make note of the trigger type, the trigger, and any additional options. Now, return to your control panel and change the relevant fields for the command.

Second, you need the **code** for the command. There will be a section called `Code` dedicated to this on the page for the command. If you hover over the code, you should see a `Copy` button on the top right. Click that. Now, return to your control panel again and change the command response to what you just copied.

Third, you need to see if any **restrictions** are necessary for the command (channel/role restrictions). If they exist, they will be noted in the `Trigger` section of the page. Make any adjustments, as needed.

Last, sometimes, you will need to configure the command. To see whether you need to do that, check if there's a `Configuration` section on the page. If so, see below for instructions on how to do that. Otherwise, you should be able to use your custom command!

### Configuring commands

Several custom commands here provide a way to change some behavior of the command without having to dig deep into the code through **configuration variables**. Custom commands that do this will typically have the following structure:

```gotmpl
{{/*
	Leading comment here.
*/}}

{{/* Configuration values */}}
{{$variable0 := 0}}
{{$variable1 := "hello"}}
{{/* ... */}}

{{/* End of configuration values */}}

{{/* Actual code */}}
```

The values that you can change safely are between the two comments `{{/* Configuration values */}}` and `{{/* End of configuration values */}}`. A description of these values will be in the `Configuration` section of the page. Refer to that to see if there's anything you wish to change.

:::tip

There are some commands with configuration values that you **must** change for it to work, e.g. channel IDs. In the `Configuration` sections, the values that you must change will have a `📌` around them to draw your attention to it. You should look at these first when configuring a command.

:::

After you've done that, your custom command should be ready to use! Feel free to give it a go :)

## Adding systems of custom commands

When adding systems of custom commands, the process is essentially the same, except you add all of the commands in the system rather than just one.

For commands with especially complicated setups, there may be an installation guide available for them in the overview. For example, see the following guides:

- [Giveaway system v2](giveaway/basic-v2/overview.md/#installation)
- [Starboard v2](fun/starboard/overview/#installation)

Otherwise, there isn't much difference between adding systems of custom commands and standalone ones.