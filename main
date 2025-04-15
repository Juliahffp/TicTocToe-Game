#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <array>

using namespace std;

mutex cout_mutex;// Mutex global para proteção da impressão do vencedor

// Classe TicTacToe
class TicTacToe {
private:
    array<array<char, 3>, 3> board; // Tabuleiro do jogo
    mutex board_mutex; // Mutex para controle de acesso ao tabuleiro
    condition_variable turn_cv; // Variável de condição para alternância de turnos
    char current_player; // Jogador atual ('X' ou 'O')
    bool game_over; // Estado do jogo
    char winner; // Vencedor do jogo
    
public:
    TicTacToe() {// Inicializar o tabuleiro e as variáveis do jogo

    current_player = 'X';// Inicializa o jogador atual
    game_over = false;// O jogo ainda não terminou
    winner = ' ';// Ainda não há vencedor
    
    for (int i = 0; i < 3; ++i) {// Inicializa todas as posições do tabuleiro com espaço em branco
        for (int j = 0; j < 3; ++j) {
            board[i][j] = ' ';
        }
    }
        
    }

    char get_cell(int row, int col) const { // Método para obter o valor de uma célula do tabuleiro
        return board[row][col];
    }

    void display_board() {// Exibir o tabuleiro no console
        
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                cout << " " << board[i][j];
                if (j < 2) cout << " |";
            }
            cout << endl;
    
            if (i < 2) {
                cout << "---+---+---" << endl;
            }
        }
        cout << endl;
    }

    bool make_move(char player, int row, int col) {

        unique_lock<mutex> lock(board_mutex); // Bloqueia o acesso ao tabuleiro com o mutex

        while (player != current_player && !game_over) {
            turn_cv.wait(lock);
        }

        if (game_over) {
            return false;
        }

        if (board[row][col] == ' ') {

            board[row][col] = player;

            display_board();// Mostra o tabuleiro atualizado

            if (check_win(player)) {
                game_over = true;
                winner = player;
                turn_cv.notify_all(); // <- Notifica para que a outra thread não fique presa
                return true;
            } else if (check_draw()) {
                game_over = true;
                winner = 'D';
                turn_cv.notify_all(); // <- Também necessário em caso de empate
                return true;
            }
        else {
            if (player == 'X') {
                current_player = 'O';
            } else {
                current_player = 'X';
            }
            turn_cv.notify_all();
            return true;
        }
        return false;
    }
    return false;
}

bool check_win(char player) { // Verificar se o jogador atual venceu o jogo
       
    for (int i = 0; i < 3; i++) { // Verifica todas as linhas do tabuleiro
        // Se todos os 3 símbolos de uma linha forem do jogador, o jogador venceu
        if (board[i][0] == player && board[i][1] == player && board[i][2] == player) {
            return true;
        }
    }
    for (int j = 0; j < 3; j++) {// Verifica a diagonal principal
        // Se todos os 3 símbolos de uma coluna forem do jogador, o jogador venceu
        if (board[0][j] == player && board[1][j] == player && board[2][j] == player) {
            return true;
        }
    }
    if (board[0][2] == player && board[1][1] == player && board[2][0] == player) {// Verifica a diagonal secundária 
        return true;
    }

    // Se nenhuma linha, coluna ou diagonal tiver três símbolos iguais do jogador, não houve vitória
    return false;
}

bool check_draw() {// Verificar se houve um empate
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[i][j] == ' ') {// Se encontrar uma célula vazia não há empate
                    return false;  // Há espaços vazios, então não é empate
                }
            }
        }
        return true; // Se todas as células estão preenchidas e ninguém ganhou, é empate
    }
        
    bool is_game_over() {// Retornar se o jogo terminou
        return game_over;
    }

    char get_winner() {// Retornar o vencedor do jogo ('X', 'O', ou 'D' para empate)

        if (game_over) {
            if (winner == 'X' || winner == 'O') {
                return winner;
            }
            else {
                return 'D';  // 'D' para empate
            }
    }
    return ' ';  // Retorna espaço vazio se o jogo ainda não acabou
  }
};



// Classe Player
class Player {
private:
    TicTacToe& game; // Referência para o jogo
    char symbol; // Símbolo do jogador ('X' ou 'O')
    string strategy; // Estratégia do jogador

public:
    Player(TicTacToe& g, char s, string strat) 
        : game(g), symbol(s), strategy(strat) {}

    void play() { // Executar jogadas de acordo com a estratégia escolhida
        if (strategy == "sequencial") {
            play_sequential();  // Jogar de forma sequencial
        } else if (strategy == "aleatorio") {
            play_random();  // Jogar de forma aleatória
        }
    }
       
private:
      
void play_sequential() {
    while (!game.is_game_over()) {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (game.make_move(symbol, i, j)) {
                    this_thread::sleep_for(chrono::milliseconds(100));
                }
            }
        }
    }
}

void play_random() {
    srand(time(0) + symbol);
    while (!game.is_game_over()) {
        int i = rand() % 3;
        int j = rand() % 3;
        if (game.make_move(symbol, i, j)) {
            this_thread::sleep_for(chrono::milliseconds(100));
        }
    }
}

};

int main() {

    // Inicializar o jogo e os jogadores

    TicTacToe game; // Criação do objeto do jogo
    Player player1(game, 'X', "sequencial");  // Jogador 1 usando a estratégia sequencial
    Player player2(game, 'O', "aleatorio");  // Jogador 2 usando a estratégia aleatória

    // Criar as threads para os jogadores

    thread t1(&Player::play, &player1);
    thread t2(&Player::play, &player2);

    // Aguardar o término das threads

    t1.join();
    t2.join();

    // Exibir o resultado final do jogo

    {
        lock_guard<mutex> lock(cout_mutex);
        char result = game.get_winner();
        if (result == 'D') {
            cout << "Empate!" << endl;
        } else {
            cout << "Vencedor: " << result << endl;
        }
    }

    return 0;
}
