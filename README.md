# Automação de Cadastro de Veículos

Este projeto utiliza Selenium e OpenPyXL para automatizar o processo de leitura de uma planilha Excel contendo informações de veículos e adicioná-los automaticamente a um sistema de CRUD de gerenciamento de veículos hospedado online.

## Funcionalidades

- **Leitura de planilha Excel:** Carrega os dados de uma tabela Excel com informações como modelo, marca e ano dos veículos.
- **Automação de cadastro:** Utiliza Selenium para navegar e interagir com o sistema CRUD, inserindo os dados de forma automática.
- **Processo repetitivo simplificado:** Reduz o tempo e o esforço de cadastrar manualmente os veículos no sistema.

## Tecnologias Utilizadas

- **Python:** Linguagem principal do script.
- **Selenium:** Biblioteca para automação de navegação web e interação com elementos da interface.
- **OpenPyXL:** Biblioteca para leitura e manipulação de arquivos Excel.
- **Chrome WebDriver:** Controla o navegador para execução do script.

## Pré-requisitos

1. **Instalar as dependências:**

   As dependências do projeto estão listadas no arquivo `requirements.txt`. Para instalá-las, use:

   ```bash
   pip install -r requirements.txt
   ```

2. **Configurar o Chrome WebDriver:**

   Certifique-se de que o Chrome WebDriver está instalado e no PATH do sistema. O WebDriver deve ser compatível com a versão do Google Chrome instalada.

3. **Criar a planilha Excel:**

   Crie uma planilha chamada `planilha.xlsx` com as informações dos veículos. Certifique-se de que a planilha possui uma aba chamada `data` e que as colunas estejam organizadas da seguinte forma:

   | Modelo   | Marca     | Ano |
   |----------|-----------|-----|
   | Fiat Uno | Fiat      | 2026|
   | Corolla  | Toyota    | 2024|

## Como Executar o Script

1. **Abra o terminal e execute o script:**

   ```bash
   python script.py
   ```

2. **Automação em execução:**

   - O navegador será iniciado automaticamente pelo Selenium.
   - Acesse o sistema CRUD no endereço [https://rafaelsobral.pythonanywhere.com/](https://rafaelsobral.pythonanywhere.com/).
   - Para cada linha da planilha Excel, o script:
     - Clica no botão `Adicionar`.
     - Preenche os campos `Modelo`, `Marca` e `Ano`.
     - Clica no botão `Enviar`.

3. **Verifique os resultados:**

   Após a execução, os veículos cadastrados estarão disponíveis no sistema.

## Estrutura do Código

```python
from selenium import webdriver 
from selenium.webdriver.common.by import By
from time import sleep 
import openpyxl 

# Configuração do WebDriver
driver = webdriver.Chrome()
driver.get('https://rafaelsobral.pythonanywhere.com/')
sleep(5)

# Carregando a planilha
modelos = openpyxl.load_workbook('./planilha.xlsx')
pagina_modelo = modelos['data']

# Loop para leitura e cadastro dos veículos
for linha in pagina_modelo.iter_rows(min_row=2, values_only=True):
    Modelo, Marca, Ano = linha

    driver.find_element(By.XPATH, "//a[contains(@class, 'btn-success') and text()='Adicionar']").click()
    sleep(5)

    modelo = driver.find_element(By.XPATH, "//input[@id='id_modelo']")
    modelo.send_keys(Modelo)

    marca = driver.find_element(By.XPATH, "//input[@id='id_marca']")
    marca.send_keys(Marca)

    ano = driver.find_element(By.XPATH, "//input[@id='id_ano']")
    ano.send_keys(Ano)

    driver.find_element(By.XPATH, "//button[contains(@class, 'btn-success') and text()='Enviar']").click()
```

## Observações

- **Delay:** O tempo de espera (`sleep`) pode ser ajustado para garantir que o site carregue completamente antes das interações.
- **Erros no cadastro:** Verifique se os elementos HTML não foram alterados no sistema CRUD, o que pode causar falhas no script.

## Licença

Este projeto está licenciado sob a Licença MIT. Veja o arquivo LICENSE para mais detalhes.

