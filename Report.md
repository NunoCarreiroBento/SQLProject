# Documentação para a Avaliação de SQL

## 1. Utilização do MySQL Workbench

Utilização do MySQL Workbench para a realização deste projeto.

## 2. Criação da Base de Dados

Criação da base de dados seguindo o modelo atribuído, com uma pequena alteração na tabela "Bilhetes", onde foi criada uma relação com os funcionários para saber qual funcionário vendeu um determinado bilhete para qual cliente.

```sql
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema Cinema
-- -----------------------------------------------------
DROP SCHEMA IF EXISTS `Cinema`;
CREATE SCHEMA IF NOT EXISTS `Cinema` ;
USE `Cinema` ;

-- -----------------------------------------------------
-- Table `Cinema`.`Filmes`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Cinema`.`Filmes` (
  `ID` INT NOT NULL AUTO_INCREMENT,
  `Título` VARCHAR(45) NOT NULL,
  `Diretor` VARCHAR(45) NOT NULL,
  `Género` VARCHAR(45) NOT NULL,
  `Duração` VARCHAR(6) NOT NULL,
  `Classificação` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`ID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Cinema`.`Clientes`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Cinema`.`Clientes` (
  `ID` INT NOT NULL AUTO_INCREMENT,
  `Nome` VARCHAR(45) NOT NULL,
  `Email` VARCHAR(45) NOT NULL,
  `Telefone` INT(9) NOT NULL,
  PRIMARY KEY (`ID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Cinema`.`Funcionários`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Cinema`.`Funcionários` (
  `ID` INT NOT NULL AUTO_INCREMENT,
  `Nome` VARCHAR(45) NOT NULL,
  `Posição` VARCHAR(45) NOT NULL,
  `Salário` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`ID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Cinema`.`Sessões`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Cinema`.`Sessões` (
  `ID` INT NOT NULL AUTO_INCREMENT,
  `FilmeID` INT NOT NULL,
  `DataHora` DATETIME NOT NULL,
  `Sala` VARCHAR(45) NOT NULL,
  `LugaresDisponiveis` INT NOT NULL,
  PRIMARY KEY (`ID`, `FilmeID`),
  INDEX `fk_Sessões_Filmes_idx` (`FilmeID` ASC) VISIBLE,
  CONSTRAINT `fk_Sessões_Filmes`
    FOREIGN KEY (`FilmeID`)
    REFERENCES `Cinema`.`Filmes` (`ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Cinema`.`Bilhetes`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Cinema`.`Bilhetes` (
  `ID` INT NOT NULL AUTO_INCREMENT,
  `FuncionárioID` INT NOT NULL,
  `ClienteID` INT NOT NULL,
  `SessãoID` INT NOT NULL,
  `Quantidade` VARCHAR(45) NOT NULL,
  `Preço` DECIMAL(5,2) NOT NULL,
  PRIMARY KEY (`ID`, `FuncionárioID`, `ClienteID`, `SessãoID`),
  INDEX `fk_Bilhetes_Funcionários1_idx` (`FuncionárioID` ASC) VISIBLE,
  INDEX `fk_Bilhetes_Clientes1_idx` (`ClienteID` ASC) VISIBLE,
  INDEX `fk_Bilhetes_Sessões1_idx` (`SessãoID` ASC) VISIBLE,
  CONSTRAINT `fk_Bilhetes_Funcionários1`
    FOREIGN KEY (`FuncionárioID`)
    REFERENCES `Cinema`.`Funcionários` (`ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Bilhetes_Clientes1`
    FOREIGN KEY (`ClienteID`)
    REFERENCES `Cinema`.`Clientes` (`ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Bilhetes_Sessões1`
    FOREIGN KEY (`SessãoID`)
    REFERENCES `Cinema`.`Sessões` (`ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

-- -----------------------------------------------------
-- Table `Cinema`.`messages`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Cinema`.`messages` (
  `message` varchar(45))
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

3. Inserção de Dados

Inserção de uma certa quantidade de dados em cada respectiva tabela.

sql

-- Inserir registros na tabela Filmes
INSERT INTO `Cinema`.`Filmes` (`Título`, `Diretor`, `Género`, `Duração`, `Classificação`) VALUES
('The Great Adventure', 'John Smith', 'Ação', '02:00', 'M/12'),
('Romantic Escape', 'Jane Doe', 'Romance', '01:45', 'M/6'),
('Horror Night', 'Eli Roth', 'Terror', '01:30', 'M/18'),
('The Animated Journey', 'Pixar', 'Animação', '01:20', 'M/3'),
('Science Future', 'Steven Spielberg', 'Ficção Científica', '02:30', 'M/12'),
('Family Tales', 'Chris Columbus', 'Comédia', '01:50', 'M/6'),
('Mystery of the Lost City', 'Agatha Christie', 'Mistério', '02:10', 'M/14'),
('Space Odyssey', 'Stanley Kubrick', 'Ficção Científica', '02:20', 'M/12'),
('Historical Drama', 'Ken Burns', 'Drama', '02:00', 'M/12'),
('Epic Saga', 'George Lucas', 'Aventura', '02:30', 'M/16');

-- Inserir registros na tabela Funcionários
INSERT INTO `Cinema`.`Funcionários` (`Nome`, `Posição`, `Salário`) VALUES
('Ana Silva', 'Caixa', '1200'),
('Carlos Sousa', 'Gerente', '2500'),
('Maria Oliveira', 'Atendente', '1100'),
('Pedro Martins', 'Caixa', '1200'),
('João Santos', 'Segurança', '1300'),
('Luís Fernandes', 'Projecionista', '1400'),
('Mariana Costa', 'Atendente', '1100'),
('Fernanda Lima', 'Caixa', '1200'),
('Rafael Alves', 'Segurança', '1300'),
('Tiago Rocha', 'Projecionista', '1400');

-- Inserir registros na tabela Clientes
INSERT INTO `Cinema`.`Clientes` (`Nome`, `Email`, `Telefone`) VALUES
('Lucas Pereira', 'lucas.pereira@mail.com', 912345678),
('Gabriela Lima', 'gabriela.lima@mail.com', 919876543),
('Ricardo Silva', 'ricardo.silva@mail.com', 918273645),
('Juliana Costa', 'juliana.costa@mail.com', 917364258),
('André Santos', 'andre.santos@mail.com', 916485372),
('Mariana Gomes', 'mariana.gomes@mail.com', 915294736),
('Fernando Alves', 'fernando.alves@mail.com', 914857362),
('Sofia Rodrigues', 'sofia.rodrigues@mail.com', 913746582),
('Paulo Ferreira', 'paulo.ferreira@mail.com', 912358476),
('Ana Sousa', 'ana.sousa@mail.com', 911764382);

-- Inserir registros na tabela Sessões
INSERT INTO `Cinema`.`Sessões` (`FilmeID`, `DataHora`, `Sala`, `LugaresDisponiveis`) VALUES
(1, '2024-07-16 18:00:00', 'Sala 1', 100),
(2, '2024-07-16 20:30:00', 'Sala 2', 75),
(3, '2024-07-17 22:00:00', 'Sala 3', 90),
(4, '2024-07-18 16:15:00', 'Sala 1', 100),
(5, '2024-07-19 18:45:00', 'Sala 2', 60),
(6, '2024-07-20 21:00:00', 'Sala 3', 50),
(7, '2024-07-21 17:30:00', 'Sala 1', 100),
(8, '2024-07-22 19:00:00', 'Sala 2', 85),
(9, '2024-07-23 20:45:00', 'Sala 3', 70),
(10, '2024-07-24 15:00:00', 'Sala 1', 100),
(1, '2024-07-25 17:15:00', 'Sala 2', 60),
(2, '2024-07-26 19:45:00', 'Sala 3', 50),
(3, '2024-07-27 21:00:00', 'Sala 1', 100),
(4, '2024-07-28 16:00:00', 'Sala 2', 85),
(5, '2024-07-29 18:30:00', 'Sala 3', 70);

-- Inserir registros na tabela Bilhetes
INSERT INTO `Cinema`.`Bilhetes` (`FuncionárioID`, `ClienteID`, `SessãoID`, `Quantidade`, `Preço`) VALUES
(1, 1, 1, '2', 15.00),
(2, 2, 2, '3', 22.50),
(3, 3, 3, '1', 7.50),
(4, 4, 4, '4', 30.00),
(5, 5, 5, '2', 15.00),
(6, 6, 6, '5', 37.50),
(7, 7, 7, '3', 22.50),
(8, 8, 8, '1', 7.50),
(9, 9, 9, '4', 30.00),
(10, 10, 10, '2', 15.00);

4. Relatórios SQL

Criação de vários relatórios para responder a perguntas sobre os dados.
4.1 Quantos bilhetes foram vendidos por cada funcionário?

sql

SELECT Funcionários.Nome, SUM(Bilhetes.Quantidade) AS TotalBilhetesVendidos
FROM Bilhetes
JOIN Funcionários ON Bilhetes.FuncionárioID = Funcionários.ID
GROUP BY Funcionários.Nome;

4.2 Qual é o total de receitas geradas por cada filme?

sql

SELECT Filmes.Título, SUM(Bilhetes.Preço) AS ReceitaTotal
FROM Bilhetes
JOIN Sessões ON Bilhetes.SessãoID = Sessões.ID
JOIN Filmes ON Sessões.FilmeID = Filmes.ID
GROUP BY Filmes.Título;

4.3 Quantos clientes compraram bilhetes para cada filme?

sql

SELECT Filmes.Título, COUNT(DISTINCT Bilhetes.ClienteID) AS TotalClientes
FROM Bilhetes
JOIN Sessões ON Bilhetes.SessãoID = Sessões.ID
JOIN Filmes ON Sessões.FilmeID = Filmes.ID
GROUP BY Filmes.Título;

4.4 Qual funcionário gerou mais receita em vendas de bilhetes?

sql

SELECT Funcionários.Nome, SUM(Bilhetes.Preço) AS ReceitaTotal
FROM Bilhetes
JOIN Funcionários ON Bilhetes.FuncionárioID = Funcionários.ID
GROUP BY Funcionários.Nome
ORDER BY ReceitaTotal DESC
LIMIT 1;

4.5 Qual é a média de bilhetes vendidos por sessão?

sql

SELECT AVG(QuantidadeVendida) AS MediaBilhetesVendidos
FROM (
    SELECT SUM(Bilhetes.Quantidade) AS QuantidadeVendida
    FROM Bilhetes
    GROUP BY Bilhetes.SessãoID
) AS TotalBilhetesPorSessão;

4.6 Qual filme teve a maior média de bilhetes vendidos por sessão?

...sql

SELECT Filmes.Título, AVG(QuantidadeVendida) AS MediaBilhetesVendidos
FROM (
    SELECT Sessões.FilmeID, SUM(Bilhetes.Quantidade) AS QuantidadeVendida
    FROM Bilhetes
    JOIN Sessões ON Bilhetes.SessãoID = Sessões.ID
    GROUP BY Bilhetes.SessãoID
) AS TotalBilhetesPorSessão
JOIN Filmes ON TotalBilhetesPorSessão.FilmeID = Filmes.ID
GROUP BY Filmes.Título
ORDER BY MediaBilhetesVendidos DESC
LIMIT 1;

...