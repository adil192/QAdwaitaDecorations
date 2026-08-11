# QAdwaitaDecorations (Windows-inspired fork)
Qt decoration plugin implementing Windows-like client-side decorations.

There is no intention of mimicking Windows exactly;
just taking the parts I like, namely the bigger titlebar buttons.

Features:
- **Bigger titlebar buttons**: The minimize, maximize, and close buttons are much easier to click.
- **System theme support**: The titlebar and buttons use colors from your QPalette. On GNOME, you may use `qt6ct` to customize your Qt theme.
- **Light/dark theme switching**: The titlebar instantly changes between light/dark mode without needing to restart the app. If the QPalette is mismatched (qt6ct takes a few seconds to update), it will fall back to Adwaita colors.

Missing features:
- **Flatpak support**: This currently only works with native packages, not Flatpaks.
- **GTK apps**: There is no counterpart for restyling GTK apps. I will probably make a custom CSS for this at some point.

## Screenshots

Here's how this project (bottom) compares to Adwaita (top) and Windows 11 (top).

<img width="279" height="289" alt="Adwaita" src="https://github.com/user-attachments/assets/4dc26a3f-72f0-46a2-92aa-cfbbc0aaff65" />
<img width="279" height="289" alt="Windows" src="https://github.com/user-attachments/assets/ff054355-c9d7-42f1-a8b5-fe5061c135a1" />

## Usage

Install this project from source.
Alternatively, install from my COPR repo (no guarantees, it's for my personal use):
```bash
sudo dnf copr enable adil192/backports
sudo dnf install qadwaitadecorations-qt6
```

Then add the following in `~/.profile`/`~/.bash_profile`:

```bash
export QT_WAYLAND_DECORATION=qadwaitadecorations
```

## How to compile
This library uses private Qt headers and
will likely not be forward or backward compatible.
This library will have to be recompiled with every Qt update.

Install dependencies, e.g. for Fedora:
```bash
sudo dnf install qt6-qtbase-devel qt6-qtbase-static qt6-qtwayland-devel qt6-qtbase-private-devel qt6-qtsvg-devel
```

Build instructions for Qt6:

```bash
mkdir build
cd build
cmake ..
make && make install
```

Qt5 is end-of-life and untested with this project.
While it can be built using Qt5 (`cmake -DUSE_QT6=false ..`),
it is recommended to get backported changes from Qt 6.
You can get these [here](https://src.fedoraproject.org/rpms/qt5-qtwayland/blob/rawhide/f/qtwayland-decoration-support-backports-from-qt6.patch).

## License
The code is under [LGPL 2.1](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.en.html) with the "or any later version" clause.

## AI disclaimer (for the Windows-inspired fork)

People on the internet are always talking about getting "left behind" if we don't embrace AI.
Since I'm less familiar with C++, I gave GitHub Copilot some small tasks.

- [`updateColors`](https://github.com/adil192/QAdwaitaDecorations/blob/950c12ef0e4aed9d2a9c5a7bf327bd5f16a5a1fc/src/qadwaitadecorations.cpp#L192):

  I used it to give me the syntax for using QPalette.
  However, the code didn't work, used the wrong color groups and roles, and its code was ugly and far too verbose.

  So I had to rewrite it the old fashioned way.
  The Adwaita fallback for light/dark mode support and other tweaks were likewise done manually.

- [`renderFlatRoundedButtonFrame`](https://github.com/adil192/QAdwaitaDecorations/blob/950c12ef0e4aed9d2a9c5a7bf327bd5f16a5a1fc/src/qadwaitadecorations.cpp#L592):

  With a non-rounded rect, the close button overflowed the window's curved corners.
  I used GitHub Copilot to generate a rounded rectangle path where only the top right corner was rounded and not the other corners.

  It needed prompting a few times and I needed to fix up the code style.
  I didn't bother using AI for anything else.

I also use AI code review to catch my newbie C++ mistakes, which is actually quite good.
