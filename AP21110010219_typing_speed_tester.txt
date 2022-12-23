#from tkinter import *
import ctypes
import random
import tkinter
 
# For a sharper window
ctypes.windll.shcore.SetProcessDpiAwareness(1)

# Setup
root = Tk()
root.title('Typing speed Tester')

# Setting the starting window dimensions
root.geometry('700x700')

# Setting the Font for all Labels and Buttons
root.option_add("*Label.Font", "consolas 30")
root.option_add("*Button.Font", "consolas 30")

# functions
def keyPress(event=None):
    
        if event.char.lower() == label_for_Right.cget('text')[0].lower():
            # Deleting one from the right side.
            label_for_Right.configure(text=label_for_Right.cget('text')[1:])
            # Deleting one from the right side.
            label_for_Left.configure(text=label_for_Left.cget('text') + event.char.lower())
            #set the next Letter Lavbel
            label_for_currentLetter.configure(text=label_for_Right.cget('text')[0])
    

def labeltext():
    # Text List
    possible_list_of_Texts = [
        'Python is a high-level, general-purpose and a very popular programming language. Python programming language (latest Python 3) is being used in web development, Machine Learning applications, along with all cutting edge technology in Software Industry. Python Programming Language is very well suited for Beginners, also for experienced programmers with other programming languages like C++ and Java.Python is currently the most widely used multi-purpose, high-level programming language.',
        'The goal of Python Code is to provide Python tutorials, recipes, problem fixes and articles to beginner and intermediate Python programmers, as well as sharing knowledge to the world. Python Code aims for making everyone in the world be able to learn how to code for free. Python is a high-level, interpreted, general-purpose programming language. Its design philosophy emphasizes code readability with the use of significant indentation. Python is dynamically-typed and garbage-collected. It supports multiple programming paradigms, including structured (particularly procedural), object-oriented and functional programming. It is often described as a "batteries included" language due to its comprehensive standard library.',
        'As always, we start with the imports. Because we make the UI with tkinter, we need to import it. We also import the font module from tkinter to change the fonts on our elements later. We continue by getting the partial function from functools, it is a genius function that excepts another function as a first argument and some args and kwargs and it will return a reference to this function with those arguments. This is especially useful when we want to insert one of our functions to a command argument of a button or a key binding.'
    ]
    # Chosing one of the texts randomly with the choice function
    text = random.choice(possible_list_of_Texts).lower()
    
    
    #Tkinter provides various controls, such as buttons, labels and text boxes used in a GUI application. These controls are commonly called widgets.
    
    # defining where the text is split
    splitPoint = 0
    # This is where the text is that is already written
    global label_for_Left
    
    #The Label is used to specify the container box where we can place the text or images.
    
    label_for_Left = Label(root, text=text[0:splitPoint], fg='grey')
    
    #place() lets you position a widget either with absolute x,y coordinates, or relative to another widget.
    
    
   # We can use relative x ( horizontal ) and y ( vertical ) position with respect to width and height of the window by using relx and rely options fraction of the height and width of the parent widget
    label_for_Left.place(relx=0.5, rely=0.5, anchor=E)#anchor − The exact spot of widget other options refer to: may be N, E, S, W, NE, NW, SE, or SW, compass directions indicating the corners and sides of widget; default is NW 

    # Here is the text which will be written
    global label_for_Right
    label_for_Right = Label(root, text=text[splitPoint:])
    label_for_Right.place(relx=0.5, rely=0.5)

    # This label shows the user which letter he now has to press
    global label_for_currentLetter
    label_for_currentLetter = Label(root, text=text[splitPoint], fg='grey')
    label_for_currentLetter.place(relx=0.5, rely=0.6)

    # this label shows the user how much shows him or her how much time is left
    global lbel_for_timeleft
    lbel_for_timeleft = Label(root, text=f'0 Seconds', fg='grey')
    lbel_for_timeleft.place(relx=0.5, rely=0.4)

    global writeAble
    writeAble = True
    #we bind every key event to the keyPress() function
    root.bind('<Key>', keyPress)

    global passedSeconds
    passedSeconds = 0

    # Binding callbacks to functions after a certain amount of time.
    root.after(60000, stopTest)#after() method uses the millisecond
    root.after(1000, addSecond)

def stopTest():
    global writeAble
    writeAble = False
    
    # Calculating the amount of words
    amountWords = len(label_for_Left.cget('text').split())
    
    # Destroy all unwanted widgets.
    lbel_for_timeleft.destroy()
    label_for_currentLetter.destroy()
    label_for_Right.destroy()
    label_for_Left.destroy()

    # Display the test results with a formatted string
    global label_for_result
    label_for_result = Label(root, text=f'Words per Minute: {amountWords}', fg='black')
    label_for_result.place(relx=0.5, rely=0.4, anchor=CENTER)

    # Display a button to restart the game
    global ResultButton
    ResultButton = Button(root, text=f'Retry', command=restart)
    ResultButton.place(relx=0.5, rely=0.6, anchor=CENTER)

def restart():
    # Destry result widgets
    label_for_result.destroy()
    ResultButton.destroy()

    # re-setup writing labels.
    labeltext()

def addSecond():
    # Add a second to the counter.
        #This function will update what is shown in the lbel_for_timeleft
    global passedSeconds
    passedSeconds += 1
    lbel_for_timeleft.configure(text=f'{passedSeconds} Seconds')

    # call this function again after one second if the time is not over.
    if writeAble:
        root.after(1000, addSecond)

# This will start the Test
labeltext()
# Start the mainloop
root.mainloop()