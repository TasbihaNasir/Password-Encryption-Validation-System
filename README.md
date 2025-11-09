# 🔐 Password Encryption & Validation System

A comprehensive password security system implemented in x86 Assembly Language (MASM32) featuring robust validation, XOR cipher encryption/decryption, and persistent file storage. This project demonstrates low-level programming concepts including string manipulation, bitwise operations, and file I/O in Assembly.

---

## ⚛️ Built With

- **x86 Assembly Language (MASM32)** — Core programming language
- **Irvine32 Library** — I/O operations and system utilities
- **Windows API** — File handling operations
- **XOR Cipher Algorithm** — Symmetric encryption technique

---

## 👨‍💻 Developers

| Name | Role | Institution |
|------|------|-------------|
| Tasbiha Nasir | Developer | FAST University |
| Zain Saqib | Developer | FAST University |
| Madiha Aslam | Developer | FAST University |

**Course**: Computer Organization and Assembly Language (COAL)  
**Project Type**: Low-Level Security Implementation

---

## 🧩 System Overview

This project implements a secure password management system entirely in x86 Assembly that combines:
1. **Password Validation Engine** - Multi-criteria password strength checker
2. **XOR Cipher Encryption** - Symmetric encryption with key-based transformation
3. **File Persistence System** - Secure storage of original and encrypted passwords
4. **Bitwise Operations** - ROL/ROR operations for additional cipher complexity

The system validates passwords against security criteria, encrypts them using XOR cipher with bit rotation, and stores both original and encrypted versions in separate text files.

---

## 🗄️ Core Features

### 🔤 Password Validation

#### Validation Criteria

**Minimum Length**: 8 characters  
**Required Components**:
- ✅ At least one uppercase letter (A-Z)
- ✅ At least one lowercase letter (a-z)
- ✅ At least one digit (0-9)

#### Validation Process
1. Password input with 20-character buffer
2. Length verification (minimum 8 characters)
3. Character-by-character analysis using flags
4. Three-flag system: `tempUpper`, `tempLower`, `tempDigit`
5. All three flags must be set for password acceptance

#### Example Validations
```
✅ "Password123"     → Valid (has all requirements)
✅ "SecurePass9"     → Valid (8+ chars, mixed case, digit)
❌ "password"        → Invalid (no uppercase, no digit)
❌ "PASSWORD123"     → Invalid (no lowercase)
❌ "Pass1"           → Invalid (too short)
❌ "PasswordOnly"    → Invalid (no digit)
```

---

### 🔐 Encryption Algorithm

The system uses a two-stage XOR cipher with bit rotation:

#### Encryption Process
```assembly
1. XOR with KEY (239)    → Byte-level encryption
2. ROL by 5 bits         → Bit scrambling for complexity
```

**Algorithm Steps**:
```
Original Character: 'P' (ASCII 80)
Step 1: XOR with 239  → 80 ⊕ 239 = 191
Step 2: ROL by 5      → Rotate left 5 bits
Result: Encrypted character
```

#### Decryption Process
```assembly
1. ROR by 5 bits         → Reverse bit rotation
2. XOR with KEY (239)    → Reverse XOR operation
```

**Key Properties**:
- **Symmetric Cipher**: Same key for encryption/decryption
- **XOR Reversibility**: A ⊕ B ⊕ B = A
- **Bit Rotation**: Adds complexity beyond simple XOR
- **Key Value**: 239 (11101111 in binary)

---

### 💾 File Storage System

The system maintains persistent storage across two files:

#### File Structure

**coal1.txt** - Original Passwords
```
Password123,SecurePass9,TestUser1,
```

**coal2.txt** - Encrypted Passwords
```
[encrypted_data],[encrypted_data],[encrypted_data],
```

#### File Operations

1. **WriteOriginalToFile**
   - Creates/opens `coal1.txt`
   - Writes plaintext password
   - Appends comma separator
   - Closes file handle

2. **WriteEncryptedToFile**
   - Dynamically modifies filename to `coal2.txt`
   - Appends encrypted password
   - Adds comma delimiter
   - Ensures data persistence

3. **MyReadFromFile**
   - Opens file for reading
   - Reads up to 100 bytes into buffer
   - Null-terminates buffer content
   - Displays file contents
   - Handles file errors gracefully

---

## 🎯 Assembly Language Concepts

### 1. Register Management
- **ESI**: Source Index for string traversal
- **EDX**: Data pointer for I/O operations
- **ECX**: Loop counter for iteration
- **EAX**: Accumulator for return values and character processing
- **AL**: 8-bit register for byte operations

### 2. Stack Operations
- **PUSHAD**: Save all general-purpose registers
- **POPAD**: Restore all registers
- Maintains register state across procedure calls

### 3. Bitwise Operations
- **XOR**: Exclusive OR for encryption/decryption
- **ROL**: Rotate Left for bit scrambling
- **ROR**: Rotate Right for decryption
- **CMP**: Compare for conditional branching

### 4. Control Flow
- **Conditional Jumps**: JE, JL, JG for decision making
- **Unconditional Jumps**: JMP for loop control
- **Loop Structure**: Password validation retry mechanism

### 5. String Manipulation
- **LEA**: Load Effective Address for pointer initialization
- **MOV BYTE PTR**: Byte-level memory access
- **INC**: Pointer increment for traversal
- Null-terminator detection (ASCII 0)

### 6. Procedure Architecture
- **PROC/ENDP**: Procedure definition
- **CALL**: Procedure invocation
- **RET**: Return to caller
- Modular code organization

---

## 🚀 Usage

### Prerequisites

```bash
# Required Software
- MASM32 SDK (Microsoft Macro Assembler)
- Visual Studio or compatible IDE
- Irvine32 library
- Windows operating system
```

### Installation & Setup

```bash
# 1. Install MASM32 SDK
Download from: http://www.masm32.com/

# 2. Set up Irvine32 library
Include Irvine32.inc in your MASM32 include directory

# 3. Verify installation
ml /c /coff yourfile.asm
```

### Compilation

```bash
# Assemble the source file
ml /c /coff /Cp password_system.asm

# Link with Irvine32 library
Link /SUBSYSTEM:CONSOLE password_system.obj Irvine32.lib kernel32.lib

# Run the executable
password_system.exe
```

### Running the Program

```
Enter your password: Pass123
Password must have at least one lowercase letter.

Enter your password: password123
Password must have at least one uppercase letter.

Enter your password: Password
Password must have at least one digit.

Enter your password: Pass
Password must be at least 8 characters long.

Enter your password: Password123
The password is valid.

Cipher text: [encrypted output]

Decrypted: Password123

[File contents displayed]
```

---

## 📊 Program Flow

```
┌─────────────────────────────────────┐
│     START: main PROC                │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  ValidatePassword                   │
│  ├─ Prompt user input               │
│  ├─ Check length (≥8 chars)         │
│  ├─ Check uppercase flag            │
│  ├─ Check lowercase flag            │
│  ├─ Check digit flag                │
│  └─ Loop until valid                │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  WriteOriginalToFile                │
│  └─ Save to coal1.txt               │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  encryption PROC                    │
│  ├─ XOR each byte with KEY          │
│  └─ ROL by 5 bits                   │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  WriteEncryptedToFile               │
│  └─ Save to coal2.txt               │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  Display Encrypted Password         │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  decryption PROC                    │
│  ├─ ROR by 5 bits                   │
│  └─ XOR with KEY                    │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  Display Decrypted Password         │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  MyReadFromFile                     │
│  └─ Display file contents           │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│     END: exit                       │
└─────────────────────────────────────┘
```

---

## 🔍 Code Structure

### Data Section (.data)

```assembly
KEY = 239                              ; Encryption key constant
BUFMAX = 128                           ; Buffer size constant

; Display strings
sEncrypt BYTE "Cipher text: ", 0
sDecrypt BYTE "Decrypted: ", 0
promptMessage BYTE "Enter your password: ", 0

; Error messages
errorMessageUpper BYTE "Password must have at least one uppercase letter.", 0
errorMessageLower BYTE "Password must have at least one lowercase letter.", 0
errorMessageDigit BYTE "Password must have at least one digit.", 0
msgLengthError BYTE "Password must be at least 8 characters long.", 0

; Buffers
password BYTE 20 DUP(0)                ; Password storage (max 20 chars)
buffer BYTE BUFMAX+1 DUP(0)            ; General purpose buffer
tempBuffer BYTE 100 DUP(0)             ; File reading buffer

; Validation flags
tempLower BYTE 0                       ; Lowercase presence flag
tempUpper BYTE 0                       ; Uppercase presence flag
tempDigit BYTE 0                       ; Digit presence flag

; File handling
filename BYTE "coal1.txt", 0
fileHandle DWORD ?
bytesWritten DWORD ?
token BYTE ",", 0                      ; CSV separator
```

### Code Section (.code)

**Main Procedures**:
1. `ValidatePassword` - Password strength validation
2. `encryption` - XOR cipher with ROL
3. `decryption` - Reverse XOR with ROR
4. `DisplayMessage` - Console output handler
5. `WriteOriginalToFile` - File write (plaintext)
6. `WriteEncryptedToFile` - File write (encrypted)
7. `MyReadFromFile` - File read and display

---

## ⚙️ Technical Implementation

### Password Validation Logic

```assembly
ValidatePassword PROC
    ; Reset flags
    MOV BYTE PTR[tempLower], 0
    MOV BYTE PTR[tempUpper], 0
    MOV BYTE PTR[tempDigit], 0
    
    ; Read password (max 20 chars)
    MOV EDX, OFFSET password
    MOV ECX, 20
    CALL ReadString
    
    ; Check minimum length (8 characters)
    CMP EAX, 8
    JL notvalidlength
    
    ; Character analysis loop
    LEA ESI, password
check_password:
    MOV AL, [ESI]
    CMP AL, 0                    ; Null terminator?
    JE done_checking
    
    ; Check uppercase (A-Z)
    CMP AL, 'A'
    JL not_upper_case
    CMP AL, 'Z'
    JG not_upper_case
    MOV BYTE PTR[tempUpper], 1
    
not_upper_case:
    ; Check lowercase (a-z)
    CMP AL, 'a'
    JL not_lower_case
    CMP AL, 'z'
    JG not_lower_case
    MOV BYTE PTR[tempLower], 1
    
not_lower_case:
    ; Check digit (0-9)
    CMP AL, '0'
    JL not_digit
    CMP AL, '9'
    JG not_digit
    MOV BYTE PTR[tempDigit], 1
    
not_digit:
    INC ESI                      ; Next character
    JMP check_password
    
done_checking:
    ; Verify all flags are set
    MOV AL, [tempUpper]
    CMP AL, 0
    JE missing_upper             ; Retry if missing uppercase
    
    MOV AL, [tempLower]
    CMP AL, 0
    JE missing_lower             ; Retry if missing lowercase
    
    MOV AL, [tempDigit]
    CMP AL, 0
    JE missing_digit             ; Retry if missing digit
    
    ; All checks passed
    MOV EDX, OFFSET validMessage
    CALL WriteString
    RET
ValidatePassword ENDP
```

### Encryption Implementation

```assembly
encryption PROC
    PUSHAD                       ; Save all registers
    MOV ECX, 20                  ; Max password length
    MOV ESI, OFFSET password     ; Point to password buffer
    
L1:
    MOV AL, [ESI]                ; Load character
    CMP AL, 0                    ; Check for null terminator
    JE DoneEncrypt               ; Exit if end of string
    
    XOR AL, KEY                  ; Step 1: XOR with key (239)
    ROL AL, 5                    ; Step 2: Rotate left 5 bits
    
    MOV [ESI], AL                ; Store encrypted character
    INC ESI                      ; Move to next character
    LOOP L1                      ; Continue until ECX = 0
    
DoneEncrypt:
    POPAD                        ; Restore all registers
    RET
encryption ENDP
```

### Decryption Implementation

```assembly
decryption PROC
    PUSHAD                       ; Save all registers
    MOV ECX, 20                  ; Max password length
    MOV ESI, OFFSET password     ; Point to password buffer
    
L2:
    MOV AL, [ESI]                ; Load encrypted character
    CMP AL, 0                    ; Check for null terminator
    JE DoneDecrypt               ; Exit if end of string
    
    ROR AL, 5                    ; Step 1: Rotate right 5 bits
    XOR AL, KEY                  ; Step 2: XOR with key (239)
    
    MOV [ESI], AL                ; Store decrypted character
    INC ESI                      ; Move to next character
    LOOP L2                      ; Continue until ECX = 0
    
DoneDecrypt:
    POPAD                        ; Restore all registers
    RET
decryption ENDP
```

### File Write Operation

```assembly
WriteOriginalToFile PROC
    PUSHAD
    
    ; Create/open file
    MOV EDX, OFFSET filename     ; "coal1.txt"
    CALL CreateOutputFile
    CMP EAX, INVALID_HANDLE_VALUE
    JE FileError
    
    ; Write password data
    MOV EDX, OFFSET password     ; Data source
    MOV ECX, 20                  ; Number of bytes
    CALL WriteToFile
    
    ; Write comma separator
    MOV EDX, OFFSET token        ; ","
    MOV ECX, 1                   ; 1 byte
    CALL WriteToFile
    
    ; Close file
    CALL CloseFile
    JMP ProcEnd
    
FileError:
    MOV EDX, OFFSET errorFileMsg
    CALL WriteString
    
ProcEnd:
    POPAD
    RET
WriteOriginalToFile ENDP
```

---

## 🎓 Educational Value

This project demonstrates:

### Assembly Language Fundamentals
- ✅ Register manipulation and optimization
- ✅ Memory addressing modes (direct, indirect)
- ✅ Stack frame management (PUSHAD/POPAD)
- ✅ Procedure calling conventions
- ✅ Flag register usage for conditionals

### Low-Level Programming Concepts
- ✅ Bitwise operations (XOR, ROL, ROR)
- ✅ String processing without high-level functions
- ✅ Manual buffer management
- ✅ Pointer arithmetic
- ✅ Loop optimization

### System Programming
- ✅ File I/O operations via Windows API
- ✅ Error handling at system level
- ✅ Resource management (file handles)
- ✅ Data persistence mechanisms

### Cryptography Basics
- ✅ Symmetric encryption implementation
- ✅ XOR cipher principles
- ✅ Key-based transformations
- ✅ Encryption/decryption reversibility

### Software Engineering
- ✅ Modular code organization
- ✅ Procedure-based architecture
- ✅ Error handling patterns
- ✅ Input validation strategies

---

## 📁 File Structure

```
Password-Encryption-System/
│
├── password_system.asm         # Main program source
│
├── Output Files/
│   ├── coal1.txt              # Original passwords (plaintext)
│   └── coal2.txt              # Encrypted passwords (ciphertext)
│
├── Dependencies/
│   ├── Irvine32.inc           # I/O library header
│   └── Irvine32.lib           # Compiled library
│
└── README.md                  # This file
```

---

## 🔒 Security Analysis

### Strengths
- ✅ Password complexity enforcement
- ✅ Symmetric encryption for data protection
- ✅ Bit rotation adds cipher complexity
- ✅ File-based persistence

### Limitations
- ⚠️ **XOR Cipher Weakness**: Vulnerable to known-plaintext attacks
- ⚠️ **Fixed Key**: Key (239) is hardcoded in source
- ⚠️ **No Salt**: Same password produces same ciphertext
- ⚠️ **Limited Key Space**: Single-byte key (256 possibilities)
- ⚠️ **Plaintext Storage**: Original passwords saved without protection

### Real-World Considerations
This implementation is **educational only**. Production systems should use:
- Industry-standard algorithms (AES, RSA)
- Secure key management systems
- Password hashing (bcrypt, Argon2)
- Salt and pepper for uniqueness
- Encrypted file storage

---

## 🧪 Testing Examples

### Test Case 1: Valid Password
```
Input: "SecurePass9"
✅ Length: 11 characters (≥8)
✅ Uppercase: S, P
✅ Lowercase: ecureass
✅ Digit: 9
Result: ACCEPTED
```

### Test Case 2: Missing Uppercase
```
Input: "password123"
✅ Length: 11 characters
❌ Uppercase: None
✅ Lowercase: password
✅ Digit: 123
Result: REJECTED (missing uppercase)
```

### Test Case 3: Too Short
```
Input: "Pass1"
❌ Length: 5 characters (<8)
Result: REJECTED (insufficient length)
```

### Test Case 4: Missing Digit
```
Input: "PasswordOnly"
✅ Length: 12 characters
✅ Uppercase: P, O
✅ Lowercase: asswordnly
❌ Digit: None
Result: REJECTED (missing digit)
```

### Encryption/Decryption Test
```
Original:  "Password123"
Encrypted: [binary ciphertext]
Decrypted: "Password123"
Verify: Original == Decrypted ✅
```

---

## 🐛 Known Issues & Limitations

### Buffer Limitations
- Maximum password length: 20 characters
- File read buffer: 100 bytes
- No dynamic memory allocation

### Input Constraints
- No special character requirement
- Single password processing per execution
- No password strength meter (weak/medium/strong)

### File Handling
- Basic error handling only
- No file locking mechanism
- CSV format without proper escaping
- Append-only mode (no update/delete)

### Encryption Weaknesses
- Educational cipher (not cryptographically secure)
- Susceptible to frequency analysis
- No initialization vector (IV)
- Deterministic encryption

### Platform Dependency
- Windows-specific (Irvine32 library)
- x86 architecture only
- MASM32 assembler required

---

## 🔮 Future Enhancements

### Phase 1: Security Improvements
- [ ] Implement AES encryption algorithm
- [ ] Add secure random key generation
- [ ] Include password salting mechanism
- [ ] Hash original passwords (SHA-256)
- [ ] Implement password expiration

### Phase 2: Functionality Extensions
- [ ] Multiple password storage
- [ ] Password update/delete operations
- [ ] Password strength scoring
- [ ] Special character requirements
- [ ] Password history tracking

### Phase 3: Advanced Features
- [ ] Two-factor authentication (2FA)
- [ ] Password recovery mechanism
- [ ] Encrypted database integration
- [ ] User account system
- [ ] Password generation utility

### Phase 4: Optimization
- [ ] Dynamic memory allocation
- [ ] Optimized encryption routines
- [ ] Multi-threading for file I/O
- [ ] Assembly optimization (fewer instructions)
- [ ] Cache-friendly data structures

### Phase 5: Cross-Platform
- [ ] Linux version (NASM assembler)
- [ ] 64-bit architecture support
- [ ] Portable executable format
- [ ] Cross-assembler compatibility

---

## 📊 Performance Metrics

### Time Complexity
- Password Validation: **O(n)** where n = password length
- Encryption: **O(n)** where n = password length
- Decryption: **O(n)** where n = password length
- File Operations: **O(1)** for single write/read

### Space Complexity
- Password Buffer: 20 bytes
- File Read Buffer: 100 bytes
- Validation Flags: 3 bytes
- Total Static Memory: ~300 bytes

### Operation Counts
- Encryption: 2 operations per character (XOR + ROL)
- Decryption: 2 operations per character (ROR + XOR)
- Validation: ~4 comparisons per character

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: "Error in file operation"**
```
Solution: 
- Check file permissions in working directory
- Ensure Irvine32.lib is properly linked
- Verify filename path is correct
```

**Issue 2: Program crashes during input**
```
Solution:
- Ensure input doesn't exceed 20 characters
- Check stack alignment
- Verify BUFMAX constant value
```

**Issue 3: Encryption produces garbage**
```
Solution:
- Verify KEY constant is set to 239
- Check ROL/ROR bit count (should be 5)
- Ensure password buffer isn't corrupted
```

**Issue 4: File contents not displaying**
```
Solution:
- Ensure files are created in correct directory
- Check file handle validity
- Verify ReadFromFile buffer size
```

---

## 📄 License

Educational project created for FAST University Computer Organization and Assembly Language course.

---

## 🙏 Acknowledgments

- **Irvine32 Library**: Kip Irvine for comprehensive Assembly I/O utilities
- **MASM32 Project**: Community-driven assembler toolkit
- **FAST University**: For COAL course structure and guidance
- **Instructor**: [Course Instructor Name] for project requirements

---

## 📚 References

### Assembly Language Resources
- Kip Irvine, *Assembly Language for x86 Processors*
- MASM32 Documentation: http://www.masm32.com/
- Intel® 64 and IA-32 Architectures Software Developer Manuals

### Cryptography Resources
- Bruce Schneier, *Applied Cryptography*
- NIST Cryptographic Standards
- XOR Cipher Analysis Papers

### System Programming
- Windows API Documentation
- File I/O in Assembly Language
- Low-Level System Calls

---

## 🏁 Conclusion

This Password Encryption & Validation System demonstrates practical application of x86 Assembly Language programming, combining low-level system operations, bitwise cryptography, and file I/O. The project showcases the power and precision of Assembly language in security-critical applications while highlighting the importance of proper password management.

The implementation serves as an educational foundation for understanding:
- Machine-level programming concepts
- Encryption algorithm implementation
- System-level resource management
- Secure coding practices at the hardware level

---

**💻 Developed By**

Tasbiha Nasir, Zain Saqib, and Madiha Aslam  
*Computer Organization and Assembly Language Project*  
*FAST University*

---

