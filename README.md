# Pré-processamento — Projet Thématique EEG et IA

Notebook de pré-processamento de sinais EEG para o projeto temático de EEG e inteligência artificial.

## Conteúdo

O notebook `Pré_processamento_Projet_Themátique_EEG_et_IA.ipynb` reúne três etapas:

1. **Exploração dos sinais:** carregamento dos 500 sinais, organização dos conjuntos A–E, criação da estrutura `MNE`, visualizações temporais e análise espectrale de amplitude.
2. **Filtragem e divisão dos dados:** aplicação de um filtro Butterworth passa-baixas de 40 Hz (ordem 4, fase zero com `filtfilt`), verificações da filtragem e divisão treino/teste com validação cruzada estratificada.
3. **Janelamento e normalização:** criação de janelas, normalização sem vazamento de dados (*data leakage*), preparação de `Dataset`/`DataLoader` do PyTorch e agregação das previsões por sinal.

A divisão dos dados é feita por **sinal inteiro**, antes do janelamento, para evitar que janelas do mesmo sinal apareçam em conjuntos diferentes.

## Dados necessários

Os dados brutos não estão incluídos neste repositório. O notebook espera encontrar os arquivos `.txt` do conjunto Bonn nesta estrutura do Google Drive:

```text
MyDrive/
└── Projet Thématique/
    ├── raw/
    │   ├── A (Z)/
    │   ├── B (O)/
    │   ├── C (N)/
    │   ├── D (F)/
    │   └── E (S)/
    ├── processado/
    └── figuras/
```

Cada uma das cinco pastas `A (Z)` a `E (S)` deve conter 100 arquivos `.txt`.

> **Importante:** o caminho está definido no notebook como `/content/drive/MyDrive/Projet Thématique/raw`. Se o nome ou a localização da pasta no seu Google Drive for diferente, ajuste a variável `path` na primeira etapa.

## Como executar no Google Colab

1. Abra o notebook no Google Colab.
2. Coloque a pasta `Projet Thématique` no diretório `MyDrive` do Google Drive, respeitando a estrutura indicada acima.
3. Execute as células em ordem. A primeira etapa monta o Google Drive.
4. Caso necessário, instale as dependências:

   ```python
   %pip install -r requirements.txt
   ```

   Se o arquivo `requirements.txt` não estiver acessível no ambiente do Colab, instale as bibliotecas diretamente:

   ```python
   %pip install numpy pandas matplotlib scipy scikit-learn mne torch
   ```

5. Para salvar os resultados processados, altere `SALVAR = True` na célula de parâmetros da terceira etapa. Com `SALVAR = False`, o notebook não grava o arquivo final da terceira etapa.

O notebook foi desenvolvido/testado com Python 3.13.18 no ambiente Google Colab. O Colab já fornece a integração `google.colab` usada para montar o Drive.

## Arquivos gerados

Quando as células correspondentes são executadas, o notebook cria:

- `figuras/`: gráficos usados para analisar e documentar o pré-processamento.
- `processado/etapa2_filtrado_split.npz`: sinais filtrados, rótulos, identificadores, índices de treino/teste e folds de validação.
- `processado/etapa3_janelas.npz`: janelas normalizadas, rótulos, identificadores dos sinais e parâmetros de preparação (quando `SALVAR = True`).

Esses arquivos são resultados gerados pelo notebook e não precisam ser versionados no GitHub se forem grandes ou puderem ser reproduzidos a partir dos dados brutos.

## Dependências

As bibliotecas Python utilizadas diretamente no notebook estão listadas em [`requirements.txt`](requirements.txt).

- NumPy e Pandas: manipulação dos dados.
- Matplotlib: visualização.
- SciPy: filtragem e análise espectral.
- scikit-learn: divisão estratificada e validação cruzada.
- MNE: estruturação e análise dos sinais EEG.
- PyTorch: `Dataset`, `DataLoader` e testes das entradas das redes neurais.
