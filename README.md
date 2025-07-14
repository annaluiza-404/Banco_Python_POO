# Banco_Python_POO

class Cliente:
    def __init__(self, nome, nascimento, cpf, endereco):
        self.nome = nome
        self.nascimento = nascimento
        self.cpf = cpf
        self.endereco = endereco

    def __str__(self):
        return f"{self.nome} | CPF: {self.cpf} | Nascimento: {self.nascimento} | Endereço: {self.endereco}"


class ContaBancaria:
    def __init__(self, cliente):
        self.cliente = cliente
        self.saldo = 0.0
        self.depositos = []
        self.saques = []
        self.saques_realizados = 0

    def depositar(self, valor):
        if valor <= 0:
            print("Valor inválido para depósito!")
            return
        self.saldo += valor
        self.depositos.append(valor)
        print(f"Depósito de R${valor:.2f} realizado com sucesso.")

    def sacar(self, valor):
        if self.saques_realizados >= 3:
            print("Limite diário de saques atingido (3 saques).")
            return
        if valor > 500:
            print("Você não pode sacar mais de R$500,00 por vez!")
        elif valor > self.saldo:
            print("Saldo insuficiente.")
        else:
            self.saldo -= valor
            self.saques.append(valor)
            self.saques_realizados += 1
            print(f"Saque de R${valor:.2f} realizado com sucesso.")

    def extrato(self):
        print("\n===== EXTRATO =====")
        print(f"Cliente: {self.cliente.nome}")
        print(f"Saldo atual: R${self.saldo:.2f}")
        print("Depósitos:")
        if not self.depositos:
            print("  Nenhum depósito realizado.")
        else:
            for d in self.depositos:
                print(f"  + R${d:.2f}")
        print("Saques:")
        if not self.saques:
            print("  Nenhum saque realizado.")
        else:
            for s in self.saques:
                print(f"  - R${s:.2f}")
        print("====================\n")


# Lista de clientes e contas
clientes = []
contas = []

# Funções auxiliares

def cadastrar_cliente():
    nome = input('Digite o nome do cliente: ')
    nascimento = input('Digite a data de nascimento (DD/MM/AAAA): ')
    cpf = input('Digite o CPF (somente números): ')

    # Verifica se CPF já está cadastrado
    for cliente in clientes:
        if cliente.cpf == cpf:
            print("CPF já cadastrado.")
            return

    endereco = input('Digite o endereço (rua, número - bairro - cidade/estado): ')
    novo_cliente = Cliente(nome, nascimento, cpf, endereco)
    clientes.append(novo_cliente)
    print(f"\nCliente {nome} cadastrado com sucesso!\n")

def criar_conta():
    cpf = input('Digite o CPF do cliente: ')
    for cliente in clientes:
        if cliente.cpf == cpf:
            # Verifica se já possui conta
            for conta in contas:
                if conta.cliente.cpf == cpf:
                    print("Este cliente já possui uma conta.")
                    return
            nova_conta = ContaBancaria(cliente)
            contas.append(nova_conta)
            print("Conta criada com sucesso!")
            return
    print("Cliente não encontrado. Cadastre o cliente primeiro.")

def acessar_conta():
    cpf = input("Digite o CPF do cliente: ")
    for conta in contas:
        if conta.cliente.cpf == cpf:
            return conta
    print("Conta não encontrada.")
    return None


# MENU PRINCIPAL
while True:
    print("\n====== MENU ======")
    print("1 - Cadastrar Cliente")
    print("2 - Criar Conta Bancária")
    print("3 - Depositar")
    print("4 - Sacar")
    print("5 - Ver Extrato")
    print("6 - Listar Clientes")
    print("7 - Sair")
    opcao = input("Escolha uma opção: ")

    if opcao == "1":
        cadastrar_cliente()
    elif opcao == "2":
        criar_conta()
    elif opcao == "3":
        conta = acessar_conta()
        if conta:
            valor = float(input("Digite o valor para depósito: R$"))
            conta.depositar(valor)
    elif opcao == "4":
        conta = acessar_conta()
        if conta:
            valor = float(input("Digite o valor para saque: R$"))
            conta.sacar(valor)
    elif opcao == "5":
        conta = acessar_conta()
        if conta:
            conta.extrato()
    elif opcao == "6":
        print("\n==== LISTA DE CLIENTES ====")
        if not clientes:
            print("Nenhum cliente cadastrado.")
        else:
            for c in clientes:
                print(c)
        print("===========================\n")
    elif opcao == "7":
        print("Encerrando o sistema bancário. Até logo!")
        break
    else:
        print("Opção inválida! Tente novamente.")
