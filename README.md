# music-player
import tkinter as tk
from tkinter import *
from tkinter import filedialog
import pygame

root = tk.Tk()
root.title('Music Player')
root.geometry('400x400')
root.minsize(400,400)
root.configure(background='#262626')
pygame.mixer.init()
song_list = tk.Listbox(root,width=60,bg='black',fg='white')
song_list.pack(pady=20)

photo1 = PhotoImage(file = r"D:\musicplayer\icons8-skip-to-start-30.png")
photo2 = PhotoImage(file = r"D:\musicplayer\icons8-play-30.png")
photo3 = PhotoImage(file = r"D:\musicplayer\icons8-pause-30.png")
photo4 = PhotoImage(file = r"D:\musicplayer\icons8-end-30.png")
photo5 = PhotoImage(file = r"D:\musicplayer\icons8-stop-30.png")

def add_song():
		songs = filedialog.askopenfilenames(initialdir='D:/musicplayer/',title='Add Music',filetypes=(('mp3 Files' , '*.mp3'),))
		for song in songs:
			songs = song.replace('D:/musicplayer/','')
			song_list.insert(END,songs)

def play_song():
	song = song_list.get(ACTIVE)
	song = f'D:/musicplayer/{song}'
	pygame.mixer.music.load(song)
	pygame.mixer.music.play(loops=0)
def stop_song():
	pygame.mixer.music.stop()
	song_list.selection_clear(ACTIVE)

global paused
paused=False
def pause_song():
	global paused
	if paused:
		pygame.mixer.music.unpause()
		paused = False
	else:
		pygame.mixer.music.pause()
		paused = True

def play_next_song():
	next_song = song_list.curselection()
	next_song = next_song[0]+1
	song = song_list.get(next_song)
	song = f'D:/musicplayer/{song}'
	pygame.mixer.music.load(song)
	pygame.mixer.music.play(loops=0)
	song_list.selection_clear(0,END)
	song_list.selection_set(next_song,last=None) 

def play_previous_song():
	next_song = song_list.curselection()
	next_song = next_song[0]-1
	song = song_list.get(next_song)
	song = f'D:/musicplayer/{song}'
	pygame.mixer.music.load(song)
	pygame.mixer.music.play(loops=0)
	song_list.selection_clear(0,END)
	song_list.selection_set(next_song,last=None) 
frame = tk.Frame(root,bg='white')
frame.pack()
previous_button = tk.Button(frame,image=photo1,command=play_previous_song,background='#383838',fg='white')
play_button = tk.Button(frame,image=photo2,command=play_song,background='#383838',fg='white')
pause_button = tk.Button(frame,image=photo3,command=pause_song,background='#383838',fg='white')
next_button = tk.Button(frame,image=photo4,command=play_next_song,background='#383838',fg='white')
stop_button = tk.Button(frame,image=photo5,command=stop_song,background='#383838',fg='white')

previous_button.pack(side=LEFT)
play_button.pack(side=LEFT)
pause_button.pack(side=LEFT)
next_button.pack(side=LEFT)
stop_button.pack(side=LEFT)

add_song_btn = tk.Button(root,text='ADD SONGS',padx=10,pady=10,command=add_song,background='#383838',fg='white')
add_song_btn.pack(pady=20)
root.mainloop()
