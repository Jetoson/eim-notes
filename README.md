# EIM Notes
```Powershell
```
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



















