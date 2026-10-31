# GUIDE.md — Do zero à primeira música (MiniLab 3 + Reaper + Linux)

> Documento **vivo**. Ele é o mapa do curso e o diário de bordo: mostra onde você está,
> o que já foi concluído e qual é o próximo passo. O agente atualiza ao fim de cada aula.
> Regras de atualização estão no fim do arquivo.

---

## Estado atual

| Campo | Valor |
|---|---|
| **Progresso** | `0 / 18` lições |
| **Nível** | 0 — começando do zero |
| **Última sessão** | — (nada registrado ainda) |
| **Próxima lição** | **0.1 — Checar se o teclado está falando com o sistema** |
| **Bloqueios** | nenhum |
| **Meta final** | compor, mixar e exportar 1 faixa própria (~3 min), sozinho |

**Como retomar:** leia este bloco → veja a próxima lição na trilha → faça só ela.

---

## Trilha de aprendizado

Legenda: `[ ]` pendente · `[~]` em andamento · `[x]` concluída

### Módulo 0 — Fundação: o teclado conversando com o sistema (2 lições)

**Objetivo:** saber que o MiniLab 3 manda MIDI de verdade e onde isso é visível.

- [ ] **0.1 — Diagnóstico do teclado**
  - **Conceito:** o MiniLab 3 é um controlador MIDI (não faz som). O Linux já o reconhece sem driver.
  - **Exercício:** com o teclado plugado, rodar
    ```bash
    cat /proc/asound/seq/clients
    pw-link -o | grep -i minilab
    ```
    e reconhecer as 4 portas (`MIDI`, `DIN THRU`, `MCU/HUI`, `ALV`).
  - **Critério:** você sabe apontar qual cliente/porta é o teclado.

- [ ] **0.2 — Ver as mensagens chegando**
  - **Conceito:** MIDI é uma lista de mensagens (`nota ligada`, `nota desligada`, `velocity`, `CC`).
  - **Exercício:** `sudo pacman -S alsa-utils && aseqdump -p 16:0`; apertar teclas, girar knobs,
    bater pads e identificar no terminal: nota, velocity, CC.
  - **Critério:** você reconhece no terminal a diferença entre apertar tecla e girar knob.

### Módulo 1 — MIDI e o MiniLab 3 (3 lições)

- [ ] **1.1 — Mapa do hardware**
  - **Conceito:** 25 teclas, 8 pads (2 bancos), 4 faders, 8 knobs, touch strips (pitch/mod),
    `Octave`, `Hold`, `Shift`, arpejador, display OLED.
  - **Exercício:** tocar cada controle e nomear o que ele faz; descobrir o que `Shift` modifica.
  - **Critério:** você acha octave, hold, arp e troca de banco dos pads sem olhar o manual.

- [ ] **1.2 — O que é velocity, aftertouch e CC**
  - **Conceito:** velocity = intensidade; aftertouch = pressão depois de apertar; CC = controle contínuo.
  - **Exercício:** tocar a mesma nota suave e forte e comparar o número de velocity no `aseqdump`.
  - **Critério:** você explica por que a mesma nota "soa diferente" conforme a força.

- [ ] **1.3 — MIDI não é áudio**
  - **Conceito:** MIDI descreve *o que tocar*; o som vem de um instrumento virtual (VSTi).
  - **Exercício:** desenhar (papel/mente) a corrente: MiniLab → Reaper → VSTi → alto-falante.
  - **Critério:** você sabe que trocar o instrumento muda o som sem mudar o MIDI.

### Módulo 2 — Primeiro som no Reaper (2 lições)

- [ ] **2.1 — Configurar o Reaper**
  - **Conceito:** entrada MIDI, dispositivo de áudio, buffer e latência.
  - **Exercício:** `Options → Preferences → MIDI Devices`: habilitar `Minilab3: MIDI (capture)`.
    Conferir `Audio → Device` (ALSA/JACK) e tocar com latência confortável.
  - **Critério:** tecla acesa na track = som imediato, sem clique duplo nem atraso incômodo.

- [ ] **2.2 — Primeira nota gravada**
  - **Conceito:** track armada + `Record: input (audio or MIDI)` + monitor.
  - **Exercício:** inserir um VSTi (Surge XT, Vital ou Dexed), gravar 4 compassos de notas soltas
    e ver os eventos no MIDI Editor.
  - **Critério:** existe um item MIDI com notas suas no projeto.

### Módulo 3 — Ritmo: pads e bateria (2 lições)

- [ ] **3.1 — Bateria com os pads**
  - **Conceito:** mapa de notas do General MIDI (kick 36, snare 38, closed hat 42…).
  - **Exercício:** gravar um groove de 2 compassos (kick/snare/hat) usando os pads.
  - **Critério:** o groove fica no tempo, com pelo menos 3 peças.

- [ ] **3.2 — Grid, quantização e humanização**
  - **Conceito:** quantizar = alinhar à grade; 100% soa robótico.
  - **Exercício:** quantizar a bateria em 60–80% de força e comparar com 100%.
  - **Critério:** você escolhe conscientemente a intensidade do quantize.

### Módulo 4 — Harmonia mínima aplicada (2 lições)

- [ ] **4.1 — Escala e tríades**
  - **Conceito:** escala maior/menor, intervalos, tríade (tônica–terça–quinta).
  - **Exercício:** tocar a tríade de Dó maior e de Lá menor no MiniLab.
  - **Critério:** você monta qualquer tríade maior/menor a partir da tônica.

- [ ] **4.2 — Progressão de 4 acordes**
  - **Conceito:** graus, progressão I–V–vi–IV e por que ela "cola".
  - **Exercício:** gravar a progressão com um pad/keys e fazer o loop tocar.
  - **Critério:** loop de 4 acordes gravado, afinado e no tempo.

### Módulo 5 — Arranjo e forma (2 lições)

- [ ] **5.1 — Estrutura de uma faixa**
  - **Conceito:** intro, verso, refrão, ponte, outro; contagem em compassos (4/8/16).
  - **Exercício:** montar um "mapa" de 2 min no papel para a sua faixa.
  - **Critério:** mapa com seções e duração definidos.

- [ ] **5.2 — Camadas: baixo, harmonia, melodia**
  - **Conceito:** papéis (grave/médio/agudo) e como evitar disputa de frequência.
  - **Exercício:** acrescentar baixo (mono) e uma melodia simples sobre o loop.
  - **Critério:** três camadas soando distintas, sem mascarar uma à outra.

### Módulo 6 — Mixagem básica (2 lições)

- [ ] **6.1 — Volume, pan e espaço**
  - **Conceito:** gain staging (picos ~-6 dB), pan, hierarquia de volumes.
  - **Exercício:** equilibrar a faixa com faders, sem nenhum plugin.
  - **Critério:** mix soa equilibrado em volume baixo e alto.

- [ ] **6.2 — EQ, compressão e reverb (o mínimo)**
  - **Conceito:** cortar antes de realçar; compressão controla dinâmica; reverb cria espaço.
  - **Exercício:** cortar graves do pad, comprimir o baixo e mandar um reverb curto na melodia.
  - **Critério:** você justifica cada plugin que colocou.

### Módulo 7 — Finalização (2 lições)

- [ ] **7.1 — Referência e loudness**
  - **Conceito:** comparar com faixa comercial, LUFS integrado, true peak.
  - **Exercício:** analisar sua faixa e a referência com medidor; ajustar nível.
  - **Critério:** faixa em volume competitivo sem distorcer.

- [ ] **7.2 — Exportar**
  - **Conceito:** WAV 24-bit para master, MP3 para ouvir no celular.
  - **Exercício:** render na região certa e conferir o arquivo final.
  - **Critério:** arquivo exportado, ouvido do início ao fim e aprovado.

### Módulo 8 — Depois da primeira faixa (1 lição)

- [ ] **8.1 — Próximos caminhos**
  - **Conceito:** sound design, gravação de voz/instrumento, colaboração, MIDI Learn avançado.
  - **Exercício:** escolher o próximo foco e escrever 3 metas.
  - **Critério:** metas escritas no fim deste arquivo.

---

## Registro de sessões

| Data | Lição | O que foi feito | Travou? |
|---|---|---|---|
| — | — | Curso criado; nenhuma aula dada ainda | — |

---

## Anotações do aluno

_(espaço livre — dúvidas, referências musicais, ideias de faixa)_

---

## Regras de atualização deste documento

1. Ao concluir uma lição: trocar `[ ]` por `[x]`.
2. Atualizar a tabela **Estado atual** (progresso, data, próxima lição, bloqueios).
3. Acrescentar uma linha em **Registro de sessões** — nunca editar linhas antigas.
4. Novas lições entram **no fim do módulo correspondente**; novos módulos, no fim da trilha.
5. Se uma lição for pulada, justificar na coluna "O que foi feito".
