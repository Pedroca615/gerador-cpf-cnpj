# gerador-cpf-cnpj
pip install Pillow
import random
from PIL import Image, ImageDraw, ImageFont

def gerar_cpf():
    """Gera um CPF válido."""
    cpf_base = [random.randint(0, 9) for _ in range(9)]
    
    # Calcula o primeiro dígito verificador
    primeiro_dv = sum((10 - i) * cpf_base[i] for i in range(9)) % 11
    primeiro_dv = 0 if primeiro_dv < 2 else 11 - primeiro_dv
    cpf_base.append(primeiro_dv)

    # Calcula o segundo dígito verificador
    segundo_dv = sum((11 - i) * cpf_base[i] for i in range(10)) % 11
    segundo_dv = 0 if segundo_dv < 2 else 11 - segundo_dv
    cpf_base.append(segundo_dv)

    return ''.join(map(str, cpf_base))

def gerar_cnpj():
    """Gera um CNPJ válido."""
    cnpj_base = [random.randint(0, 9) for _ in range(12)]
    
    # Calcula os dígitos verificadores
    for i in range(2):
        peso = 5 if len(cnpj_base) == 12 else 6
        soma = sum(int(cnpj_base[j]) * (peso - j) for j in range(len(cnpj_base)))
        digito = (soma % 11)
        digito = 0 if digito < 2 else 11 - digito
        cnpj_base.append(digito)
        peso = 6

    return ''.join(map(str, cnpj_base))

def gerar_imagem_cpf(nome, estado, data_nascimento, data_emissao):
    """Gera uma imagem com os dados do CPF."""
    cpf = gerar_cpf()
    img = Image.new('RGB', (400, 200), color='white')
    d = ImageDraw.Draw(img)
    font = ImageFont.load_default()

    d.text((10, 10), f"Nome: {nome}", fill=(0, 0, 0), font=font)
    d.text((10, 30), f"Estado: {estado}", fill=(0, 0, 0), font=font)
    d.text((10, 50), f"Data de Nascimento: {data_nascimento}", fill=(0, 0, 0), font=font)
    d.text((10, 70), f"Data de Emissão: {data_emissao}", fill=(0, 0, 0), font=font)
    d.text((10, 110), f"CPF: {cpf}", fill=(0, 0, 0), font=font)

    img.save(f"{cpf}.jpg")
    print(f"Imagem do CPF gerada: {cpf}.jpg")

def gerar_imagem_cnpj(nome_proprietario, cpf_proprietario, nome_empresa, estado, data_emissao):
    """Gera uma imagem com os dados do CNPJ."""
    cnpj = gerar_cnpj()
    img = Image.new('RGB', (400, 200), color='white')
    d = ImageDraw.Draw(img)
    font = ImageFont.load_default()

    d.text((10, 10), f"Nome Proprietário: {nome_proprietario}", fill=(0, 0, 0), font=font)
    d.text((10, 30), f"CPF Proprietário: {cpf_proprietario}", fill=(0, 0, 0), font=font)
    d.text((10, 50), f"Nome Empresa: {nome_empresa}", fill=(0, 0, 0), font=font)
    d.text((10, 70), f"Estado: {estado}", fill=(0, 0, 0), font=font)
    d.text((10, 90), f"Data de Emissão: {data_emissao}", fill=(0, 0, 0), font=font)
    d.text((10, 130), f"CNPJ: {cnpj}", fill=(0, 0, 0), font=font)

    img.save(f"{cnpj}.jpg")
    print(f"Imagem do CNPJ gerada: {cnpj}.jpg")

# Exemplo de uso
if __name__ == "__main__":
    # Gerar CPF
    gerar_imagem_cpf("João Silva", "SP", "01/01/1990", "04/11/2024")
    
    # Gerar CNPJ
    gerar_imagem_cnpj("Maria Souza", "123.456.789-09", "Empresa Exemplo LTDA", "RJ", "04/11/2024")
