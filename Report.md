# Documentação para a Avaliação de SQL

---

### 1. Desenho da Base de Dados

Utilização do Mysql Workbench para a realização deste projeto.

---

### 2. Criação da Base de Dados

Criação da base de dados seguindo o modelo atribuido fazendo uma pequena alteração na tabela "Bilhetes", onde foi feito 
uma relação com os funcionário para que podermos saber qual funcionário vendeu certo bilhete para qual cliente.

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
```

---

### 3. Inserção de Dados 

Aqui inserimos uma certa quantidade de dados em cada respetiva tabela.

```sql
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
(1, '2024-07-25 17:15:00', 'Sala 2', 90),
(2, '2024-07-26 20:00:00', 'Sala 3', 75),
(3, '2024-07-27 22:30:00', 'Sala 4', 65),
(4, '2024-07-28 16:45:00', 'Sala 1', 100),
(5, '2024-07-29 19:15:00', 'Sala 2', 55),
(6, '2024-07-30 21:30:00', 'Sala 3', 80),
(7, '2024-07-31 18:00:00', 'Sala 1', 100),
(8, '2024-08-01 20:45:00', 'Sala 2', 70),
(9, '2024-08-02 23:00:00', 'Sala 3', 90),
(10, '2024-08-03 15:30:00', 'Sala 1', 100),
(1, '2024-08-04 18:00:00', 'Sala 2', 80),
(2, '2024-08-05 20:30:00', 'Sala 3', 75),
(3, '2024-08-06 17:00:00', 'Sala 1', 100),
(4, '2024-08-07 19:30:00', 'Sala 2', 60),
(5, '2024-08-08 22:00:00', 'Sala 3', 85),
(2, '2024-08-08 23:00:00', 'Sala 2', 1);

-- Inserir registros na tabela Bilhetes
INSERT INTO `Cinema`.`Bilhetes` (`FuncionárioID`, `ClienteID`, `SessãoID`, `Quantidade`, `Preço`) VALUES
(2, 5, 1, '2', 15.00),
(1, 3, 2, '1', 7.50),
(8, 4, 3, '3', 22.50),
(6, 2, 4, '4', 30.00),
(10, 1, 5, '2', 15.00),
(3, 6, 6, '1', 7.50),
(5, 8, 7, '3', 22.50),
(4, 10, 8, '4', 30.00),
(7, 9, 9, '2', 15.00),
(2, 7, 10, '3', 22.50),
(9, 2, 11, '1', 7.50),
(6, 5, 12, '4', 30.00),
(8, 3, 13, '2', 15.00),
(1, 9, 14, '3', 22.50),
(10, 4, 15, '1', 7.50),
(7, 1, 16, '4', 30.00),
(5, 6, 17, '2', 15.00),
(4, 8, 18, '3', 22.50),
(3, 10, 19, '1', 7.50),
(9, 7, 20, '4', 30.00),
(2, 5, 21, '3', 22.50),
(6, 3, 22, '2', 15.00),
(8, 9, 23, '1', 7.50),
(1, 4, 24, '4', 30.00),
(10, 2, 25, '3', 22.50);

Select * from Bilhetes;
Select * from Clientes;
Select * from Filmes;
Select * from Funcionários;
Select * from Sessões;
```
---

### 4. Consultas SQL

#### 4.1 Aqui listo todos os filmes com os respetivos diretores e géneros

```sql
Select Título, Diretor, Género From Filmes
```

#### 4.2 Aqui listo todos as sessões realizadas num determinado periodo

```sql
Select * from Sessões
where DataHora between 'DataInicial' and 'DataFinal'

Exemplo:

Select * from Sessões
where DataHora between '2024-07-20' and '2024-07-28'
```

#### 4.3 Aqui listo todos os clientes que compraram mais do que um determinado número de Bilhetes

```sql
Select Clientes.Nome, Quantidade, Preço from Bilhetes
join Clientes using (ID)
Where Quantidade > 'Quantidade que desejar'

Exemplo:

Select Clientes.Nome, Quantidade, Preço from Bilhetes
join Clientes using (ID)
Where Quantidade > '2'
```

4.4 Aqui listo todos os Filmes de um determinado género

```sql
Select * from Filmes
where Género = 'Género que escolher'

Exemplo:

Select * from Filmes
where Género = 'Ficção Cientifica'
```
---

### 5.Triggers

#### 5.1 Aqui crio um trigger que atualiza a quantidade de lugares disponiveis numa sessão após a compra de um bilhete.


```sql
drop trigger  if exists Update_Lugares_Disponiveis;

delimiter //
create trigger update_lugares_disponiveis
after insert on Bilhetes
for each row
begin
DECLARE mensagem VARCHAR(45);
    INSERT INTO messages(message) VALUES('update_lugares_disponiveis');
    UPDATE Sessões SET LugaresDisponiveis = LugaresDisponiveis - NEW.Quantidade
    WHERE ID = NEW.SessãoID;
end;
//
```

#### 5.2 Aqui crio um trigger que impede a compra de um bilhete caso a sessão escolhida não tiver lugares disponiveis suficientes.

```sql
drop trigger  if exists insert_bilhetes;

delimiter //
create trigger insert_bilhetes
before insert on Bilhetes
for each row
begin
DECLARE mensagem VARCHAR(45);
DECLARE lugares_disponiveis INT;
    INSERT INTO messages(message) VALUES('insert_bilhetes');
    select LugaresDisponiveis into lugares_disponiveis from Sessões
    where ID = New.SessãoID;
    if lugares_disponiveis < new.Quantidade then
    set mensagem = concat('Não existe ',new.Quantidade,' vaga para esta sessão');
    signal sqlstate '45000' set message_text = mensagem;
end if;
end;
//
```
---

### 6. Stored Procedures

#### 6.1 Aqui crio um stored procedure em que registra uma nova compra de bilhetes.

```sql
DROP PROCEDURE IF EXISTS Insert_Bilhetes;
DELIMITER $$
CREATE PROCEDURE `Insert_Bilhetes`(in _Funcionario int, _Cliente int, _Sessao int, _Quantidade int, _Preco Decimal(5,2))
BEGIN
    insert into Bilhetes (FuncionárioID, ClienteID, SessãoID, Quantidade, Preço) values (_Funcionario, _Cliente, _Sessao, _Quantidade, _Preco);
END$$
DELIMITER ;

call Insert_Bilhetes(3,5,24,1,7.50)
```

#### 6.2 Aqui crio um stored procedure em que atualiza os dados de um cliente.

```sql
DROP PROCEDURE IF EXISTS update_cliente;
DELIMITER $$
CREATE PROCEDURE `update_cliente`(in _ID int, _Nome varchar(45), _Email varchar(50), _Telefone int(9))
BEGIN
    update Clientes set Nome = _Nome, Email = _Email, Telefone = _Telefone
    where ID = _ID;
END$$
DELIMITER ;

call update_cliente(11, 'Raquel Ferreira', 'raquel.ferreira@mail.com', '987654321')
```


#### 6.3 Aqui crio um stored procedure em que calcula o total de bilhetes vendidos num determinado periodo de tempo.

```sql
DROP PROCEDURE IF EXISTS total_bilhetes;
DELIMITER $$
CREATE PROCEDURE `total_bilhetes`(in _Data1 DateTime, _Data2 DateTime)
BEGIN
    select sum(Quantidade) as totalbilhetes from Bilhetes
    join Sessões using (ID)
    where DataHora between _Data1 and _Data2;
END$$
DELIMITER ;

call total_bilhetes('2024-07-26 00:00:00', '2024-07-28 23:59:59')
```

### 7. Cursores

#### 7.1 Aqui crio um cursor que faz um relatório que lista todos os filmes exibidos juntamente com a quantidade de bilhetes vendido num determinado periodo de tempo.

```sql
CREATE TEMPORARY TABLE IF NOT EXISTS TempResults (
    Título VARCHAR(45),
    Género VARCHAR(45),
    Quantidade INT
);

DROP PROCEDURE IF EXISTS Relatório_Filmes;
DELIMITER $$
CREATE PROCEDURE `Relatório_Filmes`( in _data1 datetime, _data2 datetime )
BEGIN
    DECLARE done INT DEFAULT FALSE;

    declare _Titulo varchar(45);
    declare _Género varchar(45);
    declare _Quantidade int;

declare cursor1 cursor for
    SELECT Filmes.Título, Filmes.Género, sum(Bilhetes.Quantidade) as Quantidade
    FROM Bilhetes
    join Sessões on Bilhetes.SessãoID = Sessões.ID
    join Filmes on Sessões.FilmeID = Filmes.ID
    WHERE DataHora between _data1 and _data2
    group by Filmes.Título;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
open cursor1;
read_loop: LOOP
    fetch cursor1 into _Titulo, _Género, _Quantidade;
IF done THEN
LEAVE read_loop;
END IF;
    INSERT INTO TempResults (Título, Género, Quantidade)
    VALUES (_Titulo, _Género, _Quantidade);
END LOOP;
CLOSE cursor1;
    SELECT * FROM TempResults;
    DROP TEMPORARY TABLE IF EXISTS TempResults;
END$$

call Relatório_Filmes('2024-07-26 00:00:00', '2024-07-30 23:59:59');
```