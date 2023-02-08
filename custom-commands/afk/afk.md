## Features

- Set AFK status with optional duration
- Notification in chat when an AFK member was mentioned
  - Estimated time of arrival, when set

## Installation

As usual, there are instructions describing where to put the script and which trigger to use on the pages corresponding to the individual commands. Additionally, we've documented how and where to add these scripts down below.

### Main Command

Add the [main command](#main-cc) as a new custom command. The trigger is a RegEx trigger type with `\A` or `.*`.

:::tip

If you're short on CC space, you can append the code to your already existing `\A` or `.*` trigger. Please do so only once, though.

:::

Now take your time to read through the leading comment and decide if you want to remove the AFK status of users, once they send a message. If so, leave it as is, otherwise set the config variable to `false`, as follows:

#### Disable removeAfkOnMessage

```gotmpl {2}
{{/* Configuration values */}}
{{ $removeAfkOnMessage := false}}
{{/* End of configuration values */}}
```

### Leave Feed

This is entirely optional, however generally encouraged to keep your custom command database clean. Add the code [here](leave-feed) to your leave message, which you can find like this:

- Go to your control panel
- Open up the Feeds & Notifications tab
- Click on General
- Locate the leave message

Once you've found it, just append the code at the end - don't forget to save!

:::info

The AFK system is now set up and ready to use!

:::


# Main CC


## Trigger

**Type:** `Regex`<br />
**Trigger:** `\A`

## Usage

- `-afk <message>` - To set an AFK message with no expiration.
- `-afk <message> -d <duration>` - To set an AFK message that expires in _duration_ time.

## Configuration

See the [AFK system overview](overview/#installation) for instructions regarding how to configure this command.

## Code

```gotmpl
{{/*
	Allows users to set an AFK message with optional duration.
	Can be configured to remove AFK status from users once they send a message.
	See <https://yagpdb-cc.github.io/afk/main-cc> for more information.

	Author: DaviiD1337 <https://github.com/DaviiD1337>
*/}}

{{/* Configurable values */}}
{{ $removeAfkOnMessage := true}}
{{/* End of configurable values */}}

{{ $time := 0 }}{{ $afk := dbGet .User.ID "afk" }}{{ $desc := "" }}
{{ if $args := .Args }}
	{{ if eq (lower (index $args 0)) (print .ServerPrefix "afk") }}
		{{ if and (ge (len $args) 1) (not $afk) }}
			{{ $message := "" }}
			{{ $duration := 0 }}
			{{ $skip := false }}
			{{ if ge (len .Args) 2 }}
				{{ $args = (slice .Args 1) }}
				{{ range $i, $v := $args }}
					{{- if and (gt (len $v) 1) (not $skip) }}
						{{- if and (eq $v "-d") (gt (len $args) (add $i 1)) }}
							{{- $duration = index $args (add $i 1) }}
							{{- $skip = true }}
						{{- else }}
							{{- $message = joinStr " " $message $v }}
							{{- $skip = false }}
						{{- end }}
					{{- else if not $skip }}
						{{- $skip = false }}
						{{- $message = joinStr " " $message $v }}
					{{- else if $skip }}
						{{- $skip = false }}
					{{- end -}}
				{{ end }}
			{{ end }}
			{{ $parsedDur := 0 }}
			{{ with and $duration (toDuration $duration) }} {{ $parsedDur = . }} {{ end }}
				{{ if $parsedDur }}
					{{ dbSetExpire .User.ID "afk" (or $message "No reason") (toInt $parsedDur.Seconds) }}
				{{ else }}
					{{ dbSet .User.ID "afk" (or $message "No reason") }}
				{{ end }}
			{{ .User.Mention }}, I set your AFK to `{{ or $message "No reason provided" }}`.
		{{ else if $afk }}
			Welcome back, {{ .User.Mention }}, I removed your AFK.
			{{ dbDel .User.ID "afk" }}
		{{ end }}
	{{ else if and $afk $removeAfkOnMessage }}
		Welcome back, {{ .User.Mention }}, I removed your AFK.
		{{ dbDel .User.ID "afk" }}
	{{ else if .Message.Mentions }}
		{{ with (dbGet (index .Message.Mentions 0).ID "afk") }}
			{{ $user := userArg .UserID }}
			{{ $eta := "" }}
			{{ if gt .ExpiresAt.Unix 0 }}
				{{ $eta = humanizeDurationSeconds (.ExpiresAt.Sub currentTime) | printf "*%s will be back in around %s.*" $user.Username }}
			{{ end }}
			{{ if and (eq .Value "No reason") $eta }}
				{{ $desc = printf "%s" $eta }}
			{{ else if and (not $eta) (ne .Value "No reason") }}
				{{ $desc = printf "%s" .Value }}
			{{ else if and $eta (ne .Value "No reason") }}
				{{ $desc = printf "%s\n\n%s" $eta .Value }}
			{{ else }}
				{{ $desc = "**No reason provided**"}}
			{{ end }}
			{{ sendMessage nil (cembed
				"author" (sdict "name" (printf "%s is AFK" $user.String) "icon_url" ($user.AvatarURL "256"))
				"description" $desc
				"color" (randInt 0 16777216)
				"footer" (sdict "text" "Away since")
				"timestamp" .UpdatedAt
			) }}
		{{ end }}
	{{ end }}
{{ end }}
```

## Author

This custom command was written by [@DaviiD1337](https://github.com/DaviiD1337).


# Leave Feed

:::note

This code is optional - if you don't want AFK messages to be removed when users leave, just don't add it.
The other components of the AFK system will work fine without it.

:::

This code removes the AFK messages of users who have left the server, to keep your database usage low.

For more information about the AFK system, please see [this](overview) page.

## Trigger

This is _not_ a custom command! Rather, it's meant to be added to your **Leave Feed**.

## Code

```gotmpl
{{/*
	Clears the AFK status for users who leave the server.
	See <https://yagpdb-cc.github.io/afk/leave-feed> for more information.

	Author: jo3-l <https://github.com/jo3-l>
*/}}

{{ dbDel .User.ID "afk" }}
{{/* If you already have a leave message, you can put it here. */}}

```

## Author

This code was written by [@jo3-l](https://github.com/jo3-l).




