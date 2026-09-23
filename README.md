# Backend (FastAPI)
@app.post("/login")
def login(user: UserLogin):
    if authenticate(user.email, user.password):
        return {"status": "success", "token": generate_token(user)}
    else:
        return {"status": "error", "message": "Credenciais inválidas"}


@app.post("/perfil")
def set_profile(user_id: int, perfil: str, mercados: list, limite_perda: float):
    save_profile(user_id, perfil, mercados, limite_perda)
    return {"status": "perfil configurado"}

@app.post("/operar")
def operar(user_id: int, mercado: str, tipo: str):
    if verificar_limite(user_id):
        executar_ordem(mercado, tipo)
        return {"status": "ordem executada"}
    else:
        return {"status": "erro", "message": "Limite de perda atingido"}
