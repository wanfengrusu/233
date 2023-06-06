# 233
import tkinter as tk
class PomodoroTimer():
    def __init__(self, master):
        self.master = master
        self.master.title("Pomodoro Timer")
        self.minutes = 25
        self.seconds = 0
        self.is_running = False

        # Create timer label
        self.timer_text = tk.StringVar()
        self.timer_label = tk.Label(self.master, textvariable=self.timer_text, font=("Helvetica", 48))
        self.timer_text.set(f"{self.minutes:02d}:{self.seconds:02d}")
        self.timer_label.grid(row=0, column=0, columnspan=3)

        # Create start, pause, and reset buttons
        self.start_button = tk.Button(self.master, text="Start", command=self.start_timer)
        self.pause_button = tk.Button(self.master, text="Pause", command=self.pause_timer, state=tk.DISABLED)
        self.reset_button = tk.Button(self.master, text="Reset", command=self.reset_timer, state=tk.DISABLED)
        self.start_button.grid(row=1, column=0)
        self.pause_button.grid(row=1, column=1)
        self.reset_button.grid(row=1, column=2)

    def start_timer(self):
        self.is_running = True
        self.start_button.config(state=tk.DISABLED)
        self.pause_button.config(state=tk.NORMAL)
        self.reset_button.config(state=tk.NORMAL)
        
        self.countdown()

    def countdown(self):
        if self.is_running and (self.minutes > 0 or self.seconds > 0):
            self.timer_text.set(f"{self.minutes:02d}:{self.seconds:02d}")

            if self.seconds == 0:
                self.minutes -= 1
                self.seconds = 59
            else:
                self.seconds -= 1

            self.master.after(1000, self.countdown)
        elif self.is_running and (self.minutes == 0 and self.seconds == 0):
            self.timer_text.set("Time's up!")
            self.is_running = False
            self.start_button.config(state=tk.NORMAL)
            self.pause_button.config(state=tk.DISABLED)

    def pause_timer(self):
        self.is_running = False
        self.start_button.config(state=tk.NORMAL)
        self.pause_button.config(state=tk.DISABLED)
        self.reset_button.config(state=tk.NORMAL)

    def reset_timer(self):
        self.is_running = False
        self.minutes = 25
        self.seconds = 0
        self.timer_text.set(f"{self.minutes:02d}:{self.seconds:02d}")
        self.start_button.config(state=tk.NORMAL)
        self.pause_button.config(state=tk.DISABLED)
        self.reset_button.config(state=tk.DISABLED)


if __name__ == "__main__":
    root = tk.Tk()
    timer = PomodoroTimer(root)
    root.mainloop()
