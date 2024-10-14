Este projeto se trata de um gerenciador de arquivos funcional para materia de Sistemas Operacinais solicitado pelo professor a fim de mostrar aos alunos como criar uma pagina utilizando linguagens diferentes, em geral, no gerencimento de entrada e saida de arquivos.


### Documentação do Código Flask

#### Descrição Geral
Este é um aplicativo web básico desenvolvido com o Flask que permite fazer upload, download, visualização, edição e exclusão de arquivos de diferentes tipos, como textos, documentos Word, planilhas e imagens. A aplicação organiza e gerencia esses arquivos no diretório definido para uploads.

#### Dependências
Para executar este código, são necessárias as seguintes bibliotecas:
- `Flask`: Framework web para Python.
- `python-docx`: Para manipulação de arquivos `.docx`.
- `pandas`: Para manipulação e visualização de planilhas.
- `os`: Para manipulação de arquivos e diretórios.
- `flash`: Para mensagens de notificação ao usuário.

#### Configurações Iniciais
```python
app = Flask(__name__)
app.secret_key = 'secret_key'  # Necessário para usar flash messages
UPLOAD_FOLDER = 'uploads'
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
```
O aplicativo é inicializado e configurado para utilizar um diretório específico (`uploads`) onde os arquivos serão armazenados. A `secret_key` é usada para permitir o uso de mensagens `flash` que notificam o usuário sobre ações realizadas.

#### Estrutura do Código

1. **Página Inicial (`index`)**
    - **Rota**: `/`
    - **Método**: `GET`
    - **Descrição**: Exibe uma lista de arquivos armazenados no diretório `UPLOAD_FOLDER` e permite a navegação para outras funcionalidades (upload, download, edição e exclusão).

    ```python
    @app.route('/')
    def index():
        arquivos = os.listdir(app.config['UPLOAD_FOLDER'])
        return render_template('index.html', arquivos=arquivos)
    ```

2. **Upload de Arquivos**
    - **Rota**: `/upload`
    - **Método**: `POST`
    - **Descrição**: Permite ao usuário fazer upload de um arquivo. Se o arquivo for carregado com sucesso, ele será salvo no diretório e uma mensagem de confirmação será exibida.
    
    ```python
    @app.route('/upload', methods=['POST'])
    def upload_file():
        # Código para verificar e salvar arquivo
    ```

3. **Exclusão de Arquivos**
    - **Rota**: `/delete/<filename>`
    - **Método**: `POST`
    - **Descrição**: Remove um arquivo específico do diretório. Se o arquivo existir e for removido, uma mensagem de sucesso é exibida; caso contrário, uma mensagem de erro é mostrada.
    
    ```python
    @app.route('/delete/<filename>', methods=['POST'])
    def delete_file(filename):
        # Código para deletar arquivo
    ```

4. **Download de Arquivos**
    - **Rota**: `/download/<filename>`
    - **Método**: `GET`
    - **Descrição**: Permite ao usuário baixar um arquivo específico do diretório `UPLOAD_FOLDER`.
    
    ```python
    @app.route('/download/<filename>')
    def download_file(filename):
        # Código para download do arquivo
    ```

5. **Identificação do Tipo de Arquivo**
    - **Função**: `get_file_type(filename)`
    - **Descrição**: Determina o tipo de arquivo com base na extensão. Retorna uma string indicando o tipo do arquivo (e.g., `text`, `docx`, `spreadsheet`, `image`, `unsupported`).
    
    ```python
    def get_file_type(filename):
        # Código para identificar o tipo de arquivo
    ```

6. **Edição ou Visualização de Arquivos**
    - **Rota**: `/edit/<filename>`
    - **Método**: `GET`
    - **Descrição**: Abre um arquivo específico para visualização ou edição. O comportamento depende do tipo de arquivo:
        - **Textos (`.txt`, `.md`, etc.)**: Exibe o conteúdo para edição.
        - **Documentos Word (`.docx`)**: Converte o conteúdo para texto e exibe para edição.
        - **Planilhas (`.xlsx`)**: Converte o conteúdo para HTML e exibe para visualização.
        - **Imagens (`.png`, `.jpg`, etc.)**: Exibe a imagem.
    
    ```python
    @app.route('/edit/<filename>', methods=['GET'])
    def edit_file(filename):
        # Código para editar ou visualizar o arquivo
    ```

7. **Salvar Arquivos Editados**
    - **Rota**: `/save/<filename>`
    - **Método**: `POST`
    - **Descrição**: Salva as alterações feitas em arquivos de texto ou documentos Word (`.docx`). Novas edições são aplicadas e salvas no diretório `UPLOAD_FOLDER`.
    
    ```python
    @app.route('/save/<filename>', methods=['POST'])
    def save_file(filename):
        # Código para salvar edições
    ```

8. **Início do Servidor**
    - **Bloco principal**:
        - Cria o diretório `UPLOAD_FOLDER` caso ele não exista e inicia o servidor Flask em modo debug.
    
    ```python
    if __name__ == '__main__':
        if not os.path.exists(UPLOAD_FOLDER):
            os.makedirs(UPLOAD_FOLDER)
        app.run(debug=True)
    ```

#### Considerações Finais
- **Segurança**: A aplicação utiliza mensagens `flash` para fornecer feedback ao usuário e usa uma chave secreta para essa funcionalidade. 
- **Upload Seguro**: Verifique sempre os tipos de arquivos e faça validações para evitar uploads indesejados ou potencialmente perigosos.
- **Tratamento de Erros**: As operações de upload, edição e exclusão possuem mensagens para casos de sucesso e erro.

Essa documentação fornece uma visão geral do funcionamento da aplicação, descrevendo cada rota e função principal que possibilita o gerenciamento básico de arquivos através de uma interface web.
