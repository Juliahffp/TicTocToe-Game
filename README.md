# Jogo da Velha com Programação Concorrente

![Jogo da Velha Concorrente](tictactoe.gif)

## Descrição do Projeto

Neste trabalho prático, você irá implementar um jogo da velha (Tic Tac Toe) com dois jogadores controlados pelo computador utilizando conceitos de programação concorrente em C++. 

O objetivo é praticar o uso de **mutexes** e **variáveis de condição** para gerenciar o acesso ao tabuleiro e a alternância de turnos entre os jogadores. 

A implementação irá envolver dois componentes principais:
1. **Tabuleiro do Jogo:** Onde as jogadas serão feitas.
2. **Jogadores Concorrentes:** Dois jogadores, cada um com sua estratégia de jogo, irão jogar de maneira concorrente.

## Requisitos

- Utilizar um **mutex** para garantir o acesso exclusivo ao tabuleiro durante a execução das jogadas.
- Utilizar uma **variável de condição** para coordenar a alternância de turnos entre os dois jogadores.
- Implementar duas estratégias de jogadores: uma que jogue de forma sequencial e outra que jogue de forma aleatória.

## Pseudo Código da Solução

### Estrutura Geral do Código

```pseudo
Classe TicTacToe:
    - Atributos:
        - tabuleiro: matriz 3x3 de caracteres
        - board_mutex: mutex para controlar acesso ao tabuleiro
        - turn_cv: variável de condição para coordenar os turnos
        - jogador_atual: caractere indicando o jogador atual ('X' ou 'O')
        - jogo_terminado: booleano indicando se o jogo acabou
        - vencedor: caractere indicando o vencedor ('X', 'O', ou 'D' para empate)

    - Métodos:
        - exibir_tabuleiro(): exibe o estado atual do tabuleiro
        - fazer_jogada(jogador, linha, coluna): realiza uma jogada, verificando se é válida e atualizando o estado do jogo
        - checar_vitoria(jogador): verifica se o jogador atual ganhou o jogo
        - checar_empate(): verifica se houve empate
        - is_game_over(): retorna se o jogo terminou
        - get_winner(): retorna o vencedor do jogo

Classe Player:
    - Atributos:
        - jogo: referência para o objeto TicTacToe
        - simbolo: símbolo do jogador ('X' ou 'O')
        - estrategia: estratégia do jogador ('sequencial' ou 'aleatório')

    - Métodos:
        - play(): executa as jogadas do jogador de acordo com a estratégia escolhida
        - play_sequential(): realiza jogadas de maneira sequencial no tabuleiro
        - play_random(): realiza jogadas aleatórias no tabuleir


Main:
    - Criar objeto TicTacToe
    - Criar dois objetos Player, um para cada jogador (X e O)
    - Iniciar duas threads, uma para cada jogador
    - Aguardar o término das threads
    - Exibir o resultado final do jogo
```

## Regras do Jogo

1. O jogo será executado em um tabuleiro 3x3, e os jogadores alternarão entre 'X' e 'O'.
2. Cada jogador fará uma jogada por vez, esperando sua vez utilizando a **variável de condição**.
3. O **mutex** será utilizado para garantir que apenas um jogador possa acessar o tabuleiro de cada vez.
4. O jogo termina quando um jogador faz uma linha de três símbolos consecutivos (horizontal, vertical ou diagonal) ou quando todas as posições do tabuleiro estiverem preenchidas (empate).

## Estratégias de Jogadores

- **Jogador Sequencial:** Tentará fazer uma jogada em cada posição do tabuleiro, começando da primeira até a última, até encontrar uma posição livre.
- **Jogador Aleatório:** Tentará fazer uma jogada em uma posição aleatória do tabuleiro até encontrar uma posição livre.

## Instruções de Uso

1. Clone este repositório e compile o código usando um compilador C++ compatível (ex.: `g++`).
2. Execute o programa. Duas threads serão iniciadas, uma para cada jogador.
3. Observe a execução do jogo no console. O tabuleiro será exibido a cada jogada, e o resultado final será mostrado após o término do jogo.


