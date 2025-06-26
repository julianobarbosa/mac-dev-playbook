# Full Mac Setup Process (for Jeff Geerling)

There are some things in life that just can't be automated... or aren't 100% worth the time :(

This document covers that, at least in terms of setting up a brand new Mac out of the box.

## Initial configuration of a brand new Mac

Before starting, I completed Apple's mandatory macOS setup wizard (creating a local user account, and optionally signing into my iCloud account). Once on the macOS desktop, I do the following (in order):

  - Install Ansible (following the guide in [README.md](README.md))
  - **Sign in to App Store** (since `mas` can't sign in automatically)
  - Clone mac-dev-playbook to the Mac: `git clone git@github.com:julianobarbosa/mac-dev-playbook.git`
  - Drop `config.yml` from `~/OneDrive\ -\Hypera/MacOS/Apps/Config` to the playbook (copy over the network or using a USB flash drive).
  - Run the playbook.
    - If there are errors, you may need to finish up other tasks like installing 'old-fashioned' apps first (since I try to place Photoshop in the Dock and it can't be installed automatically). Then, run the playbook again ;)
  - Start Synchronization tasks:
    - Open Photos and make sure iCloud sync options are correct
    - Open Music, make sure computer is authorized, and set Library sync options
    - Open OneDrive, sign in, and set up sync
  - Install or complete setup for old-fashioned apps:
    - Open Creative Cloud, sign in, and install needed apps
    - Open iStat Menus and configure CPU/Net/Temp Combined view
    - (If required:)
      - Install [Elgato Stream Deck](https://www.elgato.com/en/downloads)
        - Open Livestream profile inside `~/OneDrive\ -\ Hypera/MacOS/Apps/Config/Stream Deck`
      - Install [Elgato Key Light Air (Control Center)](https://www.elgato.com/en/downloads)
      - Install [Autodesk Fusion 360](https://www.autodesk.com)
      - Install Microsoft Office Home & Student 2019 (https://account.microsoft.com/services/)
      - Install [Fritzing](https://fritzing.org/download/)
  - Configure FastMail account:
    - Log into Fastmail
    - Go to settings, then Privacy & Security
    - Create a new app password, and on that page, download the configuration file
    - Open the downloaded profile, then go to System Preferences, and Device Management
    - Double-click on the Fastmail profile
    - Click 'Install...' and install it
    - Configure which accounts are enabled in the 'Internet Accounts' System Preferences pane
  - Open Calendar and enable personal Google CalDAV account (you have to manually sign in).
  - Manually copy `~/Development` folder from another Mac (to save time).
  - Manual settings to automate someday:
    - Finder:
      - Disable click-to-show Desktop: `defaults write com.apple.WindowManager EnableStandardClickToShowDesktop -bool false`
    - System Preferences:
      - Accessibility > Display > Reduce transparency
      - Keyboard > Keyboard Shortcuts... > Modifier Keys... > Caps Lock to Esc
      - Keyboard > Key repeat rate to 'Fast', Delay until repeat to 'Short'
      - Privacy & Security > Full Disk Access > enable "Terminal"
    - Safari:
      - View > Show Status Bar
      - Preferences > Advanced > "Show full website address"
      - Preferences > Advanced > "Show features for web developers"
      - Install the 'Return YouTube Dislike' Userscript in Userscripts
    - Dock:
      - Add juliano.barbosa, Downloads, Applications, and shared "mercury" folders
  - Final Cut Pro
    - Install FxFactory and sign in: https://fxfactory.com
    - Copy contents of `~/Development/youtube/fcpx` into respective directories
  - These things might be automatable, but I do them manually right now:
    - Configure Time Machine backup drive
    - Install Wireguard VPN configurations (if needed)

## To Wrap in Post-provision automation

The following tasks have to wait for the initial OneDrive sync to complete before they'll succeed. So ideally I'll stick this all in a post-provision script but somehow flag it not to run on first provision.

```
# ZSH Aliases.
ln -s /Users/juliano.barbosa/OneDrive\ -\ Hypera/MacOS/.aliases /Users/juliano.barbosa/.aliases

# Electrum BTC Wallet (open Electrum first).
ln -s /Users/juliano.barbosa/OneDrive\ -\ Hypera/MacOS/.electrum /Users/juliano.barbosa/.electrum

# Ansible setup.
sudo mkdir -p /etc/ansible
sudo ln -s /Users/juliano.barbosa/OneDrive\ -\ Hypera/MacOS/Apps/Config/ansible/ansible.cfg /etc/ansible/ansible.cfg
sudo ln -s /Users/juliano.barbosa/OneDrive\ -\ Hypera/Apps/Config/ansible/hosts /etc/ansible/hosts
mkdir -p /Users/juliano.barbosa/.ansible
ln -s /Users/juliano.barbosa/OneDrive\ -\ Hypera/MacOS/.ansible/galaxy_token /Users/juliano.barbosa/.ansible/galaxy_token
ln -s /Users/juliano.barbosa/OneDrive\ -\ Hypera/MacOS/Apps/Config/ansible/mm-vault-password.txt /Users/juliano.barbosa/.ansible/mm-vault-password.txt

# Final Cut Pro setup. (Open Motion first)
cp -r /Users/juliano.barbosa/OneDrive\ -\ Hypera/MacOS/Apps/Config/Motion/Motion\ Templates.localized/ /Users/juliano.barbosa/Movies/Motion\ Templates.localized/
cp -r /Users/juliano.barbosa/OneDrive\ -\ Hypera/MacOS/Apps/Config/Motion/Text\ Styles/ /Users/juliano.barbosa/Library/Application\ Support/Motion/Library/Text\ Styles.localized/

# Sequel Ace favorites. (Open Sequel Ace first)
cp /Users/juliano.barbosa/OneDrive\ -\ Hypera/MacOS/Apps/Config/Sequel\ Ace/Favorites.plist /Users/juliano.barbosa/Library/Containers/com.sequel-ace.sequel-ace/Data/Library/Application\ Support/Sequel\ Ace/Data/Favorites.plist

# Font setup.
cp ~/OneDrive\ -\ Hypera/MacOS/Apps/Config/Fonts/* ~/Library/Fonts/

## When formatting old Mac

  - Sign out of Adobe Creative Cloud
  - Sign out of Panic Sync in Transmit
  - Deauthorize Apple Music in iTunes/Music App
  - Make sure anything new merged into `~/OneDrive\ -\ Hypera/MacOS/Apps/Config`:
    - Fonts from ~/Library/Fonts
    - Motion Plugins from ~/Movies/Motion
    - Final Cut Pro Text Styles in ~/Library/Application Support/Motion/Library/Text Styles
    - Sequel Ace shortcuts from ~/Library/Containers/com.sequel-ace.sequel-ace/Data/Library/Application\ Support/Sequel\ Ace/Data/Favorites.plist
  - Follow Apple's guide [here](https://support.apple.com/en-au/HT212749)
