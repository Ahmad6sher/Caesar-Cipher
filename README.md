[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=18675510&assignment_repo_type=AssignmentRepo)

# Caesar Cipher Implementation
This project is a **Caesar Cipher**, a simple encryption technique that shifts letters by a fixed amount. The implementation was refined over multiple steps to ensure correctness, efficiency, and readability.

## Development Process
### **Starting with the Basics**
The project began with a basic class structure containing placeholder methods for encryption, decryption, and file handling.

```python
class CaesarCipher:
    def __init__(self, key=3):
        if not isinstance(key, int):
            raise ValueError("Key must be an integer.")
        self.key = key % 26 if key >= 0 else (26 + key % 26) % 26  # Keeps shift values within a valid range
```
This initial version set up the **key attribute** but didn’t include error handling for invalid values, which was later improved.

### **Implementing Encryption & Decryption**
Next, encryption and decryption methods were added. These were refined over time to:
- Handle **uppercase and lowercase letters properly**.
- Work efficiently without redundant calculations.

Final optimized versions:
```python
    def encrypt(self, plaintext):
        '''Encrypts a message using Caesar Cipher.'''
        return ''.join(
            chr((ord(char) - base + self.key) % 26 + base) if char.isalpha() else char
            for char in plaintext
            for base in [ord('A') if char.isupper() else ord('a')]
        )

    def decrypt(self, ciphertext):
        '''Decrypts a message using Caesar Cipher.'''
        return ''.join(
            chr((ord(char) - base - self.key) % 26 + base) if char.isalpha() else char
            for char in ciphertext
            for base in [ord('A') if char.isupper() else ord('a')]
        )
```
This ensures **all alphabetic characters** shift correctly while **leaving non-alphabetic characters unchanged**.

### **File Encryption & Decryption**
Initially, file handling lacked error checks, so improvements were made to:
- Ensure the **file exists** before reading.
- Handle **unexpected file errors**.

Final version:
```python
    def encrypt_file(self, input_filename, output_filename):
        '''Encrypts a file's contents using Caesar Cipher.'''
        from pathlib import Path
        input_path, output_path = Path(input_filename), Path(output_filename)
        if not input_path.exists():
            raise FileNotFoundError(f"File '{input_filename}' not found.")
        
        try:
            output_path.write_text(self.encrypt(input_path.read_text(encoding='utf-8')))
        except (OSError, IOError) as e:
            raise IOError(f"File error: {e}")

    def decrypt_file(self, input_filename, output_filename):
        '''Decrypts a file's contents using Caesar Cipher.'''
        from pathlib import Path
        input_path, output_path = Path(input_filename), Path(output_filename)
        if not input_path.exists():
            raise FileNotFoundError(f"File '{input_filename}' not found.")
        
        try:
            output_path.write_text(self.decrypt(input_path.read_text(encoding='utf-8')))
        except (OSError, IOError) as e:
            raise IOError(f"File error: {e}")
```
### **Final Testing & Validation**
Testing covered:
- **Basic encryption & decryption cases**.
- **Edge cases**, including non-alphabetic characters and varying shift values.
- **File operations** to confirm correctness.

Final execution:
```python
if __name__ == "__main__":
    cipher = CaesarCipher()
    sample_text = "Hello, World!"
    encrypted_text = cipher.encrypt(sample_text)
    decrypted_text = cipher.decrypt(encrypted_text)
    print("Encrypted:", encrypted_text)
    print("Decrypted:", decrypted_text)
    
    # Encrypt and decrypt files
    from pathlib import Path
    test_input_file, test_encrypted_file, test_decrypted_file = "secret_message.txt", "encrypted_secret_message.txt", "decrypted_text.txt"
    Path(test_input_file).write_text(sample_text, encoding='utf-8')
    cipher.encrypt_file(test_input_file, test_encrypted_file)
    cipher.decrypt_file(test_encrypted_file, test_decrypted_file)
    
    print("Encrypted file content:", Path(test_encrypted_file).read_text(encoding='utf-8'))
    print("Decrypted file content:", Path(test_decrypted_file).read_text(encoding='utf-8'))
```

## **Final Thoughts**
The project went through **multiple iterations**, focusing on:
- **Ensuring correctness** with different inputs.
- **Improving efficiency** by optimizing calculations.
- **Adding error handling** to prevent crashes.

The final result is a **robust, well-structured Caesar Cipher implementation** that handles text and files efficiently. 🚀
