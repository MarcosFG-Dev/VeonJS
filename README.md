# VeonJS

> **Crie, teste e distribua mini-plugins Java para Minecraft com muito mais velocidade.**

O **VeonJS** é um runtime moderno para servidores **Paper/Spigot** que permite criar mini-plugins em Java usando a API real do Bukkit/Paper, com suporte a hot-reload, dependências Maven, comandos, eventos, configs, menus GUI e distribuição em pacotes protegidos `.mplugin`.

Ele foi pensado para desenvolvedores, donos de servidores e equipes que querem criar sistemas Minecraft profissionais sem ficar preso ao ciclo tradicional de compilar JAR, desligar servidor, copiar arquivo e reiniciar tudo a cada alteração.

---

## Oferta de lançamento

O **VeonJS** está disponível por apenas:

# R$59,90

**Preço promocional por tempo limitado.**

Para comprar, tirar dúvidas ou solicitar suporte, entre em contato pelo Discord:

**Discord: `kaka_dev`**

---

## Por que usar o VeonJS?

Com o VeonJS, você pode editar seu mini-plugin Java, salvar o arquivo e recarregar diretamente no servidor:

```text
/veonjs reload MeuPlugin
```

Sem reiniciar o servidor inteiro.
Sem ficar recompilando manualmente o projeto toda hora.
Sem perder tempo em testes simples.

O foco é acelerar o desenvolvimento mantendo o poder do Java e da API real do Minecraft.

---

## Principais recursos

- **Hot-reload de mini-plugins Java**
- **Suporte à API Bukkit/Spigot/Paper**
- **Mini-plugins com múltiplas classes**
- **Comandos e permissões dinâmicas**
- **Listeners e eventos Bukkit**
- **Tasks e scheduler seguro**
- **Menus GUI como plugins reais**
- **Configs e resources próprios por mini-plugin**
- **Maven Dependency Runtime**
- **GitHub Raw Source Loader**
- **Formato `.mplugin` para distribuição protegida**
- **Auto-load de `.mplugin` direto da pasta `plugins/`**
- **Build comercial com pipeline de proteção**
- **Documentação e exemplos para começar rápido**

---

## O que é um mini-plugin?

Um mini-plugin é um plugin Java carregado pelo VeonJS. Ele pode ter comandos, eventos, configs, menus, tasks, dependências e lógica própria.

Exemplo de estrutura:

```text
plugins/VeonJS/miniplugins/MeuPlugin/
├── veon.yml
├── src/
│   └── com/exemplo/MeuPlugin.java
└── resources/
    └── config.yml
```

Exemplo de comando para carregar:

```text
/veonjs load MeuPlugin
```

Exemplo de comando para recarregar:

```text
/veonjs reload MeuPlugin
```

---

## Formato `.mplugin`

O VeonJS também permite compilar um mini-plugin para um pacote próprio:

```text
/veonjs compile MeuPlugin
```

Isso gera um arquivo:

```text
MeuPlugin.mplugin
```

Esse formato é ideal para distribuição, venda e instalação mais simples.

O cliente pode instalar assim:

```text
plugins/
├── VeonJS.jar
└── MeuPlugin.mplugin
```

O Paper ignora o `.mplugin`, e o VeonJS carrega automaticamente.

---

## Dependências Maven

Mini-plugins podem usar bibliotecas externas declaradas no `veon.yml`.

Exemplo:

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

Isso permite criar mini-plugins usando bibliotecas como:

- Mercado Pago
- OkHttp
- ZXing
- PlaceholderAPI
- Vault
- LuckPerms
- Outras APIs Java compatíveis

---

## GitHub Raw Loader

Também é possível baixar uma source Java diretamente de uma URL raw permitida:

```text
/veonjs github https://raw.githubusercontent.com/user/repo/main/MeuPlugin.java
```

O VeonJS valida domínio, nome do arquivo e permissões antes de salvar e carregar a source.

---

## Comandos principais

```text
/veonjs help
/veonjs load <nome>
/veonjs unload <nome>
/veonjs reload <nome>
/veonjs reloadall
/veonjs list
/veonjs sources
/veonjs errors <nome>
/veonjs create <nome>
/veonjs doctor
/veonjs github <rawUrl>
/veonjs maven resolve <nome>
/veonjs compile <nome>
/veonjs verify <nome>
/veonjs packages
```

---

## Permissões

```text
veonjs.admin
veonjs.github
veonjs.maven
```

Por padrão, comandos administrativos devem ser usados por operadores ou administradores autorizados.

---

## Para quem é o VeonJS?

O VeonJS é ideal para:

- Desenvolvedores de plugins Minecraft
- Donos de servidores Paper/Spigot
- Equipes que criam sistemas sob demanda
- Projetos comerciais de Minecraft
- Servidores que precisam testar features rapidamente
- Criadores que querem vender mini-plugins em formato `.mplugin`

---

## Exemplos do que você pode criar

- Sistemas de loja
- Menus GUI
- Kits
- Warps
- Scoreboards
- Sistemas de pagamentos
- Painéis web
- Integrações com APIs externas
- Comandos administrativos
- Sistemas de eventos
- Mini-plugins privados para servidores

---

## Compatibilidade

O VeonJS é focado em servidores modernos com **Java 21** e versões recentes de **Paper/Spigot**.

Algumas features avançadas podem depender da versão do servidor, da API disponível e das bibliotecas usadas pelo mini-plugin.

Para produção, sempre teste em um ambiente separado antes de colocar em um servidor público.

---

## Instalação básica

1. Coloque o `VeonJS.jar` na pasta `plugins/`.
2. Inicie o servidor uma vez para gerar as pastas.
3. Crie ou instale seus mini-plugins em:

```text
plugins/VeonJS/miniplugins/
```

ou instale pacotes `.mplugin` diretamente em:

```text
plugins/
```

4. Use os comandos do VeonJS para carregar, recarregar ou compilar.

---

## Aviso importante

O VeonJS acelera o desenvolvimento e facilita hot-reload de mini-plugins, mas boas práticas continuam importantes.

Evite:

- tarefas pesadas na main thread;
- loops por todos os jogadores a cada tick sem necessidade;
- uso incorreto de eventos de alto volume;
- dependências externas sem validação;
- expor tokens ou credenciais em configs públicas.

Para servidores grandes, teste performance antes de usar qualquer mini-plugin em produção.

---

## Comprar VeonJS

**Valor promocional:** R$59,90  
**Disponível por tempo limitado**

Contato para compra e suporte:

**Discord: `kaka_dev`**

---

## VeonJS

Java mini-plugins com hot-reload, dependências Maven e distribuição protegida para servidores Minecraft.
