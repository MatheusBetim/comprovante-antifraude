from flask import Flask, request
import requests

app = Flask(__name__)

@app.route('/')
def index():
    ip = request.headers.get('X-Forwarded-For', request.remote_addr)
    info = requests.get(f'https://ipinfo.io/{ip}/json').json()
    
    cidade = info.get('city', 'Desconhecida')
    regiao = info.get('region', 'Desconhecida')
    pais = info.get('country', 'Desconhecido')
    org = info.get('org', 'Desconhecida')

    print(f"[VISITA] IP: {ip} | Local: {cidade}, {regiao}, {pais} | Operadora: {org}")

    return f'''
    <h1>Comprovante carregado com sucesso!</h1>
    <p><b>IP:</b> {ip}</p>
    <p><b>Local:</b> {cidade}, {regiao}, {pais}</p>
    <p><b>Operadora:</b> {org}</p>
    '''
