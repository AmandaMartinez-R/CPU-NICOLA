# CPU Simples de 8 Bits 

🎬 Vídeo explicativo: https://youtu.be/3ZWsXdFyIfY 

---

## Visão Geral

Este projeto consiste na implementação de uma CPU simplificada de 8 bits, desenvolvida no *software Digital*, com o objetivo de consolidar conceitos fundamentais de arquitetura de computadores.

A CPU implementa um fluxo completo de execução, incluindo:
- Busca de instrução (fetch)
- Decodificação
- Execução de operações aritméticas e lógicas

O projeto simula, de forma prática, o funcionamento interno de uma CPU real, utilizando componentes digitais básicos como registradores, memória, barramento e unidade lógica e aritmética (ALU).


## Arquitetura da CPU

A arquitetura foi estruturada com base nos seguintes blocos principais:

- **Unidade de Controle**
- **Unidade de Execução (ALU)**
- **Memória (EEPROMs)**
- **Registradores**
- **Barramento (BUS)**
- **Clock**

A CPU segue um ciclo sequencial de execução controlado por clock.

---

## Clock

O clock é responsável por sincronizar toda a CPU.

- Cada pulso representa um ciclo de execução
- Todos os registradores atualizam seus valores com base nesse sinal
- Sem o clock, não há mudança de estado no sistema

---

## Program Counter (PC)

O **Program Counter (PC)** é um registrador que armazena o endereço da próxima instrução.

### Funcionamento:
- Envia o endereço para a memória
- Recebe incremento a cada ciclo
- Controla o fluxo de execução do programa

---

## Somador de 8 bits

O somador é responsável por incrementar o PC:
PC + 1 → próximo endereço


Esse incremento garante que a CPU percorra a memória sequencialmente.

---

## Memórias (EEPROMs)

O sistema utiliza duas EEPROMs:

### EEPROM de Operações
Armazena os códigos de operação (opcodes):

| Código | Operação |
|------|--------|
| 000 | Soma |
| 001 | Subtração |
| 010 | Shift Right |
| 011 | Shift Left |
| 100 | AND |
| 101 | XOR |
| 110 | Multiplicação |
| 111 | Divisão |

---

### EEPROM de Dados (N)

Armazena os valores numéricos utilizados nas operações.

Exemplo de valores utilizados: 5, 3, 8, 2, 10, 4, 6, 1, ...


---

## Barramento (BUS)

O BUS é responsável por interligar todos os componentes da CPU.

### Características:
- Largura de 8 bits
- Compartilhado entre múltiplos dispositivos
- Apenas um dispositivo pode escrever por vez

---

## Drivers Tri-State

Os drivers controlam o acesso ao barramento.

### Funcionamento:

| Enable | Estado |
|------|--------|
| 1 | Transmite o valor |
| 0 | Alta impedância (Hi-Z) |

### Alta Impedância

Alta impedância significa que o componente está eletricamente desconectado do barramento, não influenciando o sinal.

Isso evita conflitos, permitindo que apenas um dispositivo controle o BUS por vez.

---

## Registradores

### IR (Instruction Register)

Armazena a instrução atual.

- Recebe dados da EEPROM
- Controla a operação da ALU

---

### N (Operando)

Armazena o valor que será utilizado na operação.

Fluxo:
EEPROM → BUS → N → ALU


---

### AC (Acumulador)

Armazena o resultado das operações.

- Entrada: BUS
- Saída: ALU
- Função: manter estado entre operações

---

### MQ (Multiplier Quotient)

Registrador auxiliar utilizado em operações mais complexas, como multiplicação e divisão.

---

## Flip-Flop Tipo D

Todos os registradores são implementados com flip-flops tipo D.

### Funcionamento:
- Capturam o valor da entrada
- Armazenam no pulso de clock

---

## ALU (Unidade Lógica e Aritmética)

A ALU é responsável pelas operações da CPU.

### Entradas:
- A → AC
- B → N

### Controle:
- Bits do IR definem a operação

### Saída:
Resultado → BUS → AC


---

## Ciclo de Execução

A CPU segue o seguinte ciclo:

### 1. Busca da instrução
PC → EEPROM → IR

### 2. Busca do operando
PC → EEPROM → N

### 3. Execução
ALU processa operação


### 4. Armazenamento
Resultado → AC


## Testes Realizados

Foi implementado um conjunto de testes com 16 posições de memória, combinando operações e valores.

### Exemplo:

| Endereço | Operação | N | Resultado |
|--------|---------|----|----------|
| 0x00 | Soma | 5 | 5 |
| 0x01 | Sub | 3 | 2 |
| 0x02 | Shift Right | 8 | 4 |
| 0x03 | Shift Left | 2 | 4 |
| 0x04 | AND | 10 | 0 |
| 0x05 | XOR | 4 | 4 |
| 0x06 | Mult | 6 | 24 |
| 0x07 | Div | 1 | 24 |

---

## Conclusão

Este projeto permitiu compreender de forma prática:

- Funcionamento interno de uma CPU
- Uso de registradores e memória
- Controle por clock
- Arquitetura baseada em barramento
- Importância da alta impedância
- Execução de instruções via ALU

A implementação demonstra uma CPU funcional, capaz de executar operações sequenciais com base em instruções armazenadas em memória.

## Possíveis Melhorias

- Implementação de flags (Zero, Carry)
- Instruções condicionais (jump)
- Pipeline
- Expansão do conjunto de instruções
- Otimização da ALU

---

## Aprendizados

Durante o desenvolvimento dessa CPU, eu senti que saí completamente da teoria e realmente entendi como as coisas funcionam na prática, principalmente porque enfrentei algumas dificuldades que me forçaram a pensar de verdade sobre o que eu estava fazendo. Um dos pontos mais desafiadores foi entender o funcionamento do barramento e dos drivers tri-state, porque no começo eu não conseguia visualizar por que dava erro quando mais de um componente tentava funcionar ao mesmo tempo, e foi aí que o conceito de alta impedância realmente fez sentido pra mim. Além disso, conectar corretamente os registradores, principalmente o N e o AC, e entender quando cada um deveria ser habilitado de acordo com o ciclo de execução também foi um desafio grande, porque não era só ligar os fios, mas sim entender o porquê de cada conexão. A parte da EEPROM também exigiu bastante atenção, tanto para organizar corretamente as operações quanto os dados, e perceber como o Program Counter percorre esses valores. No geral, foi um processo em que eu precisei errar, testar e ajustar várias vezes, e isso fez com que eu realmente entendesse o fluxo completo de uma CPU, desde a busca da instrução até a execução na ALU, de uma forma muito mais clara e concreta do que só estudando teoria.

**Este projeto consolidou conceitos fundamentais de:**
- Arquitetura de computadores
- Sistemas digitais
- Organização de CPU
- Lógica combinacional e sequencial

---
