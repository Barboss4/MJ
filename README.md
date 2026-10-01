# Vinyl Dual-Track Player

Projeto de exemplo com:

- cartões de álbuns inspirados em páginas de discografia;
- vinil aparecendo atrás da capa no hover;
- vinil girando enquanto o álbum está tocando;
- player com playlist;
- forma de onda;
- duas versões da mesma música carregadas simultaneamente:
  - versão completa/com voz;
  - versão instrumental;
- troca entre as versões sem mudar a posição da música.

## Por que a troca fica sincronizada?

O navegador decodifica os dois arquivos com a Web Audio API e cria dois `AudioBufferSourceNode`.
As duas fontes são iniciadas com o MESMO `AudioContext.currentTime`.

Em vez de pausar uma música e começar a outra, as duas continuam correndo em paralelo.
O botão apenas altera o ganho:

- Com voz: `vocalGain = 1`, `instrumentalGain = 0`
- Instrumental: `vocalGain = 0`, `instrumentalGain = 1`

Há um crossfade de 12 ms para evitar estalos. Ele não desloca a posição temporal.

## Requisito essencial

As duas versões precisam vir do mesmo master/timeline.

Elas devem ter:

- exatamente o mesmo ponto de início;
- mesma duração;
- mesmo BPM;
- mesmo sample rate de preferência;
- nenhuma introdução/silêncio extra em uma das versões.

Se uma versão tiver 100 ms extras no início, elas ficarão 100 ms fora de sincronia, mesmo com o código correto.

## Colocando seus arquivos

Edite `app.js`, objeto `ALBUMS`.

Exemplo:

```js
{
  title: "Minha música",
  vocalUrl: "assets/audio/meu-album/musica-full.mp3",
  instrumentalUrl: "assets/audio/meu-album/musica-instrumental.mp3"
}
```

## Rodando localmente

Não abra apenas com duplo clique (`file://`) porque o `fetch()` dos arquivos de áudio pode ser bloqueado.

No terminal, dentro da pasta:

```bash
python -m http.server 8000
```

Depois abra:

http://localhost:8000

## Publicação

O projeto é estático e pode ser hospedado em GitHub Pages, Cloudflare Pages, Netlify etc.

Use apenas áudio e imagens que você tenha direito de publicar.


## Controle de volume

O player agora possui:

- slider de 0% a 100%;
- volume inicial em 80%;
- botão de mute/unmute;
- restauração do último volume usado ao sair do mute.

O volume é aplicado no `masterGain`, portanto afeta igualmente a versão com voz e a instrumental sem quebrar a sincronização.


## Visual do fundo

Esta versão recebeu um fundo mais inspirado em palco/show:
- base preta escura;
- reflexos dourados e vermelhos;
- brilho sutil;
- pontos de luz discretos;
- cards e player em vidro escuro.


## Botão de temas

No topo do site existe agora um botão que alterna ciclicamente entre:

1. `MJ CLASSIC` — preto, dourado e vermelho, clima de palco;
2. `BLACK & GOLD` — mais sóbrio, luxuoso e minimalista;
3. `80s NEON` — roxo, azul e rosa, estética anos 80.

Cada clique muda para o próximo tema.


## Próximo álbum automático

Ao terminar a última faixa de um álbum normal, o player muda automaticamente
para o próximo álbum e começa pela primeira faixa.

Depois do último álbum, volta para o primeiro.

No modo All Songs / Shuffle, ao terminar a lista atual, todas as músicas são
embaralhadas novamente e a reprodução continua.


## Player rápido no topo

Esta versão possui um player compacto e sticky no topo da página com:

- play/pause;
- próxima música;
- switch vocal/instrumental;
- volume;
- nome da faixa e álbum atual.

Ele usa o mesmo estado e o mesmo AudioContext do player principal, então os dois controles permanecem sincronizados.
