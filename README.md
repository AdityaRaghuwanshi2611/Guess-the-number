# Guess-the-number
import tkinter as tk
from random import randrange

window = tk.Tk()
window.title("Guessing Game")
window.geometry("480x350") 
window.config(bg='#76EEC6')

lblInst = tk.Label(window, text = "Guess a Number from 0 to 9", font='sans 16 bold', bg='#76EEC6')
lblLine0 = tk.Label(window, text = "*****************************************************************************", bg='#76EEC6')
lblNoGuess = tk.Label(window, text = "Number of Guesses: 0", font='sans 16 bold', bg='#76EEC6')
lblMaxGuess = tk.Label(window, text = "        Maximum Guess: 3", font='sans 16 bold', bg='#76EEC6')
lblLine1 = tk.Label(window, text = "*****************************************************************************", bg='#76EEC6')
lblLogs = tk.Label(window, text="Game Logs", font='sans 16 bold', bg='#76EEC6')
lblLine2 = tk.Label(window, text = "*****************************************************************************", bg='#76EEC6')

buttons = []
for index in range(0, 10):
    button = tk.Button(window, text=index, command=lambda index=index : process(index), state=tk.DISABLED, font='sans 16 bold', bg='#006400', fg='#CAFF70')
    buttons.append(button)


btnStartGameList = []
for index in range(0, 1):
    btnStartGame = tk.Button(window, text="Start Game", command=lambda : startgame(index), font='sans 16 bold', bg='#8A2BE2', fg='#FFF8DC')
    btnStartGameList.append(btnStartGame)

lblInst.grid(row=0, column=0, columnspan=5)
lblLine0.grid(row=1, column=0, columnspan=5)
lblNoGuess.grid(row=2, column=0, columnspan=3)
lblMaxGuess.grid(row=2, column=3, columnspan=2)
lblLine1.grid(row=3, column=0, columnspan=5)
lblLogs.grid(row=4, column=0, columnspan=5)  

lblLine2.grid(row=9, column=0, columnspan=5)


for row in range(0, 2):
    for col in range(0, 5):
        i = row * 5 + col  
        buttons[i].grid(row=row+10, column=col)

btnStartGameList[0].grid(row=13, column=0, columnspan=5)


guess = 0
totalNumberOfGuesses = 0
secretNumber = randrange(10)
print(secretNumber)
lblLogs = []
guess_row = 4

def init():
    global buttons, guess, totalNumberOfGuesses, secretNumber, lblNoGuess, lblLogs, guess_row
    guess = 0
    totalNumberOfGuesses = 0
    secretNumber = randrange(10)
    print(secretNumber)
    lblNoGuess["text"] = "Number of Guesses: 0"
    guess_row = 4

    for lblLog in lblLogs:
        lblLog.grid_forget()
    lblLogs = []


def process(i):
    global totalNumberOfGuesses, buttons, guess_row
    guess = i

    totalNumberOfGuesses += 1
    lblNoGuess["text"] = "Number of Guesses: " + str(totalNumberOfGuesses)

    if guess == secretNumber:
        lbl = tk.Label(window, text="Your guess was right. You won! ", fg="green", font='sans 12 bold', bg='#76EEC6')
        lbl.grid(row=guess_row, column=0, columnspan=5)
        lblLogs.append(lbl)
        guess_row += 1

        for b in buttons:
            b["state"] = tk.DISABLED
    else:
        if guess > secretNumber:
            lbl = tk.Label(window, text="Secret number is less than your current guess!!", fg="red", font='sans 12 bold', bg='#76EEC6')
            lbl.grid(row=guess_row, column=0, columnspan=5)
            lblLogs.append(lbl)
            guess_row += 1
        else:
            lbl = tk.Label(window, text="Secret number is greater than your current guess !!", fg="red", font='sans 12 bold', bg='#76EEC6')
            lbl.grid(row=guess_row, column=0, columnspan=5)
            lblLogs.append(lbl)
            guess_row += 1

    if totalNumberOfGuesses == 3:
        if guess != secretNumber:
            lbl = tk.Label(window, text="Max guesses reached. You lost!!", fg="red", font='sans 12 bold', bg='#76EEC6')
            lbl.grid(row=guess_row, column=0, columnspan=5)
            lblLogs.append(lbl)
            guess_row += 1

        for b in buttons:
            b["state"] = tk.DISABLED

    buttons[i]["state"] = tk.DISABLED


status = "none"


def startgame(i):
    global status
    for b in buttons:
        b["state"] = tk.NORMAL

    if status == "none":
        status = "started"
        btnStartGameList[i]["text"] = "Restart Game"
    else:
        status = "restarted"
        init()
    print("Game started")


window.mainloop()
