# Train-departures-LED-32x64

- Running install.sh installs the app to /opt and sets up a systemd service that:
  - starts on boot (when the Pi is plugged in)
  - restarts automatically if it crashes

Display/hardware assumptions
- Intended for 2 chained LED panels with total display size 32x64.
- Shows next train departures from Västerås, Sweden.

Prerequisites (target Pi)
- Linux with systemd (Raspberry Pi OS / Debian typical)
- Root access (you will run install with sudo)
- No .NET runtime requirements, this bundle is self-contained

Prerequisites (only if you want to BUILD the bundle yourself)
- .NET SDK matching the project (e.g. .NET 10 if the project targets net10.0)

Install (on the Pi)
1) Copy this whole folder to the Pi.
2) From inside this folder, run:
   sudo ./install.sh

After install
- Status:
  systemctl status rgbmatrix-font-example.service
- Logs:
  journalctl -u rgbmatrix-font-example.service -f
- Stop (keep installed):
  sudo systemctl stop rgbmatrix-font-example.service
- Start again:
  sudo systemctl start rgbmatrix-font-example.service
- Restart (after config changes):
  sudo systemctl restart rgbmatrix-font-example.service
- Disable auto-start on boot:
  sudo systemctl disable --now rgbmatrix-font-example.service
- Configure LED options:
  sudo nano /etc/default/rgbmatrix-font-example
  sudo systemctl restart rgbmatrix-font-example.service

Changing colors at runtime
- Interactive mode supports typing color commands into the terminal, for example:
    bg 0 0 0
    150 110 0
- As a systemd daemon, the service does not have an interactive terminal (stdin),
  so that color-input method is not available.

Workarounds:
- Stop the daemon and run interactively when you want to change colors live:
    sudo systemctl stop rgbmatrix-font-example.service
    sudo /opt/rgbmatrix/font-example/FontExample --led-gpio-mapping=Adafruit-hat
- Or set initial colors in the code and redeploy (current behavior).
