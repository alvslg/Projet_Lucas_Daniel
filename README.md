import random
import tkinter as tk

class TicTacToe:
    """ Représente le jeu du morpion, on utilise la bibliothèque Tkinter pour créer une interface graphique.
    """
    def __init__(self):
        self.board = []
        self.player = 'X' if self.get_random_first_player() == 1 else 'O'
        self.root = tk.Tk()
        self.root.title("Tic Tac Toe")
        self.buttons = []

    def create_board(self): 
        """ Crée le plateau de jeu en initialisant toutes les cases avec le symbole "-".
        """
        for _ in range(3):
            row = []
            for _ in range(3):
                row.append('-')
            self.board.append(row)

    def get_random_first_player(self):
        """ génère un nombre aléatoire pour déterminer quel joueur commence.
        """
        return random.randint(0, 1)

    def fix_spot(self, row, col, player):
        """ place le symbole du joueur sur une des cases.
        """
        self.board[row][col] = player

    def is_player_win(self, player):
        """ Vérifie si le joueur actuel a gagné.
        """
        n = len(self.board)

        # vérifie lignes
        for i in range(n):
            win = True
            for j in range(n):
                if self.board[i][j] != player:
                    win = False
                    break
            if win:
                return win

        # vérifie colonnes
        for i in range(n):
            win = True
            for j in range(n):
                if self.board[j][i] != player:
                    win = False
                    break
            if win:
                return win

        # vérifie diagonales
        win = True
        for i in range(n):
            if self.board[i][i] != player:
                win = False
                break
        if win:
            return win

        win = True
        for i in range(n):
            if self.board[i][n - 1 - i] != player:
                win = False
                break
        return win

    def is_board_filled(self): 
        """ Vérifie si toutes les cases du plateau sont remplies.
        """
        for row in self.board:
            for item in row:
                if item == '-':
                    return False
        return True

    def swap_player_turn(self, player):
        """ permet de passer au tour du joueur suivant.
        """
        return 'X' if player == 'O' else 'O'

    def show_board(self):
        """ crée les boutons cliquable de la grille du Tic Tac Toe dans l'interface graphique.
        """
        for i in range(3):
            for j in range(3):
                button = tk.Button(self.root, text=self.board[i][j], font=("Arial", 20), width=8, height=4,
                                   command=lambda row=i, col=j: self.on_button_click(row, col))
                button.grid(row=i, column=j)
                self.buttons.append(button)

    def on_button_click(self, row, col):
        """  est appelée lorsqu'un bouton est cliqué. Elle vérifie si la partie est terminée, puis met à jour
             le plateau et les symboles des boutons en fonction de l'action du joueur.
        """
        button = self.buttons[row * 3 + col]
        if self.board[row][col] == '-':
            self.fix_spot(row, col, self.player)
            button.config(text=self.player)
            button.config(state=tk.DISABLED)

            if self.is_player_win(self.player):
                print(f"Le joueur {self.player} a gagné !")
                self.root.quit()
            elif self.is_board_filled():
                print("Match Nul !")
                self.root.quit()
            else:
                self.player = self.swap_player_turn(self.player)

    def start(self):
        """  lance le jeu en créant le plateau, en affichant la grille dans l'interface graphique et en démarrant la boucle principale de Tkinter.
        """
        self.create_board()
        self.show_board()
        self.root.mainloop()


# Commence la partie
tic_tac_toe = TicTacToe()
tic_tac_toe.start()
