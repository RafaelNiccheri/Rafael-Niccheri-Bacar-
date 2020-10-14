# EP - Design de Software
# Equipe: Rafael Pascarelli Niccheri
# Data: 18/10/2020

import random
import time
import math as mt
import os
import sys

FICHAS=100

def limpa_tela():                              #limpa a tela
    os.system('cls')

class Jogador():                            #Cria jogador para conseguir implemantar as rehras avançadas
    def __init__(self, numero_chamada):
        self.valor_aposta=0                 #qunatidade de fichas que o jogador aposta
        self.escolha_aposta=0               #em quem o jogador aposta (jigador=1, banca=2, empate=3)
        self.cartas_jogador=[]              #cartas do jogador
        self.fichas=FICHAS                  #numero de fichas do jogador
        self.numero_chamada=numero_chamada  #numero que o jogador equivale
        
    def aposta(self, valor, escolha):       #definir as apostas do jogador
        self.valor_aposta=valor
        self.escolha_aposta=escolha
        
    def adiciona_carta(self, carta):        #adiciona uma cata na mão do jogador
        self.cartas_jogador.append(carta)
        
    def soma_cartas(self):
        cartas={'A':1, '2':2, '3':3, '4':4, '5':5, '6':6, '7':7, '8':8, '9':9, '10':0, 'J':0,'Q':0,'K':0}     #valores de cada carta
        soma=0                              #soma das cartas
        for carta in self.cartas_jogador:   #verifica as cartas
            soma+=cartas[carta[0]]             #atualiza a soma
        while soma >= 10:                   #verifica a soma
            soma-=10
        return soma

def cria_jogadores(numero_jogadores):          #cria jogador
    jogadores=[]                               #lista de jogadpres
    for i in range(0, numero_jogadores):       #loop para criar os njogadores
        jogadores.append(Jogador(i+1))
    return jogadores

class Baralho():                            #cria os baralhos
    def __init__(self, num_baralho):
        self.num_baralho=num_baralho        #quantos baralhos serao usados
        self.numeros=['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K'] * num_baralho            #cria as cartas
        self.naips=['Espadas', 'Paus', 'Ouros', 'Copas'] * len(self.numeros)        #cria os naips
        self.cartas_sorteadas=[]                        #cartas ja sorteadas
    
    def sorteia_carta(self):                                                    #sorteia cartas
        numero_sorteado=random.choice(self.numeros)                             #sorteia o valor da carta
        naip_sorteado=random.choice(self.naips)                                 #sorteia o naip da carta
        carta_sorteada=[numero_sorteado, naip_sorteado]                         #cria a carta
        if self.cartas_sorteadas.count(carta_sorteada) >= self.num_baralho:     #loop para verificar se a carta ainda esta no baralho
            return self.sorteia_carta()                                         #caso a carta nao esteja mais no baralho sorteia uma carta nova
        else:
            self.cartas_sorteadas.append(carta_sorteada)                        #adiciona a carta sorteada nas cartas ja sorteadas
            return carta_sorteada                                               #retorna a carta sorteada

def valor_carta(carta):                                         #varifica o valor da carta
    cartas={'A':1, '2':2, '3':3, '4':4, '5':5, '6':6, '7':7, '8':8, '9':9, '10':0, 'J':0,'Q':0,'K':0}
    valor=carta[0]
    return cartas[valor]

#---------------------------------------------------------Funções das apostas--------------------------------------------------------------------

def escolher_aposta():                          #funcao para escolher em quem apostas
    escolha_aposta=int(input("Em quem voce quer apostar? "))        #input para escolher em quem apostar
    if escolha_aposta not in [1, 2, 3]:                             #loop para verificar se a escolha é valida
        print ('Escolha invalida')
        return escolher_aposta()
    else:
        return escolha_aposta

def apostar_fichas(fichas):                                         #funcao para escolher a quantidade de fichas para apostas
    fichas_apostadas=int(input("Quantas fichas quer apostar? "))   #input para escolher a quantidade de fichas para apostar
    if fichas_apostadas > fichas:                                   #loop para verificar se a quantidade de fichas é valida
        print ('Numero de fichas apostadas invalido')
        return apostar_fichas(fichas)
    else:
        return fichas_apostadas
    
def fazer_apostas(jogadores):                                       #funcao para fazer as apostas
    for jogador in jogadores:                                       #loop para todos os jogadores apostarem
        limpa_tela()
        print('Jogador', jogador.numero_chamada)
        print('Voce tem', jogador.fichas, 'fichas disponiveis')
        print('Apostas dispooniveis')
        print('1 : Jogador')
        print('2 : Banca')
        print('3 : Empate')
        escolha_aposta=escolher_aposta()
        fichas_apostadas=apostar_fichas(jogador.fichas)
        jogador.aposta(fichas_apostadas, escolha_aposta)

#-------------------------------------------------------------Funções de distribuir as cartas--------------------------------------------------------

def terceira_carta_banca(jogadores, banca, baralhos):           #define se a banca recebera uma terceira carta
    if banca.soma_cartas() > 5:                                 #regras para a terceira carta da banca
        return False
    if len(jogadores[0].cartas_jogador) < 3 and banca.soma_cartas() < 6:    #regras para a terceira carta da banca
        return True
    if len(jogadores[0].cartas_jogador) == 3 and banca.soma_cartas() < 6:   #regras para a terceira carta da banca
        tabela=[[0, 1, 2, 3], [0, 1, 2, 3], [0, 1, 2, 3, 4], [0, 1, 2, 3, 4], [0, 1, 2, 3, 4, 5], [0, 1, 2, 3, 4, 5], [0, 1, 2, 3, 4, 5], [0, 1, 2, 3, 4, 5], [0, 1, 2], [0, 1, 2, 3]]
        terceira_carta_jogador= jogadores[0].cartas_jogador[2]
        if banca.soma_cartas() in tabela[valor_carta(terceira_carta_jogador)]:
            return True

def distribui_cartas(jogadores, banca, baralhos):               #distribui as cartas para os jogadores e para a banca
    carta1=baralhos.sorteia_carta()                             #sorteia a primeira carta dos jogadores
    carta2=baralhos.sorteia_carta()                             #sorteia a segunda carta dos jogadores
    for jogador in jogadores:
        jogador.adiciona_carta(carta1)                          #entrega a primeira carta sorteada dos jogadores para eles
        jogador.adiciona_carta(carta2)                          #entrega a segunda carta sorteada dos jogadores para eles
    carta1=baralhos.sorteia_carta()                             #sorteia a primeira carta da banca
    carta2=baralhos.sorteia_carta()                             #sorteia a segunda carta da banca
    banca.adiciona_carta(carta1)                                #entrega a primeira carta sorteada da banca para ela
    banca.adiciona_carta(carta2)                                #entrega a segunda carta sorteada da banca para ela
    if jogadores[0].soma_cartas() < 6 and banca.soma_cartas() != 8 and banca.soma_cartas() != 9:    #verifica se os jogadores devem receber uma terceira carta
        carta3=baralhos.sorteia_carta()                         #sortei a terceira carta dos jogadores se necessario
        for jogador in jogadores:
            jogador.adiciona_carta(carta3)                      #entrega a terceira carta dos jogadores para eles caso ela tenha sido sorteada
    if terceira_carta_banca(jogadores, banca, baralhos) == True: #chama a funcao que verifica se a banca deve receber uma terceira carta
        carta3=baralhos.sorteia_carta()                         #sorteia a terceira carta da banca se necessario
        banca.adiciona_carta(carta3)                            #entrega a terceira carta da banca para ela caso ela tenha sido sorteada

#------------------------------------------------------Funções para mostrar informações ao jogador---------------------------------------------------

def mostra_escolha(escolha):                                    #mostra em quem cada jogador apostou
    if escolha==1:
        return 'no Jogador'
    elif escolha==2:
        return 'na banca'
    else:
        return 'no empate'
    
def mostrar_apostas(jogadores):                             #mostra as apostas de cada jogador
    for jogador in jogadores:
        print('O Jogador', jogador.numero_chamada, 'apostou', jogador.valor_aposta, 'fichas', mostra_escolha(jogador.escolha_aposta))

def mostrar_cartas(jogadores, banca):                       #mostra as cartas dos jogadores e da banca
    print('Cartas dos jogadores')
    for carta in jogadores[0].cartas_jogador:
        print(carta[0],' ', carta[1])
    print('Cartas da banca')
    for carta in banca.cartas_jogador:
        print(carta[0],' ', carta[1])

#----------------------------------------------------------Funções para o pagamento das apostas------------------------------------------------------

def comissao(vencedor, num_baralhos):                       #define a comissão que os jogadores vencedores tem que pagar para a banca de acordo com o numero de baralhos
    if vencedor==1:
        if num_baralhos==1:
            return 0.0129
        elif num_baralhos==6:
            return 0.0124
        else:
            return 0.0124
    elif vencedor==2:
        if num_baralhos==1:
            return 0.0101
        elif num_baralhos==6:
            return 0.0106
        else:
            return 0.0106
    else:
        if num_baralhos==1:
            return 0.1575
        elif num_baralhos==6:
            return 0.1444
        else:
            return 0.1436

def jogador_vencedor(jogadores, baralhos):                  #define os vencedores e mostra quantas fichas os vencedores ganharam
    for jogador in jogadores:
        if jogador.escolha_aposta==1:
            fichas_ganhas=mt.floor(jogador.valor_aposta*(1-comissao(1, baralhos.num_baralho)))
            jogador.fichas+=fichas_ganhas
            print('O Jogador', jogador.numero_chamada, 'ganhou', fichas_ganhas, 'fichas')
        else:
            jogador.fichas-=jogador.valor_aposta
            print('O Jogador', jogador.numero_chamada, 'perdeu', jogador.valor_aposta, 'fichas')

def banca_vencedora(jogadores, baralhos):                   #define os vencedores e mostra quantas fichas os vencedores ganharam
    for jogador in jogadores:
        if jogador.escolha_aposta==2:
            fichas_ganhas=mt.floor(0.95*jogador.valor_aposta)
            fichas_ganhas=mt.floor(fichas_ganhas*(1-comissao(2, baralhos.num_baralho)))
            jogador.fichas+=fichas_ganhas
            print('O Jogador', jogador.numero_chamada, 'ganhou', fichas_ganhas, 'fichas')
        else:
            jogador.fichas-=jogador.valor_aposta
            print('O Jogador', jogador.numero_chamada, 'perdeu', jogador.valor_aposta, 'fichas')

def empate(jogadores, baralhos):                            #define os vencedores e mostra quantas fichas os vencedores ganharam
    for jogador in jogadores:
        if jogador.escolha_aposta==3:
            fichas_ganhas=8*jogador.valor_aposta
            fichas_ganhas=mt.floor(fichas_ganhas*(1-comissao(3, baralhos.num_baralho)))
            jogador.fichas+=fichas_ganhas
            print('O Jogador', jogador.numero_chamada, 'ganhou', fichas_ganhas, 'fichas')
        else:
            jogador.fichas-=jogador.valor_aposta
            print('O Jogador', jogador.numero_chamada, 'perdeu', jogador.valor_aposta, 'fichas')

def verifica_resultados(jogadores, banca, baralhos):    #verifica quem tem a mao vencedora ou se ocorre um empate
    limpa_tela()
    if jogadores[0].soma_cartas() > banca.soma_cartas():
        print('A mão do jogador ganhou')
        jogador_vencedor(jogadores, baralhos)
    elif jogadores[0].soma_cartas() < banca.soma_cartas():
        print('A mão da banca ganhou')
        banca_vencedora(jogadores, baralhos)
    else:
        print('Ouve um empate entre a mão do jogador e a da banca')
        empate(jogadores, baralhos)

#--------------------------------------------------------------Funções para preparar o jogo----------------------------------------------------------

def prepara_proxima_rodada(jogadores, banca, baralhos):                         #prepara a proxima rodada
    indices_jogadores=[]                                                        #lista dos jogadores que perderam todas as fichas na rodada anterior
    for jogador in jogadores:
        jogador.cartas_jogador=[]                                               #recolhe as cartas dos jogadores
        if jogador.fichas<=0:                                                   #verifica quais jogadores sairõo do jogo
            print('O Jogador', jogador.numero_chamada, 'perdeu todas as fichas e sairá do jogo')
            indices_jogadores.append(jogadores.index(jogador))
    indices_jogadores.sort(reverse=True)
    for jogador in indices_jogadores:                                           #remove os jpgadores sem fichas do jogo
        jogadores.pop(jogador)
    if len(jogadores)==0:                                                       #verifica se resta algum jogador
        print('Acabaram os fichas de todos os jogadores')
        input('Pressione enter para acabar o jogo')
        sys.exit()
    banca.cartas_jogador=[]                                                     #recolhe as cartas da banca
    baralhos.cartas_sorteadas=[]                                                #devolve as cartas para o baralho
    
def menu(jogadores, banca, baralhos):                           #funcao que diz as informacoes dos jogadores e da as opções de apostas
    limpa_tela()
    for jogador in jogadores:
        print('O Jogador', jogador.numero_chamada, 'tem', jogador.fichas, 'fichas')
    input('Pressione enter para começar as apostas')
    fazer_apostas(jogadores)                                    #chama a funcao para fazer as apostas
    limpa_tela()
    mostrar_apostas(jogadores)                                  #chama a funcao para mostrar as apostas
    print('Distribuindo as cartas')
    time.sleep(2)
    distribui_cartas(jogadores, banca, baralhos)                #chama a funcao para distribuir as cartas
    mostrar_cartas(jogadores, banca)                            #chama a funcao para mostrar as cartas
    input('Pressione enter para verificar os resultados')
    verifica_resultados(jogadores, banca, baralhos)             #chama a função que verifica quem tem a mao vencedora ou se ocorre um empate
    prepara_proxima_rodada(jogadores, banca, baralhos)          #chama a função que prepara a proxima rodada
    input('Pressione enter para começar a próxima rodada')
    return menu(jogadores, banca, baralhos)                     #começa a proxima rodada
    
#------------------------------------------------------------------------Codigo principal-----------------------------------------------------------

def main():
    numero_jogadores=int(input("Qunatos jogadores irão jogar? "))   #pergunta o numero de jogadores
    jogadores=cria_jogadores(numero_jogadores)                      #cria a quantidade especificada de jogadores
    while True:
        numero_baralhos=int(input('Serão usados 1, 6 ou 9 baralhos? '))     #pergunta quantos baralhos serão usados (1, 6 ou 9)
        if numero_baralhos not in [1, 6, 9]:
            print('Opção invalida')
        else:
            break
    baralhos=Baralho(numero_baralhos)    #cria a quantidade de baralhos especificada
    banca=Jogador(100)                      #cria a banca
    menu(jogadores, banca, baralhos)    #inicia o codigo

if __name__=="__main__":
    main()
