# JOURNAL.md — Diário de aprendizado

Registro **pessoal e subjetivo** da jornada com o MiniLab 3. Aqui vale escrever o que sentiu,
o que confundiu, o que deu orgulho — coisas que não cabem na tabela objetiva do `GUIDE.md`.

**Enquanto o `GUIDE.md` responde "em que passo estou?", o `JOURNAL.md` responde "como estou vivendo isso?".**

## Como escrever

- **Entradas mais novas no topo.**
- Uma entrada por sessão de estudo (ou por dia, se houver mais de uma).
- Sempre com data no formato `## AAAA-MM-DD — título curto`.
- Pode ser em tópicos. Não precisa ser bonito, precisa ser honesto.
- Nunca reescreva entradas antigas. Se mudou de ideia, escreva uma nova entrada citando a anterior.
- **O aluno escreve o conteúdo.** O agente pode ajudar a formatar, mas não inventa sensações.

### Modelo de entrada

```markdown
## AAAA-MM-DD — título curto

**Lição:** 0.1
**Tempo:** ~20 min
**O que fiz:** ...
**O que entendi:** ...
**O que travou / ficou confuso:** ...
**Sensação:** (empolgado / perdido / travado / divertido)
**Vontade para a próxima:** ...
```

---

## 2026-10-31 — Dia zero: o teclado já fala, eu ainda não

**Lição:** nenhuma ainda (curso criado hoje)
**Tempo:** ~1 h montando o ambiente

**O que fiz:**
- Pluguei o MiniLab 3 no ThinkPad e descobri que o Linux já reconhece sem instalar nada.
- Vi as quatro portas que o teclado expõe (`MIDI`, `DIN THRU`, `MCU/HUI`, `ALV`) e não entendi
  metade dos nomes.
- Tentei habilitar a superfície de controle no Reaper e levei um
  `Control Surface: Error opening MIDI inputs`.
- Limpamos a lista de MIDI do Reaper, que tinha dezenas de entradas fantasmas.

**O que entendi:**
- O teclado não faz som: ele só manda mensagens. O som vem do instrumento virtual dentro do DAW.
- "Midi-Bridge" é o PipeWire fazendo ponte entre o teclado e os programas.
- MCU/HUI é um protocolo para controlar o DAW (faders, transporte), separado do MIDI de notas.

**O que travou / ficou confuso:**
- A quantidade de nomes parecidos nas listas de MIDI.
- Ainda não sei o que `ALV` e `DIN THRU` fazem na prática.
- Não sei se vou usar os faders para mixar ou se é complicação demais no começo.

**Sensação:** empolgado, mas com a impressão de que tem muita sigla entre mim e o som.

**Vontade para a próxima:** apertar uma tecla e vê-la virar som dentro do Reaper, sem
configuração nenhuma no meio do caminho.
