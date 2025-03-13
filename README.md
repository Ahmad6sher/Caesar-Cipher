[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=18675510&assignment_repo_type=AssignmentRepo)

# Caesar Cipher Implementation
Your task in this homework is to construct a class to implement Caesar Cipher.

## Specification
### __init__() method
Define attributes that may be used in the following methods, such as key. The key represents the shift value for encryption and decryption.

```python
class CaesarCipher:
    def __init__(self, key=3):
        if not isinstance(key, int):
            raise ValueError("Key must be an integer.")
        self.key = key % 26 if key >= 0 else (26 + key % 26) % 26  # Ensures valid shift values
```

### Encrypt Message
The `encrypt()` method encrypts a message using the Caesar Cipher.

```python
    def encrypt(self, plaintext):
        ''' 
        Encrypt plaintext using Caesar Cipher.
        '''
        return ''.join(
            chr((ord(char) - base + self.key) % 26 + base) if char.isalpha() else char
            for char in plaintext
            for base in [ord('A') if char.isupper() else ord('a')]
        )
```

### Decrypt Message
The `decrypt()` method decrypts an encrypted message using Caesar Cipher.

```python
    def decrypt(self, ciphertext):
        ''' 
        Decrypt ciphertext using Caesar Cipher.
        '''
        return ''.join(
            chr((ord(char) - base - self.key) % 26 + base) if char.isalpha() else char
            for char in ciphertext
            for base in [ord('A') if char.isupper() else ord('a')]
        )
```

### Encrypt a File
The `encrypt_file()` method encrypts the content of a file using Caesar Cipher and writes the encrypted content to another file.

```python
    def encrypt_file(self, input_filename, output_filename):
        '''
        Encrypt the content of a file and store the ciphertext in another file.
        '''
        from pathlib import Path
        input_path = Path(input_filename)
        output_path = Path(output_filename)
        if not input_path.exists():
            raise FileNotFoundError(f"Input file '{input_filename}' not found.")
        
        try:
            content = input_path.read_text(encoding='utf-8')
            output_path.write_text(self.encrypt(content), encoding='utf-8')
        except (OSError, IOError) as e:
            raise IOError(f"Error processing file: {e}")
```

### Decrypt a File
The `decrypt_file()` method decrypts the content of a file using Caesar Cipher and writes the decrypted content to another file.

```python
    def decrypt_file(self, input_filename, output_filename):
        '''
        Decrypt the content of a file and store the decrypted text in another file.
        '''
        from pathlib import Path
        input_path = Path(input_filename)
        output_path = Path(output_filename)
        if not input_path.exists():
            raise FileNotFoundError(f"Input file '{input_filename}' not found.")
        
        try:
            content = input_path.read_text(encoding='utf-8')
            output_path.write_text(self.decrypt(content), encoding='utf-8')
        except (OSError, IOError) as e:
            raise IOError(f"Error processing file: {e}")
```

### Example Usage
```python
if __name__ == "__main__":
    cipher = CaesarCipher()
    sample_text = "Hello, World!"
    encrypted_text = cipher.encrypt(sample_text)
    decrypted_text = cipher.decrypt(encrypted_text)
    print("Encrypted:", encrypted_text)
    print("Decrypted:", decrypted_text)
    
    # Encrypt and decrypt files
    test_input_file = "secret_message.txt"
    test_encrypted_file = "encrypted_secret_message.txt"
    test_decrypted_file = "decrypted_text.txt"
    
    from pathlib import Path
    Path(test_input_file).write_text(sample_text, encoding='utf-8')
    cipher.encrypt_file(test_input_file, test_encrypted_file)
    cipher.decrypt_file(test_encrypted_file, test_decrypted_file)
    
    # Verify file operations
    print("Encrypted file content:", Path(test_encrypted_file).read_text(encoding='utf-8'))
    print("Decrypted file content:", Path(test_decrypted_file).read_text(encoding='utf-8'))
```
