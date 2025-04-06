+++
title = "Streamline Org-Mode Capture: Disable Browser Prompts for Org-Protocol"
author = ["Krishna Gopalakrishnan"]
date = 2025-04-07T00:00:00-07:00
tags = ["emacs", "orgmode"]
draft = false
+++

I use emacs org-mode to capture links from the browser for later reading, or for archiving. For this purpose, I use a simple [bookmarklet](https://www.gnu.org/software/emacs/manual/html_node/org/The-capture-protocol.html) to capture the link, title and any selected text, and then use an org-capture template to store the information. However, when i click on the bookmarklet, most browsers throw a pop-up asking for permission to open org-protocol:// links. This security check, while well-intentioned, adds friction every time I try to capture a link.
Here's how we can disable this confirmation prompt on Chromium based browsers.

<!--more-->


## On macOS {#on-macos}

```shell
# For Google Chrome
defaults write com.google.Chrome URLAllowlist -array-add -string 'org-protocol://*'

# For Brave
defaults write com.brave.Browser URLAllowlist -array-add -string 'org-protocol://*'
```


## On Linux {#on-linux}

We can achieve this using enterprise policy files. This however will apply to all users on the machine for this browser.

```shell
mkdir -p /etc/opt/chrome/policies/{managed,recommended}
cat <<EOF >/etc/opt/chrome/policies/managed/allow_org_protocol.json
{
  "URLAllowlist": [
    "org-protocol:*"
  ]
}
EOF
```

The above command works for Chrome, but Brave seems to store the policies at /etc/brave/policies, so update the paths accordingly.

To debug whether policies were loaded, go to chrome://policy or brave://policy, and check logs. You can also reload policies here without having to restart chrome/brave.
