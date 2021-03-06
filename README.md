# HTML5-sip-client
[![GitHub release](https://img.shields.io/github/release/ernaniaz/HTML5-sip-client.svg?maxAge=2592000)](https://github.com/ernaniaz/HTML5-sip-client)
[![GitHub license](https://img.shields.io/github/license/ernaniaz/HTML5-sip-client.svg)](https://github.com/ernaniaz/HTML5-sip-client)

A Javascript SIP client based on [SIP.js](http://sipjs.com/).

HTML5-sip-client is a Javascript based SIP client that uses WebRTC and WebSockets to connect to your SIP server.  The UI is designed to be launched as a popup from within your application.  This project was originally based on ctxSip, got some implementations from ha36d fork and many other implementations made, like Brazilian Portuguese internationalization, implementation of DTMF tones and more.

## Features

- Audio only, Hold / Resume, Mute, multiple call support.
- Auto Answer.
- Multi language.
- No plugins required, Works with WebSocket / WebRTC enabled browsers. (Firefox, Google Chrome, Safari, Edge and other modern browsers).
- Call log is saved to localStorage.
- Intuitive interface makes it easy for users.
- Easy to configure and integrate into your project.
- MIT licensed.

SSL connections for are required for this to work!

## Dependencies

- [SIP.js](http://sipjs.com/)
- [Bootstrap](http://getbootstrap.com/)
- [FontAwesome](http://fortawesome.github.io/Font-Awesome/)
- [jQuery](http://jquery.com/)
- [Moment.js](http://momentjs.com/)
- [timeago](https://timeago.yarp.com/)
- [translate.js](http://www.openxrest.com/translatejs/)

## Configuring

The webphone application has some hardcoded configurations you'll probably need to change. At js/app.js will find at line 44 the websocket URI, that point to the same server that provided the HTML webphone app page, connecting at port 443 using protocol WSS (Secure WebSocket) and at path /ws. You can change it to point to your SIP server WebSocket port to test. I didn't recomend you to leave your SIP server open to the network, but use a HTTP proxy to provide access only to the this resource at /ws. If you use NGiNX (and Asterisk server, for example), you can use the following NGiNX server configuration:

```
# Asterisk websocket connection
location /ws {
    proxy_pass http://127.0.0.1:8088/ws;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_read_timeout 86400s;
}
```

Also, the username used to authenticate at your SIP server was prepend to 'ws' string (js/app.js at line 33). Change it to reflect your server configuration.

## Test installation

To test this software, you can easily configure your system, installing a clean, minimal, CentOS 7 OS. You'll need to install Asterisk and NGiNX. You can do it installing [VoIPDomain Asterisk repo](https://packagecloud.io/voipdomain/production), with:

```bash
curl -s https://packagecloud.io/install/repositories/voipdomain/production/script.rpm.sh | sudo bash
```

After configure the repository, install the basic packages with:

```bash
yum install -y asterisk asterisk-opus asterisk-sip asterisk-pjsip nginx
```

You'll need to create the Asterisk SSL configuration files, to do it, use:

```bash
mkdir /etc/asterisk/keys
ast_tls_cert -C localhost.localdomain -O "Webphone Test" -d /etc/asterisk/keys
```

And to create the NGiNX self-signed certificate files, use:

```bash
mkdir /etc/nginx/ssl
openssl req -new -x509 -sha256 -newkey rsa:2048 -days 365 -nodes -out /etc/nginx/ssl/webphone.localnet.pem -keyout /etc/nginx/ssl/webphone.localnet.key
chmod 600 /etc/nginx/ssl/webphone.localnet.key
openssl req -new -sha256 -key /etc/nginx/ssl/webphone.localnet.key -out /etc/nginx/ssl/webphone.localnet.csr
```

Copy configuration files from configs/{asterisk,nginx} to your installation, deploy this repo to /var/www/html, start the NGiNX and Asterisk and access your machine using a modern web browser.

## Demo

Clone the repository and use the following command in the directory
```bash
python -m SimpleHTTPServer 8000
```

## License
MIT License.
