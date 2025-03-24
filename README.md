# Projeto-L-gico-de-banco-de-dados-do-zero
-- Projeto Lógico de banco de dados do zero 

CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    telefone VARCHAR(15),
    email VARCHAR(100)
);

CREATE TABLE Funcionario (
    id_funcionario INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cargo VARCHAR(50),
    salario DECIMAL(10, 2)
);

CREATE TABLE Servico (
    id_servico INT PRIMARY KEY,
    descricao VARCHAR(255) NOT NULL,
    preco DECIMAL(10, 2) NOT NULL
);

CREATE TABLE Agendamento (
    id_agendamento INT PRIMARY KEY,
    data_hora DATETIME NOT NULL,
    id_cliente INT,
    id_funcionario INT,
    id_servico INT,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente),
    FOREIGN KEY (id_funcionario) REFERENCES Funcionario(id_funcionario),
    FOREIGN KEY (id_servico) REFERENCES Servico(id_servico)
);

-- Inserção de dados

INSERT INTO Cliente (id_cliente, nome, telefone, email) VALUES 
(1, 'João Silva', '123456789', 'joao@email.com'),
(2, 'Maria Oliveira', '987654321', 'maria@email.com');

INSERT INTO Funcionario (id_funcionario, nome, cargo, salario) VALUES 
(1, 'Carlos Pereira', 'Mecânico', 2500.00),
(2, 'Ana Lima', 'Atendente', 2000.00);

INSERT INTO Servico (id_servico, descricao, preco) VALUES 
(1, 'Troca de Óleo', 100.00),
(2, 'Alinhamento', 80.00);

INSERT INTO Agendamento (id_agendamento, data_hora, id_cliente, id_funcionario, id_servico) VALUES 
(1, '2025-03-01 10:00:00', 1, 1, 1),
(2, '2025-03-02 11:00:00', 2, 2, 2);

-- Consultas SQL

-- 1. Recuperações Simples com SELECT Statement
SELECT * FROM Cliente;

-- 2. Filtros com WHERE Statement
SELECT * FROM Agendamento WHERE data_hora >= '2025-03-01';

-- 3. Atributos Derivados
SELECT id_agendamento, data_hora, 
       CONCAT('Agendamento de ', (SELECT nome FROM Cliente WHERE id_cliente = Agendamento.id_cliente)) AS cliente_nome 
FROM Agendamento;

-- 4. Ordenação com ORDER BY
SELECT * FROM Servico ORDER BY preco DESC;

-- 5. Filtros em Grupos com HAVING Statement
SELECT id_funcionario, COUNT(*) AS total_agendamentos 
FROM Agendamento 
GROUP BY id_funcionario 
HAVING COUNT(*) > 1;

-- 6. Junções entre Tabelas
SELECT a.id_agendamento, c.nome AS cliente_nome, f.nome AS funcionario_nome, s.descricao AS servico
FROM Agendamento a
JOIN Cliente c ON a.id_cliente = c.id_cliente
JOIN Funcionario f ON a.id_funcionario = f.id_funcionario
JOIN Servico s ON a.id_servico = s.id_servico;
