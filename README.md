# IMAGE-STEGANOGRAPHY
ENCODING AND DECODING HIDDEN DATA WITHIN AN IMAGE FILE

**Project Prerequisites:**
To build this Python Image Steganography project, we will need the following libraries:
1. **Tkinter** – To create the GUI
2. **PIL** – Python Image Manipulation library. It is used to perform operations on images in python.
While the Tkinter library comes pre-installed with Python, the PIL library does not, so you will have to run the following command to install it.
**python -m pip install pillow**

**Project File Structure:**

Here are the steps you will need to execute to build this Image Steganography project in Python:
:white_check_mark:Initializing the root window and placing all components in it

:white_check_mark:Defining all the backend encryption and decryption functions

:white_check_mark:Defining the GUI functions of decoding and encoding

**Initializing the root window and placing all components in it:**

	root = Tk()
	root.title('Proton Image Steganography')
	root.geometry('300x200')
	root.resizable(0, 0)
	root.config(bg='NavajoWhite')
	Label(root, text='Proton Image Steganography', font=('Comic Sans MS', 15), bg='NavajoWhite', wraplength=300).place(x=40, y=0)
	Button(root, text='Encode', width=25, font=('Times New Roman', 13), bg='SteelBlue', command=encode_image).place(x=30, y=80)
	Button(root, text='Decode', width=25, font=('Times New Roman', 13), bg='SteelBlue', command=decode_image).place(x=30, y=130)
	root.update()
	root.mainloop()

**Explanation:**

We will assign the Tk() class to the root variable to initialize the window. The methods and attributes that need to be set during initializing are:

The .title() method is used to give a title to the window.

The .geometry() method is used to define the initial dimensions of the window in pixels.

The .resizable() method specifies whether the user will be allowed to resize the window or not. It takes only truth and falsy values in the form of (width, height).

The .config() method is used to configure other attributes of the window, such as the bg attribute which denotes the colour of the background of the window.

The .update() and .mainloop() methods put the window in a loop to prevent it from closing until told otherwise.

**Note:**

These two lines will be the last lines in a GUI script that the Python interpreter will run in it.
The Label class is used to create a Label on the window that displays static text on the window. Its parameters that need to be set during assignment are:

:white_check_mark: The master parameter, the positional argument root here, is the parent widget this widget is associated with.

:white_check_mark: The text parameter refers to the text that will be displayed on the label.

:white_check_mark: The font parameter defines the font family, size and effects that will be applied to the text displayed on the label.

:white_check_mark: The wraplength parameter specifies the number of pixels after which the text has to be moved to the next line.The Button class is used to add a button to the window that runs a function when it is pressed. Its parameters are:

:white_check_mark: The command parameter is used to define the function that will run when the button is pressed. You will not need the lambda keyword if the function to run does not need any arguments.The .place() method is used to place a widget on its parent widget as though the parent is a Cartesian Plane, with the Northwest corner being the origin of that plane.

:white_check_mark: The x,y parameters define the horizontal and vertical offsets of the widget, respectively.

:white_check_mark: The relx,rely parameters define the horizontal and vertical offsets as a decimal number between 0.0 and 1.0

**Defining all the backend encryption and decryption functions:**

	def generate_data(pixels, data):
 	data_in_binary = []
	for i in data:
	binary_data = format(ord(i), '08b')
	data_in_binary.append(binary_data)
	length_of_data = len(data_in_binary)
	image_data = iter(pixels)
	for a in range(length_of_data):
	pixels = [val for val in image_data.__next__()[:3] + image_data.__next__()[:3] + image_data.__next__()[:3]]
	for b in range(8):
	if (data_in_binary[a][b] == '1') and (pixels[b] % 2 != 0):
	pixels[b] -= 1
	elif (data_in_binary[a][b] == '0') and (pixels[b] % 2 == 0):
	if pixels[b] == 0:
	pixels[b] += 1
	pixels[b] -= 1
	if (length_of_data-1) == a:
	if pixels[-1] % 2 == 0:
	if pixels[-1] == 0:
	pixels[-1] += 1
	else:
	pixels[-1] -= 1
	pixels = tuple(pixels)
	yield pixels[:3]
	yield pixels[3:6]
	yield pixels[6:9]
	def encryption(img, data):
	size = img.size[0]
	(x, y) = (0, 0)
	for pixel in generate_data(img.getdata(), data):
	img.putpixel((x, y), pixel)
	if size-1 == x:
	x = 0; y += 1
	else:
	x += 1
	def main_encryption(img, text, new_image_name):
	image = Image.open(img, 'r')
	if (len(text) == 0) or (len(img) == 0) or (len(new_image_name) == 0):
	mb.showerror("Error", 'You have not put a value! Please put all values before pressing the button')
	new_image = image.copy()
	encryption(new_image, text)
	new_image_name += '.png'
	new_image.save(new_image_name, 'png')
	def main_decryption(img, strvar):
	image = Image.open(img, 'r')
	data = ''
	image_data = iter(image.getdata())
	decoding = True
	while decoding:
	pixels = [value for value in image_data.__next__()[:3] + image_data.__next__()[:3] + image_data.__next__()[:3]]
	binary_string = ''
	for i in pixels[:8]:
	if i % 2 == 0:
	binary_string += '0'
	else:
	binary_string += '1'
	data += chr(int(binary_string, 2))
	if pixels[-1] % 2 != 0:
	strvar.set(data)
