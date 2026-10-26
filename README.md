# Página de links com música ao vivo (Discord + Spotify + YouTube)

Uma página estática (um único `index.html`) no estilo "Liquid Glass" que mostra:

- sua foto, nome e status do Discord;
- a música que você está ouvindo no Spotify, com barra de progresso;
- a **última música ouvida** quando você não está ouvindo nada;
- botão de **play** que toca a música pelo YouTube (barra com vinil girando no topo) e botão de **pesquisa**;
- seus links com ícones que mudam de cor conforme a capa do álbum;
- sem banco de dados e sem servidor: é só hospedar o arquivo.

## 1. Pegue o seu ID do Discord

1. No Discord, abra **Configurações → Avançado** e ligue o **Modo desenvolvedor**.
2. Clique com o botão direito no seu nome (ou foto) e escolha **Copiar ID do usuário**.
3. Entre no servidor do **Lanyard** (o link está no repositório [Phineas/lanyard](https://github.com/Phineas/lanyard)). É ele que entrega a sua presença para o site. Sem entrar, nada aparece.
4. No Discord, em **Configurações → Conexões**, conecte o **Spotify** e ligue **Mostrar o Spotify como seu status**.
5. Teste abrindo `https://api.lanyard.rest/v1/users/SEU_ID` no navegador. Se aparecer `"success": true`, está certo.
6. No `index.html`, procure por `CFG` e cole o ID em `DISCORD_ID`.

## 2. Chave do YouTube (opcional, mas necessária para o botão de play)

O site procura a música no YouTube usando a YouTube Data API v3.

1. Acesse o [Google Cloud Console](https://console.cloud.google.com/) e crie um projeto.
2. Em **APIs e serviços → Biblioteca**, ative a **YouTube Data API v3**.
3. Em **Credenciais → Criar credenciais → Chave de API**, copie a chave.
4. **Restrinja a chave** (obrigatório, porque ela fica visível no código do site):
   - **Restrições de aplicativo:** *Sites (referenciadores HTTP)* e coloque o endereço do seu site (ex.: `https://seusite.com/*`).
   - **Restrições de API:** marque só a *YouTube Data API v3*.
5. Cole a chave em `YT_KEY`.

Observações:

- A cota grátis é limitada e cada busca gasta bastante dela. Confira os números atuais na documentação do Google. O site guarda as músicas já buscadas durante a visita para gastar menos.
- Sem chave, o botão de play avisa que falta a chave, e o botão de lupa continua abrindo a busca no YouTube.
- **Nunca** coloque uma chave sem restrição em um repositório público.

## 3. Seus links e cores

No começo do script:

```js
const LINKS=[
  {n:"TikTok",u:"https://www.tiktok.com/@seu_usuario",i:"tiktok"},
  {n:"Roblox",u:"https://www.roblox.com/users/ID/profile",i:"roblox",color:"#e2231a"}
];
```

- `n` é o texto, `u` é o endereço, `i` é o ícone.
- Ícones prontos: `tiktok`, `youtube`, `instagram`, `discord`, `roblox`, `spotify`, `twitch`, `steam`.
- `color` (opcional) muda a cor do ícone só desse link.
- `CFG.ICON_COLOR`: `"tema"` usa a cor da capa da música, `"marca"` usa a cor oficial de cada rede.

Também em `CFG`: `NAME` (nome mostrado; vazio usa o do Discord), `HANDLE` (o @), `BIO` (texto de apresentação), `TITLE` (título da aba) e `FAVICON` (ícone da aba: um emoji ou o endereço `https://...` de uma imagem).

**Quer personalizar mais (estilo, layout, Firebase)?** Use o arquivo `PROMPT-PARA-IA.md`: copie o texto para qualquer IA, anexe o seu `index.html` e diga o que quer mudar.

## 4. Publicar

Qualquer hospedagem de arquivos estáticos serve: GitHub Pages, Netlify (arraste a pasta) ou Vercel. Não precisa de build.

## Como funciona (e limites)

- **Última música:** fica salva no navegador de cada visitante (`localStorage`). Quem entra pela primeira vez, com você sem ouvir nada, vê só o aviso vazio. Para mostrar a sua última música a todos, é preciso guardá-la em um servidor (por exemplo, Firebase ou uma função da Vercel).
- **Player do YouTube:** é pequeno e fica na barra do topo, que recolhe sozinha e deixa só o vinil. O YouTube recomenda um player maior e visível; em alguns navegadores (principalmente no iPhone) o som pode não tocar. O som só começa depois de um toque do visitante, por regra dos navegadores.
- **Privacidade:** o site mostra publicamente o que você escuta e o seu status do Discord. Se não quiser, não use o Lanyard.

## Créditos dos ícones

- TikTok, YouTube e Roblox: [Simple Icons](https://simpleicons.org/) (CC0).
- Instagram e Discord: [Bootstrap Icons](https://icons.getbootstrap.com/) (MIT).
- Spotify e Twitch: [Font Awesome Free](https://fontawesome.com/) Brands (CC BY 4.0), © Fonticons, Inc.
- Steam: [IcoMoon Free](https://icomoon.io/) (CC BY 4.0).

Os nomes e logos pertencem às respectivas empresas.
