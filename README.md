# Group Name: SYDN 28
# Group Members:4
# Name MISHOK SAHA - ID S398552
# Name Md FAYSAL AHMED - ID S400482
# Name DOREEN DEE - ID S400212
# Name ABDULLA KHAN - ID S111111

def encrypt_char(ch, shift1, shift2):
    if 'a' <= ch <= 'm':
        shift = shift1 * shift2
        start = ord('a')
        new_ch = chr((ord(ch) - start + shift) % 26 + start)
        return "{L1}" + new_ch

    elif 'n' <= ch <= 'z':
        shift = shift1 + shift2
        start = ord('a')
        new_ch = chr((ord(ch) - start - shift) % 26 + start)
        return "{L2}" + new_ch

    elif 'A' <= ch <= 'M':
        shift = shift1
        start = ord('A')
        new_ch = chr((ord(ch) - start - shift) % 26 + start)
        return "{U1}" + new_ch

    elif 'N' <= ch <= 'Z':
        shift = shift2 ** 2
        start = ord('A')
        new_ch = chr((ord(ch) - start + shift) % 26 + start)
        return "{U2}" + new_ch

    else:
        return ch


def decrypt_marked_char(marker, ch, shift1, shift2):
    if marker == "{L1}":
        shift = shift1 * shift2
        start = ord('a')
        return chr((ord(ch) - start - shift) % 26 + start)

    elif marker == "{L2}":
        shift = shift1 + shift2
        start = ord('a')
        return chr((ord(ch) - start + shift) % 26 + start)

    elif marker == "{U1}":
        shift = shift1
        start = ord('A')
        return chr((ord(ch) - start + shift) % 26 + start)

    elif marker == "{U2}":
        shift = shift2 ** 2
        start = ord('A')
        return chr((ord(ch) - start - shift) % 26 + start)

    return ch


def encrypt_text(text, shift1, shift2):
    result = ""
    for ch in text:
        result += encrypt_char(ch, shift1, shift2)
    return result


def decrypt_text(text, shift1, shift2):
    result = ""
    i = 0

    while i < len(text):
        if text[i:i+4] in ["{L1}", "{L2}", "{U1}", "{U2}"]:
            marker = text[i:i+4]
            if i + 4 < len(text):
                ch = text[i + 4]
                result += decrypt_marked_char(marker, ch, shift1, shift2)
                i += 5
            else:
                i += 4
        else:
            result += text[i]
            i += 1

    return result


def encrypt_file(shift1, shift2):
    with open("raw_text.txt", "r", encoding="utf-8") as file:
        text = file.read()

    encrypted = encrypt_text(text, shift1, shift2)

    with open("encrypted_text.txt", "w", encoding="utf-8") as file:
        
