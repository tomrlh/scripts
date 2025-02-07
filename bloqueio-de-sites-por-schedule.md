####Setando o Cron para limitar acessos a site via launchd:

###### 1 criar o arquivo para bloquear

`sudo nano /Library/LaunchDaemons/com.blockyoutube.json`

```
{
  "Label": "com.blockyoutube",
  "ProgramArguments": [
    "/bin/bash",
    "-c",
    "echo '127.0.0.1 youtube.com' | sudo tee -a /etc/hosts"
  ],
  "StartCalendarInterval": [
    {
      "Hour": 1,
      "Minute": 0
    }
  ],
  "RunAtLoad": true
}
```

#### 2 criar arquivo para desbloquear

`sudo nano /Library/LaunchDaemons/com.unblockyoutube.json`

```
{
  "Label": "com.unblockyoutube",
  "ProgramArguments": [
    "/bin/bash",
    "-c",
    "sudo sed -i '' '/youtube.com/d' /etc/hosts"
  ],
  "StartCalendarInterval": [
    {
      "Hour": 11,
      "Minute": 30
    }
  ],
  "RunAtLoad": true
}
```

#### 3 converter o JSON para XML (formato aceito pelo launchd)

`sudo plutil -convert xml1 -o /Library/LaunchDaemons/com.blockyoutube.plist /Library/LaunchDaemons/com.blockyoutube.json`

`sudo plutil -convert xml1 -o /Library/LaunchDaemons/com.unblockyoutube.plist /Library/LaunchDaemons/com.unblockyoutube.json`

#### 4 carregar o job

`sudo launchctl load -w /Library/LaunchDaemons/com.blockyoutube.plist`

`sudo launchctl load -w /Library/LaunchDaemons/com.unblockyoutube.plist`

#### 5 validar se está rodando

`launchctl list | grep youtube`

resultado esperado

```
- 0    com.blockyoutube
- 0    com.unblockyoutube
```

```

```
