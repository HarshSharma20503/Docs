Search for extensions in the search bar.
You will get option there.
From there you can remove icons display or turn it on.

## Install gnome Extension

### Installing GNOME Shell Extensions

Now that you know what GNOME Shell Extensions are, let’s see how to install them.

There are three ways you can use GNOME Extensions:

1. Use a minimal set of extensions from Ubuntu (or your Linux distribution)
2. Find and install extensions in your web browser
3. Install extensions using Extension Manager App
4. Download and manually install extensions

Although GNOME Tweaks is not required for extensions to work, [installing GNOME Tweak Tool in Ubuntu](https://itsfoss.com/gnome-tweak-tool/) or whichever distribution you are using is better.

```
sudo apt install gnome-tweaks
```

At times, you would also need to know the version of GNOME Shell you are using. This helps determine whether an extension is compatible with your system. You can use the command below to find it:

```
gnome-shell --version
```

### Method 1: Use the gnome-shell-extensions package

Ubuntu (and several other Linux distributions such as Fedora) provide a package with a minimal set of GNOME extensions. You don’t have to worry about compatibility here as it is tested by your Linux distribution.

If you want a no-brainer, just get this package and you’ll have 8-10 GNOME extensions installed.

```
sudo apt install gnome-shell-extensions
```

Once installed, log out and re-login to your system. After that, start GNOME Extensions App from Overview. This extensions app will be installed as part of `gnome-shell-extensions` package.

You’ll find a few extensions installed. You can just toggle the button to start using an installed extension.