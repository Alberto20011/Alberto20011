import requests
from bs4 import BeautifulSoup
import pandas as pd

# URLs to be compared
urls = {
    "Universidad de Almería": "https://www.ual.es/universidad/secretaria-general/normativa-universitaria/planes-de-inclusion",
    "Universidad de Cádiz": "https://inclusion.uca.es/normativa-accesibilidad/",
    "Universidad de Córdoba": "https://www.uco.es/servicios/sad/novedades/64-ultimas-noticias/1890-programa-daciu.html",
    "Universidad de Granada": "https://canal.ugr.es/evento/presentacion-del-i-plan-sobre-accesibilidad",
    "Universidad de Huelva": "https://www.uhu.es/atencion-personas-discapacidad",
    "Universidad del País Vasco": "https://www.ehu.eus/es/web/discapacidades/normativa",
    "Universidad de Oviedo": "https://www.uniovi.es/recursos/inclusion",
    "Universidad Politécnica de Cartagena": "https://www.upct.es/universidad/normativa",
    "Universidad Católica San Antonio de Murcia": "https://www.ucam.edu/normativa",
    "Universidad de Murcia": "https://www.um.es/web/discapacidades/normativa"
}

# Function to extract text from URLs
def extract_text_from_url(url):
    try:
        response = requests.get(url)
        response.raise_for_status()
        soup = BeautifulSoup(response.content, 'html.parser')
        paragraphs = soup.find_all('p')
        text_content = " ".join([p.get_text() for p in paragraphs])
        return text_content
    except requests.RequestException as e:
        return str(e)

# Extract text for each URL
url_texts = {name: extract_text_from_url(url) for name, url in urls.items()}

# Create a DataFrame for the comparison
df_comparison = pd.DataFrame(url_texts.items(), columns=["Universidad", "Contenido"])

# Save the comparison to a CSV file
df_comparison.to_csv("comparacion_politicas_inclusivas.csv", index=False)

# Display the DataFrame
print(df_comparison)

