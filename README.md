# sistema-de-clinica-medica-python-direto-no-console
compiler

import json
import os

# Arquivos de dados
PACIENTES_FILE = "pacientes.json"
MEDICOS_FILE = "medicos.json"
CONSULTAS_FILE = "consultas.json"

# Funções de utilidade
def carregar_dados(arquivo):
    if os.path.exists(arquivo):
        with open(arquivo, "r") as f:
            return json.load(f)
    return []

def salvar_dados(arquivo, dados):
    with open(arquivo, "w") as f:
        json.dump(dados, f, indent=4)

# Funções principais
def cadastrar_paciente():
    pacientes = carregar_dados(PACIENTES_FILE)
    nome = input("Nome do paciente: ")
    idade = input("Idade: ")
    cpf = input("CPF: ")
    paciente = {"id": len(pacientes)+1, "nome": nome, "idade": idade, "cpf": cpf}
    pacientes.append(paciente)
    salvar_dados(PACIENTES_FILE, pacientes)
    print("✅ Paciente cadastrado com sucesso!")

def cadastrar_medico():
    medicos = carregar_dados(MEDICOS_FILE)
    nome = input("Nome do médico: ")
    especialidade = input("Especialidade: ")
    medico = {"id": len(medicos)+1, "nome": nome, "especialidade": especialidade}
    medicos.append(medico)
    salvar_dados(MEDICOS_FILE, medicos)
    print("✅ Médico cadastrado com sucesso!")

def agendar_consulta():
    pacientes = carregar_dados(PACIENTES_FILE)
    medicos = carregar_dados(MEDICOS_FILE)
    consultas = carregar_dados(CONSULTAS_FILE)

    if not pacientes or not medicos:
        print("⚠️ Cadastre ao menos um paciente e um médico antes.")
        return

    print("\n--- Pacientes ---")
    for p in pacientes:
        print(f"{p['id']} - {p['nome']}")
    pid = int(input("ID do paciente: "))

    print("\n--- Médicos ---")
    for m in medicos:
        print(f"{m['id']} - {m['nome']} ({m['especialidade']})")
    mid = int(input("ID do médico: "))

    data = input("Data da consulta (AAAA-MM-DD): ")
    hora = input("Hora (HH:MM): ")

    consulta = {"paciente_id": pid, "medico_id": mid, "data": data, "hora": hora}
    consultas.append(consulta)
    salvar_dados(CONSULTAS_FILE, consultas)
    print("✅ Consulta agendada com sucesso!")

def listar_consultas():
    consultas = carregar_dados(CONSULTAS_FILE)
    pacientes = carregar_dados(PACIENTES_FILE)
    medicos = carregar_dados(MEDICOS_FILE)

    if not consultas:
        print("❌ Nenhuma consulta agendada.")
        return

    print("\n📅 Consultas Agendadas:")
    for c in consultas:
        paciente = next((p['nome'] for p in pacientes if p['id'] == c['paciente_id']), "Desconhecido")
        medico = next((m['nome'] for m in medicos if m['id'] == c['medico_id']), "Desconhecido")
        print(f"{c['data']} às {c['hora']} - {paciente} com Dr. {medico}")

# Menu principal
def menu():
    while True:
        print("\n--- Clínica Médica ---")
        print("1. Cadastrar Paciente")
        print("2. Cadastrar Médico")
        print("3. Agendar Consulta")
        print("4. Listar Consultas")
        print("0. Sair")

        opcao = input("Escolha uma opção: ")

        if opcao == "1":
            cadastrar_paciente()
        elif opcao == "2":
            cadastrar_medico()
        elif opcao == "3":
            agendar_consulta()
        elif opcao == "4":
            listar_consultas()
        elif opcao == "0":
            print("Encerrando...")
            break
        else:
            print("Opção inválida.")

# Início do programa
menu()
