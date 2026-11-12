# Guia: como criar um addon do Station-Z

Este guia ensina, passo a passo, a escrever um addon num editor de texto, sem usar o portal. No fim você terá um arquivo `.json` que funciona no app, no seu celular ou para todo mundo.

## 1. O que é um addon

Um addon é um arquivo de texto com links extras para os jogos do Station-Z. Os links aparecem na página do jogo, no botão **"Tudo que precisa"**, logo acima de "Impulsionar port".

Exemplos do que dá para pôr: fanart, vídeos de gameplay, fotos, patches, traduções, saves, mods, manuais e trilhas sonoras.

No app, os addons ficam em **Configurações › Meus repositórios**. Só funcionam com a conta logada.

## 2. Do que você precisa

- **Um editor de texto simples:** Bloco de Notas (Windows), TextEdit em modo texto simples (Mac), VS Code ou o editor do próprio GitHub. No celular, qualquer app de "editor de texto" serve.
- **Não use Word nem Google Docs:** eles trocam as aspas retas (`"`) por aspas curvas (`“ ”`) e o arquivo deixa de funcionar.
- **O slug de cada jogo:** o "nome de link" do jogo (veja o passo 4).

## 3. O modelo

Copie este modelo, cole no editor e salve com a extensão `.json`, por exemplo `meu-addon.json`.

```json
{
  "name": "Meu Pacote de Extras",
  "description": "Fanart, vídeos e traduções dos meus jogos favoritos",
  "games": {
    "super-mario-world": [
      {
        "type": "video",
        "title": "Gameplay completo",
        "url": "https://www.youtube.com/watch?v=EXEMPLO"
      },
      {
        "type": "fanart",
        "title": { "pt": "Fanart em 4K", "en": "4K fanart" },
        "url": "https://www.deviantart.com/search?q=super+mario+world"
      }
    ],
    "chrono-trigger": [
      {
        "type": "translation",
        "title": "Tradução PT-BR",
        "url": "https://exemplo.com/traducao-chrono-trigger"
      }
    ]
  }
}
```

Troque o nome, a descrição, os slugs e os links pelos seus. O resto do formato fica igual.

### Como ler o modelo

O arquivo é um grande bloco entre `{` e `}` com três partes:

| Campo | Precisa? | O que é |
|---|---|---|
| `name` | não | Nome do addon, mostrado na lista do app. Sem ele, o app usa o nome do arquivo ou o fim do link. |
| `description` | não | Uma frase sobre o addon. |
| `games` | **sim** | Os jogos. Cada jogo é um **slug** seguido da lista dos links dele, entre `[` e `]`. |

Cada link da lista é um bloco entre `{` e `}`:

| Campo | Precisa? | O que é |
|---|---|---|
| `title` | **sim** | O texto do botão. Pode ser só texto (`"Fanart"`) ou ter tradução (`{"pt": "Fotos", "en": "Photos"}`). O app mostra o idioma da pessoa; sem tradução, mostra o texto que tiver. |
| `url` | **sim** | O endereço que abre ao tocar. Precisa começar com `https://` (ou `http://`). |
| `type` | não | O tipo do link, que escolhe o ícone (tabela abaixo). Sem ele, vira um link comum. |

### Tipos de link

| `type` | Aparece como |
|---|---|
| `fanart` | Fanart |
| `video` | Vídeo |
| `photos` | Fotos |
| `patch` | Patch |
| `translation` | Tradução |
| `save` | Save |
| `mod` | Mod |
| `manual` | Manual |
| `soundtrack` | Trilha sonora |
| `other` | Link (também vale para qualquer tipo que o app não conheça) |

## 4. Descubra o slug de cada jogo

O slug identifica o jogo: letras minúsculas, sem acentos, com hífens. Exemplos: `super-mario-world`, `chrono-trigger`.

O slug nem sempre é igual ao nome do jogo (o jogo "Black", por exemplo, pode ter o slug `black-android`), então não adivinhe: **peça a lista de slugs à equipe do Station-Z**.

Um slug errado não quebra nada: aquele jogo só fica sem o botão "Tudo que precisa".

## 5. Regras da escrita (onde quase todo mundo erra)

1. **Aspas retas sempre:** todo texto fica entre `"` e `"`.
2. **Vírgula entre itens, nunca depois do último.** Certo:
   ```json
   [
     { "title": "A", "url": "https://a.com" },
     { "title": "B", "url": "https://b.com" }
   ]
   ```
   Errado (vírgula sobrando depois do último item):
   ```json
   [
     { "title": "A", "url": "https://a.com" },
     { "title": "B", "url": "https://b.com" },
   ]
   ```
3. **Feche tudo o que abrir:** cada `{` tem o seu `}` e cada `[` tem o seu `]`.
4. **Um slug só aparece uma vez em `games`.** Para pôr vários links no mesmo jogo, coloque todos na mesma lista.
5. **Limites:** arquivo de até 1 MB e até 50 links por jogo.

Um link com problema (sem título ou com endereço inválido) é ignorado sozinho, e o resto do addon continua funcionando.

## 6. Confira antes de usar

1. Cole o conteúdo do arquivo em [jsonlint.com](https://jsonlint.com) e toque em **Validate JSON**.
2. Se aparecer "Valid JSON", está pronto.
3. Se aparecer erro, o site mostra a linha. Quase sempre é vírgula sobrando, vírgula faltando ou aspas curvas.

## 7. Use o addon no seu celular

1. Mande o `.json` para o celular (WhatsApp, Google Drive, cabo USB…).
2. No app, entre na sua conta.
3. Vá em **Configurações › Meus repositórios › Importar arquivo** e escolha o arquivo.
4. Abra um dos jogos do arquivo: o botão **"Tudo que precisa"** aparece acima de "Impulsionar port".

O arquivo fica guardado no celular. Se você mudar o arquivo, importe de novo.

## 8. Publique por link (atualiza sozinho)

Com um link, você muda o arquivo e quem adicionou o link recebe a mudança sozinho: o app busca o arquivo de novo a cada 5 minutos enquanto está aberto.

**No GitHub (recomendado):**

1. Crie um repositório **público**. Repositório privado não funciona, porque o app não tem acesso.
2. Envie o arquivo `meu-addon.json`.
3. Abra o arquivo no GitHub e copie o endereço da página, por exemplo `https://github.com/voce/meus-addons/blob/main/meu-addon.json`. O app converte esse endereço sozinho.

**Em outro lugar:** serve qualquer endereço `https://` que entregue o conteúdo do arquivo direto, sem página de download no meio. Links de compartilhamento do Google Drive e do Dropbox abrem uma página, não o arquivo, então não funcionam.

Depois, no app: **Configurações › Meus repositórios › Adicionar por link**, cole o link e toque em **Adicionar**. O app baixa e confere o arquivo antes de guardar.

**Para todo mundo:** mande o arquivo ou o link para a equipe do Station-Z. Se aprovado, ele aparece em **Do StationZ** para todos os usuários.

## 9. Se der erro

| Mensagem no app | O que fazer |
|---|---|
| "O arquivo não é um JSON válido." | Erro de escrita. Passe o arquivo no [jsonlint.com](https://jsonlint.com) (passo 6). |
| "O arquivo precisa ter o objeto "games"." | Falta a parte `"games": { ... }`, ou ela está com outro nome. |
| "O arquivo não tem nenhum link válido." | Todos os links estão sem `title` ou com `url` que não começa com `https://`. |
| "O arquivo passa de 1 MB." | Divida em dois addons ou tire links. |
| "Não foi possível baixar o arquivo do link." | O link não entrega o arquivo direto (Drive, Dropbox, repositório privado) ou está errado. Veja o passo 8. |
| O botão não aparece no jogo | O slug está errado (confira na lista do passo 4), o addon não está anexado em **Meus repositórios** ou você não entrou na conta. |

## 10. Lista final

- [ ] Arquivo salvo com a extensão `.json`.
- [ ] Todos os slugs conferidos na lista da equipe.
- [ ] Todo link tem `title` e `url` com `https://`.
- [ ] Passou no jsonlint.com.
- [ ] Testado no celular: o botão "Tudo que precisa" aparece no jogo.

Só use links para conteúdo que pode ser compartilhado (vídeos, fanart, guias, traduções e mods dos próprios autores). Links para cópias piratas de jogos não são aceitos em addons do Station-Z.
