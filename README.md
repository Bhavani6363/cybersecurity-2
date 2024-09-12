# cybersecurity-2
develop a simple image encryption tool using pixel manipulation support operations like swapping pixel values or applying a basic mathematical operation to each pixel
from PIL import Image
import numpy as np

def swap_pixels(image):
    pixels = np.array(image)
    height, width, _ = pixels.shape
    # Swap pixels in a simple manner: swap each pixel with the one to its right
    for y in range(height):
        for x in range(0, width - 1, 2):  # Swap every two pixels horizontally
            pixels[y, x], pixels[y, x + 1] = pixels[y, x + 1], pixels[y, x]
    return Image.fromarray(pixels)

def apply_math_operation(image, operation, value):
    pixels = np.array(image)
    if operation == "add":
        # Add a constant value to each pixel
        pixels = np.clip(pixels + value, 0, 255)  # Ensure pixel values are within valid range
    elif operation == "subtract":
        # Subtract a constant value from each pixel
        pixels = np.clip(pixels - value, 0, 255)
    elif operation == "multiply":
        # Multiply each pixel value by a constant
        pixels = np.clip(pixels * value, 0, 255)
    elif operation == "divide":
        # Divide each pixel value by a constant
        pixels = np.clip(pixels / value, 0, 255)
    return Image.fromarray(pixels.astype(np.uint8))

def main():
    while True:
        print("Image Encryption Tool")
        print("1. Swap Pixels")
        print("2. Apply Mathematical Operation")
        print("3. Exit")
        
        choice = input("Enter your choice (1/2/3): ")
        
        if choice == '3':
            print("Exiting...")
            break
        
        image_path = input("Enter the path to the image: ")
        try:
            image = Image.open(image_path)
        except IOError:
            print("Error opening the image. Please check the path and try again.")
            continue
        
        if choice == '1':
            encrypted_image = swap_pixels(image)
            encrypted_image.save("swapped_" + image_path)
            print("Image pixels swapped and saved as 'swapped_" + image_path + "'.")
        
        elif choice == '2':
            operation = input("Enter the operation (add, subtract, multiply, divide): ")
            if operation not in ["add", "subtract", "multiply", "divide"]:
                print("Invalid operation. Please select one of: add, subtract, multiply, divide.")
                continue
            value = float(input("Enter the value for the operation: "))
            encrypted_image = apply_math_operation(image, operation, value)
            encrypted_image.save("math_operation_" + image_path)
            print("Mathematical operation applied and saved as 'math_operation_" + image_path + "'.")
        
        else:
            print("Invalid choice. Please select 1, 2, or 3.")

if __name__ == "__main__":
    main()
