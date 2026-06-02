# mathgame
Code for math game

import tkinter as tk
import random
import matplotlib.pyplot as plt

class MathGame:
    def __init__(self, root):
        self.root = root
        self.root.title("Math Game")
        
        self.question_label = tk.Label(root, text="", font=("Arial", 16))
        self.question_label.pack(pady=20)
        
        self.answer_entry = tk.Entry(root, font=("Arial", 16))
        self.answer_entry.pack(pady=10)
        
        self.submit_button = tk.Button(root, text="Submit", command=self.check_answer, font=("Arial", 16))
        self.submit_button.pack(pady=10)
        
        self.feedback_label = tk.Label(root, text="", font=("Arial", 16))
        self.feedback_label.pack(pady=20)
        
        self.questions = [(8, i) for i in range(1, 13)] + [(9, i) for i in range(1, 13)]
        random.shuffle(self.questions)
        self.current_question_index = 0
        self.attempts = 0
        
        self.new_question()
    
    def new_question(self):
        if self.current_question_index < len(self.questions):
            self.num1, self.num2 = self.questions[self.current_question_index]
            self.correct_answer = self.num1 * self.num2
            self.question_label.config(text=f"What is {self.num1} x {self.num2}?")
            self.answer_entry.delete(0, tk.END)
            self.feedback_label.config(text="")
            self.attempts = 0
        else:
            self.question_label.config(text="Congratulations! You've completed all the questions.")
            self.answer_entry.config(state=tk.DISABLED)
            self.submit_button.config(state=tk.DISABLED)
    
    def check_answer(self):
        try:
            user_answer = int(self.answer_entry.get())
            if user_answer == self.correct_answer:
                self.feedback_label.config(text="Correct! Great job!", fg="green")
                self.current_question_index += 1
                self.root.after(2000, self.new_question)
            else:
                self.attempts += 1
                if self.attempts == 1:
                    self.feedback_label.config(text="Incorrect. Try again!", fg="red")
                else:
                    self.feedback_label.config(text="Incorrect. Here's a visual representation:", fg="red")
                    self.show_visual_representation()
        except ValueError:
            self.feedback_label.config(text="Please enter a valid number.", fg="red")
    
    def show_visual_representation(self):
        tens = self.correct_answer // 10
        ones = self.correct_answer % 10
        
        fig, ax = plt.subplots()
        ax.text(0.1, 0.8, 'Tens:', fontsize=15, verticalalignment='center')
        ax.text(0.1, 0.6, 'Ones:', fontsize=15, verticalalignment='center')
        
        for i in range(tens):
            ax.add_patch(plt.Rectangle((0.2 + i*0.05, 0.75), 0.04, 0.04, color='green'))
        
        for i in range(ones):
            ax.add_patch(plt.Rectangle((0.2 + i*0.05, 0.55), 0.04, 0.04, color='blue'))
        
        ax.set_xlim(0, 1)
        ax.set_ylim(0, 1)
        ax.axis('off')
        
        plt.show()

if __name__ == "__main__":
    root = tk.Tk()
    game = MathGame(root)
    root.mainloop()
