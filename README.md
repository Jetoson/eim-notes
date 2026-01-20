# EIM Notes

## Version Control
### Github Setup
1. New Repository.
2. Name: `YOUR-PROJECT-NAME`
3. Public/Private: `Public`.
4. Initialize this repository with:
&nbsp;&nbsp;&nbsp;&nbsp; - Check Add a `README` file.
&nbsp;&nbsp;&nbsp;&nbsp; - Add `.gitignore`: Select Android from the dropdown.
&nbsp;&nbsp;&nbsp;&nbsp; - Choose a license: Select `MIT License`.
5. Click `Create repository`.
6. Edit README: With `SEID Jemal Ahmed, 341C2`
7. Clone: Click the green `Code` button -> `Copy URL`.
&nbsp;&nbsp;&nbsp;&nbsp; - Open Terminal/Command Prompt.
&nbsp;&nbsp;&nbsp;&nbsp; - Run: git clone `PASTE_URL`

### Android Studio Creation
1. Open Android Studio -> `New Project`.
2. Select Template: Choose `Empty Views Activity`.
3. Configure Project:
&nbsp;&nbsp;&nbsp;&nbsp; - Name: `YOUR-PROJECT-NAME`
&nbsp;&nbsp;&nbsp;&nbsp; - Package name: `ro.pub.cs.systems.eim.YOUR-PROJECT-NAME`
&nbsp;&nbsp;&nbsp;&nbsp; - Save location: Point this to a `Temp` folder.
&nbsp;&nbsp;&nbsp;&nbsp; - Language: `Kotlin`
&nbsp;&nbsp;&nbsp;&nbsp; - Minimum SDK: `API 16: Android 4.1 (Jelly Bean)`.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - If your Android Studio hides API 16, select the lowest available (e.g., API 24).
        You can lower it in build.gradle later, but usually, the wizard limitation is accepted.
&nbsp;&nbsp;&nbsp;&nbsp; - Click `Finish`.
### Naming
1. Activity
&nbsp;&nbsp;&nbsp;&nbsp; - Right-click on `MainActivity.kt` in the project view.
&nbsp;&nbsp;&nbsp;&nbsp; - Select `Refactor` -> `Rename`.
&nbsp;&nbsp;&nbsp;&nbsp; - Change name to: `YOUR-PROJECT-NAMEMainActivity`.
&nbsp;&nbsp;&nbsp;&nbsp; - Check all boxes (Search in comments/strings) so it updates the `Manifest` automatically.
&nbsp;&nbsp;&nbsp;&nbsp; - Click `Refactor`.
2. Layout:
&nbsp;&nbsp;&nbsp;&nbsp; - Check `res/layout`
&nbsp;&nbsp;&nbsp;&nbsp; - Right-click `activity_main.xml` -> `Refactor` -> `Rename` -> Enter `activity_YOUR-PROJECT-NAME_main`

### Merging Project into Repo
1. Open your file explorer.
2. Copy all files from inside the `Temp` folder.
3. Paste them into your cloned `YOUR-PROJECT-NAME` git repository folder.
4. Open the git folder in Android Studio now to verify it works.
5. Commit:
&nbsp;&nbsp;&nbsp;&nbsp; - Open Terminal in Android Studio.
&nbsp;&nbsp;&nbsp;&nbsp; - `git add .`
&nbsp;&nbsp;&nbsp;&nbsp; - `git commit -m "Initial project setup"`
&nbsp;&nbsp;&nbsp;&nbsp; - `git push`

## Google API
### Configuring OAuth Client ID
1. Go to [here](https://console.developers.google.com/)
2. Select `OAuth consent screen` from the left menu bar
3. Click `Create OAuth client`:
&nbsp;&nbsp;&nbsp;&nbsp; - Application Type: Android
&nbsp;&nbsp;&nbsp;&nbsp; - Name: EIM-Test
&nbsp;&nbsp;&nbsp;&nbsp; - Package name: ro.pub.cs.systems.eim.PROJECT-NAME
&nbsp;&nbsp;&nbsp;&nbsp; - SHA-1 certificate fingerprint:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Run the command below
```Powershell
& "C:\Program Files\Android\Android Studio1\jbr\bin\keytool.exe" -list -v -keystore "$env:USERPROFILE\.android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Copy the line that starts with SHA1: followed by a long string of hex pairs (like AA:BB:CC...)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Paste it to the google console
 
## Comunicația prin Sockeți în Android

### Pentru USB tethering, pe telefon, se conecteaza telefonul cu calculato prin USB si apoi se accesează:
`Settings` → `Network and Internet` → `Hotspot and tethering` și se selectează opțiunea `USB Tethering`
### adb 
Path: 
```Powershell
C:\Users\Jemal\AppData\Local\Android\Sdk\platform-tools
```
Verify USB tethering using commands:
```Powershell
.\adb.exe devices # list all devices
```
```Powershell
.\adb.exe -s <device-name> shell # open shell access
```
```Powershell
ifconfig # or ip add
```
Run the command `ipconfig` on windows to verify that the IP address you get from the adb interface
is now a default gateway for one of the interfaces of the windows machine.

Logcat:
> package:mine tag:activitylifecycle
> tag-ul sa gaseste in fisierul Constants in forma:
> const val TAG = <nume>

Accessing  the emulator using Telnet:
```Powershell
.\adb.exe devices   # check whether the emulator is up and running
```
```Powershell
telnet localhost 5554
```
```Powershell
auth 2tjDga2gJ3stZHgQ  # tha auth string can be found at C:\Users\Jemal\.emulator_console_auth_token
```
### Sockets
Permisiuni speciale:
```Bash
<manifest ...>
  <!-- other application properties or components -->
  <uses-permission
    android:name="android.permission.INTERNET" />
  <!-- other application properties or components --> 
</manifest>
```
Pasi pentru comunicatia prin intermediul unui obiect de tip Socket:
1. Deschiderea unui socket
```Java
String hostname = "localhost";
int port = 2000;
Socket socket = new Socket(hostname, port);
```
2. Crearea unui output stream
```Java
BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(socket.getOutputStream());
PrintWriter printWriter = new PrintWriter(bufferedOutputStream, true);
```
3. Crearea unui flux de intrare
```Java
InputStreamReader inputStreamReader = new InputStreamReader(socket.getInputStream());
BufferedReader bufferedReader = new BufferedReader(inputStreamReader);
```
N.B: Se recomandă ca obținerea de referințe către fluxul de intrare respectiv fluxul de 
ieșire asociate unui obiect de tip Socket să fie realizată prin intermediul unor metode statice
definite în cadrul unor clase ajutător:

```Java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class Utilities {

  public static BufferedReader getReader(Socket socket) throws IOException {
    return new BufferedReader(new InputStreamReader(socket.getInputStream()));
  }
  
  public static PrintWriter getWriter(Socket socket) throws IOException {
    return new PrintWriter(socket.getOutputStream(), true);
  }

}
```
4. închiderea obiectului Socket
```Java
socket.close();
```



















