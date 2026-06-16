# Twitch Chat in Game Chat (Patched)

This is a patched version of "Twitch Chat in Game Chat" that fixes broken Twitch connectivity caused by outdated IRC WebSocket handling.

### ca/raindoggames/twitchintegration/irc/Irc.class

Before: `ws://irc-ws.chat.twitch.tv:80`
After: `wss://irc-ws.chat.twitch.tv:443`

Original mod:
[https://www.curseforge.com/minecraft/mc-mods/twitch-chat-in-game-chat/](https://www.curseforge.com/minecraft/mc-mods/twitch-chat-in-game-chat/)

### Reconstructed from bytecode:

```java
package ca.raindoggames.twitchintegration.irc;

import ca.raindoggames.twitchintegration.TwitchIntegration;
import ca.raindoggames.twitchintegration.config.ConfigManager;
import ca.raindoggames.twitchintegration.irc.Client;
import ca.raindoggames.twitchintegration.server.TokenManager;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.net.URI;
import java.net.URISyntaxException;

public class Irc implements ActionListener {
    Client client;
    boolean connected = false;
    TokenManager tokenManager;

    public Irc(TokenManager tokenManager) {
        this.tokenManager = tokenManager;
        try {
            URI uri = new URI("wss://irc-ws.chat.twitch.tv:443");
            this.client = new Client(uri);
        } catch (URISyntaxException e) {
            TwitchIntegration.LOGGER.info("Failed to connect to twitch");
            TwitchIntegration.LOGGER.error((Object)e);
        }
    }

    public void connect() {
        ConfigManager configManager = new ConfigManager();
        String accessToken = this.tokenManager.getAccessToken();
        String account = configManager.getAccount();
        this.client.setPassword(accessToken);
        this.client.setAccount(account);
        this.client.connect();
        this.connected = true;
    }

    public void close() {
        this.client.close();
        this.connected = false;
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        TwitchIntegration.LOGGER.info("Event received");
        TwitchIntegration.LOGGER.info((Object)e);
    }
}
```

I used [https://github.com/GNOME/ghex](Ghex) & [https://github.com/Col-E/Recaf](Recaf) to edit the bytecode 

This build is only intended to restore functionality and is not an official release.

Please leave a star ⭐
