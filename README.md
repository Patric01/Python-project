# Python-project
# La apelul API-ului fără parametri suplimentari, acesta va scrie tot conținutul într-un fișier csv și va returna mesajul: "informația a fost scrisă în numele_fisierului.csv" în caz de succes, iar în caz contrat se va returna mesajul "Eroare: Nu am putut scrie informația în numele_fisierului.csv" 
import csv
import requests

def call_api(url):
    try:
        response = requests.get(url)
        if response.status_code == 200:
            return response.json()
        else:
            return None
    except Exception as e:
        print(f"Eroare la apelul API-ului: {e}")
        return None

def write_to_csv(url, file_name):
    api_data = call_api(url)  # Obținem datele de la API
    
   try:
        if api_data:
            with open(file_name, 'w', newline='') as csvfile:
                fieldnames = api_data[0].keys()
                writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
                
  writer.writeheader()
 for row in api_data:
    writer.writerow(row)
            
   return f"Informația a fost scrisă în {file_name}"
        else:
            return f"Eroare: Nu s-au putut obține date de la API pentru a scrie în {file_name}"
    except Exception as e:
        return f"Eroare: Nu am putut scrie informația în {file_name}: {str(e)}"

# Testăm funcția write_to_csv
url = "https://api.exemplu.com/data"
file_name = "date.csv"
result_message = write_to_csv(url, file_name)
print(result_message)
