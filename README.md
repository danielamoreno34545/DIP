# DIP
#Hola! aquí encontrarás el código para realizar mejoras a imágenes medicas. 
#CODIGO ABAJO!!!!

from tkinter import *
from tkinter import filedialog
from os import path
from skimage import io, exposure, img_as_ubyte
from PIL import ImageTk, Image
import numpy as np
import cv2 

def clicked():
    global file
    file = filedialog.askopenfilename()
    image=Image.open(file)
    image = image.resize((400, 400)) 
    img = ImageTk.PhotoImage(image)
    label.config(image=img)
    label.image = img 
    label.grid(column=0, row=0)
     
    img1 = ImageTk.PhotoImage(image)
    label1.config(image=img1)
    label1.image = img1 
    label1.grid(column=1, row=0)
    
def ecualizar():
    global file
    img=io.imread(file)
    img_adapteq = exposure.equalize_adapthist(img, clip_limit=float(entry.get()))
    image1 = Image.fromarray(img_as_ubyte(img_adapteq))
    image1 = image1.resize((400, 400)) 
    img1 = ImageTk.PhotoImage(image1)
    label1.config(image=img1)
    label1.image = img1 
    label1.grid(column=1, row=0)


def adjust_brightness(img, adjustment): # Aumentar o disminuir el brillo de una imagen en escala de grises
    if adjustment != 0:
        return np.clip((img.astype(np.int16)+adjustment), 0, 255).astype(np.uint8)
    else:
        return img
def brillo():
    global file
    img=io.imread(file)
    dim_image = adjust_brightness(img,float(entry3.get()) )
    image1 = Image.fromarray(img_as_ubyte(dim_image))
    image1 = image1.resize((400, 400)) 
    img1 = ImageTk.PhotoImage(image1)
    label1.config(image=img1)
    label1.image = img1 
    label1.grid(column=1, row=0)


def contrast_adjustment(img, adjustment): # Ajuste de contraste para una imagen en escala de grises
    return np.uint8(np.clip((adjustment * img), 0, 255))
def contraste():
    global file
    img=io.imread(file)
    dim_image = contrast_adjustment(img, float(entry4.get()))
    image1 = Image.fromarray(img_as_ubyte(dim_image))
    image1 = image1.resize((500, 500)) 
    img1 = ImageTk.PhotoImage(image1)
    label1.config(image=img1)
    label1.image = img1 
    label1.grid(column=1, row=0)



def bordes():
    global file
    img=io.imread(file)
    dim_image  = cv2.Sobel(src=img, ddepth=cv2.CV_8U, dx=1, dy=1, ksize=5) # Combined X and Y Sobel Edge Detection
    image1 = Image.fromarray(img_as_ubyte(dim_image))
    image1 = image1.resize((400, 400)) 
    img1 = ImageTk.PhotoImage(image1)
    label1.config(image=img1)
    label1.image = img1 
    label1.grid(column=1, row=0)

window = Tk()
window.geometry('1200x600')

window.title("ImageMedix")

lbl = Label(window, text="Seleccione un archivo: ", font=("Arial Bold", 12))

lbl.grid(column=3, row=0)

btn = Button(window, text="Buscar", command=clicked)

btn.grid(column=4, row=0)

lbl = Label(window, text="Imagen original", font=("Arial Bold", 10))
lbl.grid(column=0, row=1)
image=Image.open('Radriografias torax\\radiografia.jpeg')
image = image.resize((400, 400)) 
img = ImageTk.PhotoImage(image)
label = Label(image=img)
label.image = img 
label.grid(column=0, row=0)

lbl1 = Label(window, text="Imagen procesada", font=("Arial Bold", 10))
lbl1.grid(column=1, row=1)
img1 = ImageTk.PhotoImage(image)
label1 = Label(image=img1)
label1.image = img1 
label1.grid(column=1, row=0)

label2 = Label(window, text="Valor de ecualización: ")
label2.grid(row=1, column=3, sticky="w", padx=5, pady=5)
entry = Entry(window)
entry.grid(row=1,column=4, padx=5, pady=5)
entry.config(justify="right", state="normal")
btn2 = Button(window, text="Ecualización", command=ecualizar)
btn2.grid(column=5, row=1)



label3 = Label(window, text="Valor de brillo: ")
label3.grid(row=2, column=3, sticky="w", padx=5, pady=5)
entry3 = Entry(window)
entry3.grid(row=2,column=4, padx=5, pady=5)
entry3.config(justify="right", state="normal")
btn3 = Button(window, text="Brillo", command=brillo)
btn3.grid(column=5, row=2)



label4 = Label(window, text="Valor de contraste: ")
label4.grid(row=3, column=3, sticky="w", padx=5, pady=5)
entry4 = Entry(window)
entry4.grid(row=3,column=4, padx=5, pady=5)
entry4.config(justify="right", state="normal")
btn4 = Button(window, text="Contraste", command=contraste)
btn4.grid(column=5, row=3)




label5 = Label(window, text="Valor de realce de bordes: ")
label5.grid(row=4, column=3, sticky="w", padx=5, pady=5)
entry5 = Entry(window)
entry5.grid(row=4,column=4, padx=5, pady=5)
entry5.config(justify="right", state="normal")
btn5 = Button(window, text="Bordes", command=bordes)
btn5.grid(column=5, row=4)


window.mainloop()
