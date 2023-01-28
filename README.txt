from tkinter import *
from tkinter import Tk
from tkinter import TkVersion
from traceback import FrameSummary
from PIL import ImageTk,Image
from tkinter import messagebox
import ast
import maskpass
import tkinter
import sqlite3 as sl
w=Tk()
w.geometry('900x400')
w.title('Login')
w.configure(bg='#ff4f5a')
############
con = sl.connect("my-app.db")

with con:
    con.execute(
        """
        CREATE TABLE  IF NOT EXISTS signup(
           Username TEXT,
           Password TEXT
        );
        """
    )
###########

def signin():  
    global Username
    global Password
    Username =  tkinter.StringVar()
    Password =  tkinter.StringVar()

    signin_win=Frame(w,width=2560,height=1600,bg='white')
    signin_win.place(x=0,y=0)
    f1=Frame(signin_win,width=350,height=350,bg='white')
    f1.place(x=480,y=100)
    
    global img1
    img1 = ImageTk.PhotoImage(Image.open("nutritionsignin(2).jpeg"))
    Label(signin_win,image=img1,border=0,bg='white').place(x=2,y=2)

    l2=Label(signin_win,text="Sign in",fg='#000000',bg='white')
    l2.config(font=('Microsoft YaHei UI Light',23, 'bold'))
    l2.place(x=600,y=60)

    def on_enter(e):
        e1.delete(0,'end')    
    def on_leave(e):
        if e1.get()=='':   
            e1.insert(0,'Username')

    
    e1 =Entry(f1,width=25,fg='black',border=0,bg='white')
    e1.config(font=('Microsoft YaHei UI Light',11, ))
    e1.bind("<FocusIn>", on_enter)
    e1.bind("<FocusOut>", on_leave)
    e1.insert(0,'Username')
    e1.place(x=30,y=60)

    Frame(f1,width=295,height=2,bg='black').place(x=25,y=87)

    #------------------------------------------------------

    def on_enter(e):
        e2.delete(0,'end')    
    def on_leave(e):
        if e2.get()=='':   
            e2.insert(0,'Password')

    
    e2 =Entry(f1,width=21,fg='black',border=0,bg='white')
    e2.config(font=('Microsoft YaHei UI Light',11, ))
    e2.bind("<FocusIn>", on_enter)
    e2.bind("<FocusOut>", on_leave)
    e2.insert(0,'Password')
    e2.place(x=30,y=130)

    Frame(f1,width=295,height=2,bg='black').place(x=25,y=157)

    #-mech------------------------------------------------
    def signin_cmd():
        Username = e1.get()
        Password = e2.get()
        cursor = con.cursor()
        sql = "SELECT * FROM signup WHERE Username = ? AND Password = ? " 
        cursor.execute(sql , (Username,Password))

        if cursor.fetchall():           
            messagebox.showinfo("","     successfully logged in    ")
            w.destroy()
            import gender
        else:
            messagebox.showwarning('try again', 'invalid username or password')


    #------------------------------------------------------
    Button(f1,width=39,pady=7,text='Sign in',bg='#000000',fg='black',border=0,command=signin_cmd).place(x=35,y=204)
    l1=Label(f1,text="Don't have an account?",fg="black",bg='white')
    l1.config(font=('Microsoft YaHei UI Light',9, ))
    l1.place(x=75,y=250)

    b2=Button(f1,width=6,text='Sign up',border=0,bg='white',fg='#ff4f5a',command=signup)
    b2.place(x=215,y=250)




def signup():
    signup_win=Frame(w,width=925,height=500,bg='white')
    signup_win.place(x=0,y=0)
    f1=Frame(signup_win,width=350,height=350,bg='white')
    f1.place(x=480,y=70)

    
    global img2
    img2 = ImageTk.PhotoImage(Image.open("nutritionsignup(2).jpeg"))
    Label(signup_win,image=img2,border=0,bg='black').place(x=30,y=90)

    l2=Label(signup_win,text="Sign up",fg='#ff4f5a',bg='white')
    l2.config(font=('Microsoft YaHei UI Light',23, 'bold'))
    l2.place(x=600,y=60)

    def on_enter(e):
        e1.delete(0,'end')    
    def on_leave(e):
        if e1.get()=='':   
            e1.insert(0,'Username')

    
    e1 =Entry(f1,width=25,fg='black',border=0,bg='white')
    e1.config(font=('Microsoft YaHei UI Light',11, ))
    e1.bind("<FocusIn>", on_enter)
    e1.bind("<FocusOut>", on_leave)
    e1.insert(0,'Username')
    e1.place(x=30,y=60)

    Frame(f1,width=295,height=2,bg='black').place(x=25,y=87)

    #------------------------------------------------------

    def on_enter(e):
        e2.delete(0,'end')    
    def on_leave(e):
        if e2.get()=='':   
            e2.insert(0,'Password')

    
    e2 =Entry(f1,width=21,fg='black',border=0,bg='white')
    e2.config(font=('Microsoft YaHei UI Light',11, ))
    e2.bind("<FocusIn>", on_enter)
    e2.bind("<FocusOut>", on_leave)
    e2.insert(0,'Password')
    e2.place(x=30,y=130)

    Frame(f1,width=295,height=2,bg='black').place(x=25,y=157)

    def on_enter(e):
        e3.delete(0,'end')    
    def on_leave(e):
        if e3.get()=='':   
            e3.insert(0,'Confirm Password')

    
    e3 =Entry(f1,width=21,fg='black',border=0,bg='white')
    e3.config(font=('Microsoft YaHei UI Light',11, ))
    e3.bind("<FocusIn>", on_enter)
    e3.bind("<FocusOut>", on_leave)
    e3.insert(0,'Confirm Password')
    e3.place(x=30,y=130+70)

    Frame(f1,width=295,height=2,bg='black').place(x=25,y=157+70)    

    
    #Mechenism------------------------------------------------
    
    def signup_cmd():
        key=e1.get()
        value=e2.get()
        value2=e3.get()
        if value==value2:
            
            sql = "INSERT INTO signup VALUES(?,?)"
            data=(key,value)
            con.execute(sql,data)
            con.commit()
            
            messagebox.showinfo("","     successfully signed up     ")  
        else:
            messagebox.showwarning('try again', 'password should match ')

    #-------------------------------------------------------
    Button(f1,width=39,pady=7,text='Sign up',bg='#000000',fg='black',border=0,command=signup_cmd).place(x=35,y=204+60)
    l1=Label(f1,text="Already have an account?",fg="black",bg='white')
    l1.config(font=('Microsoft YaHei UI Light',9, ))
    l1.place(x=70,y=250+63)

    b2=Button(f1,width=6,text='Sign in',border=0,bg='black',fg='#000000',command=signin)
    b2.place(x=210,y=250+63)

signin() #default screen

w.mainloop()
