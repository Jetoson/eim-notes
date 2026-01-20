# EIM Notes

## Version Control
### Github Setup
1. New Repository.
2. Name: `YOUR-PROJECT-NAME`
3. Public/Private: `Public`.
4. Initialize this repository with:
        - Check Add a `README` file.
        - Add `.gitignore`: Select Android from the dropdown.
        - Choose a license: Select `MIT License`.
5. Click `Create repository`.
6. Edit README: With `SEID Jemal Ahmed, 341C2`
7. Clone: Click the green `Code` button -> `Copy URL`.
        - Open Terminal/Command Prompt.
        - Run: git clone `PASTE_URL`

### Android Studio Creation
1. Open Android Studio -> `New Project`.
2. Select Template: Choose `Empty Views Activity`.
3. Configure Project:
        - Name: `YOUR-PROJECT-NAME`
        - Package name: `ro.pub.cs.systems.eim.YOUR-PROJECT-NAME`
        - Save location: Point this to a `Temp` folder.
        - Language: `Kotlin`
        - Minimum SDK: `API 16: Android 4.1 (Jelly Bean)`.
                - If your Android Studio hides API 16, select the lowest available (e.g., API 24).
                        You can lower it in build.gradle later, but usually, the wizard limitation is accepted.
        - Click `Finish`.

### Naming
1. Activity
        - Right-click on `MainActivity.kt` in the project view.
        - Select `Refactor` -> `Rename`.
        - Change name to: `YOUR-PROJECT-NAMEMainActivity`.
        - Check all boxes (Search in comments/strings) so it updates the `Manifest` automatically.
        - Click `Refactor`.
2. Layout:
        - Check `res/layout`
        - Right-click `activity_main.xml` -> `Refactor` -> `Rename` -> Enter `activity_YOUR-PROJECT-NAME_main`

### Merging Project into Repo
1. Open your file explorer.
2. Copy all files from inside the `Temp` folder.
3. Paste them into your cloned `YOUR-PROJECT-NAME` git repository folder.
4. Open the git folder in Android Studio now to verify it works.
5. Commit:
        - Open Terminal in Android Studio.
        - `git add .`
        - `git commit -m "Initial project setup"`
        - `git push`

## Google API
### Configuring OAuth Client ID
1. Go to [here](https://console.developers.google.com/)
2. Select `OAuth consent screen` from the left menu bar
3. Go to `Clients` and then `Create a client`:
        - Application Type: Android
        - Name: EIM-Test
        - Package name: ro.pub.cs.systems.eim.PROJECT-NAME
        - SHA-1 certificate fingerprint:
        - Run the command below from the Android Studio Terminal
```Powershell
& "C:\Program Files\Android\Android Studio1\jbr\bin\keytool.exe" -list -v -keystore "$env:USERPROFILE\.android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```
- Copy the line that starts with SHA1: followed by a long string of hex pairs (like AA:BB:CC...)
```bash
 22:A8:89:0A:B5:C3:CE:BA:CC:4B:59:33:2C:F9:F5:E2:B9:FD:4B:FE
```
- Paste it to the google console
 
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



















