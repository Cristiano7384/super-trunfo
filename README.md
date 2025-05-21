// Super Trunfo - Jogo no terminal em C++ (nível intermediário em português)

#include <iostream>
#include <vector>
#include <algorithm>
#include <ctime>
#include <cstdlib>

using namespace std;

struct Carta {
    string nome;
    int forca;
    int velocidade;
    int inteligencia;
};

void exibirCarta(const Carta& carta) {
    cout << "--------------------------\n";
    cout << "Carta: " << carta.nome << "\n";
    cout << "1. Força:       " << carta.forca << "\n";
    cout << "2. Velocidade:  " << carta.velocidade << "\n";
    cout << "3. Inteligência:" << carta.inteligencia << "\n";
    cout << "--------------------------\n";
}

int obterValorAtributo(const Carta& carta, int escolha) {
    if (escolha == 1) return carta.forca;
    if (escolha == 2) return carta.velocidade;
    if (escolha == 3) return carta.inteligencia;
    return 0;
}

void inicializarBaralho(vector<Carta>& baralho) {
    baralho = {
        {"Dragão", 90, 70, 60},
        {"Fênix", 80, 90, 85},
        {"Troll", 95, 40, 30},
        {"Elfo", 60, 80, 95},
        {"Gigante", 85, 50, 45},
        {"Mago", 55, 60, 98},
        {"Lobo", 70, 85, 60},
        {"Guerreiro", 75, 75, 75}
    };
    srand(time(nullptr));
    random_shuffle(baralho.begin(), baralho.end());
}

int main() {
    vector<Carta> baralho;
    inicializarBaralho(baralho);

    vector<Carta> cartasJogador(baralho.begin(), baralho.begin() + baralho.size()/2);
    vector<Carta> cartasComputador(baralho.begin() + baralho.size()/2, baralho.end());

    int rodada = 1;
    while (!cartasJogador.empty() && !cartasComputador.empty()) {
        cout << "===========================\n";
        cout << "        Rodada " << rodada << "\n";
        cout << "===========================\n";

        Carta cartaJogador = cartasJogador.front();
        Carta cartaComputador = cartasComputador.front();

        cout << "Sua carta:\n";
        exibirCarta(cartaJogador);

        int escolha;
        cout << "Escolha um atributo para comparar (1-Força, 2-Velocidade, 3-Inteligência): ";
        cin >> escolha;

        int valorJogador = obterValorAtributo(cartaJogador, escolha);
        int valorComputador = obterValorAtributo(cartaComputador, escolha);

        cout << "\nCarta do computador:\n";
        exibirCarta(cartaComputador);

        if (valorJogador > valorComputador) {
            cout << "\nVocê venceu a rodada!\n";
            cartasJogador.push_back(cartaJogador);
            cartasJogador.push_back(cartaComputador);
        } else if (valorComputador > valorJogador) {
            cout << "\nComputador venceu a rodada.\n";
            cartasComputador.push_back(cartaComputador);
            cartasComputador.push_back(cartaJogador);
        } else {
            cout << "\nEmpate! Cada um mantém sua carta.\n";
            cartasJogador.push_back(cartaJogador);
            cartasComputador.push_back(cartaComputador);
        }

        cartasJogador.erase(cartasJogador.begin());
        cartasComputador.erase(cartasComputador.begin());
        cout << "\nCartas restantes - Você: " << cartasJogador.size() << " | Computador: " << cartasComputador.size() << "\n";

        cout << "\nPressione Enter para a próxima rodada...";
        cin.ignore();
        cin.get();
        rodada++;
    }

    cout << "\n===========================\n";
    if (cartasJogador.empty()) cout << "Fim de jogo. Você perdeu.\n";
    else cout << "Parabéns! Você venceu o jogo!\n";
    cout << "===========================\n";

    return 0;
}
