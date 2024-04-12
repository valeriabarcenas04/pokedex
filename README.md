# pokedex
import os
import requests
import json

def buscar_pokemon(nombre):
    url = f"https://pokeapi.co/api/v2/pokemon/{nombre.lower()}"
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        return data
    else:
        return None

def obtener_informacion_pokemon(pokemon_data):
    nombre = pokemon_data['name']
    peso = pokemon_data['weight']
    altura = pokemon_data['height']
    habilidades = [habilidad['ability']['name'] for habilidad in pokemon_data['abilities']]
    tipos = [tipo['type']['name'] for tipo in pokemon_data['types']]
    movimientos = [movimiento['move']['name'] for movimiento in pokemon_data['moves']]
    imagen_url = pokemon_data['sprites']['front_default']
    return {
        'nombre': nombre,
        'peso': peso,
        'altura': altura,
        'habilidades': habilidades,
        'tipos': tipos,
        'movimientos': movimientos,
        'imagen_url': imagen_url
    }

def mostrar_informacion_pokemon(info):
    print(f"Nombre: {info['nombre']}")
    print(f"Peso: {info['peso']}")
    print(f"Altura: {info['altura']}")
    print("Habilidades:", ", ".join(info['habilidades']))
    print("Tipos:", ", ".join(info['tipos']))
    print("Movimientos:", ", ".join(info['movimientos']))
    print("Imagen:")
    print(info['imagen_url'])

def guardar_en_json(info):
    if not os.path.exists('pokedex'):
        os.makedirs('pokedex')
    
    with open(f"pokedex/{info['nombre']}.json", 'w') as file:
        json.dump(info, file, indent=4)

nombre_pokemon = input("Introduce el nombre de un Pokémon: ")
pokemon_data = buscar_pokemon(nombre_pokemon)
if pokemon_data:
    informacion_pokemon = obtener_informacion_pokemon(pokemon_data)
    mostrar_informacion_pokemon(informacion_pokemon)
    guardar_en_json(informacion_pokemon)
else:
    print("¡Error! Pokémon no encontrado.")
