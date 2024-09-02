**1. Install a WiFi FTP Server on Android**
1. **Download an FTP Server App**: Install an app like **“WiFi FTP Server”** or any similar app from the Google Play Store.
2. **Configure the FTP Server**:
	• Open the app and configure the FTP server settings (port number, username, password, etc.).
	• Start the FTP server.
**2. Connect Your Linux Laptop to the Android Hotspot**
1. **Enable Mobile Hotspot on Android**:
	• Go to **Settings** > **Network & internet** > **Hotspot & tethering** > **Wi-Fi hotspot** and enable it.
	• Make a note of the hotspot’s SSID (name) and password.
2. **Connect Your Linux Laptop to the Android Hotspot**:
	• On your Linux laptop, open the Wi-Fi settings and connect to the Android hotspot using the provided SSID and password.
**3. Access the FTP Server from Linux**
1. **Open Your File Manager**:
	• On your Linux laptop, open your file manager (e.g., Nautilus, Dolphin, Thunar).
2. **Connect to the FTP Server**:
	• In the file manager, find the option to connect to a server (often under the “Other Locations” or “Network” tab).
	• Enter the FTP URL you configured in the Android app, typically in the format: `ftp://192.168.x.x:port` for example `ftp://192.168.115.185:2221`
	• If prompted, enter the username and password you set up in the FTP server app. Alternatively, choose “Anonymous” if you set the FTP server to allow anonymous connections.
**4. Access Files:**
	• Once connected, you’ll see the Android device’s storage as a folder in your file manager. You can now transfer files between your Linux laptop and Android device.

