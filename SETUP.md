# Setup

Die Bereitstellung der Komponenten des TAS sowie der zum Betrieb erforderlichen Anwendungen (i.e., ILIAS, Learning Locker), erfolgt in Form von Appstack-Apps. Informationen zu Appstack und zum Setup von Appstack finden sich in der zugehörigen [Dokumentation](https://gitlab.com/eqsoft-appstack/doc). Zum Start eines Appstack-Stacks mit den erforderlichen Konfigurationen kann die folgende Konfigurationsdatei verwendet werden:

```yaml
appstack:
  host: "8.8.8.8"
  sub_net: "172.18.100.0/24"
  external_domain: "verdatas.example.de"
  config_image: "docker.io/eqsoft4/as-generic-config:bullseye-slim"
  socks_domains:
    - "*.example.com"
    - "*verdatas*"
stacks:
  - id: "global"
    type: "global"
    apps:
      - name: "launcher"
        repo:
          uri: "gitlab.com/eqsoft-appstack/apps/launcher"
        config:
          env:
            log_folder: "/var/log/as-global-launcher-sshd"
      - name: "nettool"
        repo:
          uri: "gitlab.com/eqsoft-appstack/apps/nettool"
      - name: "certs"
        repo:
          uri: "gitlab.com/eqsoft-appstack/apps/certs"
      - name: "mathjax"
        repo:
          uri: "gitlab.com/eqsoft-appstack/apps/mathjax"
  - id: "verdatas"
    type: "workload"
    apps:
    - name: "rel8"
      repo:
         uri: "gitlab.com/eqsoft-appstack/apps/ilias"
      files:
        - path: "files/config/plugins.json"
          content: |-
            [
              {
                  "name": "XapiProgress",
                  # TODO: check url
                  "gitUrl": "https://gitlab.com/ukohnle/XapiProgress.git",
                  "pluginPath": "Services/UIComponent/UserInterfaceHook/XapiProgress",
                  "patchFileDirPath": "patches/release_8"
              },
              {
                  "name": "H5P",
                  "gitUrl": "https://github.com/srsolutionsag/H5P.git",
                  "pluginPath": "Services/Repository/RepositoryObject/H5P"
              },
              {
                  "name": "H5PPageComponent",
                  "gitUrl": "https://github.com/srsolutionsag/H5PPageComponent.git",
                  "pluginPath": "Services/COPage/PageComponent/H5PPageComponent"
              },
              {
                  "name": "VerDatAsBot",
                  "gitUrl": "https://github.com/VerDatAs/tud-chatbot-plugin.git",
                  "pluginPath": "Services/UIComponent/UserInterfaceHook/VerDatAsBot"
              },
              {
                  "name": "VerDatAsDsh",
                  "gitUrl": "https://github.com/VerDatAs/tud-dashboard-plugin.git",
                  "pluginPath": "Services/COPage/PageComponent/VerDatAsDsh"
              }
            ]
      config:
        env:
          repo_uri: "github.com/ILIAS-eLearning/ILIAS"
          repo_branch: "release_8"
          repo_user: ""
          repo_token: ""
          repo_opts: ""
          traefik: true
          traefik_net: "proxy"
        storage: {
          active: "true"
        }
    - name: "ll71"
      repo:
        uri: "gitlab.com/eqsoft-appstack/apps/learninglocker"
      config:
        env:
          traefik: true
          traefik_net: "proxy"
        storage: {
          active: "true"
        }
    - name: "tas"
      repo:
      # TODO: fix uri
        uri: ""
      config:
        env:
          nginx_docs_basic_auth_credentials: "doc:$apr1$2vTZU7VS$AJ56ci0qCtP9gsz5gfhsu/"
          jwt_secret_key: "<JWT_SECRET_KEY>"
          cors_allowed_origins: "https://rel8verdatas.verdatas.example.de"
          ll_password: "<LL_PASSWORD>"
          ll_url: "https://ll71verdatas.verdatas.example.de"
          ll_username: "75aa90a571b22fddb0ecf07c332e8589181e0837"
          statement_sender_password: "<STATEMENT_SENDER_PASSWORD>"
          statement_sender_username: "stuser"
          disabled_assistance_types: ""
          ollama_url: ""
          traefik: true
          traefik_net: "proxy"
        storage: {
          active: "true"
        }
```

Mit Hilfe dieser Konfiguration wird eine Appstack-Workload-Stack mit dem Namen `verdatas` definiert, der die folgenden Appstack-Apps enthält: [ILIAS](https://gitlab.com/eqsoft-appstack/apps/ilias8) mit dem Name `rel8`, [Learning Locker](https://gitlab.com/eqsoft-appstack/apps/learninglocker) mit dem Namen `ll71` und das [TAS](TODO) mit dem Namen `tas`. Weitere Informationen zu bzw. Beispielwerte für die angegebenen Umgebungsvariable finden sich in den verlinkten Repositories der einzelnen Appstack-Apps bzw. der zugehörigen Applikationen und Services. Die Konfiguration setzt ein Appstack-Setup mittels Traefik voraus (siehe [Appstack Trafik](https://gitlab.com/eqsoft-appstack/traefik)), kann nach Entfernung der entsprechenden Konfigurationen (i.e., `traefik: true` und `traefik_net: "proxy"`) aber auch problemlos ohne Traefik betrieben werden. In dieser Konfiguration wird beispielhaft die fiktive Domain `verdatas.example.de` herangezogen, auf welche in den weiteren Einrichtungsschritten Bezug genommen wird. Bei der Anwendung der obigen Konfiguration muss diese Domain (Konfigurationsparameter mit dem Schlüssel `external_domain`) sowie die IP-Adresse des Servers, auf dem das Setup durchgeführt wird (Konfigurationsparameter mit dem Schlüssel `141.76.56.188` beispielhaft auf `8.8.8.8` gesetzt), angepasst werden.

## Einrichtung

Im Folgenden werden die erforderlichen manuellen Einrichtungsschritte für die einzelnen Appstack-Apps beschrieben. Einige dieser Einrichtungsschritte beziehen sich auf die Erstellung von Credentials welche anschließend in der Appstack-Konfiguration hinterlegt werden müssen. Nach dem Update der Konfiguration kann der Appstack-Stack, wie in der [Dokumentation](https://gitlab.com/eqsoft-appstack/doc) beschrieben, mit dem folgenden Befehl neugestartet werden:

```bash
appsctl update && appsctl restart --id verdatas
```

## TAS

* Erstellung eines Long-Lived Tokens
  * Abfrage eines Authentifikationstokens für einen Nutzer mit der Rolle `ADMIN` mittels POST an `/api/v1/auth/login` (z.B. `https://tasverdatas.verdatas.example.de/api/v1/auth/login`) mit:
  ```json
  {
    "actorAccountName": "admin",
    "password": "<PASSWORD>"
  }
  ```
  * Erstellung eines Long-Lived Token mittels PUT an `/api/v1/users/me/token` (z.B. `https://tasverdatas.verdatas.example.de/api/v1/users/me/token`) mit dem abgefragten Authentifikationstoken im Header mit dem Key `Authorization` und dem Value `Bearer <TOKEN>`

### LearningLocker

* Aufruf von [https://ll71verdatas.verdatas.example.de](https://ll71verdatas.verdatas.example.de)
* Erstellung eines neuen Stores über `Settings -> Stores -> Add new` zum Beispiel mit dem Namen `TAS Store` (Mit dem neuen Store wird automatisch ein entsprechender Client unter `Settings -> Clients` erzeugt. Der Client kann optional umbenannt werden zum Beispiel zu `Client for TAS Store`. Die Credentials des Clients müssen in der Konfiguration der Appstack-App `tas` hinterlegt werden.)
* Einrichtung der Weiterleitung der Statements an das TAS über `Data -> Statement Forwarding -> Add new`
  * Angabe des TAS-Statement-Endpunkts (z.B. [https://tasverdatas.verdatas.example.de/api/v1/statements](https://tasverdatas.verdatas.example.de/api/v1/statements))
  * Angabe des Long-Lived Tokens des TAS (siehe Einrichtung TAS) als `Secret Key` für `Auth Type` `token`

## ILIAS

* Aufruf von [https://rel8verdatas.verdatas.example.de](https://rel8verdatas.verdatas.example.de)
* Anlegung eines Learning Record Stores über `Administration -> Extending ILIAS -> LRS -> Add LRS-Type`:
  * Angabe des Learning Locker Endpoints, welcher unter [https://ll71verdatas.verdatas.example.de](https://ll71verdatas.verdatas.example.de) über `Settings -> Clients` ausgelesen werden kann, zum Beispiel [https://ll71verdatas.verdatas.example.de/data/xAPI](https://ll71verdatas.verdatas.example.de/data/xAPI)
  * Angabe der Credentials des Learning Locker Clients, welche unter [https://ll71verdatas.verdatas.example.de](https://ll71verdatas.verdatas.example.de) über `Settings -> Clients -> <Name des Clients>` ausgelesen werden können
* Aktivierung der erforderlichen Plugins über `Administration -> Extending ILIAS -> Plugins`
  * Installation aller in der Appstack-Konfiguration definierter Plugins (i.e., `H5PPageComponent`, `VerDatAsDsh`, `H5P`, `VerDatAsBot` und `XapiProgress`) über Dropdown rechts vom Plugin und `Install`
  * Zuweisung des angelegten Learning Record Stores über Dropdown rechts von `XapiProgress` und `Konfigurieren -> Basiskonfiguration` und über Dropdown rechts von `VerDatAsDsh` und `Configure` und über Dropdown rechts von `VerDatAsBot` und `Configure`
  * Angabe der TAS-Backend URL (z.B. [https://tasverdatas.verdatas.example.de](https://tasverdatas.verdatas.example.de)) über Dropdown rechts von `VerDatAsDsh` und `Configure` und über Dropdown rechts von `VerDatAsBot` und `Configure`
  * Optional Konfiguration des xAPI-Statement-Sendeverhaltens über Dropdown rechts von `XapiProgress` und `Configure` (z.B. Auswahl von Events und Auswahl von H5P-Verben)
  *  Aktivierung aller in der Appstack-Konfiguration definierter Plugins (i.e., `H5PPageComponent`, `VerDatAsDsh`, `H5P`, `VerDatAsBot` und `XapiProgress`) über Dropdown rechts vom Plugin und `Activate`
