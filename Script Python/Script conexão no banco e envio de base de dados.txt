import pandas as pd
from sqlalchemy import create_engine

# 1. Configuração da conexão local
# Formato: postgresql://USUARIO:SENHA@localhost:5432/NOME_DO_BANCO
USER = 'postgres'
PASSWORD = '123456'
HOST = 'localhost'
PORT = '5432'
DB_NAME = 'olist_db'  # Nome do banco que você criou no pgAdmin

engine = create_engine(f'postgresql://{USER}:{PASSWORD}@{HOST}:{PORT}/{DB_NAME}')

# Carregar os arquivos CSV
df_Customers = pd.read_csv('df_Customers.csv')
df_OrderItems = pd.read_csv('df_OrderItems.csv')
df_Orders = pd.read_csv('df_Orders.csv')
df_Payments = pd.read_csv('df_Payments.csv')
df_Products = pd.read_csv('df_Products.csv')

# 2. Carregar seus DataFrames limpos (Exemplo de mapeamento)
datasets = {
    'customers': df_Customers,
    'order_items': df_OrderItems,
    'orders': df_Orders,
    'payments': df_Payments,
    'products': df_Products
}

# 3. Exportar cada DataFrame como uma tabela no PostgreSQL
for nome_tabela, df in datasets.items():
    df.to_sql(nome_tabela, engine, if_exists='replace', index=False)
    print(f"Tabela '{nome_tabela}' enviada com sucesso!")