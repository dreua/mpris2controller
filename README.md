# mpris2controller [![Code Health](https://landscape.io/github/icasdri/mpris2controller/master/landscape.svg?style=flat-square)](https://landscape.io/github/icasdri/mpris2controller/master)
**mpris2controller** is a small user daemon for GNU/Linux written in python that intelligently controls [MPRIS2](http://specifications.freedesktop.org/mpris-spec/latest/)-compatible media players.

The daemon responds to commands/controls via multimedia keys or the terminal, and controls all open media players at once, doing what one would expect.
* Play/Pause control
	* Pauses all players if one or more are currently playing, OR
    * Plays the most recently paused player if none are currently playing
* Next/Previos control
	* Advances to the next or previous song only on the player that is playing.

The daemon accomplishes this by keeping track of the playback status of all MPRIS2 players currently active in the user's session.

This daemon alleviates the problem of only being able to have keybindings for one specific player. It also avoids the problems plaguing other implementations (Desktop plugins, etc.) with all players playing at once, etc.

## Installation
**mpris2controller** is writen in Python 3, and requires the python 3 versions of the following dependencies:
* `dbus-python` (packaged as `python-dbus` on some distributions)
* `python-gobject` (for the glib mainloop)

To install, simply clone this repo and run `python setup.py install`

	git clone https://github.com/icasdri/mpris2controller.git
    cd mpris2controller
    python3 setup.py install

For Arch Linux users, [mpris2controller-git](https://aur.archlinux.org/packages/mpris2controller-git/) is available in the AUR.

## Usage
**mpris2controller** allows you to control all your MPRIS2-compatible multimedia players at once via the terminal using the `mpris2controller` command, which can be bound to multimedia keys (see Multimedia Keys below).

###### PlayPause
	mpris2controller PlayPause

###### Next
	mpris2controller Next

###### Previous
	mpris2controller Previous

On the first run of any of these commands, mpris2controller will attempt start its daemon on demand and detect running media players. In most cases, this launch-on-demand system works -- however, in some cases it may be better to start the daemon manually before using the controls (simply run `mpris2controller` with no arguments) or autostart the daemon with the desktop session: see Autostart below for instructions.

### Autostart
In most cases, autostart of the daemon is not necessary -- the daemon will launch on demand.

To allow for autostarting the daemon, however, the installation includes a disabled autostart file at `/etc/xdg/autostart/mpris2controller.desktop`. To enable autostart for XDG-compatible desktops, delete the line `Hidden=true` in this file.

If for some reason, this does not start mpris2controller with your desktop, please consult your desktop environment's documentation for autostarting applications: set `mpris2controller --no-fork` to autostart.

##### user-python-daemon
mpris2controller can also run as part of [user-python-daemon](https://github.com/icasdri/user-python-daemon). Simply add `[mpris2controller.daemon]` to your `user_python_daemon.conf`.

### Multimedia keys

To control the daemon via mutlimedia keys, add keyboard shortcuts that bind your multimedia keys to the commands listed at the beginning of the Usage section.

Detailed instructions for [GNOME](http://gnome.org), [i3](http://i3wm.org), and [XFCE](http://xfce.org) are provided below for convenience. For users of other Desktop Environments or Window Managers, follow your DE or WM's documentation for adding keyboard shortcuts.

##### GNOME
For [GNOME](http://gnome.org) useres, open GNOME Control Center, go to Keyboard, then Shortcuts. Or execute `gnome-control-center keyboard shortcuts`.

Then go to Custom Shortcuts, then the "+" sign at the bottom. Give it any descriptive name, then give it one of the commands listed at the beginning of the Usage section. Finally, click "Disabled" in the second column next to the shortcut you just created, and press your respective multimedia key.

Note: mpris2controller works with the [Media Player Indicator](https://extensions.gnome.org/extension/55/media-player-indicator/) GNOME Shell Extension. GNOME will simply prompt you to override its keybindings when adding the custom shortcuts.

##### i3
For [i3](http://i3wm.org) users, add the following to your i3 config.

	bindsym XF86AudioPlay exec mpris2controller PlayPause
    bindsym XF86AudioPrev exec mpris2controller Previous
    bindsym XF86AudioNext exec mpris2controller Next

##### XFCE
For [XFCE](http://xfce.org) users, open the XFCE Settings Manager from the panel menu. Then click on Keyboard and navigate to the Application Shortcuts tab.

Click on the Add button towards the bottom, enter one of the commands listed at the beginning of the Usage section, press OK, and finally press your respective multimedia key.

### DBus
The daemon can also be controlled via methods exposed on  the [DBus](http://www.freedesktop.org/wiki/Software/dbus/) session bus.

* *Bus Address*: `org.icasdri.mpris2controller`
* *Interface*: `/org/icasdri/mpris2controller`
* *Methods*:
    * `org.icasdri.mpris2controller.PlayPause`
    * `org.icasdri.mpris2controller.Next`
    * `org.icasdri.mpris2controller.Previous`

