import tkinter as tk
from tkinter import messagebox

def check_winner():
    global winner
    for combo in [[0,1,2], [3,4,5], [6,7,8], [0,3,6], [1,4,7], [2,5,8], [0,4,8], [2,4,6]]:
        if buttons[combo[0]]["text"] == buttons[combo[1]]["text"] == buttons[combo[2]]["text"] != "":
            buttons[combo[0]].config(bg="green")
            buttons[combo[1]].config(bg="green")
            buttons[combo[2]].config(bg="green")
            winner = True
            if current_player == "x":
                messagebox.showinfo("Tic-Tac-Toe", f"Congratulations {player1_name}! You won the match!")
            else:
                messagebox.showinfo("Tic-Tac-Toe", f"Congratulations {player2_name}! You won the match!")
            root.quit()
    if all(button['text'] != "" for button in buttons) and not winner:
        messagebox.showinfo("Tic-Tac-Toe", "It's a draw!")
        root.quit()

def button_click(index):
    global winner
    if buttons[index]["text"] == "" and not winner:
        if current_player == "x":
            buttons[index]["text"] = "★"  
            buttons[index].config(bg="yellow")
        else:
            buttons[index]["text"] = "♥" 
            buttons[index].config(bg="pink")
        
        check_winner()
        toggle_player()

def toggle_player():
    global current_player
    current_player = "o" if current_player == "x" else "x"
    if current_player == 'x':
        label.config(text=f"Player {player1_name}'s turn (★)", fg="white")
    else:
        label.config(text=f"Player {player2_name}'s turn (♥)", fg="black")

root = tk.Tk()
root.title("Tic-Tac-Toe")

player1_name = input("Enter Player 1 name (★):\n")
player2_name = input("Enter Player 2 name (♥):\n")

main_frame = tk.Frame(root, bg="gray")
main_frame.pack(padx=20, pady=20)

label1 = tk.Label(main_frame, text=f"{player1_name} (★)", font=("normal", 16), bg="gray", fg="white")
label1.grid(row=0, column=0)

label2 = tk.Label(main_frame, text=f"{player2_name} (♥)", font=("normal", 16), bg="gray", fg="black")
label2.grid(row=0, column=2)

buttons = []
for i in range(9):
    button = tk.Button(main_frame, text="", font=("normal", 25), width=10, height=2,
                       command=lambda i=i: button_click(i), bg="white", fg="black",
                       highlightbackground="black", highlightthickness=1)
    button.grid(row=(i // 3) + 1, column=i % 3)
    buttons.append(button)

current_player = "x"
winner = False

label = tk.Label(main_frame, text=f"Player {current_player}'s turn ({player1_name})", font=("normal", 16), bg="gray", fg="black")
label.grid(row=4, column=0, columnspan=3)

root.mainloop()
