import os
import shutil
import time
from time import sleep
from datetime import date
from selenium import webdriver
from webdriver_manager.firefox import GeckoDriverManager
from selenium.webdriver.firefox.service import Service
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.common.alert import Alert
from dotenv import load_dotenv

# Carregar variáveis de ambiente (crie um arquivo .env com os dados de login)
load_dotenv()

USUARIO = os.getenv("USUARIO_SISTEMA")
SENHA = os.getenv("SENHA_SISTEMA")

navegador_options = Options()
navegador_options.add_argument('--headless')  # Rodar o navegador em segundo plano

navegador = webdriver.Firefox(options=navegador_options,service=Service(GeckoDriverManager().install()))

def criar_pasta(caminho: str):
    try:
        os.makedirs(caminho, exist_ok=True)
        print(f"Pasta criada (ou já existente): {caminho}")
    except Exception as e:
        print(f"Erro ao criar a pasta: {e}")

def espera(by, xpath, texto_input):
    tentativas = 0
    while tentativas < 300:
        try:
            elemento = navegador.find_element(by, xpath)
            if texto_input.strip():
                elemento.send_keys(texto_input)
            else:
                elemento.click()
            return True
        except:
            try:
                mensagem = navegador.find_element(By.XPATH, "td/table[3]/tbody/tr[1]/td[2]").text
                if "desconhecido" in mensagem:
                    return False
            except:
                pass
            print(f"Aguardando: {tentativas}/300")
            sleep(1)
            tentativas += 1
    return False

def Imovel_acao():
    ativo = True

    while ativo:
        data = date.today()
        dia = f"{data.day:02d}"
        mes = f"{data.month:02d}"
        ano = str(data.year)
        hj = f"{mes}.{ano}"

        print(f'Iniciando Arquivo Imóvel Ação - Dia {dia}\n')

        pasta_download = os.path.expanduser("~/Downloads")

        # Acessar página de login
        navegador.get('https://exemplo.com/login')  # SUBSTITUA PELO LINK FICTÍCIO

        sleep(10)

        # Realizar login
        if not espera(By.XPATH, '//input[@id="usuario"]', USUARIO):
            break
        if not espera(By.XPATH, '//input[@id="senha"]', SENHA):
            break
        if not espera(By.XPATH, '//button[@type="submit"]', ''):
            break

        sleep(10)

        # Acessar página de relatório
        navegador.get('https://exemplo.com/relatorio')  # SUBSTITUA PELO LINK FICTÍCIO

        if not espera(By.XPATH, '//button[@id="baixar_relatorio"]', ''):
            break

        # Aguardar download do relatório
        for i in range(400):
            arquivos = os.listdir(pasta_download)
            for arquivo in arquivos:
                if arquivo.startswith("Imovel_Acoes_") and arquivo.endswith(".xlsx"):
                    for i in range(0,120):
                        num = 120 - i
                        try:
                            Alert(navegador).accept()
                        except:
                            pass
                        print(f"""
                            Arquivo encontrado...
                            Esperando: {num}, para continuarmos""")
                        sleep(1)
                    i = 400
            sleep(1)

        navegador.quit()

        # Mover arquivo
        for arquivo in os.listdir(pasta_download):
            if "Imovel_Acoes" in arquivo and arquivo.endswith(".xlsx"):
                origem = os.path.join(pasta_download, arquivo)
                destino_pasta = os.path.join("G:/Meu Drive/teste/Imovel Ação", ano, hj)
                criar_pasta(destino_pasta)
                destino = os.path.join(destino_pasta, arquivo)
                shutil.move(origem, destino)
                print(f"Arquivo movido para: {destino}")
                ativo = False
    # Se ainda estiver ativo após o loop, fechar o navegador
    if ativo:
        navegador.quit()

if __name__ == "__main__":
    Imovel_acao()
