# 🚀 SOLIDAPI - API TypeScript com SOLID, Testes e Arquitetura Limpa

**Uma API Node.js/TypeScript focada em Clean Architecture, SOLID, Testes Unitários e organização por features.**

## 📌 Visão Geral
Projeto desenvolvido como **estudo avançado de arquitetura de software**, aplicando:
- ✅ **SOLID** (Princípios fundamentais para código limpo e escalável)
- ✅ **Package by Feature** (Organização modular por funcionalidade)
- ✅ **Testes Unitários** (Jest/Vitest)
- ✅ **Inversão de Dependência** (Dependency Injection)

## 🛠 Tecnologias Utilizadas
- **Node.js** + **TypeScript**
- **Jest** ou **Vitest** (Testes unitários)
- **Express** (Framework HTTP - opcional)

## 📦 Estrutura do Projeto (Package by Feature)

## 🔍 Princípios SOLID Aplicados (TypeScript)
### 1️⃣ **Single Responsibility Principle (S)**
- `CreateUserUseCase` tem **uma única responsabilidade**: criar usuários e validar existência.
  ```typescript
  class CreateUserUseCase {
    constructor(private usersRepository: IUsersRepository) {}

    async execute(user: User): Promise<void> {
      if (await this.usersRepository.exists(user.email)) {
        throw new Error("User already exists");
      }
      await this.usersRepository.create(user);
    }
  }

### 2️⃣ Liskov Substitution Principle (L)
Depende da interface IUsersRepository, não da implementação:

typescript
interface IUsersRepository {
  exists(email: string): Promise<boolean>;
  create(user: User): Promise<void>;
}
**Qualquer repositório (PostgreSQL, MongoDB, Mock) pode ser injetado.

###3️⃣ Dependency Inversion Principle (D)
Injeção de dependência via interfaces:

typescript
// Implementação concreta (ex: PostgreSQL)
class UsersRepository implements IUsersRepository {
  // ...código do banco de dados
}

// Uso no caso de uso
const usersRepo = new UsersRepository();
const createUserUseCase = new CreateUserUseCase(usersRepo); // ✅

🧪 Testes Unitários (Jest/Vitest)
typescript
import { Mock } from "vitest";
import { CreateUserUseCase } from "./CreateUserUseCase";

describe("CreateUserUseCase", () => {
  it("should not create user if email exists", async () => {
    const mockRepo = {
      exists: vi.fn().mockResolvedValue(true),
      create: vi.fn(),
    } as unknown as IUsersRepository;

    const useCase = new CreateUserUseCase(mockRepo);
    await expect(useCase.execute({ email: "exists@test.com" }))
      .rejects.toThrow("User already exists");
  });
});

🚀 Como Executar
Instalação:

npm install
Configuração:

Crie um arquivo .env baseado no .env.example.

Testes:

npm test
Executar:

npm start
Acesse a documentação: http://localhost:3000/docs

📝 Melhorias Futuras
Adicionar Container DI (ex: TSyringe).

Implementar Logging centralizado.

Integrar Prisma para acesso a dados.

👨‍💻 Autor
[João Antônio Pereira de Araújo Tercius] - GitHub

💡 Nota Técnica:
"Aplicar SOLID em TypeScript reforça que boas práticas são independentes de linguagem. O desafio foi manter a tipagem forte aliada à flexibilidade das interfaces."
