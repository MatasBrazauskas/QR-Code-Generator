# QRGraphics

Welcome to my first GitHub repository! 🎉  

## *About*  
This is my personal project where I aim to re-create a QR code generator. Through this journey, I am learning how to:  
- Work with graphical interfaces.  
- Gather information from the internet about QR codes and their creation.  
- Get familiar with Git and GitHub by committing and managing my code.  

## Goals  
- Build a functional QR code generator.  
- Improve my understanding of GUI development.  
- Gain experience with Git and GitHub workflows.

## Color Meaning  
- **Grey**: Areas not filled with information  
- **Red**: Finder patterns  
- **Green**: Separators  
- **Orange**: Timing pattern  
- **Cyan**: Encoding mode  
- **Pink**: Length of a message  

## *QR Code Creation Explained*

Most examples will use the version 1 QR grid formation (21x21 pixels). The formula to calculate the length of a QR grid formation is:  
**N = 4 * V + 17**  
Where:
- **V**: Version  
- **N**: Length in pixels  

### 1. Window  
Initially, I create a screen that is all white. The only important thing at this step is that the dimensions of the QR code output are correct. The screen needs to be square, and the input/output dimensions may differ slightly.

![image](https://github.com/user-attachments/assets/a200bc4e-fc07-44eb-9187-add399e50599)

### 2. Grid Values  
In the second step, I store and display the QR grid pixel values in an integer matrix. I use this method because I also use color to denote different zones and their functionality. Grey pixels represent the QR grid area, while white pixels represent the quiet zone (padding) that helps QR readers detect the code.

![image](https://github.com/user-attachments/assets/afae2fc8-3f41-4509-a197-f84425ddb233)

### 3. Position Finders  
Position and orientation finders are used to detect the rotation of the QR code. These are three squares at the top-right, top-left, and bottom-left corners. Each square is 7 pixels wide and long. The position finders do not change or move between different versions of the QR code.

![image](https://github.com/user-attachments/assets/e9859aa8-de59-4318-867c-7a63d41b43f9)

### 4. Separators  
Separators help improve QR code decoding and separate the position areas from other areas of the grid.

![image](https://github.com/user-attachments/assets/e7685331-cc3d-4efb-b7f5-f63924c6ca42)

### 5. Timing Pattern  
Timing patterns are used to specify the QR code's version. They are located between the position finders on the top-left, bottom-left, and top-right corners. The length of a timing pattern is **N - 16**. The first and last pixels of a timing pattern are always black, and the pattern alternates between black and white pixels.

![image](https://github.com/user-attachments/assets/13980e4e-5376-4f02-9f5e-0cd7bfacc397)

### 6. Alignment Pattern  
The alignment pattern helps interpret the QR code at an angle. It appears in version 2 and higher; version 1 does not have one. The first alignment pattern is placed at coordinates **(N-8, N-8)**. As the version increases, more alignment patterns are added.

![image](https://github.com/user-attachments/assets/00e52e22-3747-424d-805a-bd5749f6d9fb)
Version 2 with alignment pattern:

![image](https://github.com/user-attachments/assets/bd100bdc-dca3-42d1-aec0-c46ce836c9f3)
Version 7:

![image](https://github.com/user-attachments/assets/78c9702f-02a3-4eaf-9ff6-708f1f82a318)

The number of alignment patterns for each version can be calculated with the formula:  
**C = N // 7 + 2**  
Where **C** is the count of alignment patterns. The distance between them is given by:  
**D = (N - 13) // (C - 1)**

### 7. One Single Pixel  
The pixel at position **(8, N-8)** is always black.

![image](https://github.com/user-attachments/assets/964925a3-770f-4c38-bcbf-683bac88fab3)

### 8. Encoding Mode  
QR codes support four encoding modes:  
- **Numeric**: 0-9  
- **Alphanumeric**: 0–9, A–Z (upper-case only), space, $, %, *, +, -, ., /  
- **Byte**: Extended ASCII symbols  
- **Kanji**: Chinese symbols used in Japanese language  

In this example, the encoding is set to **Byte** with a half-byte of `0b0100`.

![image](https://github.com/user-attachments/assets/94e7b489-8dd0-4b99-9203-e77ca59246f0)

### 9. Message Length  
The message length is stored in the top byte near the encoding mode half-byte. For versions 1-9, the length is stored in one byte; for versions 10 and above, it is stored in two bytes. This length is also represented using the Z-pattern.

![image](https://github.com/user-attachments/assets/7b3fe7da-733f-4e02-885d-e49db228afb7)

### 10. Format Information and Error Correction  
The format information stores the error correction level and the mask pattern of the QR code. The error correction levels are:  
- **Low (L)**: Bits = `01`, Integer = 1  
- **Medium (M)**: Bits = `00`, Integer = 0  
- **Quartile (Q)**: Bits = `11`, Integer = 3  
- **High (H)**: Bits = `10`, Integer = 2  

The mask pattern index is represented as a 3-bit value, e.g., pattern 2 in binary is `010`.

![image](https://github.com/user-attachments/assets/ba97a422-23fa-4622-8230-1ca233d2aa6e)

The format and error correction information wrap around the position finders.

![image](https://github.com/user-attachments/assets/0fbb4497-e4cd-48f0-a2c6-85ff71e2ce21)

For versions 7 and above, additional format information is included.

![image](https://github.com/user-attachments/assets/45f60cbd-4fcb-4bb6-aa9b-501786994d6f)

### 11. The Data Itself  
The data is encoded based on the chosen encoding mode (e.g., Byte encoding uses ASCII values converted to binary). The data is represented using a Z-pattern.

![image](https://github.com/user-attachments/assets/511777d5-6cfa-474f-8546-a045b4d84628)

### 12. Masking Pattern  
Masking patterns improve QR code readability and scanning accuracy. They are applied to ensure better differentiation between black and white pixels.

![image](https://github.com/user-attachments/assets/ea10b6ec-82fe-45b0-a0bb-b1edd5d22dcc)

### 13. Error Correction for the Data  
Error correction adds redundancy to the QR code to recover data even if part of the code is damaged. The data is encoded and corrected accordingly.

