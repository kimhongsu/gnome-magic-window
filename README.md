# gnome-magic-window

Bind a key to a specific program in Gnome Shell:

- When the key is pressed and this program is in background, its window is
  brought up to front.
- When the key is pressed and this program is already in front, the last window
  if brought in front again.
- When the key is pressed and the program isn't launched yet, it is spawned.

It is comparable to _Guake_ and _Compiz Put_, except that gnome-magic-window
works with Gnome Shell and the new Wayland display server. (It does not use
xdotool and wmctrl, that worked with X11 but not with Wayland.)

## Demo

![pseudo-video demonstration](demo.gif)

### 1. Install the extension

Since Gnome 41, gnome-magic-window is shipped as a Gnome extension. To install
this extension from the Git repository:

```shell
sudo apt install -y gnome-shell-extensions

cd ~/.local/share/gnome-shell/extensions
git clone https://github.com/adrienverge/gnome-magic-window gnome-magic-window@adrienverge
```

### 2. Customize

Edit `extension.js` to set your favorite key, window name and command to run:

```javascript
const bindings = [
  {
    shortcut: '<Super>v',
    title: 'Code',
    command: '/usr/bin/code',
  },
  {
    shortcut: '<Super>f',
    title: 'firefox',
    command: '/usr/local/bin/firefox',
  },
  {
    shortcut: '<Super>n',
    title: 'notion-app',
    command: '/usr/bin/notion-app',
  },
  {
    shortcut: '<Super>t',
    title: 'tabby',
    command: '/usr/bin/tabby',
  },
  {
    shortcut: '<Super>c',
    title: 'Google-chrome',
    command: '/usr/bin/google-chrome',
  },
  {
    shortcut: '<Super>s',
    title: 'sublime_merge',
    command: '/opt/sublime_merge/sublime_merge',
  },
];
```

### 3. Enable the extension

After installing files and customizing, you probably need to close your session
and log in again in order for Gnome to the extension.

Either run:

```shell
gnome-extensions enable gnome-magic-window@adrienverge
```

Or open Gnome "Extensions" utility to enable the extension.

You're set! Try pressing your magic key.

### 4. Debug

In case it doesn't work, you may need to add your gnome version in
`metadata.json` and reload your session, or debug further.

실행되는 프로그램의 title을 알기 위해서 log(this.debug())의 주석을 해제한 후 단축키를 누르면 journalctl의 로그를 통해서 현재 실행되고 있는 프로그램들의 title을 알 수 있다.

```javascript
  magic_key_pressed(title, command) {
    // For debugging:
    // Util.spawn(['/bin/bash', '-c', `echo '${this.debug()}' > /tmp/test`]);
    // throw new Error(this.debug());
    log(this.debug());
```

```shell
journalctl -f -o cat /usr/bin/gnome-shell
```

```shell
# Alt+F2↵ r↵
gdbus call --session --dest org.gnome.Shell --object-path /org/gnome/Shell/Extensions/GnomeMagicWindow --method org.gnome.Shell.Extensions.GnomeMagicWindow.magic_key_pressed Terminator terminator
```

### 5. 삭제

extension 디렉토리를 삭제한 재시작

```shell
rm -rf ~/.local/share/gnome-shell/extensions/gnome-magic-window@adrienverge
```

## For Gnome versions < 41

Use this repo on commit 26230da or before, and read the README file from that
version.
