# VeonJS 2.7.3

> **Crie, teste e distribua mini-plugins Java e JavaScript para Minecraft com hot-reload, Maven runtime, segurança avançada e pacotes `.mplugin`.**

O **VeonJS** é um runtime moderno para servidores **Paper/Spigot** que permite criar mini-plugins em **Java** e **JavaScript**, carregar alterações em runtime, recarregar sistemas sem reiniciar o servidor inteiro, resolver dependências Maven, empacotar produtos em `.mplugin` e preparar distribuições comerciais com diagnóstico, segurança e DX profissional.

Ele foi pensado para desenvolvedores, donos de servidores e equipes que querem acelerar o ciclo de desenvolvimento sem abandonar a API real do Bukkit/Paper.

---

## Oferta de lançamento

O **VeonJS** está disponível por apenas:

# R$59,90

**Preço promocional por tempo limitado.**

Para comprar, tirar dúvidas ou solicitar suporte, entre em contato pelo Discord:

**Discord: `kaka_dev`**

---

## Demonstração em vídeo

Vídeo demonstrativo da base do conceito:

https://www.youtube.com/watch?v=UFXXDSUhd8I&t=3s

O vídeo mostra uma das primeiras versões funcionais do VeonJS: criação e recarregamento de mini-plugins Java diretamente no servidor.

Desde essa demonstração, o projeto evoluiu bastante e agora inclui suporte a JavaScript, `.mplugin` v2/v3, carregamento em memória, scanner de risco, detector de leak, leitura otimizada com cache, DX com `/inspect`, `/doctor` e `/errors`, além de hardening para uso comercial.

---

## Por que usar o VeonJS?

Com o VeonJS, você pode editar um mini-plugin, salvar o arquivo e recarregar diretamente no servidor:

```text
/veonjs reload MeuPlugin
```

Sem reiniciar o servidor inteiro.  
Sem copiar JAR manualmente a cada teste.  
Sem perder tempo em ciclos longos de build.

O foco do VeonJS é unir velocidade de desenvolvimento com recursos reais de produto comercial:

- API Bukkit/Paper de verdade;
- hot-reload controlado;
- dependências Maven;
- mini-plugins Java e JavaScript;
- pacotes `.mplugin`;
- ferramentas de diagnóstico;
- fluxo de desenvolvimento amigável;
- segurança configurável para produção.

---

## Principais recursos

### Runtime e desenvolvimento

- Hot-reload de mini-plugins.
- Suporte a mini-plugins **Java**.
- Suporte a mini-plugins **JavaScript** com GraalJS.
- Modos `native`, `bridge`, `module`, `plugin` e `js`.
- Criação rápida de templates com `/veonjs create`.
- Comandos, permissões e aliases dinâmicos.
- Listeners/eventos Bukkit.
- Tasks e scheduler seguro.
- Configs e resources próprios por mini-plugin.
- Geração de estrutura para IDE.
- Exportação para distribuição.

### Performance

- Cache de leitura no `SourceScanner`.
- Evita reler `.java`, `.js`, `veon.yml` e resources quando nada mudou.
- Fingerprint com leitura em buffer.
- Ignora diretórios pesados como `.git`, `target`, `build`, `node_modules`, `.idea` e similares.
- `/veonjs doctor` mostra status do cache e da performance.

### Segurança e estabilidade

- Runtime com cleanup gerenciado de tasks, listeners, commands, permissions e resources.
- Registry para `AutoCloseable` e recursos gerenciados.
- Detector de possível vazamento de ClassLoader.
- Cleanup agressivo opcional para reduzir resíduos de hot-reload.
- Scanner estático de risco para detectar padrões perigosos antes do load.
- Modo de produção com bloqueio para findings de risco alto.
- Melhor tratamento de erros com sugestões práticas.
- CommercialShield com bloqueio para builds não protegidos quando configurado.

### Distribuição comercial

- Formato `.mplugin` para distribuição.
- `.mplugin` v2 com AES-256-GCM, HMAC e segredo externo.
- `.mplugin` v3 com manifesto assinado, payload criptografado e validação online de licença.
- Zero plaintext em disco por padrão no v3.
- Binding por servidor, licença, hardware ou combinações.
- Watermark por cliente/licença.
- Verificação com `/veonjs verify`.

---

## O que é um mini-plugin?

Um mini-plugin é uma unidade carregada pelo VeonJS. Ele pode ter comandos, eventos, configs, resources, dependências, tasks e lógica própria.

Exemplo de estrutura Java:

```text
plugins/VeonJS/miniplugins/MeuPlugin/
├── veon.yml
├── src/
│   └── com/exemplo/MeuPlugin.java
└── resources/
    └── config.yml
```

Exemplo de estrutura JavaScript:

```text
plugins/VeonJS/miniplugins/HelloJS/
├── veon.yml
└── index.js
```

Carregar:

```text
/veonjs load MeuPlugin
```

Recarregar:

```text
/veonjs reload MeuPlugin
```

Descarregar:

```text
/veonjs unload MeuPlugin
```

---

## Mini-plugin Java em arquivo único

```java
import com.marcosfgdev.net.veonjs.api.CommandContext;
import com.marcosfgdev.net.veonjs.api.VeonMiniPlugin;
import org.bukkit.command.CommandSender;

import java.util.List;

public final class HelloJava extends VeonMiniPlugin {

    @Override
    public void onEnable() {
        registerCommand(
                "hellojava",
                "Comando de exemplo Java",
                "/hellojava",
                List.of("hj"),
                "",
                this::execute
        );

        log("HelloJava enabled.");
    }

    private boolean execute(CommandContext context) {
        CommandSender sender = context.sender();
        sender.sendMessage("§aHello from Java + VeonJS!");
        return true;
    }
}
```

---

## Mini-plugin JavaScript

Exemplo de `veon.yml`:

```yaml
name: HelloJS
version: 1.0.0
mode: js
language: javascript
main: index.js
```

Exemplo de `index.js`:

```js
exports = {
  onEnable() {
    veon.command('hellojs', 'Comando JS de exemplo', '/hellojs', [], '', function(ctx, sender) {
      sender.sendMessage(veon.color('&aHello from JavaScript + VeonJS!'));
      return true;
    });

    veon.on('org.bukkit.event.player.PlayerJoinEvent', function(event) {
      event.player().sendMessage(veon.color('&bWelcome from JS.'));
    });

    veon.log('HelloJS enabled.');
  },

  onDisable() {
    veon.log('HelloJS disabled cleanly.');
  }
};
```

A API JS foi endurecida para produção: por padrão, o runtime usa acesso por allowlist e wrappers seguros. Acesso bruto ao host, Bukkit ou objetos Java sensíveis deve ser habilitado explicitamente.

---

## Modos de runtime

| Modo | Uso recomendado |
|---|---|
| `native` | Mini-plugin Java jarless com hot-reload e classloader próprio. |
| `bridge` | Mini-plugin com identidade Bukkit separada, melhor isolamento para alguns cenários. |
| `module` | Modo legado simples baseado em `VeonMiniPlugin`. |
| `plugin` | Modo experimental/legado para plugins reais. |
| `js` | Mini-plugin JavaScript com lifecycle `onLoad`, `onEnable`, `onDisable`. |

---

## Formato `.mplugin`

O VeonJS permite compilar um mini-plugin para um pacote distribuível:

```text
/veonjs compile MeuPlugin --force
```

Isso gera:

```text
MeuPlugin.mplugin
```

Instalação no cliente:

```text
plugins/
├── VeonJS.jar
└── MeuPlugin.mplugin
```

O Paper ignora o `.mplugin`; o VeonJS detecta e carrega o pacote.

---

## `.mplugin` v3

O `.mplugin` v3 foi criado para dificultar extração, cópia, alteração indevida e abertura offline de pacotes comerciais.

Arquitetura:

```text
.mplugin v3
├── manifest público assinado
├── payload AES-256-GCM
├── content key aleatória por pacote
├── validação online de licença
├── binding por server-id/hardware/license
├── classloading em memória
├── zero plaintext em disco
├── watermark por cliente
└── verificações de integridade
```

No v3, o runtime local não deve carregar sozinho um pacote premium: ele valida a licença e obtém autorização do license server.

Aviso honesto: nenhuma proteção local impede 100% um atacante com controle total do ambiente de runtime. O objetivo do v3 é impedir abertura offline simples, cópia casual, alteração indevida e reuso não autorizado, além de elevar bastante o custo de engenharia reversa.

---

## License server

O `.mplugin` v3 depende de um servidor de licença para validação e liberação segura do pacote.

Configuração base:

```yaml
mplugin:
  security:
    package-format: 'v3'
    binding-mode: 'server+license'
    license-file: 'plugins/VeonJS/license.id'

  v3:
    zero-plaintext: true

    license-server:
      url: 'https://seu-servidor.com'
      wrap-path: '/v3/mplugin/wrap'
      unwrap-path: '/v3/mplugin/unwrap'
      timeout-ms: 3000
      max-attempts: 2
      retry-backoff-ms: 250
      circuit-breaker-failures: 3
      circuit-breaker-open-ms: 30000
```

O cliente de licença usa parser JSON robusto, retry com backoff e circuit breaker para evitar tentativas repetidas quando o serviço remoto estiver indisponível.

---

## Dependências Maven

Mini-plugins podem declarar bibliotecas externas no `veon.yml`.

```yaml
maven:
  repositories:
    - id: central
      url: https://repo.maven.apache.org/maven2/

  dependencies:
    - groupId: com.google.zxing
      artifactId: core
      version: 3.5.3
```

Isso permite usar bibliotecas e APIs como OkHttp, ZXing, Gson/Jackson, PlaceholderAPI, Vault, LuckPerms e outras APIs Java compatíveis.

Por segurança, URLs `http://` e `file://` podem ser bloqueadas por padrão em ambientes de produção.

---

## GitHub Raw Loader

O VeonJS tem suporte a carregar source remota por URL raw:

```text
/veonjs github <rawUrl> --confirm-root
```

Esse recurso deve ser usado apenas por administradores confiáveis, pois baixa código-fonte, compila e carrega no servidor.

Em modo produção:

- o GitHub loader vem desativado por padrão;
- pode exigir uso pelo console;
- exige confirmação `--confirm-root`;
- exige allowlist de repositórios;
- limita tamanho do source baixado.

A permissão `veonjs.github` deve ser concedida com extremo cuidado.

---

## Segurança contra memory leak e hot-reload

Hot-reload seguro não é apenas trocar uma classe. O VeonJS cria classloaders novos e tenta limpar referências externas antes de soltar o classloader antigo.

O runtime rastreia e limpa:

- tasks;
- listeners;
- commands;
- permissions;
- services;
- plugin messaging channels;
- resources `AutoCloseable`;
- scheduler wrappers;
- caches de load.

Além disso, o VeonJS inclui detector de possível vazamento de ClassLoader.

Para melhor segurança, mini-plugins devem usar a API gerenciada do VeonJS em vez de registrar recursos direto no Bukkit.

Bom:

```java
registerEvents();
runTaskTimer(...);
registerCommand(...);
manage(resource);
```

Evite em hot-reload:

```java
// registrar recursos diretamente fora da API gerenciada do VeonJS
// criar threads próprias sem encerrar no onDisable
// manter caches estáticos mutáveis sem cleanup
```

---

## Scanner de risco

O `SourceRiskScanner` detecta padrões perigosos antes do load/compile, como:

- scheduler direto;
- listeners diretos;
- threads ou executors sem tracking;
- collections estáticas mutáveis;
- plugin messaging direto;
- services diretos;
- acesso JS a APIs sensíveis;
- objetos host expostos de forma ampla.

Em produção, o recomendado é fail-closed:

```yaml
safety:
  static-scan:
    enabled: true
    log-findings: true
    fail-on-high-risk: true
```

---

## Configuração de produção recomendada

```yaml
settings:
  github:
    enabled: false
    require-console: true
    require-confirm-root: true
    require-allowed-repository: true
    allowed-repositories: []

javascript:
  enabled: true
  host-access-policy: 'allowlist'
  trusted-host-access-all: false
  expose-bukkit-globals: false
  expose-host-plugin: false
  expose-host-objects: false
  allow-io: false

safety:
  classloader-leak-detector: true
  aggressive-classloader-cleanup: true
  static-scan:
    enabled: true
    log-findings: true
    fail-on-high-risk: true

security:
  commercial-protection:
    block-if-unprotected: true
```

---

## Comandos principais

```text
/veonjs help
/veonjs create <name> [native|bridge|module|plugin|js]
/veonjs load <name>
/veonjs reload <name>
/veonjs unload <name>
/veonjs reloadall
/veonjs list
/veonjs sources
/veonjs inspect <name>
/veonjs errors <name>
/veonjs doctor
/veonjs reloadconfig
/veonjs ide <name>
/veonjs export <name>
/veonjs deps [name|scan|classpath]
/veonjs maven <resolve|report|cache|clear> [miniPlugin]
/veonjs github <rawUrl> --confirm-root
/veonjs compile <name> [--force] [--no-libs]
/veonjs loadfile <name>
/veonjs loadcompiled <name>
/veonjs verify <name>
/veonjs packages
/veonjs mplugins
```

---

## Permissões

```text
veonjs.admin
veonjs.github
veonjs.maven
veonjs.compile
```

Notas:

- `veonjs.admin` deve ser restrita a administradores confiáveis.
- `veonjs.github` permite carregar source remota e deve ser altamente restrita.
- `veonjs.maven` permite resolver dependências externas.
- `veonjs.compile` permite gerar pacotes distribuíveis.

---

## Diagnóstico e DX

O VeonJS inclui comandos para melhorar a experiência de desenvolvimento:

```text
/veonjs doctor
/veonjs inspect <name>
/veonjs errors <name>
/veonjs sources
/veonjs deps scan
/veonjs maven report
```

`/veonjs inspect <name>` mostra:

- nome;
- modo;
- main class/script;
- versão;
- sources;
- fingerprint;
- findings do scanner estático.

`/veonjs errors <name>` mostra erros recentes com timestamp e sugestões de correção.

---

## Compatibilidade

O VeonJS é focado em servidores modernos com:

- Java 21;
- Paper/Spigot recentes;
- API Bukkit/Paper;
- ambiente compatível com compilação Java em runtime.

Alguns recursos avançados podem depender de bibliotecas opcionais, GraalJS, Maven Resolver, versão do servidor e configuração do ambiente.

Para produção, sempre teste em um servidor separado antes de colocar em público.

---

## Instalação básica

1. Coloque o `VeonJS.jar` na pasta `plugins/`.
2. Inicie o servidor uma vez para gerar as pastas.
3. Crie mini-plugins em:

```text
plugins/VeonJS/miniplugins/
```

ou instale pacotes `.mplugin` diretamente em:

```text
plugins/
```

4. Use os comandos do VeonJS para carregar, recarregar, diagnosticar ou compilar.

---

## Para quem é o VeonJS?

O VeonJS é ideal para:

- desenvolvedores de plugins Minecraft;
- donos de servidores Paper/Spigot;
- equipes que criam sistemas sob demanda;
- projetos comerciais de Minecraft;
- servidores que precisam testar features rapidamente;
- criadores que querem vender mini-plugins em `.mplugin`;
- devs que querem Java e JS no mesmo ecossistema;
- quem quer reduzir o ciclo tradicional de build/copy/restart.

---

## Exemplos do que você pode criar

- sistemas de loja;
- kits;
- warps;
- menus GUI;
- scoreboards;
- comandos administrativos;
- eventos customizados;
- sistemas de pagamento;
- integrações com APIs externas;
- automações internas para servidores;
- mini-plugins privados;
- produtos comerciais em `.mplugin`.

---

## Avisos importantes

O VeonJS acelera o desenvolvimento, mas segurança e performance continuam dependendo do código carregado.

Evite:

- executar código de origem desconhecida;
- dar `veonjs.github` para pessoas não confiáveis;
- rodar tarefas pesadas na main thread;
- criar threads sem cleanup;
- registrar listeners/tasks direto no Bukkit quando quiser hot-reload limpo;
- expor tokens e credenciais em configs públicas;
- usar dependências externas sem validar origem.

Para produtos comerciais, use `.mplugin` v3 com license server, assinatura, binding e zero plaintext.

---

## Comprar VeonJS

**Valor promocional:** R$59,90  
**Disponível por tempo limitado**

Contato para compra e suporte:

**Discord: `kaka_dev`**

---

## VeonJS

Java, JavaScript, hot-reload, Maven runtime, DX, segurança, `.mplugin` e distribuição comercial para servidores Minecraft.
