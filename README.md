/* primeira projeção de Banco de Dados, empresa de transporte ded ônibus chamada "Ok Viagens" */
/*Script conta com criação de tabelas, inserts nas tabelas e consultas às tabelas */



CREATE DATABASE ok_viagens;

USE ok_viagens;



-- Tabela cidades
CREATE TABLE cidades(
  id_cidade INT NOT NULL,
  nome VARCHAR(60) NULL DEFAULT NULL,
  uf CHAR(2) NULL DEFAULT NULL,
  PRIMARY KEY (id_cidade)
);

-- Tabela fabricantes
CREATE TABLE fabricantes(
  id_fabricante INT NOT NULL,
  FABRICANTE VARCHAR(60) NULL DEFAULT NULL,
  PRIMARY KEY (id_fabricante)
);

-- Tabela funcao
CREATE TABLE funcao(
  id_funcao INT NOT NULL,
  nome VARCHAR(60) NULL DEFAULT NULL,
  PRIMARY KEY (id_funcao)
);

-- Tabela funcionarios
CREATE TABLE funcionarios(
  id_funcionario INT NOT NULL,
  nome VARCHAR(60) NULL DEFAULT NULL,
  data_nascimento DATE NULL DEFAULT NULL,
  cpf CHAR(11) NULL DEFAULT NULL,
  fk_id_funcao INT NOT NULL,
  PRIMARY KEY (id_funcionario),
  FOREIGN KEY (fk_id_funcao) REFERENCES funcao(id_funcao)
);

-- Tabela linhas
CREATE TABLE linhas(
  id_linha INT NOT NULL,
  nome VARCHAR(60) NULL DEFAULT NULL,
  tempo TIME NULL DEFAULT NULL,
  PRIMARY KEY (id_linha)
);

-- Tabela modelos
CREATE TABLE modelos(
  id_modelo INT NOT NULL,
  modelo VARCHAR(60) NULL DEFAULT NULL,
  fk_id_fabricante INT NOT NULL,
  PRIMARY KEY (id_modelo),
  FOREIGN KEY (fk_id_fabricante) REFERENCES fabricantes(id_fabricante)
);

-- Tabela onibus
CREATE TABLE onibus(
  id_onibus INT NOT NULL,
  numero_lugares INT NULL DEFAULT NULL,
  ano_fabricacao INT NULL DEFAULT NULL,
  numero INT NULL DEFAULT NULL,
  fk_id_modelo INT NOT NULL,
  PRIMARY KEY (id_onibus),
  FOREIGN KEY (fk_id_modelo) REFERENCES modelos(id_modelo)
);

-- Tabela passageiro
CREATE TABLE passageiro(
  id_passageiro INT NOT NULL,
  nome VARCHAR(60) NULL DEFAULT NULL,
  data_nascimento DATE NULL DEFAULT NULL,
  telefone VARCHAR(15) NULL DEFAULT NULL,
  email VARCHAR(60) NULL DEFAULT NULL,
  numero_documento VARCHAR(15) NULL DEFAULT NULL,
  PRIMARY KEY (id_passageiro)
);

-- Tabela viagens
CREATE TABLE viagens(
  id_viagem INT NOT NULL,
  data_hora_saida TIMESTAMP NULL DEFAULT NULL,
  data_hora_chegada TIMESTAMP NULL DEFAULT NULL,
  num_lugares_disp INT NULL DEFAULT NULL,
  valor DECIMAL(15,2) NULL DEFAULT NULL,
  fk_id_linha INT NOT NULL,
  fk_id_onibus INT NOT NULL,
  PRIMARY KEY (id_viagem),
  FOREIGN KEY (fk_id_linha) REFERENCES linhas(id_linha),
  FOREIGN KEY (fk_id_onibus) REFERENCES onibus(id_onibus)
);

-- Tabela passagens
CREATE TABLE passagens(
  id_passagem INT NOT NULL,
  data_hora_emissao TIMESTAMP NULL DEFAULT NULL,
  data_hora_saida TIMESTAMP NULL DEFAULT NULL,
  valor DECIMAL(15,2) NULL DEFAULT NULL,
  id_viagem INT NULL DEFAULT NULL,
  id_funcionario INT NULL DEFAULT NULL,
  id_passageiro INT NULL DEFAULT NULL,
  id_cidade_destino INT NOT NULL,
  id_cidade_origem INT NOT NULL,
  PRIMARY KEY (id_passagem),
  FOREIGN KEY (id_funcionario) REFERENCES funcionarios(id_funcionario),
  FOREIGN KEY (id_passageiro) REFERENCES passageiro(id_passageiro),
  FOREIGN KEY (id_cidade_destino) REFERENCES cidades(id_cidade),
  FOREIGN KEY (id_cidade_origem) REFERENCES cidades(id_cidade),
  FOREIGN KEY (id_viagem) REFERENCES viagens(id_viagem)
);

-- Tabela trajetos
CREATE TABLE trajetos(
  tempo TIME NULL DEFAULT NULL,
  tipo VARCHAR(30) NULL DEFAULT NULL,
  fk_id_linha INT NOT NULL,
  fk_id_cidade INT NOT NULL,
  PRIMARY KEY (fk_id_linha, fk_id_cidade),
  FOREIGN KEY (fk_id_linha) REFERENCES linhas(id_linha),
  FOREIGN KEY (fk_id_cidade) REFERENCES cidades(id_cidade)
);

-- Inserts na tabela cidades
INSERT INTO cidades (id_cidade, nome, uf) VALUES 
(1, 'São Paulo', 'SP'), 
(2, 'Rio de Janeiro', 'RJ'), 
(3, 'Belo Horizonte', 'MG'), 
(4, 'Porto Alegre', 'RS'), 
(5, 'Curitiba', 'PR'),
(6, 'Salvador', 'BA'),
(7, 'Fortaleza', 'CE'),
(8, 'Brasília', 'DF'),
(9, 'Manaus', 'AM'),
(10, 'Recife', 'PE');

SELECT * FROM cidades;

-- Inserts na tabela fabricantes
INSERT INTO fabricantes (id_fabricante, nome) VALUES
(1, 'Mercedes-Benz'),
(2, 'Volvo'),
(3, 'Scania'),
(4, 'Marcopolo'),
(5, 'Volkswagen'),
(6, 'Neobus'),
(7, 'Iveco'),
(8, 'Comil'),
(9, 'Agrale'),
(10, 'BYD');

SELECT * FROM fabricantes;

-- Inserts na tabela funcao
INSERT INTO funcao (id_funcao, nome) VALUES
(1, 'Motorista'),
(2, 'Cobrador'),
(3, 'Mecânico'),
(4, 'Supervisor'),
(5, 'Gerente'),
(6, 'Ajudante'),
(7, 'Auxiliar Administrativo'),
(8, 'Vendedor de Passagem'),
(9, 'Diretor'),
(10, 'Fiscal');

SELECT * FROM funcao;

-- Inserts na tabela funcionarios
INSERT INTO funcionarios (id_funcionario, nome, data_nascimento, cpf, fk_id_funcao) VALUES
(1, 'João Silva', '1985-06-15', '12345678901', 1),
(2, 'Maria Santos', '1990-03-22', '98765432100', 2),
(3, 'Carlos Pereira', '1979-09-13', '45678912345', 3),
(4, 'Ana Oliveira', '1988-11-05', '32165498700', 4),
(5, 'Pedro Souza', '1992-02-14', '85296374152', 5),
(6, 'Fernanda Costa', '1986-07-25', '14725836900', 6),
(7, 'Luiz Ferreira', '1975-12-09', '36925814700', 7),
(8, 'Roberto Lima', '1993-04-18', '95175385263', 8),
(9, 'Juliana Alves', '1994-10-23', '15975348621', 9),
(10, 'Paulo Garcia', '1991-01-30', '35795165482', 10);

SELECT * FROM funcionarios;

-- Inserts na tabela linhas
INSERT INTO linhas (id_linha, nome, tempo) VALUES
(1, 'Linha SP-RJ', '06:30:00'),
(2, 'Linha BH-RJ', '07:15:00'),
(3, 'Linha POA-CTBA', '09:45:00'),
(4, 'Linha SP-CTBA', '05:50:00'),
(5, 'Linha SSA-FOR', '08:20:00'),
(6, 'Linha BSB-MAO', '10:30:00'),
(7, 'Linha MAO-RCE', '11:50:00'),
(8, 'Linha BSB-SP', '13:40:00'),
(9, 'Linha BSB-RJ', '14:25:00'),
(10, 'Linha SP-BH', '06:00:00');

SELECT * FROM linhas;

-- Inserts na tabela modelos
INSERT INTO modelos (id_modelo, nome, fk_id_fabricante) VALUES
(1, 'Série 500', 1),
(2, 'Série 700', 2),
(3, 'Série K', 3),
(4, 'Paradiso', 4),
(5, 'Série G', 1),
(6, 'Série F', 2),
(7, 'Urbanus', 4),
(8, 'Série 400', 5),
(9, 'Neobus N10', 6),
(10, 'BYD E-Bus', 10);

SELECT * FROM modelos;

-- Inserts na tabela onibus
INSERT INTO onibus (id_onibus, numero_lugares, ano_fabricacao, numero, fk_id_modelo) VALUES
(1, 45, 2018, 101, 1),
(2, 40, 2019, 102, 2),
(3, 48, 2017, 103, 3),
(4, 50, 2020, 104, 4),
(5, 42, 2016, 105, 5),
(6, 44, 2021, 106, 6),
(7, 47, 2019, 107, 7),
(8, 49, 2022, 108, 8),
(9, 46, 2018, 109, 9),
(10, 51, 2023, 110, 10);

SELECT * FROM onibus;

-- Inserts na tabela passageiro
INSERT INTO passageiro (id_passageiro, nome, data_nascimento, telefone, email, numero_documento) VALUES
(1, 'Marcos Dias', '1980-01-15', '11987654321', 'marcos@gmail.com', 'RG123456'),
(2, 'Clara Mendes', '1995-03-10', '21965478932', 'clara@gmail.com', 'RG654321'),
(3, 'Paulo Lima', '1992-05-18', '31974125896', 'paulo@hotmail.com', 'RG987654'),
(4, 'Fernanda Carvalho', '1993-07-30', '41925896314', 'fernanda@yahoo.com', 'RG321654'),
(5, 'Ricardo Oliveira', '1990-12-02', '51975395126', 'ricardo@gmail.com', 'RG147258'),
(6, 'Amanda Souza', '1987-09-14', '61987412589', 'amanda@hotmail.com', 'RG369258'),
(7, 'Rafael Costa', '1989-02-28', '71963587412', 'rafael@yahoo.com', 'RG951753'),
(8, 'Lucas Nunes', '1994-08-12', '81978541236', 'lucas@gmail.com', 'RG159753'),
(9, 'Carla Braga', '1985-04-21', '91962478512', 'carla@hotmail.com', 'RG357951'),
(10, 'Julio Almeida', '1991-06-08', '31978451236', 'julio@gmail.com', 'RG753951');

SELECT * FROM passageiro;

-- Inserts na tabela viagens
INSERT INTO viagens (id_viagem, data_hora_saida, data_hora_chegada, 
num_lugares_disp, valor, fk_id_linha, fk_id_onibus) VALUES
(1, '2024-09-15 08:00:00', '2024-09-15 14:30:00', 10, 150.00, 1, 1),
(2, '2024-09-16 09:00:00', '2024-09-16 15:30:00', 15, 200.00, 2, 2),
(3, '2024-09-17 07:30:00', '2024-09-17 14:00:00', 8, 175.00, 3, 3),
(4, '2024-09-18 10:00:00', '2024-09-18 16:30:00', 5, 220.00, 4, 4),
(5, '2024-09-19 08:45:00', '2024-09-19 15:15:00', 12, 180.00, 5, 5),
(6, '2024-09-20 06:30:00', '2024-09-20 13:00:00', 7, 190.00, 6, 6),
(7, '2024-09-21 11:00:00', '2024-09-21 17:30:00', 4, 210.00, 7, 7),
(8, '2024-09-22 09:15:00', '2024-09-22 15:45:00', 6, 230.00, 8, 8),
(9, '2024-09-23 07:45:00', '2024-09-23 14:15:00', 9, 240.00, 9, 9),
(10, '2024-09-24 08:30:00', '2024-09-24 15:00:00', 3, 250.00, 10, 10);

SELECT * FROM viagens;

-- Inserts na tabela passagens
INSERT INTO passagens (id_passagem, data_hora_emissao, data_hora_saida, valor, id_viagem,
 id_funcionario, id_passageiro, id_cidade_destino, id_cidade_origem) VALUES
(1, '2024-09-01 12:30:00', '2024-09-15 08:00:00', 150.00, 1, 1, 1, 1, 2),
(2, '2024-09-02 13:45:00', '2024-09-16 09:00:00', 200.00, 2, 2, 2, 3, 4),
(3, '2024-09-03 14:00:00', '2024-09-17 07:30:00', 175.00, 3, 3, 3, 5, 6),
(4, '2024-09-04 15:30:00', '2024-09-18 10:00:00', 220.00, 4, 4, 4, 7, 8),
(5, '2024-09-05 16:45:00', '2024-09-19 08:45:00', 180.00, 5, 5, 5, 9, 10),
(6, '2024-09-06 17:00:00', '2024-09-20 06:30:00', 190.00, 6, 6, 6, 1, 2),
(7, '2024-09-07 18:30:00', '2024-09-21 11:00:00', 210.00, 7, 7, 7, 3, 4),
(8, '2024-09-08 19:45:00', '2024-09-22 09:15:00', 230.00, 8, 8, 8, 5, 6),
(9, '2024-09-09 20:00:00', '2024-09-23 07:45:00', 240.00, 9, 9, 9, 7, 8),
(10, '2024-09-10 21:30:00', '2024-09-24 08:30:00', 250.00, 10, 10, 10, 9, 10);

SELECT * FROM passagens;

-- Inserts na tabela trajetos
INSERT INTO trajetos (tempo, tipo, fk_id_linha, fk_id_cidade) VALUES
('06:00:00', 'Direto', 1, 1),
('06:30:00', 'Direto', 1, 2),
('07:00:00', 'Parcial', 2, 3),
('07:15:00', 'Direto', 2, 4),
('08:45:00', 'Direto', 3, 5),
('09:00:00', 'Parcial', 3, 6),
('09:30:00', 'Direto', 4, 7),
('10:00:00', 'Direto', 4, 8),
('11:30:00', 'Parcial', 5, 9),
('11:45:00', 'Direto', 5, 10),
('12:00:00', 'Direto', 6, 1),
('12:30:00', 'Parcial', 6, 2),
('13:45:00', 'Direto', 7, 3),
('14:15:00', 'Direto', 7, 4),
('14:45:00', 'Parcial', 8, 5);

SELECT * FROM trajetos;

-- Consulta para mostrar a quantidade de funcionários por função
SELECT f.nome AS funcao, COUNT(func.id_funcionario) AS total_funcionarios
FROM funcionarios func
JOIN funcao f ON func.fk_id_funcao = f.id_funcao
GROUP BY f.nome
ORDER BY total_funcionarios DESC;

-- Soma do valor das passagens vendidas por linha
SELECT l.nome AS linha, SUM(p.valor) AS receita_total
FROM passagens p
JOIN viagens v ON p.id_viagem = v.id_viagem
JOIN linhas l ON v.fk_id_linha = l.id_linha
GROUP BY l.nome
ORDER BY receita_total DESC;

-- Quantidade de viagens por cidade (destino)
SELECT c.nome AS cidade_destino, COUNT(p.id_passagem) AS total_passagens
FROM passagens p
JOIN cidades c ON p.id_cidade_destino = c.id_cidade
GROUP BY c.nome
ORDER BY total_passagens DESC;

-- Consulta para ver quais são as características do ônibus
SELECT FABRICANTE, MODELO, ANO_FABRICACAO, NUMERO, NUMERO_LUGARES
FROM fabricantes
INNER JOIN MODELOS ON ID_FABRICANTE = id_modelo 
INNER JOIN ONIBUS ON ID_ONIBUS = ID_MODELO;

-- CONSULTA PARA VER QUAL ONIBUS CADA FUNCIONÁRIO CORRESPONDE
SELECT FABRICANTE, MODELO, NUMERO, NOME 
FROM FABRICANTES 
INNER JOIN MODELOS ON ID_FABRICANTE = ID_MODELO 
INNER JOIN ONIBUS ON ID_ONIBUS = ID_MODELO 
INNER JOIN FUNCIONARIOS ON ID_FUNCIONARIO = ID_ONIBUS;


