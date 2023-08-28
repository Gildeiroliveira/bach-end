from fastapi import FastAPI, HTTPException, status
from pydantic import BaseModel

app = FastAPI()

class Animal(BaseModel):
    id: int | None = None
    nome: str
    ano_nascimento: int
    cor:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            str  

prox_id = 3

a1 = Animal(id=1, nome='Rabito', ano_nascimento=2012, cor='Branco')    
a2 = Animal(id=2, nome='Pipoca', ano_nascimento=2015, cor='Cisnza Mesclado')


animais = [a1,a2]


@app.get('/hello')
def hello():
    return {"mensagem": "Olá seja bem-vindo!"}


@app.get('/animais')
def lista_animais():
    return animais #{'mensagem': 'Serão listados aq'}



# aula, 14/08 
# atualização do backend, adicionando o id dos animais, e 
# atualizado o status code 201,204 ,,

@app.post('/animais', status_code=status.HTTP_201_CREATED)
def novo_animal(animal: Animal):
    global prox_id
    animal.id = prox_id
    prox_id +=1
    animais.append(animal)
    return animal

#funcao para buscar animal na lista ...


def buscar_animal_por_id(id: int):
    for animal in animais:
        if animal.id == id:
            return animal
    return None

@app.get('/animais/{id}')
def obeter_um_animal(id: int):
    animal = buscar_animal_por_id(id)

    if animal == None:
        raise HTTPException(status_code=404, detail='animal não localizado')

    return animal

#remove um animal da lista... 

@app.delete('/animais/{id}', status_code=status.HTTP_204_NO_CONTENT)
def remover_um_animal(id: int):
    animal = buscar_animal_por_id(id)

    if not animal:
        raise HTTPException(status_code=404, detail='Animal ñ localizado')
    
    animais.remove(animal)# bach-end
