# Projeto: Análise de Documentos Anti-fraude com Azure AI

## Descrição do Projeto
O objetivo deste projeto é utilizar os recursos do Azure AI para a validação de cartões de crédito, reduzindo o risco de fraudes em transações financeiras. A integração com o Azure AI permite realizar uma análise avançada e precisa dos documentos, garantindo maior segurança e confiabilidade.

## Funcionalidades
- **Validação de Cartões de Crédito**: Verificação automática da autenticidade dos cartões de crédito.
- **Análise de Documentos**: Uso de algoritmos de machine learning para análise e validação de documentos associados.
- **Detecção de Fraudes**: Identificação de padrões e comportamentos suspeitos que possam indicar fraudes.

## Tecnologias Utilizadas
- **Azure AI**: Serviços de IA da Microsoft para análise e processamento de dados.
- **Python**: Linguagem de programação para desenvolvimento dos algoritmos de validação.
- **API REST**: Integração e comunicação entre os diferentes componentes do sistema.

import streamlit as st
from services.blob_service import upload_blob
from services.credit_card_service import analize_credit_card

def configure_interface():
    st.title("Upload de Arquivo DIO - Desafio 1 - Azure - Fake Docs")
    upload_file = st.file_iploader("Escolha uma arquivo", type=["png", "jpg", "jpeg"])

    if upload_file is not None:
        fileName = upload_file.name
        blob_url = upload_blob(upload_file, fileName)
        if blob_url:
            st.write(f"Arquivo {fileName} enviar com sucesso para o Azure Blob Storage")
            credit_card_info = analize_credit_card(blob_url)
            show_image_and_validation(blob_url, credit_card_info)
        else:
            st.write(f"Erro ao enviar o arquivo {fileName} para o Azure Blob Storage")

def show_image_and_validation(blob_url, credit_card_info):
    st.image(blob_url, caption="imagem enviada", use_column_width=True)
    st.write("Resultado da validação:")
    if credit_card_info and credit_card_info["card_name"]:
        st.markdown(f"<h1 style='color: green;'>Cartão Válido</h1>")
        st.write(f"Nome do Titular: {credit_card_info['card_name']}")
        st.write(f"Banco Emissor: {credit_card_info['bank_name']}")
        st.write(f"Data Válidade: {credit_card_info['expiry_date']}")
    else:
        st.markdown(f"<h1 style='color: red;'>Cartão Inválido</h1>", unsafe_allow_html=True)
        st.write("Este não é um cartão de crédito válido.")


    if __name__ == "__main__":
        configure_interface()
