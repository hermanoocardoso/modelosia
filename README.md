# Importação de Bibliotecas

import os
from PyPDF2 import PdfReader

# Configurações
diretorio = "C:/Projetos/Nova Pasta"  # Substitua pelo caminho da pasta

# Verifica se o diretório existe
if not os.path.exists(diretorio):
    print(f"Diretório '{diretorio}' não encontrado.")
    exit()

# Função para extrair texto de todas as páginas de um PDF
def extrair_todas_paginas_pdf(caminho_pdf):
    try:
        reader = PdfReader(caminho_pdf)
        texto_completo = ""
        total_paginas = len(reader.pages)
        for i in range(total_paginas):
            texto = reader.pages[i].extract_text()
            texto_completo += f"\n--- Página {i + 1} ---\n{texto}"
        return texto_completo if texto_completo else "Nenhum texto encontrado."
    except Exception as e:
        return f"Erro ao processar {caminho_pdf}: {str(e)}"

# Varredura na pasta
arquivos_pdf = [f for f in os.listdir(diretorio) if f.lower().endswith(".pdf")]

if not arquivos_pdf:
    print("Nenhum arquivo PDF encontrado no diretório.")
    exit()

# Processa cada arquivo PDF
print(f"Processando arquivos PDF na pasta '{diretorio}':\n")
for arquivo in arquivos_pdf:
    caminho_pdf = os.path.join(diretorio, arquivo)
    print(f"Arquivo: {arquivo}")
    texto = extrair_todas_paginas_pdf(caminho_pdf)
    print(f"Texto extraído do documento '{arquivo}':\n{texto}\n{'='*80}")
    
    # Opcional: Salvar o texto extraído em um arquivo de texto
    arquivo_txt = os.path.splitext(caminho_pdf)[0] + ".txt"
    with open(arquivo_txt, "w", encoding="utf-8") as f:
        f.write(texto)
    print(f"Texto salvo em: {arquivo_txt}\n{'-'*50}")

