<h1 align="center" style="border-bottom: none;">📦⚡️Casdoor flutter example</h1>
<h3 align="center">An example of casdoor-flutter-sdk</h3>

## 	The example uses the following Casdoor demo site server:

The server: https://door.casdoor.com/

## Quick Start

- download the code

```bash
git clone git@github.com:casdoor/casdoor-flutter-example.git
```

- install dependencies

```shell
flutter pub get
```
## Configure
Initialization requires 6 parameters, which are all str type:
|  Name (in order)   | Must  | Description |
|  ----  | ----  |----  |
| clientId  | Yes | Application.client_id |
| serverUrl  | Yes | Casdoor Server Url, such as `https://door.casdoor.com` |
| organizationName  | Yes | Organization name |
| appName  | Yes | Application name |
| redirectUri  | Yes | URI of Web redirection |
| callbackUrlScheme  | Yes | URL Scheme |

```
  final AuthConfig _config =  AuthConfig(
      clientId: "014ae4bd048734ca2dea",
      serverUrl: "https://door.casdoor.com",
      organizationName: "casbin",
      appName: "app-casnode",
      redirectUri: "http://localhost:9000/callback.html",
      callbackUrlScheme: "casdoor"
  );
```

## Run

```
flutter run -d chrome --web-port 9000
```

## Notes for different platforms

### Windows 10

Download the WebView2 runtime from [here](https://developer.microsoft.com/en-us/microsoft-edge/webview2/#download-section) and install it.

The WebView2 runtime is included in Windows 11 by default.

## Linux and macOS

Add the package `desktop_webview_window: ^0.2.3` inside *dependencies* to the *pubspec.yaml* file.

Modify the *main* function to look like the following:

```
void main(List<String> args) async {
  WidgetsFlutterBinding.ensureInitialized();
  if (runWebViewTitleBarWidget(args)) {
    return;
  }
  runApp(const MyApp());
}
```

### Web
On the Web platform an endpoint needs to be created that captures the callback URL and sends it to the application using the JavaScript postMessage() method. In the ./web folder of the project, create an HTML file with the name e.g. callback.html with content:

```
<!DOCTYPE html>
<title>Authentication complete</title>
<p>Authentication is complete. If this does not happen automatically, please
close the window.
<script>
  if (window.opener == null) {
    localStorage.setItem('casdoor-auth', window.location.href);
  } else {
    window.opener.postMessage({
      'casdoor-auth': window.location.href
    }, window.location.origin);
  };
  window.close();
</script>

```
Redirection URL passed to the authentication service must be the same as the URL on which the application is running (schema, host, port if necessary) and the path must point to created HTML file, /callback.html in this case, like  `callbackUri = "${_config.redirectUri}.html"`. The callbackUrlScheme parameter of the authenticate() method does not take into account, so it is possible to use a schema for native platforms in the code.It should be noted that when obtaining a token, cross domain may occur

For the Sign in with Apple in web_message response mode, postMessage from https://appleid.apple.com is also captured, and the authorization object is returned as a URL fragment encoded as a query string (for compatibility with other providers).

## After running, you will see the following  interfaces:
|  **Android**   | **iOS**  | **Web** |
|  ----  | ----  |----  |
| ![Android](screen-andriod.gif) |![iOS](screen-ios.gif)  |![Web](screen-web.gif) |
